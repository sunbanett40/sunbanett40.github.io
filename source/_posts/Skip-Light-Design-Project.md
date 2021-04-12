---
title: Skip Light Design Project
date: 2019-05-21 12:34:00
tags:
    - Electronics
    - Subsystem Design
---

### Design Brief
You own a skip hire company. People keep hitting your skips at night in their cars and causing damage. You need an electronic device that will warn motorists at night of the presence of the skip.

### Specification
●	The device should flash a light at night to warn motorists of the presence of the skip.
●	The light must only flash in the dark to save power.
●	The light must flash at a frequency of 5Hz ± 10%.
●	The light must run off a 9V battery.
●	The light must be bright enough to be visible from 25 m away.

### Subsystem Blocks

<p style="text-align:center;"><img src="/images/skip-light/skip_light_block_diagram.png" alt="Block diagram" style="width: 75%; height: 75%">

#### Subsystem (1):
I will use a potential divider with a LDR to measure a change in the light level, this will tell the circuit it is dark. I used a potentiometer in the circuit to allow me to dial in the resistance so that the output voltages for light and dark are above and below the switching threshold. Finally the  schmitt not gate is in this system to give exact logic levels with no flickering, this is caused by the gates hysteresis effect.
<p style="text-align:center;"><img src="/images/skip-light/skip_light_subsystem_1.png" alt="Subsystem 1" style="width: 33%; height: 33%">

#### Subsystem (2):
I will use a schmitt not gate astable to create a 5Hz pulse for the skip light. The use of a schmitt not gate astable instead of a 555 astable because I was already using a schmitt not gate in my design.
<p style="text-align:center;"><img src="/images/skip-light/skip_light_subsystem_2.png" alt="Subsystem 2" style="width: 33%; height: 33%">

#### Subsystem (3):
For this part of the circuit I intend to just use an and gate as this very effectively combines the light sensors logic level and the astable, so that it outputs 5Hz when it is dark.
<p style="text-align:center;"><img src="/images/skip-light/skip_light_subsystem_3.png" alt="Subsystem 3" style="width: 33%; height: 33%">

#### Subsystem (4):
To drive the LED I will use a N-channel MOSFET. This will allow me to drive the LED at higher currents than the and gate will be able to output. This is necessary as the LED I intend to use will be a very powerful one, requiring more current. I have decided on a MOSFET instead of a NPN transistor as it will allow me to drive the LED with negligible current from the and gate. In addition the MOSFETs much faster switching speed will help maintain the clear square wave of the astable.
<p style="text-align:center;"><img src="/images/skip-light/skip_light_subsystem_4.png" alt="Subsystem 4" style="width: 33%; height: 33%">

#### Subsystem (5):
To provide the output for this system I am going to use a super bright LED. I have calculated the protection resistor needed to be about 910Ω I have tested the two nearest resistor values and found that both will work continuously. I have decided to use the smaller value as <p style="text-align:center;"><img src="/images/skip-light/skip_light_subsystem_5.png" alt="Subsystem 5" style="width: 33%; height: 33%">

### Circuit Diagram:

<p style="text-align:center;"><img src="/images/skip-light/skip_light_circuit_diagram_1.png" alt="Circuit diagram" style="width: 75%; height: 75%">

For the final circuit diagram I combined all of the different subsystems I had built into one design. Once I did this I noticed that I could condense the design down further into one chip instead of two, using nand gate simplification. To keep the hysteresis quality of the schmitt not gate I used a schmitt nand gate instead of a normal one. This meant each subsystem works the same but using the same chip.
<p style="text-align:center;"><img src="/images/skip-light/skip_light_circuit_diagram_2.png" alt="NAND simplification" style="width: 75%; height: 75%">

### Photo of Working circuit:
Once built I needed to get my circuit working. To get it working, I had to do two things. First I had to find the right resistance for the potentiometre. This was so it outputted above and below the threshold voltage of the schmitt nand gate I used. To do this I used a multimeter and measured the output voltage. I found that a resistance of about 2.5KΩ was a very effective value. Secondly, I found that my LED was always dimly on. I found that the problem was my MOSFET was leaking current. To fix this I replaced the model of my MOSFET.
<img src="/images/skip-light/skip_light_working_1.png" alt="Finished circuit" style="width: 50%; height: 50%"><img src="/images/skip-light/skip_light_working_2.png" alt="Light flashing" style="width: 50%; height: 50%">

###	Testing against specification
●	The device should flash a light at night to warn motorists of the presence of the skip - My system flashes a light when it is dark. 
●	The light must only flash in the dark to save power - My system uses a LDR to flash a light only when it is dark.
●	The light must flash at a frequency of 5Hz ± 10% - My system uses an astable that flashes at 5.23Hz, within the 10% tolerance.
<img src="/images/skip-light/SL3.BMP" alt="DSO display" style="width: 50%; height: 50%">
●	The light must run off a 9V battery - The light runs off of a 9V power supply, meaning it could be run from a PP3 battery. However, the power consumption so it couldn’t last long.
●	The light must be bright enough to be visible from 25 m away - The light I used was the brightest light I had available to me, it was brighter than the lamp that I initially tested, suggesting it reaches this criteria. If it does not I don’t have any way to improve it, unfortunately.

###	Evaluation

During this project, I have been efficient with my time, making and testing circuit especially quickly, I feel this is partly due to the efficient design, that I came up with. I feel that during this project my evidence gathering has been quite week, in future projects I should remember to gather more evidence, whilst doing it.

I have noticed that the LED can occasionally interfere with the light sensing subsystem. To remedy this when designing a case for the product the LED and LDR should be facing away from each other. Another issue that this design requires a lot of power, and so will not last long on a PP3 9V battery, to improve this you could use multiple in parallel or a LiPo instead, I feel the design can’t be made more power efficient as this power is mostly used to light the LED. 

To improve the design of this project, I would look at reducing the number of gates used even further. Even though the number of chips would not be changed, it may be possible to get it down to a smaller chip size. I would use a potentiometer in the astable system, as I found the calculated value for frequency was off of the actual and this would allow you to tune it to frequency you want. For more improvements I may consider, allowing the user to adjust the on/off time, this would especially help with the battery life.

The system that I designed was small enough to fit on half a board and only used one chip. I would say this is highly efficient and as it met the specification also very successful. I am happy that the wiring is both neat and reliable, enough to be used effectively as a demonstrative piece.