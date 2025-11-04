+++
title = "Automated Tensile/Compression Tester"
weight = 5
description = "High school senior research thesis and internship project with Drake Labs and Orbital Composites where I designed and built a custom automated tester to measure deformation in 3D-printed carbon fiber composites, later presenting the work at the annual Science Research Fair."
logo = "/nexus.png"

[taxonomies]
tags = ["Robotics", "Mechanical Design", "Solidworks", "Ansys Mechanical", "CNC Mill", "Electrical Integration", "Python", "Microcontroller", "Sensors"]

[extra]
+++

During high school Senior summer and school year I completed a virtual internship with startup Drake Labs & strategic partner Orbital Composites. As part of my internship and Senior Thesis for the Horace Mann Science Reserach Program, I indepenetly designed, developed, machined, and programed a custom automated tensile tester to quantify deformation in Continous Carbon Fiber (CCF) Drake Labs LORE 3D Printed bike shoe.

### Summary
- Designed and completely in house manufactured a custom automated tensile and compression tester with the goal of quantifying the integrity of a 3D printed continuous carbon fiber bike shoe
- 1/4in steel stock slabs were machined to provide clearance for metal struts. Struts were welded to top slab with bottom was free to glide vertically for a variety of mounting heights.
- Main electrical components were a raspberry pie, a “main circuit” to control the actuator (discussed in paper), a 100kg load cell, and a  1320 lb linear actuator, and a safety enclosure to mount all components
- Programmed scripts to control linear actuator and execute actions via feedback from load cell 
- Documented work by writing a formal research paper and creating poster for the yearly Science Research Fair (both at the bottom of page)

<div style="display:flex; gap:20px; justify-content:center; align-items:flex-start; flex-wrap:wrap;">

  <!-- Left image -->
  <figure style="flex:1; min-width:300px; max-width:48%; margin:0; text-align:center;">
    <img src="/TensileTester1.png" alt="Tensile Tester Custom Structure" 
         style="width:100%; height:auto; border-radius:8px;">
    <figcaption>Tensile Tester Custom Structure</figcaption>
  </figure>

  <!-- Right column with two stacked images -->
  <div style="flex:1; min-width:300px; max-width:48%; display:flex; flex-direction:column; gap:20px;">

    <figure style="margin:0; text-align:center;">
      <img src="/LORESHOE.jpg" alt="LoreOne CCF 3DP Shoe" 
           style="width:100%; height:auto; border-radius:8px;">
      <figcaption>LoreOne CCF 3DP Shoe</figcaption>
    </figure>

    <figure style="margin:0; text-align:center;">
      <img src="/ElectricalTensile.png" alt="Electrical Tensile Tester" 
           style="width:100%; height:auto; border-radius:8px;">
      <figcaption>Electrical Tensile Tester</figcaption>
    </figure>

  </div>
</div>




### Results
- Successful in applying a tensile or compressive load to the shoe until target load is met
- Next steps: add a camera for image processing to quantify deformation

### Science Research Paper and Presentation

<iframe src="/Docs/Sci Tech Poster.pdf" width="100%" height="400px"></iframe>

<iframe src="/Docs/Custom Automated Tensile Tester - Lucca Correia.pdf" width="100%" height="400px"></iframe>



