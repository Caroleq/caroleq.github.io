---
layout: post
title: DIY Audio Compressor Project 
date: 2026-01-15 00:00:00 +0000 
tags:
  audio
  electronics
---

This article will describe an audio compressor which I built recently as a DIY project. The goal was mainly to build an electronic device that will teach me something new rather than to use the compressor for music production.

The article contains requirements for the compressor, design of the electronic circuit, description of how the device has been assembled and tests which I run check the compressor operation.

Prerequisites: The article does not explain what is an audio compressor, basic concepts of electronics, electronic elements and building blocks of electronic circuits that are widely known and their descriptions can be easily found on the Internet. 

Acknowledgements: Thank you to Joachim for helping me with this project. 

## 1. Assumptions
Before starting the design I assumed the following characteristics of input signal:   
- voltage range - I assumed that voltage range will fit into range from -1V to +1V. I picked this range, because the peak-to-peak voltage measured on my PC was almost 2V.  
- frequencies range - humans are able to hear sounds in a frequency range from about 10Hz to 20kHz. Therefore I assumed that frequencies of input signal would be in a range 10Hz-20kHz.   

Assumption for the amplifier or other device connected to the compressor:
- input impedance - TODO
- accepted voltage range

## 2. Requirements
Following parameters were the required to be present for the compressor:
- release  
- attack     
- ratio  
- threshold  
- soft knee and hard knee 
- amplifier for compressed signal  
I did not specify ranges of values for release, attack, ratio, threshold and amplifier, because my attitude was that I want a minimalistic working device.  

- maximum output voltage - around 2.5V (TODO: check)  
- output impedance - TODO: NEEDED?



- release - from 1ms to 1s (later I had to modify the requirement)  
- attack - from 0ms to 100ms  

## 3. Resources  
My design is heavily based on the compressor design presented in these YouTube videos:  
- [Designing a simple audio compressor from scratch](https://www.youtube.com/watch?v=Wag-yTyAxPA)  
- [Designing a production-ready audio compressor](https://www.youtube.com/watch?v=OMeKERW1E60)  

I also used the following articles (todo: po co) :  
- [Analog Compressor Design](https://web.archive.org/web/20250326195231/https://mattrottinghaus.com/2017/07/20/analog-compressor-design/)  
- [More op-amp circuits](https://www.physics.udel.edu/~nowak/phys645/More_opamp_circuits.htm)  


## 4. Electronic design 
The high-level idea for the design is that the audio input is split into two paths. The first path leads to voltage-controlled amplifier (VCA). The second path processes the input and, according to parameters values, produces a signal that is applied to the VCA and controls whether and if yes, how much, the output from the VCA should be reduced (see diagram below). 

![_config.yml]({{ site.baseurl }}/images/compressor/compressor-high-level.png)

### 4.1 Determining compressing signal
The subsystem determining compressing signal can be considered a path that starts with an input audio signal and ends with a signal that controls gain of VCA. 
Subsystem determining compressing signal is comprised of a few logical steps: 
1. full wave rectification - the input audio signal is converted into its absolute (positive) value.
2. envelope detection/peak detection - converts the rectified signal into its envelope. The shape of the envelope depends on values of attack, release and soft/hard knee parameters, which ultimately impacts how quickly the audio signal reduction will be applied and removed.
3. subtracting threshold - value of the threshold parameter is subtracted from the envelope. If threshold value is grater than envelope, the step outputs zero.  
4. signal amplification - the value from the previous step is multiplied by a value dependent on the ratio parameter (for this compressor this value was less than 1) and applied as control signal to VCA.  

TODO: add diagram  

I'll elaborate on each of these steps in the following subsections.  

### 4.1.1 Wave rectification

### 4.1.2 Envelope detection

### 4.1.3 Subtracting threshold

### 4.1.4 Signal amplification

### 4.2 Voltage Control Amplifier (VCA)

### 4.3 End amplifier

### 4.4 Limiting output signal

