+++
title = "Lab 5: Linear PID control and Linear interpolation"
date = 2025-03-08
weight = 8
[taxonomies]
tags = ["Robotics", "C++", "Sensors", "Python", "Embedded Software", "Microcontroller" ]
+++

## Bluetooth Communication

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

The main loop is responsible for running the `runPID()` function which collects the ToF data and internally runs the `pid_speed_ToF()` responsible for calculating the speed based on PID control feedback. The calculated speed is then passed into the `forward()` and `reverse()` functions from Lab 4:

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

After the data is collected, it is sent back to the computer over Bluetooth via the `sendPIDData()` function.

