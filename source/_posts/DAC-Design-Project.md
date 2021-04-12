---
title: DAC Design Project
date: 2019-12-19 09:42:00
tags:
    - Electronics
---

### Introduction
As part of my A-Level Electronics we have had to cover a lot of systems with operational amplifiers, at times it has been more like A-Level Op-Amps. One particular circuit that we have had to cover is the digital to analogue converter. I have found the ADC and DAC interesting; these are particularly important circuits as in an increasingly digital age it has only become more important to be able to interact between the digital world of computers and the analogue world around us. 

When learning about the circuit we were shown a demonstration circuit. Although the circuit was sufficient in showing how the process would work, it was not well designed as the steps were inconsistent and there was significant noice. I felt that I could design and construct a better circuit.
<img src="/images/dac/DAC6.jpg" alt="Demo circuit" style="width: 50%; height: 50%"><img src="/images/dac/DAC12.jpg" alt="Demo results" style="width: 50%; height: 50%">


### Circuit Design
My main concern when designing this circuit was to ensure that there was even spacing between the output steps. To this I ensured that each input was a bigger than the last by a multiple of two. To ensure these steps using the resistors I had to hand, mostly E24 series resistors, I had to use multiple resistors to create a combined resistance. I combined resistors in series as it was easier to calculate, neater to diagram, and easier to breadboard.
<img src="/images/dac/DAC13.png" alt="Circuit Diagram" style="width: 100%; height: 100%">

Although not drawn in the circuit diagram, I replicated the 8 bit counter made from a CMOS 4060 and an RC filter. This was to create the binary counter used as the input to the DAC.

### Circuit Building
Building the circut I generally followed the layout of the the demonstration. I made sure to keep my wiring neat and trimmed my resistors to make sure they were not touching.
<img src="/images/dac/DAC3.jpg" alt="Circuit building" style="width: 50%; height: 50%"><img src="/images/dac/DAC5.jpg" alt="Circuit building" style="width: 50%; height: 50%">
<img src="/images/dac/DAC9.jpg" alt="Circuit building" style="width: 50%; height: 50%"><img src="/images/dac/DAC11.jpg" alt="Circuit building" style="width: 50%; height: 50%">

### Conclusion
I was pleased to see that the steps on the output were roughly consistent, more so than the demonstration circuit. Although the noice on the output compared to the demonstration circuit, I would still say that this was an issue that needed addressing. Overall, I was pleased with this circuit, I feel that there is plenty to explore with both ADCs and DACs, these are topics I may come back to in the future.

Unfortunately, at the time of writing I cannot find the image of the results to my project, I suspect it lost to a disposable USB. 