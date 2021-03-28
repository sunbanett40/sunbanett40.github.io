---
title: 'RFID Sensor Project Part 1 - Analogue Simulation'
date: 2021-02-11 11:56:00
tags:
    - Electronics
    - Circuit Simulation
---

### LCR Circuit
I first created my LCR circuit using modelsim. This required placing basic components and connecting them with wires.
<img src="/images/RFID1.png" alt="LCR circuit" style="width: 33%; height: 33%">

I then performed an AC analysis to determine the 3dB point of this bandpass filter. I found the 3dB to be 195KHz.
<img src="/images/RFID2.png" alt="AC sweep" style="width: 100%; height: 100%">

I then changed the source type to a pulse. This allowed me to perform a transient analysis of the band pass filter.
<img src="/images/RFID3.png" alt="LCR circuit" style="width: 33%; height: 33%">

I found that the response would fluctuate.
<img src="/images/RFID4.png" alt="Transient responce" style="width: 50%; height: 50%"><img src="/images/RFID5.png" alt="Transient responce" style="width: 50%; height: 50%">

I then increased the frequency to 125KHz. I found that the response stabilised and that it rapidly decayed.
<img src="/images/RFID6.png" alt="Transient responce" style="width: 50%; height: 50%"><img src="/images/RFID7.png" alt="Transient responce" style="width: 50%; height: 50%">

However, this simulation is not a realistic case for a digital microcontroller pin which has an output impedance of around 50Ω. 
<img src="/images/RFID8.png" alt="LCR Circuit" style="width: 33%; height: 33%">

Modifying the circuit to include the 50Ω impedance I that the response decayed much more rapidly.
<img src="/images/RFID9.png" alt="Transient responce" style="width: 50%; height: 50%"><img src="/images/RFID10.png" alt="Transient responce" style="width: 50%; height: 50%">

### AM Demodulator
Creating the AM demodulator I was able to perform both AC and transient analysis on each of the “wires”.
<img src="/images/RFID11.png" alt="AM Demodulator" style="width: 33%; height: 33%">

I found that the output removes the information signal from the carrier frequency. It tended towards an attenuation of -50 dB.
<img src="/images/RFID12.png" alt="AC sweep" style="width: 100%; height: 100%">
<img src="/images/RFID13.png" alt="Transient responce" style="width: 50%; height: 50%"><img src="/images/RFID14.png" alt="Transient responce" style="width: 50%; height: 50%">

### MFB Active Low Pass Filter
<img src="/images/RFID15.png" alt="MFB filter" style="width: 75%; height: 75%">

The active filter has a gain of 50dB. Its 3dB point is 45KHz.
<img src="/images/RFID16.png" alt="AC sweep" style="width: 100%; height: 100%">

The transient response of the circuit shows that there is a small delay between the input and the output response, this is due to the slew rate of the op-amp used.
<img src="/images/RFID17.png" alt="Transient responce" style="width: 100%; height: 100%">