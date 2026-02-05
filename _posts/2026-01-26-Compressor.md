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
The high-level idea for the design is that the audio input is buffered and then split into two paths. The first path leads to voltage-controlled amplifier (VCA). The second path processes the input and, according to parameters values, produces a signal that is applied to the VCA and controls whether and if yes, how much, the output from the VCA should be reduced. The compressed signal is then amplified by a parameterized gain and returned as the device output signal. Protection circuit limits signals whose amplitude exceed a maximum predefined level. The diagram below illustrates the compressor design.  

{:refdef: style="text-align: center;"}
![_config.yml]({{ site.baseurl }}/images/compressor/compressor-high-level.png)   
*Diagram of high level compressor design*
{: refdef}

### 4.1. Input buffer

### 4.1 Determining compressing signal
The subsystem determining compressing signal can be considered a path that starts with an input audio signal and ends with a signal that controls gain of VCA. 
Subsystem determining compressing signal is comprised of a few logical steps: 
1. full wave rectification - the input audio signal is converted into its absolute (positive) value.
2. envelope detection/peak detection - converts the rectified signal into its envelope. The shape of the envelope depends on values of attack, release and soft/hard knee parameters, which ultimately impacts how quickly the audio signal reduction will be applied and removed.
3. subtracting threshold - value of the threshold parameter is subtracted from the envelope. If threshold value is grater than envelope, the step outputs zero.  
4. signal amplification - the value from the previous step is multiplied by a value dependent on the ratio parameter (for this compressor this value was less than 1) and applied as control signal to VCA. 

The diagram below illustrates the process of generating compressing signal.

{:refdef: style="text-align: center; "}
![_config.yml]({{ site.baseurl }}/images/compressor/compression-signal.png)   
*Determining compressing signal steps*
{: refdef}

I'll elaborate on each of the steps to create the VCA controlling signal in the following subsections.  

#### Wave rectification
Below picture shows circuit that performs full wave rectification:  

{:refdef: style="text-align: center;"}
![_config.yml]({{ site.baseurl }}/images/compressor/wave_rectifier.png)   
*Wave rectification circuit*
{: refdef}

The circuit is similar to inverting operational amplifier opamp, but a diode is plugged at the opamp's output. Its operation can be split into two cases:
1. The input signal is negative - the opamp will produce positive output and current will flow through the diode similar as in the classical inverting amplifier. Because $$R_{17} = R_{18}$$, absolute values of input signal and rectified signal will be equal.  
2. The input signal is positive - the diode will be reverse biased. Current will not flow through the diode. Thus if the circuit is not loaded, the input signal and rectified signal will be equal. Output of the circuit has impedance equal to $$R_{17} + R_{18}$$, so it cannot be connected to a circuit with a low input impedance.   

#### Envelope detection
Below picture shows circuit that performs envelope detection:  

{:refdef: style="text-align: center;"}
![_config.yml]({{ site.baseurl }}/images/compressor/envelope.png)   
*Envelope detection circuit*
{: refdef}

The circuit begins with a buffer U1A that prevents overloading wave rectification circuit (see previous section). The output of U1A is set according to the rectified signal. Current flowing from U1A's output loads capacitor C1. The D1 diode prevents current from flowing into the output of U1A. The charge accumulated on C1 is leaked through chain formed by R1 resistor and RV4 potentiometer. RV4 regulates how fast C1 is discharged. This part of the circuit is connected to the buffer U1B, which outputs value equal to voltage at C1 capacitor. This voltage is used to load C6 capacitor through RV2 potentiometer. RV2 regulates how fast C6 is charged. The signal at C6 capacitor will have the shape of the rectified signal envelope. 
RV2 determines how fast value of the envelope will rise after the input signal volume increases. RV2 potentiometer is an attack parameter. Similarly RV4 determines how fast the value of the envelope will fall after the input signal value decreases: after the input signal value decreases, C1 will be discharged through R1 and RV4. This will result in discharging C6 through RV2 and decreasing envelope signal. Thus RV4 is a release parameter. 
Note: In this circuit the RV2 (attack parameter) also impacts how fast the envelope signal decreases. This is not desired, but I decided to accept that.




Depending of SW1 switch position the buffer feedback loop is wired to the anode or to the cathode side of the diode D1. 

#### Subtracting threshold

#### Signal amplification

### 4.2 Voltage Control Amplifier (VCA)

### 4.3 End amplifier

### 4.4 Limiting output signal

### 4.5 Selection of electronic elements


### 4.6 Putting this all together