---
layout: post
title: DIY Audio Amplifier Project
date: 2023-07-23 00:00:00 +0000
tags:
  audio-amplifier
  electronics
---


This post contains a description of an audio amplifier I built as a DIY project for the purpose of learning electronics. The post discusses design of electronic circuit of the amplifier. It also discusses simulations and analyses that were run on the KiCad amplifier model to check if the amplifier operation meets expectations (simulation behavior can differ from physical device behavior, so desired results of simulation do not guarantee desired operation of real amplifier). KiCad project with electronic circuit schematic and PCB design is attached here(todo). Finally the post describes physical construction of the device and attempts to start the built amplifier. While designing the amplifier I used Audio Power Amplifier Design Handbook as a reference. Most of the ideas used to create the project is taken from this book, although my amplifier is not a direct copy of any amplifier presented there.

Prerequisites: The article does not explain basic concepts of electronics, electronic elements and building blocks of electronic circuits that are widely known and their descriptions can be easily found on the Internet. 

Caution: If you plan to use this post to built your own amplifier be careful about your speaker and computer (or other equipment) which you connect to the amplifier. If the amplifier draws too big current from the computer, the soundcard may get damaged. Also if the amplifier transmits too much power to the speaker, the speaker may get damaged. Check how much current can be drawn from the soundcard and how much power can be provided to the speaker before connecting the amplifier to it. I give no guarantee that electronic equipment will not break after being connected to the amplifier I present in this article. 

Acknowledgements: Thank you to Joachim for helping me with this project.

