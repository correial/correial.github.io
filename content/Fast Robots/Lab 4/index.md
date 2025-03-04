+++
title = "Lab 4: Motor Drivers and Open Loop Control"
date = 2025-03-04
weight = 0
[taxonomies]
tags = ["Robotics", "C++", "Sensors", "Python", "Embedded Software", "Microcontroller" ]
+++

## Prelab

### System Wiring Schematic 

I decided to use pins **13, A14, A15, A16** for control on the Artemis. This pin proximity does increase risk of shorting connections, however it allows for a more compact design and greater area of the board to be used for mounting to the car rather than having to avoid largely seperated pins. Furthermore, according to the data sheet pins marked with a (~) indicate PWM capability:

<img src="/Fast Robots Media/Lab 4/Artemis Schematic.png" height = 450 alt="Alt text" style="display:block;">
<figcaption>Artemis Schematic</figcaption>

The diagram for the mtor driver schematic is shown as below:

<img src="/Fast Robots Media/Lab 4/Wiring Schematic.png" alt="Alt text" style="display:block;">
<figcaption>Drivetrain System Wiring Schematic</figcaption>

### Battery Discussion

The Artemis and the motor drivers/motors are powered by separate batteries to help ensure operational stability, protect components, reduce noise, and increase battery lifetime. The two car motors can act as a disruptive inductive load, potentially creating large amounts of noise and posing a damage risk to sensitive parts. Thus powering the Artemis by a second battery allows the motors to run for longer and protect the Artemis circuit (all connected by signal wires) from the motor interferance and inductive load.

## Lab Tasks

### Power Supply and Oscilloscope Hookup

One motor driver was tested at a time by connecting the four `OUT` pins to the scope while connecting `VIN` and `GND` to the power supply to power the driver. The power supply is set to **3.7V** to simulate the 3.7V that would be supplied by the 850mAh motor battery. Below is a picture of the setup (the power supply and oscilloscope probing wires are drawn in to show the setup used to obtain the PWM signal before I soldered the parts to the car):

<img src="/Fast Robots Media/Lab 4/scopepower.png" height = 550 alt="Alt text" style="display:block;">
<figcaption>Scope and Power Supply Connection to Motor Driver</figcaption>

The following code was used to visualize the motor driver output:

```c++
#define AB1IN_LEFT 16
#define AB2IN_LEFT 15

void setup() {
  pinMode(AB1IN_LEFT,OUTPUT);
  pinMode(AB2IN_LEFT,OUTPUT);
}
void loop() {
  analogWrite(AB1IN_LEFT,100); 
  analogWrite(AB2IN_LEFT,0);
}
```

The `analogWrite(AB1IN_LEFT,100)` line indicated a 40% (100/255) duty cycle to pin 16. This was captured on the oscilloscope:

<img src="/Fast Robots Media/Lab 4/scopePWM.jpeg" height = 450 alt="Alt text" style="display:block;">
<figcaption>Oscilloscope PWM Output (40% Duty)</figcaption>

### Motor Testing



### Lower PWM Limit

I found that the minimum PWM value for which the robot needs to move forward, backward, and on axis by starting with a arbitrary small value and increasing it by 5 until the car no longer stalled but instead moved very slowly. I found that on a fully charged battery the forward and backward lower limit PWD value us **35** and **110** for spinning on axis.

Here is a video of testing using those PWM values to go forward, in reverse, then spin on axis in both directions:

<iframe width="450" height="315" src="https://www.youtube.com/embed/UzNFp05yqZ8"allowfullscreen></iframe>
<figcaption>Lower PWM Limit</figcaption>

### Complete Hardware Integration on Car

Below are images of the car after mounting all components. Holes were drilled and zip ties were used to strap down the IMU and both motor drivers at the front. Zip ties were also used to bundle the Artemis and 750mAh battery as well as for wire managment. Double sided tape was used to mount the two ToF sensors (one on the side and one at the front) and the Artemis/battery module at the rear.

<img src="/Fast Robots Media/Lab 4/topcar.png" height = 400 alt="Alt text" style="display:block;">
<figcaption>Top of Car</figcaption>

<div style="display: flex; justify-content: center; gap: 20px;">
    <div>
        <img src="/Fast Robots Media/Lab 4/bottom.jpeg" style="max-height: 450px; width: auto;" alt="Alt text">
        <figcaption>Bottom of Car</figcaption>
    </div>
    <div>
        <img src="/Fast Robots Media/Lab 4/sidebottom.jpeg" style="max-height: 450px; width: auto;" alt="Alt text">
        <figcaption>Battery Connection on Bottom</figcaption>
    </div>
</div>

<img src="/Fast Robots Media/Lab 4/front.png" height="400" alt="Alt text" style="display:block;">
<figcaption>Front of Car</figcaption>

### Open Loop Testing and Calibration

To test a range of car capabilities, I had the car move straight, then in reverse, then rotate on axis in both directions before moving straight left at the end (note each tile is 13"x13"):

<iframe width="450" height="315" src="https://www.youtube.com/embed/Zv6U8mtepHE"allowfullscreen></iframe>
<figcaption>Open Loop Test</figcaption>

### Collaboration

I collaborated extensively on this project with [Jack Long](https://jack-d-long.github.io/) and [Trevor Dales](https://trevordales.github.io/). I referenced [Wenyi's site](https://mavisfu.github.io/lab3.html) for my wiring setup, initial motor control code, and general outline for creating funtions to drive forward, spin right, etc. Special thanks to all TA's that helped debug my electronics and car for hours before we realized I probably needed a new one!