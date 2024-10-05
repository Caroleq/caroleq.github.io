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
The loop was made of soft copper wire. Its outer diameter was 10 mm and inner diameter was around 7.7mm. The loop circumference had around 2.95 m. I aimed at one quarter of wavelength for 28 MHz band. Since the 28 MHz wavelength is around 10.71 m, a quarter of the wavelength is 2.67 m. This was around 30 cm less than circumference of my loop, but this discrepancy was acceptable for me. At two ends of the loop the wire was prolonged by around 15 cm vertically to the loop plane. These additional pieces of wire served as location to mount capacitor, balloon etc. The loop was formed using pipe bender. Shape of the resulting was not an ideal circle. Length of the "diameter" (it was not a diameter to be precise) in shortest point was 91 cm and in the longest point it was 97 cm. Therefore area enclosed by the loop could vary between 0.65 $$m^2$$ and 0.74 $$m^2$$. For my needs this inaccuracy was acceptable.       

## 2.1. Mast for the loop 
The loop needed a stable stand enabling moving the antenna from one place to another. One important aspect I had to keep in mind was that no metal elements should be present in the near of the loop - that would impact signal reception. From a few possible options I chose to construct a mast on which the loop could be hung. In wanted the mast to be at least two meters high so that the loop were placed on a reasonable height. The actual mast was constructed of polypropylene pipes. The pipes could be be easily connected one to another. I bought four pieces of polypropylene pipes (two pieces of TODO cm and two pieces of TODO cm) and two tee connectors. 

I connected these in the following configuration:  
TODO

Rola pionowych i poziomych czesci rur

Such standalone mast could not stand up straight. To provide a basis, I put the mast inside a Christmas tree stand:    
TODO

Standalone Christmas tree stand was insufficient for stabilization of the mast, because of the following problems:   
1. The stand weighted TODO and the loop + mast weighted around TODO. The mast with the loop hung would instantly fall, because the stand was too light and it hold the mast only at height of around TODO centimeters.   
2. The diameter of the mast pipes was TODO cm narrower than diameter of the slot for the tree trunk in Christmas tree stand. Taking into account the weight of mast + loop and their mounting at height of TODO cm, the bottom of the mast would easily sway within one slot:   
3. Mast did not form an ideally straight line after hanging a loop on it. Pipes that formed the mast were connected by inserting one into another. They bent under the weight of the loop.     

Problem 3 was solved by threading strained rope throughout the mast. On 3D printer I printed two caps and installed them at the bottom and on top of the mast. These caps served as mounting points for the rope. I tied the start of the rope to the bottom cap, threaded it all the way through the mast, wrapped it around the top cap and threaded it back to the bottom cap, where I tied the end of the rope to the bottom cap. The string was strained. This greatly reduced bending of mast when the loop was hung.  


To address Problem 1 an additional weight was added to christmas tree stand. I assembled a few wooden boards and mounted them around the stand. This added an additional TODO kg which prevented the antenna from failing. Boards were connected using metal screws, but the distance from the receiving loop was long enough that they should not impact signal reception.   

Problem 2 was solved by adjusting the diameter of the bottom cap (printed to solve problem 3) so that it precisely fit into the Christmas tree slot. This reduced the issue, but the antenna still swayed a little bit. To provide more stabilization I added two string connections between top cap (printed to solve problem 3) and wooden boards (added to solve problem 1). Strings prevented the mast from swaying. In the end the mast was not ideally straight, but the leaning was so small (TODO degrees) that I decided to accept that.   

TODO: wozek do jezdzenia


Mounting the 
After addressing above problems I wanted to mount the loop on the mast. I drilled holes in pipes and threaded strings (TODO: clamps?) through them. Then I hung the loop on the upper horizontally oriented pipe and tied the loop to the mast with these strings.


## Balloon
Welding

## Capacitor



## Measurements

zaklocenia

Waga: stojak+antena 4.8kg/5.1kg