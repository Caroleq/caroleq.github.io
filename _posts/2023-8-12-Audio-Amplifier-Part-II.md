---
layout: post
title: DIY Audio Amplifier Project Part II
date: 2023-08-12 00:00:00 +0000
tags:
  audio-amplifier
  electronics
---

This post is a continuation of the two-part series of posts describing the audio amplifier I built. The first article can be accessed at this [link]({{ site.baseurl }}/Audio-Amplifier-Part-I/). This post presents the physical construction of the device. It discusses defects of the amplifier of which I became aware after constructing the physical device. Process of starting the device, adding corrections to address the found issues and performance of the amplifier is also provided here.   

Prerequisites: Requirements, design and SPICE simulations of the amplifier are discussed in the [previous article]({{ site.baseurl }}/Audio-Amplifier-Part-I/), so it may be helpful to read it first. The article does not explain basic concepts of electronics, electronic elements and building blocks of electronic circuits that are widely known and their descriptions can be easily found on the Internet.   

Acknowledgements: Thank you to Joachim for helping me with this project.

## 1. Physical construction of the amplifier
Electronic circuitry is split into main amplifier board containing the amplifier and powering board for providing power supply to the amplifier board. 

Choosing to have two boards instead of one has the following advantages:
 - complexity of electronic circuitry of separate boards for powering and amplifying modules is smaller than complexity of one board having both modules. It is easier to analyze and debug.
 - amplifying module can be tested separately from powering module by using (electric power supply???) to temporarily provide power supply. 
 - it is possible to construct amplifying and powering modules on different types of board - board for amplifier module was PCB and board for powering module was XXX.

## 1.1 Main amplifier board
I made a PCB for the main amplifier part. To design the PCB I used PCB Editor from KiCad. Maximum current that is supposed to flow through the first two amplifier stages is not expected to exceed a few mA. Therefore I could use SMD mounting for their elements. SMD allows to limit surface needed for elements compared to THT and avoids drilling the board as in case of THT. The output stage is intended to pass a relatively big current (up to around 1A). SMD elements could get burned in such a current, so THT mounting needed to be used. SMD elements were located on one side of PCB board, THT elements on the other. More details about the PCB can be found in [KiCad project]({{ site.baseurl }}/attachments/kicad-audio-amplifier.zip). Output stage transistors are attached to radiators for heat dissipation (radiators are not present in KiCad model). Resistors R4 and R18 are cement power resistors able to conduct current of the order of 1A.

Photos of the PCB board I built are provided below. These photos were taken before fixing issues I detected after starting-up the amplifier.

Front side of the board (according to KiCad PCB project). It contains SMD elements from input stage and voltage amplifier stage:  

![_config.yml]({{ site.baseurl }}/images/audio-amplifier/pcb-front.jpg)

Back side of the board (according to KiCad PCB project). It contains THT elements from output stage:  

![_config.yml]({{ site.baseurl }}/images/audio-amplifier/pcb-back.jpg)

## 1.2 Powering board


## 1.3 Casing

## 1.4 Putting all together


## 2. Starting up the amplifier 
After constructing the main amplifier board I tested its operation (separately from powering board).

