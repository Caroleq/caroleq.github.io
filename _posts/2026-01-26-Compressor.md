---
layout: post
title: DIY Audio Compressor Project 
date: 
tags:
  audio
  electronics
---

This article will describe an audio compressor which I built recently as a DIY project. The goal was mainly to build an electronic device that will teach me something new rather than to use the compressor for music production.

The article contains requirements for the compressor, design of the electronic circuit, description of how the device has been assembled and tests which I run check the compressor operation.

Prerequisites: The article does not explain what is an audio compressor, basic concepts of electronics, electronic elements and building blocks of electronic circuits that are widely known and their descriptions can be easily found on the Internet. 

Acknowledgements: Thank you to Joachim for helping me with this project. 

## 1. Requirements and assumptions
Before starting the design I assumed the following characteristics of input signal:   
Input signal:
- voltage range - I assumed that voltage range will fit into range from -1V to +1V. I picked this range, because the peak-to-peak voltage measured on my PC was almost 2V.  
- frequencies range - humans are able to hear sounds in a frequency range from about 10Hz to 20kHz. Therefore I assumed that frequencies of input signal would be in a range 10Hz-20kHz.   
