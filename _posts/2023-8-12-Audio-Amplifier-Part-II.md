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

## 1.2 Powering board


## 1.3 Casing

## 1.4 Putting all together


## 2. Results of the initial start-up attempts




## 3. Defects of the amplifier


## 4. Performance of the final amplifier device


## 5. Attachments
1. [Main amplifier project in KiCad]({{ site.baseurl }}/attachments/kicad-audio-amplifier.zip) 
2. [Amplifier powering project in KiCad]({{ site.baseurl }}/attachments/kicad-powering.zip) 