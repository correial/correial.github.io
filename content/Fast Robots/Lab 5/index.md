+++
title = "Lab 5: Linear PID control and Linear interpolation"
date = 2025-03-08
weight = 8
[taxonomies]
tags = ["Robotics", "C++", "Sensors", "Python", "Embedded Software", "Microcontroller" ]
+++

## Bluetooth Communication and PID Code

Data communication to and from the computer and Artemis were handeled similar to previous labs. The `PID.CONTROL` command is used to send the start flag and the Kp, Ki, Kd, and target distance: `ble.send_command(CMD.PID_CONTROL, ".05|0|6|300") # P|I|D`. The Arduino recieves the gains:

```c++
    case PID_CONTROL: {
    pid_start = 1;
    // Get PID parameters over BLE
    success = robot_cmd.get_next_value(Kp);
    if (!success) return;
    success = robot_cmd.get_next_value(Ki);
    if (!success) return;
    success = robot_cmd.get_next_value(Kd);
    if (!success) return;
    success = robot_cmd.get_next_value(target_tof);
    if (!success) return;
    tx_estring_value.clear();
    }
```

The main loop is responsible for running the `runPID()` function which collects the ToF data and internally runs the `pid_speed_ToF()` responsible for calculating the speed based on PID control feedback. The calculated speed is then passed into the `forward()` and `reverse()` functions from Lab 4. The function is run until the PID data array is filled (500 in this case).

```c++
void runPID(){
  start_time_pid = millis();

  for (int i = 0; i < pid_len; i++) {
    distanceSensor1.startRanging();
    pid_time[i] = millis() - start_time_pid;

    pid_tof[i] = distanceSensor1.getDistance();

    distanceSensor1.clearInterrupt();
    distanceSensor1.stopRanging();

    pid_speed[i] = pid_speed_ToF(Kp, Ki, Kd, pid_tof[i], target_tof);
    pid_p[i] = Kp * pos_error;
    pid_i[i] = Ki * integral_error;
    pid_d[i] = Kd * d_term;
  }
  stop();
  pid_start = 0;
  sendPIDData();     
}
```

Below is the `int pid_speed_ToF()` function. Note that after calculating the PID speed is calcualted, there are various IF statments to account for the motor deadband, direction, stopping tolerance, and max speed. Effectively, the function logic flows as follows:

1. Calculate PID Speed from gains and error
2. If the speed is <1 it means we are effectively at our target and can stop
3. If we are not yet close enough to the target we check to ensure our set speed is lower than the max speed (in either direction). If it isn't we bring the speed down to maxSpeed
4. If the speed falls within our deadband range from Lab 4 (PWM of 35), the PWM signal is brought up to 35
5. The set speed is passed into the reverse() or forward() function based on its sign

```c++
int pid_speed_ToF(float Kp, float Ki, float Kd, float pos_cur, float pos_targ){
  
  // Varaible Init
  time_cur = millis();
  dt = time_cur-time_prev;
  time_prev = time_cur;
  pos_error = pos_cur-pos_targ; // Position error
  d_term = (pos_error-prev_error)/dt; // Derivative term
  integral_error = integral_error + (pos_error*dt); // Integral Term

  // Set Speed PID Calculation
  int speed_set = ((Kp*pos_error) + (Ki*pos_error*dt) + (Kd*d_term)); // PID speed control

  // Set tolerance
  if (abs(speed_set) < 1){
    stop();
  }
  // If saturated (pos dir), set speed to max speed 
  if (speed_set > maxSpeed){
    speed_set = maxSpeed;
  }
  // If saturated (neg dir), set speed to max speed 
  if (speed_set < (-1*maxSpeed)) {
      speed_set = -1*maxSpeed;
  }
  // Set min speed to min PWM value from Lab 4
  if ((speed_set < minSpeed) && (speed_set > 0)) {
      speed_set = minSpeed;
  }
  // Set min speed to min PWM value from Lab 4
  if ((speed_set > (-1*minSpeed)) && (speed_set < 0)) {
      speed_set = -1*minSpeed;
  }
  // Set forward speed
  if (speed_set > 0){
    forward(speed_set);
  }
    // Set backwards speed
  if (speed_set < 0){
    reverse(-1*speed_set);
  }

prev_error = pos_error;
return speed_set;

}
```

After the data is collected, it is sent back to the computer over Bluetooth via the `sendPIDData()` function. On my computer the data is piped into a CSV file via my notification handler. From there, the data is plotted for visualization.

## PID Control and Tuning

### Range/Sampling Time Discussion

Consider the range and sampling time you choose for your TOF sensor; it may be worth lowering the accuracy for faster updates. Note that the medium range is only available if you are using the (ToF Pololu library).

### Proportional Control

For this lab the car starts roughly 2m-4m from the wall and is intended to stop 300mm from the wall. As a result, ToF readings (in mm) start at 2000-4000. For this lab, the intention is to keep the car speed to no more than ~150. If we focus on the very start of running the car, the error is roughly 3000-300 = 2700. Thus, my propotional gain values (which get multiplied by the 2700mm error) were in the range of **.05 - .08** throughout tuning to keep us in the 100-150 PWM when at these large distances from the wall at the start. When the error gets very small, the Kp term is not large enough to provide the minimum PWM to move the car. As a result, the deadband minimum speed was implemented.

Below is a video of only proportional control implementation with the car starting at ~2500mm from the wall:

### PD Control

While soley propotional control proved to be fairly accurate (effectively stopping <4mm from the target), overshoot and ramming into the walls was still an issue at higher speeds. Thus, since I wanted to try increasing my speed I have to implement a derivative term. The derivative term helps to slow the car down proportional to the slope of the error. As the car approaches the wall the error is decreasing resulting in a negative derivative term that helps reduce the overall speed by subtracting off

FINISH THIS
Discrete Kd values can be seen as the ToF values are unfotunely slow. When there is not a new ToF reading, the derivative term is zero as the current and previous error terms are the same until a new ToF reading comes in. To address this issue we extrapolate. Effectively, the derivative term is only slowing the car down at discrete intervals which is not enough to slow the car down in time.
FINISH THIS

## Extrapolation

My ToF returns new data every 108.21 ms on average which corresponds to a rate of **9.24 Hz**. This raises issues as mentioned above with the lack of derivative effect.  