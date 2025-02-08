+++
title = "Lab 2: IMU"
date = 2025-02-20
weight = 2
[taxonomies]
tags = ["Robotics", "C++", "Sensors", "Python", "Embedded Software", "Microcontroller" ]
+++

### IMU Setup

#### AD0_VAL & Initial Data Observations

The AD0_VAL represents the last bit of the I2C address. In our case, 1 is set as the default and that shouldn’t be changed unless the ADR on the board is closed via solder and then should be set to 0. 

After testing with the example code as well as the lecture 4 code, it appears that the accelerometer and gyroscope data print as expected. Three axis are printed for both sensor as well as the corresponding unit (mg for acceleration and DPF for the gyroscope). As discussed in lecture, in the data you can see accelerations and rotations being tracked but not absolute position; changes in the printed values only occur with movement. Additionally, you can see that the changing values are dependent on the axis being rotated about and the sensor being observed. When rotating around the z-axis you can see that the accelerometer data does not change, however the gyroscope does. When accelerating the board along the x-axis you can see a change in value for the accelerometer x-axis but not the gyroscope. 

<img src="/Fast Robots Media/Lab 2/IMUandArtemis.png" height = 400 alt="Alt text" style="display:block;">
<figcaption>Artemis and IMU Connection with BLUE LED Indicator</figcaption>

### Accelerometer

#### Accelerometer Data to Pitch and Roll Conversion

``` c++
pitch_a = atan2(myICM.accX(), myICM.accZ()) * 180 / M_PI; 
roll_a  = atan2(myICM.accY(), myICM.accZ()) * 180 / M_PI; 
```

<iframe width="450" height="315" src="https://www.youtube.com/embed/JZWDCYHxN64" allowfullscreen></iframe>
<figcaption>Video of IMU Testing - full screen to reduce blur</figcaption>

<div style="display: flex; gap: 10px; justify-content: center; align-items: center; white-space: nowrap; overflow-x: auto;">
    <figure style="text-align: center; margin: 0;">
        <img src="/Fast Robots Media/Lab 2/0 Ouput.png" alt="Alt text">
        <figcaption>0°</figcaption>
    </figure>
    <figure style="text-align: center; margin: 0;">
        <img src="/Fast Robots Media/Lab 2/P -90.png" alt="Alt text">
        <figcaption>Pitch @ -90°</figcaption>
    </figure>
    <figure style="text-align: center; margin: 0;">
        <img src="/Fast Robots Media/Lab 2/P 90.png" alt="Alt text">
        <figcaption>Pitch @ 90°</figcaption>
    </figure>
    <figure style="text-align: center; margin: 0;">
        <img src="/Fast Robots Media/Lab 2/R -90.png" alt="Alt text">
        <figcaption>Roll @ -90°</figcaption>
    </figure>
    <figure style="text-align: center; margin: 0;">
        <img src="/Fast Robots Media/Lab 2/R 90.png" alt="Alt text">
        <figcaption>Roll @ 90°</figcaption>
    </figure>
</div>

As you can see from the frozen frames, the accelerometer is very accurate with vary little variation in angle from the expected value. As a result I do not think it is nessesary to do a two-point calibration.

#### Data Collection and Plotting Code

The following code was used to collect the data in arrays and then use Juypter to pipe the data from the Artemis into a CSV file and graph. The graphs will be shown later in the labs for analysis

Artemis Aruino Code:
```c++
case GET_ACC_READINGS: {
    
    float Millis_Cur2 = 0;
    float pitch_a = 0;
    float roll_a = 0;

    if (!success) {
        return;
    }

    // First, collect all data
    for (int i = 0; i < lenarr; i++) { 
    
    // Ensure new data is available before recording
    while (!myICM.dataReady()) {
        // Wait for new data to be ready
    }
    myICM.getAGMT();

    Millis_Cur2 = millis();
    pitch_a = atan2(myICM.accX(), myICM.accZ()) * 180 / M_PI; 
    roll_a  = atan2(myICM.accY(), myICM.accZ()) * 180 / M_PI; 
    
    millisArray[i] = Millis_Cur2;
    RollArray[i] = roll_a;
    PitchArray[i] = pitch_a;
}

// Then, send the collected data
for (int x = 0; x < lenarr; x++) { 
    tx_estring_value.clear();
    tx_estring_value.append("T:");
    tx_estring_value.append(millisArray[x]);
    tx_estring_value.append(",");
    tx_estring_value.append("P:");
    tx_estring_value.append(PitchArray[x]);
    tx_estring_value.append(",");
    tx_estring_value.append("R:");
    tx_estring_value.append(RollArray[x]);
    tx_characteristic_string.writeValue(tx_estring_value.c_str());
}

break;
}
```


