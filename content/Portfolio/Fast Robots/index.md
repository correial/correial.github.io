+++
title = "MAE 4190 Fast Robots"
weight = 2
description = "Cornell Engineering course where I designed and fabricated a fast autonomous car with dynamic system modeling and reactive control via integrating gyroscope and time-of-flight sensor feedback on an embedded processor."

[taxonomies]
tags = ["Robotics", "C++", "Electrical Integration", "Sensors", "Python", "PID Control", "Microcontroller", "Localization", "Path Planning"]

[extra]
logo = "/CornellLogo.png"

+++
This page showcases my work in MAE 4190 Fast Robots, a course centered on the systems-level design and implementation of dynamic autonomous robots. Over the semester, I designed and fabricated a fast autonomous car with dynamic system modeling and reactive control via integrating gyroscope and time-of-flight sensor feedback on an embedded processor. The course also explored the advantages of partial off-board computation, low-latency software, and noise-tolerant implementation. 

## Highlights from Each Lab

Below are highlights from each lab building up to the final lab where I achieved fully autonomous navigation and path planning via real time localization and linear/angular PID control. For detailed documentation of each lab with code reference my 
<a href="https://correial.github.io/LuccaFastRobots/Fast%20Robots%20Stuff/" target="_blank" rel="noopener noreferrer">Fast Robots site</a>.

### Lab 1: Artemis Setup and Communcation

#### Key Takeaways:
- Installed and configured the Artemis successfully
- Set up a reliable communication link between Artemis (peripheral) and computer (client)
- Built methods for both real-time and batch data collection to compare responsiveness and accuracy in sampling
- Customized UUIDs to avoid device conflicts and wrote Python scripts for BLE command transmission and processing

### Lab 2: IMU

#### Key Takeaways: 
- Configured ICM-20948 IMU over I2C, verified accelerometer and gyroscope readings.
- Converted accelerometer outputs to pitch and roll; validated accuracy across ±90°.
- Implemented FFT + low-pass filter (cutoff 5 Hz) to characterize and reduce noise while preserving motion signals.
- Integrated gyro drift-prone data with accelerometer stability using a complementary filter (α = 0.0876).
- Achieved ~350 Hz sample rate, 25 seconds of buffered IMU data, and confirmed smooth motion capture during stunts.

<img src="/Fast Robots Media/Lab 2/LPF_FFT_RAW_OSS.png" alt="Alt text" style="display:block;">
<figcaption>Raw and Fourier Transform Data with Low Pass Filter - Hand Osscilations</figcaption>

<img src="/Fast Robots Media/Lab 2/CompResults.png" alt="Alt text" style="display:block;">
<figcaption>Complementary Filter Gyro vs. Low Pass Filter Accelerometer</figcaption>

### Lab 3: Time of Flight Sensors

#### Key Takeaways: 

- Used XSHUT pin to reassign I2C addresses, enabling two ToF sensors in parallel.
- Wired sensors to Qwiic breakout with soldered battery leads, tested mounting layout.
- Verified accuracy across 100–200 mm; identified noise below 40 mm due to sensor limits.
- Measured ~11 Hz sampling rate; confirmed ToF was the bottleneck compared to ~98 Hz IMU.
- Collected synchronized ToF + IMU data over BLE, validating angle/distance correlation.
- Characterized ToF noise 

<img src="/Fast Robots Media/Lab 3/150mm.png" alt="Alt text" style="display:block;">
<figcaption>150 mm Test</figcaption>

### Lab 4: Motor Drivers and Open Loop Control

#### Key Takeaways: 

- Designed drivetrain wiring: Artemis pins (13, A14, A15, A16) to dual motor drivers with PWM control.
- Used separate batteries for Artemis vs. motors to reduce noise and protect sensitive electronics.
- Verified PWM duty cycles on oscilloscope and tested forward/reverse/spin maneuvers.
- Determined lower PWM thresholds (35 forward/reverse, 110 spin).
- Calibrated wheel imbalance with multiplicative correction factors (1.05, 1.3) to achieve straight-line motion.
- Fully integrated IMU, ToF, motor drivers, and power onto the chassis for open-loop tests.

<style>
  .side-by-side {
    display: flex;
    justify-content: center;
    gap: 20px;
    flex-wrap: wrap;
  }
  .side-by-side figure {
    margin: 0;
    text-align: center;
    flex: 1 1 45%;
  }
  .side-by-side img {
    max-width: 100%;
    height: auto;
    border-radius: 8px;
  }
</style>

<div class="side-by-side">
  <figure>
    <img src="/Fast Robots Media/Lab 4/front.png" alt="Front of Car">
    <figcaption>Front of Car</figcaption>
  </figure>
  <figure>
    <img src="/Fast Robots Media/Lab 4/topcar.png" alt="Top of Car">
    <figcaption>Top of Car</figcaption>
  </figure>
</div>

### Lab 5: Linear PID Control and Linear Interpolation

#### Key Takeaways: 

- Implemented BLE command PID_CONTROL to send PID gains (Kp, Ki, Kd) and target ToF distance to the Artemis.
- Developed pid_speed_ToF() function with logic for tolerance, saturation, motor deadband, and direction switching.
- Tuned Proportional control (Kp = 0.05–0.08) to keep PWM speeds reasonable at large errors; deadband added for small errors.
- Added Derivative control (Kd ≈ 6) to slow the robot proportionally to error slope, preventing overshoot into the wall.
- Found ToF sampling limited to ~9.24 Hz, causing discrete jumps in derivative term.
- Implemented extrapolation to achieve ~172 Hz loop speed, interpolating distances between ToF readings.
- Verified with PD + extrapolation (Kp = 0.2, Kd = 3): robot could start 2.7 m from wall and still stop at 1 ft target without overshoot.
- Demonstrated near-zero steady-state error at higher speeds, eliminating need for integral term.

