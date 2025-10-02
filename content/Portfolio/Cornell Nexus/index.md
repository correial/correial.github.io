+++
title = "Cornell Nexus"
weight = 3
description = "Cornell Engineering Project team currently working on a fully autonamous beach rover to address environmental concerns associated with microplastic pollution"

[taxonomies]
tags = ["Robotics", "Mechanical Design", "Solidworks", "Ansys Mechanical", "CNC Mill", "Systems Engineering", "PID Control", "Sensors", "Leadership", "Electrical Integration"]

[extra]
logo = "/nexus.png"
+++

## Team

[Cornell Nexus](https://www.cornellnexus.com/) is a Cornell Engineering project team developing a fully autonomous beach rover to tackle microplastic pollution. Since joining freshman year (2022), I’ve contributed to both the filtration and drivetrain projects, and last year in 2024 I led the fourteen-member mechanical team as **Subteam Lead**. In that role, I not only guided technical mechanical design but also stepped into a systems engineering position, defining system-level requirements, coordinating across electrical and software teams, and integrating subsystems onto the robot to ensure they worked seamlessly together. Below are a few highlights from the projects I’ve contributed to and supervised.

## Drivetrain Mechanical Design

I recently redesigned our drivetrain to overcome the limitations of tank steering on sand. Last year, I built a prototype with a front-mounted swerve module and a simplified rear drivetrain. Tank drive struggles in granular terrain because turning relies on lateral skidding, which the sand resists, causing energy loss, wheel sinkage, and high current draw. By adding swerve steering, the wheels align with the direction of travel, improving traction and efficiency on soft surfaces.

**A few key design elements:**
- Belt-driven turning mechanism to significantly reduce stress on the steering motor shaft
- Dual-bearing support to minimize load on the drive motor shaft
- Motor enconder mounted on drive shaft for all motors
- Stainless steel shaft + coupler to stiffen the motor output interface
- Custom wheel hub for easy tire removal
- Upper bearing connection between the rigid top plate and rotating swerve module
- Integrated belt tensioner for reliable operation and easier belt replacment

<div style="display: flex; flex-wrap: wrap; gap: 20px; justify-content: center;">

  <div style="flex: 1 1 45%; text-align: center;">
    <img src="/Swerve.png" alt="Simplified Swerve CAD" style="max-width:100%; height:auto; border-radius:8px;">
    <figcaption>Simplified Swerve CAD</figcaption>
  </div>

  <div style="flex: 1 1 45%; text-align: center;">
    <img src="/FrontPic.jpeg" alt="Left Swerve Module" style="max-width:100%; height:auto; border-radius:8px;">
    <figcaption>Left Swerve Module</figcaption>
  </div>

  <div style="flex: 1 1 45%; text-align: center;">
    <img src="/RearCad.png" alt="Rear Drivetrain CAD" style="max-width:100%; height:auto; border-radius:8px;">
    <figcaption>Rear Drivetrain CAD</figcaption>
  </div>

  <div style="flex: 1 1 45%; text-align: center;">
    <img src="/RearPic.png" alt="Left Rear Drivetrain" style="max-width:100%; height:auto; border-radius:8px;">
    <figcaption>Left Rear Drivetrain</figcaption>
  </div>

</div>

## Systems Engineering / Integration

Throughout my three years on Nexus I not only worked on technical projects but also took on a leadership systems engineering and integration role. I worked across mechanical, electrical, and software teams to set system-level requirements and semesterly test cases to ensure subsystems worked together reliably and motivate constant robot progress. Beyond defining performance targets and interfaces, I also supported integration and troubleshooting to bring everything onto the robot as a cohesive system. 

### Filtration

Our electrostatic filtration system uses image forces at high voltage to separate microplastics from sand on a grounded drum. I contributed by outlining and defining system-level requirements as well as proposing key testing variables and design tradeoffs. In parallel, I collaborated with the intake team to design an Archimedean screw mechanism that lifts sand from the ground and delivers it to the filtration unit. Together, these subsystems are integrated and have demonstrated effective performance in testing.

<div style="display:flex; gap:20px; align-items:flex-start;">
  <div style="flex:1; text-align:center;">
    <img src="/FilterIRL.png" alt="Alt text" height="700" style="max-width:100%; height:auto; display:block; margin:0 auto; margin-bottom:10px;">
    <figcaption>Electrostatic Filter Testing Jig (No Bucket)</figcaption>
  </div>

  <div style="flex:1; text-align:center;">
    <img src="/FilterCAD.png" alt="Alt text" height="400" style="max-width:100%; height:auto; display:block; margin:0 auto; margin-bottom:10px;">
    <figcaption>Electrostatic Filter Testing Jig CAD (No Bucket)</figcaption>
  </div>
</div>


<div style="display: flex; justify-content: center; gap: 20px; align-items: flex-start;">

  <div style="text-align: center;">
    <img src="/ConveyorCAD.png" alt="Alt text" style="max-height:340px; width:auto; border-radius:8px;">
    <figcaption>Electrostatic Filter CAD</figcaption>
  </div>

  <div style="text-align: center;">
    <img src="/ScrewCAD.png" alt="Alt text" style="max-height:340px; width:auto; border-radius:8px;">
    <figcaption>Conveyor Between Intake and Electrostatic Filter CAD</figcaption>
  </div>

</div>

### Electromechanical Integration

In addition to mechanical project integration, as subteam lead I also directed the electromechanical integration effort, ensuring independently developed subsystems were mounted securely for sand testing. I worked with the electrical team to position critical components: motor drives, encoders, battery, GPS RTK antennas, kill switch, and PDB so they were both robustly supported and accessible to software. For example, I collaborated on custom mounts to place the battery at the optimal center of mass while accommodating surrounding electronics. Ongoing work also focuses on protecting sensitive electronics from the high-voltage electrostatic filter.

<img src="/ElectroMechanicalInt.png" alt="Alt text" style="display:block;">
<figcaption>Conveyor Between Intake and Electrostatic Filter CAD</figcaption>






