+++
title = "Lab 8: Stunts"
date = 2025-04-08
weight = 5
[taxonomies]
tags = ["Robotics", "C++", "Sensors", "Python", "Embedded Software", "Microcontroller" ]
+++

<img src="/Fast Robots Media/Lab 8/FRCars.png" alt="Alt text" style="display:block;">
<figcaption>Cars 3 Way Tie</figcaption>

## Flip Implementation

The flip code was implemented 

### Bluetooth Data Transmission

The entire flip program runs when called over bluetooth via the Jupyter notebook line: `ble.send_command(CMD.STUNT, "1200")`. The parameter passed in is the distance away from the wall that the car should begin to reverse at. Note that 1200 mm is larger than the distance away from the wall of the mat (300 mm) as the car needs time to slow down before ultimately flipping on the mat. This parameter allows for easy flip tuning for various different starting positions and flip locations.

### Arduino Code
The `STUNT` command is responsible for ensuring the first kalman filter value is stored and  calling the essential `Flip()` and `sendStuntData()` functions. Here is the command:

```c++
stuntStartTime = millis();

// Loop until first kalman value is recieved
while (!kalmanReady) {
Serial.print("waiting for ToF");
if (distanceSensor1.checkForDataReady()) {
    collectTOF();
    getKalmanData();
    mu = { TOF1[0], 0 };

    kalmanReady = 1;
    // stuntSpeed[stuntCounter] = 255;
    stuntCounter = stuntCounter + 1;
}
delay(10);
}

// Stunt logic
Flip();
sendStuntData();
stop();
```

The functions to note are:

-  `Flip()`runs the logic to drive at the wall then flip the robot. While the robot has yet to reach the flip distance, it drives forward at full speed. Once it reaches the target distance, it runs in reverse at full speed for 2300 ms. Note that during both loops, ToF and Kalman data is being collected.

```c++
if (!flipNow) {
    while (kf[stuntCounter-1] > (stunt_distance)) {
     
      stuntTime[stuntCounter] = millis() - stuntStartTime;
      collectTOF();
      getKalmanData();

      forward(255);
      stuntSpeed[stuntCounter] = 255;
      stuntCounter = stuntCounter + 1;
    }
    flipNow = 1;
  }

  if (flipNow) {

    Serial.println("flipping");

    flipStartTime = millis();
    while ((millis() - flipStartTime) < 2300) {

      stuntTime[stuntCounter] = millis() - stuntStartTime;
      collectTOF();
      getKalmanData();

      reverse(255);
      stuntSpeed[stuntCounter] = -255;
      stuntCounter = stuntCounter + 1;
    }
    flipNow = 0;
  }

    stop();
```

- `collectTOF()` responsible for collecting current ToF values and updating the global `TOF1` array with distances
- `getKalmanData()` reads in the current `TOF1` array value (updated by the collectTOF() function). In return it updates the global `kf` array with the kalman filter calculated distance which is used for tracking distance to the wall for the stunt

```c++
if (update) {
    KalmanResult result = kalman(mu, sigma, Matrix<1, 1>{ (pid_speed[stuntCounter] / 255.0) * 1000 }, Matrix<1, 1>{ TOF1[stuntCounter] });

    mu = result.mu;
    sigma = result.sigma;

    kf[stuntCounter] = mu(0, 0);
    update = 0;
  }

  else {
    // Same as above except pass in false for fifth kalman() parameter to flip update to false
  }

```

- `sendStuntData()` loops through crucial arrays and sends them over bluetooth to be processed by the notification hanlder and piped into a csv for graphing and post prosessing.

## Results

### Stunt Videos 

<iframe width="450" height="315" src="https://www.youtube.com/embed/nGscvDKEgaM"allowfullscreen></iframe>
<figcaption>Flip Trial 1</figcaption>

<iframe width="450" height="315" src="https://www.youtube.com/embed/f26MszdWCjk"allowfullscreen></iframe>
<figcaption>Flip Trial 2</figcaption>

<iframe width="450" height="315" src="https://www.youtube.com/embed/3NoplLiZmkU"allowfullscreen></iframe>
<figcaption>Flip Trial 3</figcaption>

### Graphs for Trial 3

From the graph you can see Kalman Filter Position vs. Time as well as Speed vs. Time. Note that the speed flips from 255 to -255 once the robots hits the target distance of 1200 mm. This distance was played around with until finally deciding that 1200 mm was just enough to have the robot flip on the mat (300 mm from the wall) without hitting the wall or flipping too early.

<img src="/Fast Robots Media/Lab 8/StuntGraph.png" alt="Alt text" style="display:block;">
<figcaption>Trial 3 Graph</figcaption>

### Blooper Edit and Original Video

Here is my blooper video!
<iframe width="450" height="315" src="https://www.youtube.com/embed/rXx_Q_o5dlI"allowfullscreen></iframe>
<figcaption>Blooper</figcaption>

## Collaboration

I collaborated extensively on this project with [Jack Long](https://jack-d-long.github.io/) and [Trevor Dales](https://trevordales.github.io/). ChatGPT was used to help plot graphs.