<iframe width="450" height="315" src="https://www.youtube.com/embed/0FQvu8WyHYI"allowfullscreen></iframe>
<figcaption>Proportional Control #1</figcaption>

<iframe width="450" height="315" src="https://www.youtube.com/embed/TU_ZbfE4Ph0"allowfullscreen></iframe>
<figcaption>PD Control #2 - Extrapolation</figcaption>

### Lab 6: Orientation Control

#### Key Takeaways: 

- Implemented PID turning control loop using IMU DMP yaw data.
- Mapped PWM outputs into robot’s floor/ceiling bounds to achieve smooth PID instead of binary switching.
- Added low-pass filter (𝛼 = 0.1) on derivative term to reduce noise and derivative kick.
- Fixed overshoot by inserting 1-second low-PWM delay before rotation.
- Tuned gains to Kp=1.65, Kd=130, achieving ±2° accuracy in disturbance tests and waypoint navigation.
- Verified IMU DMP sampling at ~549 Hz, sufficient for high-speed control loops.

<img src="/Fast Robots Media/Lab 6/NoMappingP.png" alt="Alt text" style="display:block;">
<figcaption>No Mapping Control</figcaption>

<iframe width="450" height="315" src="https://www.youtube.com/embed/S883NBawxys"allowfullscreen></iframe>
<figcaption>Disturbance Correction Kp=1.65 | Kd=130</figcaption>

<img src="/Fast Robots Media/Lab 6/FinalBumper.png" alt="Alt text" style="display:block;">
<figcaption>Disturbance Correction Kp=1.65 | Kd=130</figcaption>

### Lab 7: Kalman Filter

#### Key Takeaways: 

- Modeled car dynamics with drag and mass parameters estimated from ToF velocity profiles  
  - m ≈ 0.39 kg, d ≈ 0.41 kg/s  
- Built Python Kalman simulation to fuse ToF data with state-space dynamics  
  - Tested trust weighting by varying sigma1, sigma2, sigma3  
- Confirmed filter could run at ToF sampling rate (95 ms) and faster PID loop rate (20 ms)  
- Integrated filter on-board: predicted state when ToF was unavailable, improving control continuity  
- Demonstrated proportional control overshot wall stops, but **PD + Kalman** achieved smooth approach and near-zero overshoot at 300 mm target  
- Discussed parameter tuning:  
  - m, d set dynamics  
  - u scaling improved model fit  
  - sigma values balanced model vs. sensor trust  

<img src="/Fast Robots Media/Lab 7/ToFandVelo.png" alt="Alt text" style="display:block;">
<figcaption>ToF and Velocity Data</figcaption>

<style>
  .side-by-side {
    display: flex;
    justify-content: center;
    gap: 20px;
    flex-wrap: wrap;
  }
  .side-by-side figure {
    margin: 0;
    text-align: center;
    flex: 1 1 45%;
  }
  .side-by-side img {
    max-width: 100%;
    height: auto;
    border-radius: 8px;
  }
</style>

<div class="side-by-side">
  <figure>
    <img src="/Fast Robots Media/Lab 7/CLoseToF.png" alt="Close ToF">
    <figcaption>dt = 0.095 | dx = 0.47 | $\sigma_1$ &amp; $\sigma_2$ = 65 | $\sigma_3$ = 57.5</figcaption>
  </figure>
  <figure>
    <img src="/Fast Robots Media/Lab 7/Sped up.png" alt="Sped Up">
    <figcaption>dt = 0.02 | $\sigma_1$ &amp; $\sigma_2$ = 32.4 | $\sigma_3$ = 57.5</figcaption>
  </figure>
</div>


<img src="/Fast Robots Media/Lab 7/State Space On Robot.png" alt="Alt text" style="display:block;">
<figcaption>Kalman Filter Dynamics Test</figcaption>

<img src="/Fast Robots Media/Lab 7/AT .12|0|60|300.png" alt="Alt text" style="display:block;">
<figcaption>Kalman Filter PD Kp = .12 | Kd = 60</figcaption>

<iframe width="450" height="315" src="https://www.youtube.com/embed/HvY___Nhg7A"allowfullscreen></iframe>
<figcaption>Kalman Filter PD Control</figcaption>

### Lab 8: Stunts

#### Key Takeaways: 

## Lab 8: Stunts

**Key Takeaways:**

- Designed flip stunt triggered via Bluetooth (CMD.STUNT), parameterized by wall distance for flexible tuning  
- Used ToF + Kalman fusion to track distance in real time during stunt execution  
- Tuned state-space A/B matrices to improve distance predictions; still noted velocity underestimation in KF  
- Achieved consistent flips on mat placed 300 mm from wall; best trials completed in about 2.3 s  
- Corrected drift with motor scaling factors (0.95 forward, 0.90 reverse) to keep trajectory straight  
- Explored added front mass for nose-dive, but found battery charge level had greater effect on success  

<iframe width="450" height="315" src="https://www.youtube.com/embed/3NoplLiZmkU"allowfullscreen></iframe>
<figcaption>Flip Trial 3 | Time: 2.30s</figcaption>

### Lab 9: Mapping

### Lab 10: Grid Localization using Bayes Filter

### Lab 11: Localization on the Real Robot

### Lab 12: Path Planning and Final Project

