---
title: 'RFID Sensor Project Part 2 - PCB Design'
date: 2021-03-11 12:14:00
tags:
    - Electronics
    - PCB Design
---

### Circuit Diagram
As the majority of the ciruit was designed previously in spice, the Eagle schematic for this file was proivided with those sections filled in. The rest of the file was filled in by importing sympol parts, and setting the model and package.
<img src="/images/RFID18.png" alt="RFID schematic" style="width: 100%; height: 100%">

### PCB Outline
I then created a corresponding board file to the the schematic, and was presented by the following screen.
<img src="/images/RFID19.png" alt="Board outline" style="width: 100%; height: 100%">

As we had been provided by the outline of a box that the PCB was to fit in, I modified the shape of the PCB to fit within it. To maximise the space available I made cut outs for the areas where the screw connections of the case will be. I then removed the deadspace under where the buttons will be. Finally, I rounded off the corners so the board is comfortable to pick up and work on once it has been fabricated.
<img src="/images/RFID20.png" alt="Board outline" style="width: 75%; height: 75%">

### Part Placement
The first step in designing the board layout was to decide where the major components (i/o and ICs) were going to be placed. With this decided the remaining area would dictate where the passive components and the print inductor were to be placed. To ease in placing the passive components I lined them around the board edge, woughly in the order they were to be connected.
<img src="/images/RFID21.png" alt="Part placement" style="width: 75%; height: 75%">

Once I had finalised the position of the major components, the next step was to place the passive components. I did this in such a way as to minimise the routing needed between them. In this design I also took care to seperate the digital and analogue sections of the design.  
<img src="/images/RFID22.png" alt="Part placement" style="width: 75%; height: 75%">

### Routing
When routing, I started by connecting all the data (signal) lines. These were connected with 10 mil traces and were relatively simple to connect as the traces were usually short owing to the components being placed together in the design. 
<img src="/images/RFID23.png" alt="Routing" style="width: 75%; height: 75%">

The next step was then to connect the power lines. Not all the power lines needed connecting as the design was going to utilise ground and power planes. However, in connecting seperated sections of the design, such as power connections on the wrong side of the board, vias and 20 mil traces were used.
<img src="/images/RFID24.png" alt="Routing" style="width: 75%; height: 75%">

### Print Inductor
By this point most of the design had been completed the final party of placement and routing was to create a print inductor 
<img src="/images/RFID25.png" alt="Inductor" style="width: 50%; height: 50%"><img src="/images/RFID26.png" alt="Inductor" style="width: 50%; height: 50%">

<img src="/images/RFID27.png" alt="Board outline" style="width: 75%; height: 75%">

### Ground & Power Planes
when 
<img src="/images/RFID28.png" alt="Power plane" style="width: 50%; height: 50%"><img src="/images/RFID29.png" alt="Power Plane" style="width: 50%; height: 50%">

### Silkscreen
when 
<img src="/images/RFID30.png" alt="Silkscreen" style="width: 50%; height: 50%"><img src="/images/RFID31.bmp" alt="Silkscreen" style="width: 50%; height: 50%">

### Final Product
Finally, a design rule check was run to ensure that the design conformed to the fabrication house's capabilities. Although a few errors were found, these were expected owing to how the print inductor was created. This is now the completed board ready for fabrication, this was finally exported as a gerber file and submitted for production.
<img src="/images/RFID33.png" alt="Final DRC" style="width: 50%; height: 50%"><img src="/images/RFID34.png" alt="Finished Board" style="width: 50%; height: 50%">