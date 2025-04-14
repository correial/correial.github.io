+++
title = "Lab 9: Mapping"
date = 2025-04-08
weight = 4
[taxonomies]
tags = ["Robotics", "C++", "Sensors", "Python", "Embedded Software", "Microcontroller" ]
+++

## Orientation Control Implementation
**Orientation control** via the IMU's DMP to set target angles for the robot over the course of data collection. With success and high accuracy using the DMP in Lab 6, I figured that this was the best approach to ensure that the robot was reliably turning a set number of degrees throughout its rotation. 

### BLE and Data Transmission
The `SPIN` case was used handle initiating and running the turning logic. The case was called over bluetooth via the command: `ble.send_command(CMD.SPIN, "4.6|0|0")`. The parameters are used to pass through PID gains for more efficient tuning and are ordered as follows (Kp|Ki|Kd). I found that with the small changes in target angles and the goal of moving fairly slow that only proportional control was needed to reliably come within a degree or two of the target. At the slow speeds there was very minial, if any, overshoot thus a derivative term was not needed. 

### Arduino Implementation

The `SPIN` code is shown below. The robot spins a full 360 degrees over the course of 30 rotations and pauses (12 degress between each rotaion). The next target angle (adding 12 to `target_turn`) is set every 2 seconds to allow for ample data collection time at a fixed angle for maximum ToF accuracy. Note that I have have ToF sensors collecting data at all time to increase the number of data points. While this does come at the cost of decreased accuracy while rotating between each two second break, it did not seem to greatly impact the ToF readings and the increased resultion was well worth the slightly decreased accuracy at a few points.

```c++
distanceSensor1.startRanging();
success = robot_cmd.get_next_value(Kp_turn);
if (!success) return;
// Same for Ki and Kd

// Time varaibles
spin_time = millis();
start_time_pid_turn = millis();
last_rotation_time = millis();

while ((spin_counter < spin_len) && (rot_counter <= 30)) {
  getDMP();
  runPIDRot();
  collectTOF();
  spinTime[spin_counter] = millis() - spin_time;
  
  elapsed_time = millis() - last_rot_time;
  if (elapsed_time > 2000) {
    last_rot_time = millis();
    rot_counter = rot_counter + 1;
    target_turn = target_turn + 12;
  }   
    spin_counter = spin_counter + 1;
}
stop();
sendPIDTURNData();
break;
```

There are a few important functions to note, many of which have been used in my previous labs:
- `getDMP()` is responsible for updating the global yaw array with the current angle reading
- `runPIDRot()` (shown below) is responsible for handling all rotational PID logic–it passes in the gains, target, and current angle into the `pid_turn_Gyro()` function which is explained in detail in Lab 6
- `collectTOF()` updates the global ToF arrays with the current ToF readings regardless if whether a new one is ready
- `sendPIDTURNData()` is responsible for sending over all relevant arrays over BLE

```c++
timesROT[counter_turn] = millis() - start_time_pid_turn;

  curDistance_turn = yaw_gy;

  pidTurn_speed[counter_turn] = pid_turn_Gyro(Kp_turn, Ki_turn, Kd_turn, curDistance_turn, target_turn);

  pidTurn_p[counter_turn] = Kp_turn * pos_error_turn;
  // Same array populating for propotional, derivative, LPF derivative, and integral terms

  counter_turn = counter_turn + 1;
```

### Data for Target Setting

Note you can see that for roughly two thirds of rotations, there was no overshoot as a reverse correction input was not needed. For instances of small overshoots, they were quickly corrected as seen from a spike in speed in the opposite direction. If I wanted to completely eliminate these overshoots, it would make the rotations much slower which could cause a memoery issue. The 12 degree timing every two seconds comes close to maxing out the dynamic memory on the Artemis. 

<img src="/Fast Robots Media/Lab 9/RotPIDControl.png" alt="Alt text" style="display:block;">
<figcaption>Rotational PID and Target Setting</figcaption>

## Mapping Data and Post Processing

### Data at Global Coordinates
For all runs the robot was started at zero degrees facing the same way as shown in the video(O)()()()()

<img src="/Fast Robots Media/Lab 9/MapPoints.png" alt="Alt text" style="display:block;">
<figcaption>Mapping Scans</figcaption>

### Transformation Matrices

<img src="/Fast Robots Media/Lab 9/Coordinates.png" alt="Alt text" height=350 style="display:block;">
<figcaption>Robot Coordinate System</figcaption>

A few transformations had to be done to convert the raw ToF data into the global arena coordinate system. I first started by taking into account the front ToF sensor location relative to the robot's center via the robot's coordinate axis (shown above). I found that the ToF sensor ("TOF" in the matrix) is positioned 70 mm $\hat{x}$, 0 mm $\hat{y}$ from the robots center.

The following position vector is yielded

<div id="math-p1"></div>
<script>
  document.addEventListener("DOMContentLoaded", function () {
    katex.render(String.raw`
      P_1 =
      \begin{bmatrix}
        \mathrm{TOF}_1 \\[0.2em]
        0 \\[0.2em]
        1
      \end{bmatrix}
    `, document.getElementById("math-p1"), {
      displayMode: true
    });
  });
</script>

Next, a transformation is needed to convert the robot's local angular yaw coordinate (from the DMP) to global $\hat{x}$ and $\hat{y}$ coordinates

<div id="math-rotz"></div>
<script>
  document.addEventListener("DOMContentLoaded", function () {
    katex.render(String.raw`
      R_z(\theta) =
      \begin{bmatrix}
        \cos\theta & -\sin\theta & x \\[0.2em]
        \sin\theta & \cos\theta & y \\[0.2em]
        0 & 0 & 1
      \end{bmatrix}
    `, document.getElementById("math-rotz"), {
      displayMode: true
    });
  });
</script>

### Final Map

After the transformations are applied to the raw ToF and DMP data, the following map is created

## Collaboration

I collaborated extensively on this project with [Jack Long](https://jack-d-long.github.io/) and [Trevor Dales](https://trevordales.github.io/). ChatGPT was used to help plot and format graphs.