### 2.1 Initial starting-up attempts
I connected powering pins to TODO. I set supply voltage to 9V and put initial current constraint to around 100mA (less than expected value drawn when amplifying a signal) and turned on power supply to see how amplifier will behave before connecting any input. The current constraint was hit right after turning supply on. This did not necessarily mean that there was flaw in the amplifier - maybe this circuit just needed so much current in idle state. I connected generator(??? todo) to input of the amplifier and checked its response to sine wave by measuring amplifier output on oscilloscope. The output curve on oscilloscope did not resemble sine wave at all, this was a big (a few volts) garbage signal. I started to check voltages in different places on the board with multimeter. This lead me to tracking a few short circuits on the board. I fixed them, but it didn't help. Oscilloscope was still showing garbage. Also the current constraint of (todo) was still hit. The positive news was that no element seemed to be burnt, which could be the case if e.g. the element was connected the wrong way. Futhermore, no element appeared to be overheated, because too big current was passing through it. I decided to gradually relax constraints on current - perhaps the circuit just needed more current, 100mA was much less than current drawn by output stage on SPICE simulations. This carried a risk of breaking electronic elements if too big current passed through some elements, but I run out of ideas what else could be wrong. I increased the current constraint a few times up to around 1.7A. At this point the amplifier stopped hitting the constraint - it needed around 1.5A to operate. Along with increasing maximum current I noticed, that radiators connected to transistors from output stage were getting hot. It must be output stage that consumed more than 1A, which caused dissipation of heat. After a few minutes I had to turn the powering off, to prevent break electronic elements from overheating. However the output on the oscilloscope finally started resembling an amplified sine wave. After cooling down the radiators I connected amplifier input (with minijack?) to computer and output to speaker. I turned on the power again and played some music from the computer. The loudspeaker played the this music - my amplifier worked!

### 2.2 Measuring performance of the amplifier
I run FFT analysis to check linearity of the amplifier. To do that I exported amplifier responses from oscilloscope software to csv, then using MATLAB script I generated their FFT plots. I considered two cases: when no input was provided to the amplifier and when amplifier got 1kHz sine wave with amplitude 200mA. TODO: add the script?

FFT plots of the response when no input is provided:  
1. Linear plot:   
![_config.yml]({{ site.baseurl }}/images/audio-amplifier/fft-no-input.png)   
1. Logarithmic plot:   
![_config.yml]({{ site.baseurl }}/images/audio-amplifier/fft-no-input-log.png)   
  
Logarithmic FFT plot of the amplifier response to sine wave with frequency 1kHz and amplitude 200mV:  
![_config.yml]({{ site.baseurl }}/images/audio-amplifier/fft-1khz.png)

From these plots it is clear that the amplifier runs into self-oscillations. When no input is provided, the amplifier generates a signal at frequencies being approximately multiples of 100kHz. 
When the 1kHz signal is provided as an input signal of the amplifier, 1kHz frequency is also present as the highest stripe on the FFT plot. However, there are also visible small stripes at multiples of 1kHz, which indicates a high THD. Moreover, there is also the self-oscillation at frequencies being multiples of 100kHz (including 100kHz frequency). This is similar to the previous plot, but the amplitudes of the self-oscillation stripes are smaller, when input is provided to the amplifier.  

Self-oscillations are unacceptable for the amplifier. These oscillations were not audible directly, because they were beyond frequency range of human hearing, but they might affect quality of sound and from aesthetical point of view it was really ugly. THD was also too high - I did not calculate it, because technically stripes coming from self-oscillation which are not a part of THD would need to be included.

## 3. Defects of the amplifier
I identified the following problems with my amplifier:
## 3.1. Overeating of radiators
Within a few minutes of turning on radiators were becoming hot. The amplifier needed to be turned off then to cool radiators, because of the risk of damaging the electronic elements. To address this problem I installed a ventilator each of the radiators. This partially solved the problem. Radiator were getting hot much slower, however after a some longer time (todo:) they were becoming hot again. Also cement resistors R4 and R18 were becoming hot after some time since turning on.
## 3.2. Self oscillations of the circuit 
This was a major problem, since it might affect quality of sound and it was not clear where was the source of the issue. I did not put decoupling capacitors on supply rail, which for example could be a source of a negative feedback loop which in turn could be a source of oscillations. 
## 3.3 Large current consumption
This is a root cause of overheating of the elements. Current drawn by the output stage of the amplifier was much bigger than what I obtained on simulations (415mA root mean square). Unfortunately lots of the drawn current was wasted by dissipating into heat on power transistors and resistors instead of being delivered to loudspeaker.

## 4. Operation of the final amplifier device


## 5. Attachments
1. [Main amplifier project in KiCad]({{ site.baseurl }}/attachments/kicad-audio-amplifier.zip) 
2. [Amplifier powering project in KiCad]({{ site.baseurl }}/attachments/kicad-powering.zip) 