---
layout: post
title: DIY Audio Amplifier Project Part II
date: 2023-08-12 00:00:00 +0000
tags:
  audio-amplifier
  electronics
---

This post is a continuation of the two-part series of posts describing the audio amplifier I built. The previous article can be accessed at this [link]({{ site.baseurl }}/Audio-Amplifier-Part-I/).

Finally the post describes physical construction of the device and attempts to start the built amplifier.


## 1. Physical construction of the amplifier
The most important decisions regarding physical construction of the amplifier were:
1. How (if at all) to split different amplifier components into multiple boards. 
2. The way of mounting electronic elements on the board (SMD or THT). 
I split the main amplifier module and the powering module to separate boards. 
I made a PCB for the main amplifier part. To design the PCB I used PCB Editor from KiCad. Maximum current that is supposed to flow through the first two amplifier stages is not expected to exceed a few mA. Therefore I could use SMD mounting for their elements. SMD allows to limit surface needed for elements compared to THT and avoids drilling the board as in case of THT. The output stage is intended to pass a relatively big current (up to around 1A). SMD elements could get burned in such a current, so THT mounting needed to be used. SMD elements were located on one side of PCB board, THT elements on the other. More details about the BCB can be found in KiCad project attachment. Output stage transistors are attached to radiators for heat dissipation (radiators are not present in KiCad model). Resistors R4 and R18 are cement power resistors able to conduct current of the order of 1A.
Powering board: 



## 2. Results of the initial start-up attempts




## 3. Defects of the amplifier


## 4. Performance of the final amplifier device


## 5. Attachments
1. [Main amplifier project in KiCad]({{ site.baseurl }}/attachments/kicad-audio-amplifier.zip) 
2. [Amplifier powering project in KiCad]({{ site.baseurl }}/attachments/kicad-powering.zip) 