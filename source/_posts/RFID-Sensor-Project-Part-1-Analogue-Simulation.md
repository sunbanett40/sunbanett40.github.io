---
title: 'RFID Sensor Project Part 1 - Analogue Simulation'
date: 2021-02-11 11:56:00
tags:
    - Electronics
    - Circuit Simulation
---

### LCR Circuit
I first created my LCR circuit using modelsim. This required placing basic components and connecting them with wires.
<p style="text-align:center;"><img src="/images/rfid1/RFID1.png" alt="LCR circuit" style="width: 33%; height: 33%">

I then performed an AC analysis to determine the 3dB point of this bandpass filter. I found the 3dB to be 195KHz.
<img src="/images/rfid1/RFID2.png" alt="AC sweep" style="width: 100%; height: 100%">

I then changed the source type to a pulse. This allowed me to perform a transient analysis of the band pass filter.
<p style="text-align:center;"><img src="/images/rfid1/RFID3.png" alt="LCR circuit" style="width: 33%; height: 33%">

I found that the response would fluctuate.
<img src="/images/rfid1/RFID4.png" alt="Transient responce" style="width: 50%; height: 50%"><img src="/images/rfid1/RFID5.png" alt="Transient responce" style="width: 50%; height: 50%">

I then increased the frequency to 125KHz. I found that the response stabilised and that it rapidly decayed.
<img src="/images/rfid1/RFID6.png" alt="Transient responce" style="width: 50%; height: 50%"><img src="/images/rfid1/RFID7.png" alt="Transient responce" style="width: 50%; height: 50%">

However, this simulation is not a realistic case for a digital microcontroller pin which has an output impedance of around 50Ω. 
<p style="text-align:center;"><img src="/images/rfid1/RFID8.png" alt="LCR Circuit" style="width: 33%; height: 33%">

Modifying the circuit to include the 50Ω impedance I that the response decayed much more rapidly.
<img src="/images/rfid1/RFID9.png" alt="Transient responce" style="width: 50%; height: 50%"><img src="/images/rfid1/RFID10.png" alt="Transient responce" style="width: 50%; height: 50%">

### AM Demodulator
Creating the AM demodulator I was able to perform both AC and transient analysis on each of the “wires”.
<p style="text-align:center;"><img src="/images/rfid1/RFID11.png" alt="AM Demodulator" style="width: 33%; height: 33%">

I found that the output removes the information signal from the carrier frequency. It tended towards an attenuation of -50 dB.
<img src="/images/rfid1/RFID12.png" alt="AC sweep" style="width: 100%; height: 100%">
<img src="/images/rfid1/RFID13.png" alt="Transient responce" style="width: 50%; height: 50%"><img src="/images/rfid1/RFID14.png" alt="Transient responce" style="width: 50%; height: 50%">

### MFB Active Low Pass Filter
<p style="text-align:center;"><img src="/images/rfid1/RFID15.png" alt="MFB filter" style="width: 75%; height: 75%">

The active filter has a gain of 50dB. Its 3dB point is 45KHz.
<img src="/images/rfid1/RFID16.png" alt="AC sweep" style="width: 100%; height: 100%">

The transient response of the circuit shows that there is a small delay between the input and the output response, this is due to the slew rate of the op-amp used.
<img src="/images/rfid1/RFID17.png" alt="Transient responce" style="width: 100%; height: 100%">