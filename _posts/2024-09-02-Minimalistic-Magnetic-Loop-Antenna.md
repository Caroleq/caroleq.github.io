---
layout: post
title: A minimalistic magnetic loop antenna
date: TODO
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
2. Covered frequency range 
I wanted to build an antenna that would receive signals in high frequency (HF) range. In general, the higher frequencies, the more difficult it becomes to build an antenna (?) or radio for it. Since in the future I plan to build my own radio receiver, I preferred not to construct an antenna for too high frequencies like UHF. On the other hand, with decreasing frequencies the geometric size of antenna increases (more on this in next requirements) and the antenna could not be too big. A compromise was high frequency range. I aimed at 20 MHz (TODO).  
3. Mobility  
It should be possible to easily move antenna from one place to another.  
4. Geometric size  
I live in an apartment in a block with a balcony. I wanted to be able to keep antenna indoors and move it to the balcony when needed. Therefore size of the antenna was limited by apartment and balcony dimensions. The ceiling is at a height of around 3 meters and the balcony has width and height equal to TODO respectively. I decided that the antenna device should not be higher than 2.5 m and wider than 1.5 m when standing vertically. Bigger size would be probably possible to build, but storing and operating it would be problematic.   
5. Time to build  
I wanted to have a working antenna as fast as possible at price of efficiency and accuracy.   
6. Input impedance  
The typical input impedance of antennas is 50 Ohm. I decided to stick to this standard. 


Dipole antennas (which are easy to build) are typically at least half-wave long. For 20 MHz half-wave is around 7.5 meters. That is far more than acceptable. After doing some research I came across magnetic loop antennas which are much smaller than other types of antennas and would fit into my geometric  




przeniesc z domu na balkon i z powrotem
szybki do zrobienia
Rozmiar - balkon
1. range frequency
2. Q factor
Odbieranie jakichkolwiek sygnalow
1. 50 Ohm, maximum SWR

2. Tunability

Antenna type

Supported range width 

Mobility

## 1. Mechanical construction 

## Capacitor

## Balloon

## Measurements

zaklocenia