## 1. Requirements and assumptions
Before starting the design I set assumptions for input signal and output load (i. e. loudspeaker) parameters:   
Input signal:
- voltage range - I assumed that voltage range will fit into range from -0.5V to +0.5V. I picked this range, because it fits [consumer line level](https://en.wikipedia.org/wiki/Line_level). However later after designing electrical circuit and creating physical device I measured voltage on minijack on my PC, the peak-to-peak voltage reached almost 2V. This is a different result than what I expected. As a workaround I decided to add a potentiometer in series with the whole amplifier.
- frequencies range - humans are able to hear sounds in a frequency range from about 10 Hz to 20 kHz. Therefore I assumed that frequencies of input signal would be in a range 10Hz-20kHz.   

Speaker connected to amplifier:   
- input impedance - impedance of the most of loudspeakers is in range from 4Ω to 12Ω. I assumed that impedance of speakers connected to my amplifier will be around 8Ω. Loudspeaker with impedance lower than 8Ω can be connected to the amplifier in series with a resistor of value (8 - loudspeaker resistance)Ω.
- maximum input power - most of loudspeakers can get maximum power value of 10-20W. I assumed that speakers connected to my amplifier can get up to 20W. 

I also set requirements for my amplifier:
- power - main goal of audio amplifiers is to increase power of the audio signal. As mentioned before most loudspeakers can get maximum power of 10-20W. I set a requirement that the amplifier should be able to deliver least 15 W. (is it dependent on the impedance?)
- voltage gain -  The higher the voltage gain, the higher power of the signal. Therefore I decided that the amplifier should increase voltage level of the signal at least by 4. 
- variability of voltage gain across signal frequency range - voltage gain should not change much for different frequencies - otherwise sound volume would vary depending on frequency. I did not set precise requirement for maximum change of gain across frequency range. Instead I decided to visually evaluate diagram of amplifier gain in frequency domain. The shape of the curve has to remain flat in the supported frequency range.     
- phase shift of the response across signal frequency range - variance of phase shift across frequency range could impact the amplifier sound. Therefore I set a loose requirements, that phase shift should remain as constant as possible within frequency range. Additionally phase shift could matter when connecting feedback loop (described later) - for stability purpose the best option seems to be intuitively to have no phase shift. However I decided not to put any strict requirements on the phase shift and to verify system stability by observing its output.
- shape of output response - amplifier should behave linearly. Input signal within supported range should not cause amplifier saturation.
- input impedance of the amplifier - input impedance should be relatively high to prevent drawing too high current from the input signal source. I assumed that 1kΩ should be enough, although most of commercial amplifiers has higher input impedance e.g. 10kΩ.


## 2. General idea (todo: rename?)
Audio amplifiers are divided into a few [classes](https://www.analog.com/en/technical-articles/types-of-audio-amplifiers.html). I decided to build an amplifier of class AB, because it is relatively easy to construct as a DIY project and has relatively few disadvantages compared to other classes e.g. class A. My amplifier consists of three main parts: differential pair, cascode and output part. Differential pair and cascode are widely known building blocks of electronic circuits, so this article does not describe their operation (if you do not know how they work - google it). Role of each amplifier part is specified in a separate subsection. 

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

### 2.2 Cascode
The second stage of the amplifier is cascode. Its function is to amplify voltage signal coming from the previous stage. Cascode provides the largest part of voltage amplification of all stages. I chose cascode as a second stage, because of moderately high input impedance and wide bandwidth. If I needed to increase overall amplifier voltage gain I could use two cascodes connected in series instead of one. Operating points of cascode transistors are set with voltage divider. Signal from cascode is transmitted to output stage.


### 2.3 Output stage 
Output stage is the final stage of the amplifier. Its main function is to provide high power to the load (i.e. loud speaker). Voltage amplified signal is taken from the previous stage and passed to the load. Output stage enables load to draw sufficiently high current. Therefore it provides a high power to the load (power is product of voltage and current).
Apart from supplying sufficient power, the stage has to produce output signal with characteristics class AB amplifier. That is operation of output stage should be more efficient than operation of class A amplifier and crossover distortion should not occur.
The stage is implemented as a biased push-pull amplifier. Two complementary Sziklai pairs are components of push-pull pair which alternately turn on and off. I chose Sziklai pairs, because they can supply greater current than a single transistor.
Output of this stage is connected to differential pair forming feedback loop.

### 2.4 Simplified schema of the amplifier
To better illustrate operaton of the amplifier I added its simplified schema. Many elements which are present in the real amplifier are skipped here, because the goal is to demonstrate general operation of the amplifier. Diagram is split by vertical lines into three parts, one for each stage. 

![_config.yml]({{ site.baseurl }}/images/audio-amplifier/amplifier-simplified.png)

Role of some of the electronic elements from the schema is listed below: 
1. Differential stage:
- transistor Q1 - takes amplifer input signal 
- transistor Q2 - takes feedback loop signal 
- transistors Q3, Q4, Q5 - form Wilson(?) current mirror
2. Cascode:
- resistors R1, R2, R3 - set operating point of cascode transistors
- transistor Q7 - forms common emitter part of cascode
- transistor Q6 - forms common base part of cascode
3. Output stage:
- diodes D1, D2 - add biasing of push-pull pair
- transistors Q8, Q10 - Sziklai pair in NPN configuration 
- transistors Q9, Q11 - Sziklai pair in PNP configuration 

## 3. Electronic design
Electronic design of the amplifier is split into two modules: first module contains a circuit introduced in previous section for amplifying a signal, the second module serves as a reliable (and stable?) power supply. Each module has its own electronic board. Circuits were modeled using KiCad software. Beforehand mentioned modules are described in the following subsections. 

### 3.1 Details of main amplifier module
This section provides complete electronic design of the amplifier. General idea of amplifier operation and its schema is discussed in the previous chapter, so here I just describe details of the electronic design, which were not provided so far (like usage of additional elements, choosing elements values, expected current and voltage levels etc.). 

#### 3.1.1 Differential Pair
Screen of the KiCad model of the subcircuit is provided below. 


Notes regarding the schema: 
1. Current source. I implemented current source as a transistor biased with two resistors. Additionally there is potentiometer, which can be used to regulate current value a little bit. When potentiometer's resistance is split equally on both sides, the current source should be able to produce TODO mA.
2. Resistor R5 - sets operating point of transistor Q1. Current flowing through Q1 is controlled by the current source. For the current to flow through Q1's collector, Q1's base also needs to draw current from somewhere. Current cannot be drawn from input since its direct current component is cut off by C1 capacitor. Therefore a resistor connected to ground is added to Q1's base to enable current flow through the base. A side effect of adding the resistor is a limitation of an input impedance of the amplifier. Too law input impedance could possibly destroy computer network card. Computing the voltage/current ratio of input signal showed that input impedance is around 1k$$\Omega$$, which hopefully would be sufficiently high.
3. Resistors R9 and R10
4. Input capacitor C1 - cuts off direct current component of the input signal.
5. Voltage divider of feedback loop - direct connecting feedback signal to amplifier did not give good simulation results. I had to calibrate the weight of feedback loop signal by multiplying it by gain (less than 1) of voltage divider. It was hard to find resistor values which would give satisfactory amplifier behaviour on the simulation. Changing resistor values of the divider by 1 $$\Omega$$ could drastically change the shape of amplifier response (1 $$\Omega$$ is often more than tolarance of a resistor). A possible explanation is that there were some errors in KiCad calculations of the response. Finally I managed to find resistor values which gave expected gain and response shape. However there was no guarantee that the real amplifier will behave exactly the same as the simulation. Therefore I added two potentiometers to enable tuning of feedback signal weight.

Final simulation results: 
Amplifier input was a sine wave of +0.

TODO: describe result of analysis. 

#### 3.1.2 Cascode
Screen of the KiCad model of the subcircuit is provided below. 

Notes regarding the schema: 
1. Potentiometer RV3 - voltage amplification of cascode during simulation was satisfactory, but I decided to add possibility to change operating point of cascode transistors in case physical model differs from simulation.
2. Connecting output of differential pair and cascode through capacitor C4 - capacitor cuts DC component from signal so it does not impact cascode operating point. 
3. Capacitor C3 - lowers value of Q16 emitter resistance without an influence on operating point of transistors. Resistor R27 impacts the operating point of the cascode. For DC current capacitor is a break in the circuit, so C3 does not influence  the operating point of the cascode's transistors. For amplified AC signal, C3 is an element with finite impedance. Therefore C3 connected parallelly with R27 lowers Q16 emitter resistance for amplified AC signal, which increases cascode gain.  
4. Input impedance of cascode - Differential pair has high output impedance, so impedance of C4 (together with whole cascode ?) should not be also be too low, because it would disturb operation of differential pair.
5. 

#### 3.1.3 Output stage
Screen of the KiCad model of the subcircuit is provided below. 


Notes regarding the schema: 
1. Connecting output stage and output from cascode with capacitor - capacitor cuts DC component from signal so it does not impact output stage operating point. 
2. Using transistors as diodes (Q10, Q11) - instead of conventional diodes two transistors with bases connected to collectors are used. For some reason implementing diodes with transistors from Sziklai pairs (the same transistor model) gave far better results on simulation than ordinary diodes. I suppose this is because characteristics of such implemented diodes are similar to characteristics of the first transistors of Sziklai pairs (Q9 and Q12).
3. resistors R21 and R19 - these resistors prevent leakage current and improve turn-off speed of Sziklai pairs. It is described in more datail in [3].
4. resistors R4 and R18
5. Connecting output stage and amplfier output with capacitor - capacitor cuts DC component from signal. Only AC current should be provided to a speaker, DC current could break it (why?).

### 3.2 Powering module
I chose -9V and +9V to be supply voltages of the amplifier. I decided to use 230V electrical lines for powering (instead of batteries). To make electrical line supply applicable for my device, the powering needed to be converted from 230V AC to -9/+9V DC. Also powering had to be stable. Any voltage spikes from supply module could possibly break electronic elements.



## 4. Simulations
THD
## 4. Frequency analysis   
To check if the amplifier response meets requirements accross amplification frequency range I ran a frequency analysis. I computed gain and phase shift of the amplifier in frequency domain using SPICE program [Description how to click it?]. Subsections below present analysis results for ....   
Diagrams below present gain and pahe shift for a final version of the amplifier. When I initially created the electronic cricuit the results did not meet requirements and values of some resistors and capacitors needed to be tuned to improve the shape of gain and shift-phase responses.  
### 4.1 Analysis of amplitude gain
As mentioned in the requirements section, changes of gain values in frequency range 10Hz-20kHz should be small. [Zapas??]  
TODO: Opisać wykres  
When I first run the analysis for the circuit gain value was not constant/stable? accross considered frequency range. At the same time value of the gain had to remain acceptably high. Also I had to pay attention to the shape of the amplifier response (any non-linearities are unacceptable). 
With this in mind I changed values of resistors and capacitors in a few places in the whole project to flatten magnitude/gain response. 
### 4.2 Analysis of phase shift
The phase shift along the change of frequencies should remain as small as possible? Why? Instability? Sound?  

## 5. Physical construction
The most important decisions regarding physical construction of the amplifier were:
1. How (if at all) to split different amplifier components into multiple stages. 
2. The way of mounting electronic elements on the board (PCB or THT). 
Main amplifier board: maximum current that is supposed to flow through the first two amplifier stages is not expected to exceed X mA. Therefore I decided to use PCB mounting for their elements. The output stage is intended to pass a relatively big current (up to Y mA). PCB elements which are 
Powering board: 




todo: opisać typy obudowy
TODO: opisać co było w THT, a co w PCB i dlaczego.
1. Transistor models. I used transistors BC817 and BC807, because ... . 
### 5.1 PCB design 

## 6. Starting-up the amplifier


## 7. References
1. Paul Horwitz, Winfield Hill, The Art of Electronics, 3rd edition (Cambridge University Press, 2015). Section 2.3.8: Differential amplifiers  
2. Douglas Self, Audio Power Amplifier Design Handbook, 4th edition (Focal Press, 2006). Section: TODO
3. Paul Horwitz, Winfield Hill, The Art of Electronics, 3rd edition (Cambridge University Press, 2015). Section 2.4.2 Darlington connection  