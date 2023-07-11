---
layout: post
title: DIY Audio Amplifier Project
date: 2018-03-13 00:01:00 +0000
tags:
  cpplang-slack
  how-to
  web
---
This article(post?) contains a description of an audio amplifier I built as a DIY project for the purpose of learning electronics. The post discusses design of electronic circuit of the amplifier. It also discusses simulations and analyses that were run on the KiCad amplifier model to check if the amplifier operation meets requirements(?) (simulation behaviour can differ from physical device behaviour, so desired results of simulation do not guarantee desired operation of real amplifier). KiCad project with electronic circuit schematic and PCB design is attached here(todo). Finally the post describes physical construction of the device and attempts to start the built amplifier.

Do czego służy wzmacniacz audio? 

Prerequisites: The article does not explain basic concepts of electronics, electronic elements and building blocks of electronic circuits that are widely known and their descriptions can be easily found on the Internet. 

Caution: If you plan to use this post to built your own amplifier to be careful about your speaker and computer (or other equipment) which you connect to the amplifier. If the amplifier draws too big current from the computer, the soundcard may get damaged. Also if the amplifier transmits too much power to the speaker, the speaker may get damaged. Check how much current can be drawn from the soundcard and how much power can be provided to the speaker before connecting the amplifier to it (more spacs?). I give no guarantee that electronic equipment will not break after being connected to the amplfier I present in this article. 

Acknowledgements: Thank you to Joachim for helping me with this project.

## Table of contents
1. [Requirements and constraints] Requirements and constraints



## 1. Requirements, constraints, assumptions
Before starting the design of the amplifier I set requirements for:   
1. Input signal:
- voltage range: I set it to -0.5V - +0.5V. TODO: explain why?  
- frequencies range: Humans are able to hear sounds in a frequency range from about 10 Hz to 20 kHz. Therefore I assumed that frequencies of input signal frequency would be in a range 10Hz-20kHz   
2. Speaker connected to amplifier:  
- impedance of the speker: Impedance of the most of loudspeakers is in range from 4 $$\Omega$$ to 12 $$\Omega$$. I assumed that impedance of speaker will be 8 $$\Omega$$. TODO: napisać coś o połączeniach szeregowych.  
- maximum input power of the speker:  ???
3. Amplifier:  
- (voltage/power?) gain - 
- variability of voltage gain accross signal frequency range - voltage gain should not change much for different frequencies - otherwise sound volume would vary depending on frequency. I did not set precise requirement for maximum change of gain accross frequency range. Instead I decided to visually evaluate diagram (curve?) of amplifier gain in frequency domain. The shape of the curve has to remain flat in the supported frequency range.     
- phase shift of the response accross signal frequency range
- shape of output response: THD, saturation etc
- power/current requirements? 
- input impedance of the amplifier - check input imedance of existing amplifiers


## 2. General idea (todo: rename?)
Currently used audio amplifiers are divided into a few classes (link). I decided to build an amplifier of class AB, because it is relatively easy to construct as a DIY project and has relatively few disadvantages compared to other classes e.g. class A. My amplifier consists of three main parts (or maybe components?): differential pair, cascode and Shiklay pair. All of them are widely known building blocks of electronic circuits, so this article does not describe their operation (if you do not know how they work - google it). Role of each amplifier part is specified in a separate subsection.


### 2.1 Differential Pair
Differential pair (also known as long-tailed pair) is the first stage (czy to jest dobrze sformułowane?) of the amplifier.  

Differential pair performs several functions:  
- accepts amplfier input - amplifier input is connected to one of differential pair inputs.  
- amplifies input by a small factor - further stages have grater gain allowing to reach desired amplifiaction.  
- accepts feedback from amplifier output - amplifier output is connected (via capacitor and voltage divider) to one of differential pair inputs. Feedback loop improves stability and linearity of the circuit.  
- ensures sufficiently high input impedance of the amplifier - TODO

Differential pair is connected to the cascode amplifier described in the next subsection. 

I decided to use a current source at input transistors emmiters to improve CMRR (basic version of long-tailed pair has a resistor at input transistors emitters). Also I applied a current-mirror to split current more equally between the two input transistors. The mirror tries to split current equally. TODO.

Reference no 1 and 2 in References section are a good source of information about differential pairs.   

TODO: formula for feedback?
TODO: schema?

### 2.2 Cascode
The second stage is cascode. Its function is to amplify voltage signal coming from the previous stage. Cascode provides the largest part of voltage amplification of all stages. I chose cascode as a second stage, because of moderately high input impedance and wide bandwidth. If I needed to increase overall amplifier voltage gain I could use two cascodes connected in series instead of one. Operating points of cascode transistors are set with voltage divider.


### 2.3 Shiklay pair 

todo: 2.4 to describe components connections?


## 3. Electronic design
Electronic design of the amplifier is split into two modules: first module contains a circuit introduced in previous section for amplifying a signal, the second module serves as a reliable (and stable?) power supply. Circuits were modeled using KiCad software. Beforehand mentioned modules are described in the following subsections. 

### 3.1 Main amplifier module

Especially big impact on gain response shape had voltage divider of feedback loop (Resistors ??? and ???). Minor change of 

### 3.2 Powering module


## 4. Frequency analysis   
To check if the amplifier response meets requirements accross amplification frequency range I run a frequency analysis. I computed gain and phase shift of the amplifier in frequency domain using SPICE program [Description how to click it?]. Subsections below present analysis results for ....   
Diagrams below present gain and pahe shift for a final version of the amplifier. When I initially created the electronic cricuit the results did not meet requirements and values of some resistors and capacitors needed to be tuned to improve the shape of gain and shift-phase responses.  
### 4.1 Analysis of amplitude gain
As mentioned in the requirements section, changes of gain values in frequency range 10Hz-20kHz should be small. [Zapas??]  
TODO: Opisać wykres  
When I first run the analysis for the circuit gain value was not constant/stable? accross considered frequency range. At the same time value of the gain had to remain acceptably high. Also I had to pay attention to the shape of the amplifier response (any non-linearities are unacceptable). 
With this in mind I changed values of resistors and capacitors in a few places in the whole project to flatten magnitude/gain response. 
### 4.2 Analysis of phase shift
The phase shift along the change of frequencies should remain as small as possible? Why? Instability? Sound?  

## 5. Physical construction


### 5.1 PCB design 

## 6. Starting-up the amplifier


## 7. References
1. Paul Horwitz, Winfield Hill, The Art of Electronics, 3rd edition (Cambridge University Press, 2015). Section 2.3.8: Differential amplifiers  
2. Douglas Self, Audio Power Amplifier Design Handbook, 4th edition (Focal Press, 2006). Section: TODO