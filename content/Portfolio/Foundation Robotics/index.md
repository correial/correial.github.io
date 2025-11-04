+++
title = "Foundation Robotics"
weight = 1
description = "Summer 2025 internship at a San Francisco humanoid robotics startup where I accelerated development of next-generation robots by improving build workflows, designing precision fixtures, and assembling full-scale systems"

[taxonomies]
tags = ["Robotics", "Mechanical Design", "Actuators", "Manufacturing", "Solidworks", "Leadership", "CNC Mill", "Systems Engineering", "Sensors"]

[extra]
logo = "/FoundationLogo.png"
+++

This past summer (2025) I had the incredible opportunity of interning at Foundation, a startup humanoid robotics company based out of San Fransisco, CA. During my time there I worked as an extremely hands on mechanical/robotics engineer where I designed manufacturing fixtures and was partially responsible for the assembly, workflow, debugging, and build instructions of entire Phantom 1 Humanoids. Below are a few internship highlights:

## Humanoid Assembly & Debugging
- Independently completed the assembly of entire Phantom MK1 Humanoids including structures, linkages, motor encoder soldering, power and signal harnesses fabrication, actuator calibration
- Complete initial robot bring-up via simulation that involved URDF visualization and actuator signals
- Worked on manufacturing floor to complete humanoid repairs and often independent debugging

<div style="display:flex; justify-content:center; gap:20px;">

  <figure style="text-align:center;">
    <img src="/FoundationBot.jpeg" height="400" alt="Alt text">
    <figcaption>Independent entire humanoid assembly</figcaption>
  </figure>

  <figure style="text-align:center;">
    <img src="/FoundationLegs.png" height="400" alt="Alt text">
    <figcaption>Lower body actuator debugging</figcaption>
  </figure>

</div>
 
## Manufacturing Design Projects
- Designed various manufacturing and work holding fixtures in OnShape for CNC milling. Examples include a jig to clamp down actuator encoder magnet rings to be milled down as well as an ankle zeroing fixture to establish neutral ankle position during bring-up 
- Utilized CAM software to generate GCODE paths for the various work holding fixtures
- Set up and operated 3-axis CNC mill for fabrication of the jobs

<div style="display:flex; justify-content:center; gap:30px;">

  <figure style="margin:0; text-align:center;">
    <img src="/zerofix.png" height="300" alt="Ankle Zeroing Fixture Model">
    <figcaption>Ankle Zeroing Fixture Model</figcaption>
  </figure>

  <figure style="margin:0; text-align:center;">
    <img src="/zerofixGCODE.jpg" height="300" alt="Ankle Zeroing Fixture GCODE">
    <figcaption>Ankle Zeroing Fixture GCODE</figcaption>
  </figure>

</div>

The **encoder magnet rings** were one of my key design and machining projects. The team needed a repeatable process to remove a 3 mm bore and, if required, enlarge the ID to 11 mm. My contributions included:
- **Fixture Design:** Created a CNC fixture in CAD to clamp the magnet ring without damaging the encoder magnet. Key features included a pedestal to protect the magnet, precise pocket depth for clamping, minimal offset between clamp surfaces to prevent deformation, and a transition-fit pocket for accurate alignment.
- **CAM & Machining of Fixture:** Generated tool paths, researched feeds/speeds, selected appropriate bits, and programmed GCODE for the mill.
- **Ring Modification:** Machined AISI 430 housings using a 4-flute ¼″ TiAlN-coated carbide end mill with conservative depth-of-cut parameters. Programed a circular toolpath in CAM followed by a larger diameter bit for a 70 µm finishing pass.
- **Documentation:** Authored detailed work instructions for technicians to reliably repeat the modification process.

<div style="display:flex; justify-content:center; align-items:flex-start; gap:24px;">

  <!-- LEFT: rectangle -->
  <figure style="margin:0; text-align:center;">
    <img src="/ringcompare.png" height="300" alt="Before and After Modification">
    <figcaption>Before and After Modification</figcaption>
  </figure>

  <!-- RIGHT: stacked squares -->
  <div style="display:flex; flex-direction:column; gap:1px;">
    <figure style="margin:0; text-align:center;">
      <img src="/ringfinalpass.png" height="120" alt="Final Finishing Pass">
      <figcaption>Final Finishing Pass</figcaption>
    </figure>
    <figure style="margin:0; text-align:center;">
      <img src="/ringpath.png" height="120" alt="Ring GCODE">
      <figcaption>Ring GCODE</figcaption>
    </figure>
  </div>

</div>

<div style="display:flex; justify-content:space-between; align-items:flex-start;">

  <!-- Left text -->
  <div style="max-width:60%;">
    <h2>Work Instructions and Workflow Process</h2>
    <p>
      While completing humanoid assembly, I developed comprehensive CAD-based build 
      instructions and an assembly workflow for the entire lower body. I collaborated 
      with process engineers and technicians to gather feedback and refine both the 
      instructions and overall workflow for efficiency and clarity.
    </p>
  </div>

  <!-- Right image -->
  <figure style="margin:0; text-align:center;">
    <img src="/FoundationCAD.png" height="400" alt="Humanoid CAD">
    <figcaption>Humanoid CAD used for comprehensive work instructions</figcaption>
  </figure>

</div>