Jupyter Code: 
```python
def notification_handler(sender, data):
   
    try:
        # Decode bytes to string & split by ";" (separating blocks)
        entry = data.decode("utf-8").strip()

        # Split by "," then extract values using fast unpacking
        p, r, t = entry.split(",")
        time_value = float(p[2:])  # Skip "P:"
        pitch = float(r[2:])   # Skip "R:"
        roll = float(t[2:])  # Skip "T:"

        # Check if file is empty, then write headers
        with open("data.csv", "a+") as f:
            f.seek(0)
            if f.read(1) == "":
                f.write("Time,Pitch,Roll\n")
            f.write(f"{time_value},{pitch},{roll}\n")

    except Exception as e:
        print(f"Error parsing data: {e}")

```
```python
ble.start_notify(ble.uuid['RX_STRING'], notification_handler)
print("Started notifications on RX_STRING")
```

```python
# Clear the CSV file before sending the command
with open("data.csv", "w") as f:
    f.write("time,pitch,roll\n")  # Rewriting headers after clearing data

# Send the command to get accelerometer readings
ble.send_command(CMD.GET_ACC_READINGS, "")
```

```python
import pandas as pd
import matplotlib.pyplot as plt

# Load the CSV file
df = pd.read_csv("data.csv")

# Plot Pitch and Roll as separate graphs
fig, axs = plt.subplots(2, 1, figsize=(10, 8), sharex=True)

# Plot Pitch
axs[0].plot(df["time"], df["pitch"], label="Pitch", color="blue")
axs[0].set_ylabel("Pitch (°)")
axs[0].set_title("Pitch vs Time")
axs[0].set_ylim(-250, 250)  # Set y-axis limits
axs[0].legend()
axs[0].grid()

# Plot Roll
axs[1].plot(df["time"], df["roll"], label="Roll", color="red")
axs[1].set_xlabel("Time (ms)")
axs[1].set_ylabel("Roll (°)")
axs[1].set_title("Roll vs Time")
axs[1].set_ylim(-250, 250)  # Set y-axis limits
axs[1].legend()
axs[1].grid()

# Show the plots
plt.tight_layout()
plt.show()
```

#### Fourier Transform and Low Pass Filter Plotting

The raw data shows some noise in the higher frequencies however it is fairly negligable. This is due to the fact that the IMU has a low pass filter implemented already. Regardless, I will add a low pass filter 


When collecting dat in the proximity of the running car, the most noise appeared to be in the range of 0 Hz and 5 Hz; I will make the cutoff at 5 Hz. The lowpass filter effects the output by limiting faster frequencies (which we are defining as noise) from being shown in the data. If the frequency chosen is too small, you will still have unwanted noise in the smaller frequency range. However, if you pick too high of a cutoff frequency you run the risk of ignoring data points that may actually be important and a correct reflection of the robot's movement (maybe a sharp turn on a flip). 

In order to apply a low pass filter I had to calculate my alpha value as **0.0876** using following equations:

$$\alpha = \frac{T}{T + RC}$$

$$ f_c = \frac{1}{2\pi RC} $$

- T = sampling rate
- $f_c$ = cutoff frequency 


<img src="/Fast Robots Media/Lab 2/LPF_FFT_RAW.png" alt="Alt text" style="display:block;">
<figcaption>Raw and Fourier Transform Data with Low Pass Filter - Car in Proximity</figcaption>

<br /> 

<img src="/Fast Robots Media/Lab 2/LPF_FFT_RAW_OSS.png" alt="Alt text" style="display:block;">
<figcaption>Raw and Fourier Transform Data with Low Pass Filter - Hand Osscilations</figcaption>

### Gyroscope



### RC Stunts

<iframe width="450" height="315" src="https://www.youtube.com/embed/Hd9J3dOaupc" allowfullscreen></iframe>
<figcaption>Stunt 1</figcaption>

<iframe width="450" height="315" src="https://www.youtube.com/embed/fK9v2iRqGfE" allowfullscreen></iframe>
<figcaption>Stunt 2</figcaption>

The car is quick at direction changing and accelerating. When spinning it is able to hold its position on the ground without drifting much. Note that the car's speed can not be changed while moving, it can only stop and change directions. 

### Collaboration
I collaborated extensively on this project with [Jack Long](https://jack-d-long.github.io/) and [Trevor Dales](https://trevordales.github.io/). I referenced [Daria's site](https://pages.github.coecis.cornell.edu/dak267/dak267.github.io/#page-top) for code debugging and specific help with `SEND_TIME_DATA` and `GET_TEMP_READINGS`. ChatGPT was heavily used to write plotting code for the Raw, FFT and LPF data. It also helped me write my FFT function as the provided link had some syntax error and missing pictures.