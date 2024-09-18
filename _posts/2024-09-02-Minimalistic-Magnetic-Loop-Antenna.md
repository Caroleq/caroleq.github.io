---
layout: post
title: A minimalistic magnetic loop antenna
date: 2024-09-18 00:00:00 +0000
tags:
  antenna
  electronics
  ham radio
---

This article describes a magnetic loop antenna I built to listen to signals in high frequency range. I wanted to put minimum effort to have a device that enables reception of signals in high frequency range. Efficiency, accuracy and tunability were secondary issues. [This article](https://www.nonstopsystems.com/radio/frank_radio_antenna_magloop-2turn.htm) is an example description of antenna that was designed with paying much more attention to these features. 
After building an antenna that receives high frequency signals I plan to build a DIY ham radio that could process these signals (for the moment I use HackRF One to receive signals from the antenna).  

Prerequisites: To understand the article it would be beneficial to be familiar with operation of magnetic loop antenna, radio transmission, basic concepts of electronics and equipment used to measure antenna parameters and performance.   

Caution: If you plan to use this post to build your own antenna, keep in mind that it is intended only for receiving signals. Construction of the antenna may not be suitable for signal transmission for a few reasons:   
1. While transmitting voltage on magnetic loop capacitor may reach a few kV. Electrical breakdown will probably occur on the capacitor used in this antenna after applying such a high voltage.   
2. If SWR of an antenna is too high, the output stage of transmitter may get broken. I did not analyze SWR of the built antenna in terms of risk of destroying transmitter.      

Acknowledgements: Thank you to Joachim and TODO for helping me to build the antenna.

## 1. Requirements 
Below a list of initial requirements for my antenna is provided: 
1. Communication direction  
As mentioned above this antenna is intended to be a receiving antenna. 
2. Directivity  
The antenna may be either directional or omnidirectional.   
3. Target frequency 
I wanted to receive signals in high frequency (HF) range. In general, the higher frequencies, the more difficult it becomes to build an antenna (?) or radio for it. Since in the future I plan to build my own radio receiver, I preferred not to construct an antenna for too high frequencies like UHF. On the other hand, with decreasing frequencies the geometric size of antenna increases (more on this in next requirements) and the antenna could not be too big. A compromise was high frequency range. I aimed at 28 MHz (TODO).  
4. Frequency range  
Frequency range of the antenna should be at least 1 MHz. If the bandwidth is narrower than that, antenna should be equipped with some tuning mechanism.  
5. Mobility  
It should be possible to easily move antenna from one place to another.  
6. Geometric size  
I live in an apartment in a block with a balcony. I wanted to be able to keep antenna indoors and move it to the balcony when needed. Therefore size of the antenna was limited by apartment and balcony dimensions. The ceiling is at a height of around 3 meters and the balcony has width and height equal to TODO respectively. I decided that the antenna device should not be higher than 5.35 m and wider than 1.5 m when standing vertically. Bigger size would be probably possible to build, but storing and operating it would be problematic.   
7. Time to build  
I wanted to have a working antenna as fast as possible even at price of efficiency and accuracy.  
8. Costs  
Constructing the antenna should be as cheap as possible.  
9. Input impedance  
The typical input impedance of antennas is 50 Ohm. I decided to stick to this standard.
10. Selectivity  
I did not put any precise constraints on selectivity. The only demand was to be able to recognize human speech.  

**Choosing the antenna type**  
Based on the above requirements I chose magnetic loop as a type of the antenna to build. Initially I thought about constructing a dipole antenna. Dipoles are far more easy to build than magloops. There is however one problem. Dipole antennas are typically at least half-wave long. For 28 MHz half-wave is around 7.5 meters. That is far more than acceptable, because of space limitations. Magnetic loop was the only type of antenna I found that did not require straight piece of wire of length less than half-wave.   



## 2. Constructing the antenna  

## 2.1 Loop

## 2.1. Mast for the loop 
The loop needed a stable stand enabling moving the antenna from one place to another. From a few possible options I chose to construct a mast on which the loop could be hung. In wanted the mast to be at least two meters high so that the loop were placed on a reasonable height. The actual mast was constructed of polypropylene pipes. The pipes could be be easily connected one to another. I bought four pieces of polypropylene pipes (two pieces of TODO cm and two pieces of TODO cm) and two tee connectors. 

I connected these in the following configuration:  
TODO

Such standalone mast could not stand up straight. To provide a basis, I put the mast inside a christmas tree stand:    
TODO


Christmas tree stand

## Capacitor

## Balloon

## Measurements

zaklocenia