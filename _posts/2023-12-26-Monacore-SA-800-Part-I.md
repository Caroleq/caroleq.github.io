---
layout: post
title: Monacor SA-800 Part I
date: 2023-12-26 00:00:00 +0000
tags:
  audio-amplifier
  electronics
---

The post is the first post in a cycle concerning analysis of electronic circuit of Monacor Solid State Stereo SA-800 Amplifier. It is an audio amplifier produced in the 70s [1]. Official amplifier documentation not available at the manufacturer website. The main source of information about the amplifier was [www.radiomuseum.org](www.radiomuseum.org), which has the amplifier documentation (including electronic circuit). It is available at [1].   

As mentioned before the cycle concerns the electronic circuit of the amplifier. This post describes inputs and outputs of the device and gives a high-level overview on the main circuit parts. In subsequent articles I will discuss these parts in detail. I plan to redraw the circuit parts from the document downloaded from www.radiomuseum.org into KiCad and attach them to posts. However some electronic components does not have SPICE models (probably because most of them are no longer produced). For such components I will use replacement components which have available SPICE models. Another problem is that values of some electronic elements in the image with the circuit are blurred/vague. Therefore I may unintentionally assume bad element values.   

TODO: picture of monacor amplifier

## 1. Inputs and outputs description
Documentation of device inputs and outputs is available at [1] in "SCHEMATICS" tab. It is written in German. Below I provide a translated and simplified version of inputs and outputs description. I skipped information that is not necessary to understand the operation of the amplifier circuit.

Device inputs:
- PHONO 1 - magnetic gramophone input. Possible input impedance of amplifier for PHONO 1 can take values 30kΩ or 50kΩ and is regulated witch the switch (todo)    
- PHONO 2 - magnetic gramophone input. Input impedance of amplifier for PHONO 2 is 50kΩ.
- AUX 1 - high level crystal and ceramics gramophone input.
- AUX 2 - crystal and ceramics gramophone input.
- Tuner - FM- and AM/FM-Stereo receiver input. Receivers can also be connected to AUX inputs.
- MAIN IN - discussed along with PRE OUT output
- power supply - 


Device outputs:
- Tape - output for magnetophon that can record sound coming from the selected input. Recorded sound is not altered by amplifier regulators (like treble).
- Tape out - additional output for magnetophon that records sound from the selected input after applying all available regulators. Sound coming from microphone can also be recorded through this output
- PRE OUT - this output enables plugin of an additional device (e.g. echo unit), between boards NPP-OIA and NPA-OI (see next section for boards description). Input of such device is connected to PRE OUT (output of NPP-OIA) and output is connected to MAIN IN (input of NPA-OI). 
- MONO OUT - the output conveys signal from combined two stereo canals into a single signal. Signal coming from MONO OUT connector is not as amplified as signals passed to main speakers output. An additional amplifier is needed to be located between the MONO OUT and loudspeakers.


Control elements:
- PRE OUT/MAIN IN control switch - the switch can be in positions NORMAL and SEPARATE. In NORMAL position boards NPA-OI and NPP-OIA are connected. In SEPARATE positions these boards are disconnected and connection needs to be formed by them using PRE OUT and MAIN IN.

returns input signal after applying adjustments and some amplification.    



https://en.wikipedia.org/wiki/Phonograph (Crystal and ceramic)

## 2. Electronic circuit 
Image of the circuit is provided below. I downloaded it from [2].  


There are four boards on the diagram. Borders of each board are marked with dashed lines. Their identifiers can be found at the left bottom of the board areas. Part of electronic elements, inputs and outputs are not located on any board of this circuit. I highlighted the following main functional components of the amplifier circuit (it is my subjective division based o circuit analysis - there may exist better ways to split the circuit into parts):
1. Switch - selects signal from one of the amplifier inputs and passes it to the NPP-OIC board. It also takes output from NPP-OIC board and conveys it to further parts of the amplifier. Following inputs are connected to the switch: PHONE 1, PHONE 2, AUX1, AUX2, TUNER.
2. Power supply - connects to outlet with AC, converts it into DC and supplies the whole circuit with symmetrical voltage -38V/+38V. 
3. Voltage amplifier board no 1 - located on board NPP-OIC. It amplifies voltage level of raw input signals from the switch. 
4. Various switches and regulators - this circuit part takes amplified input signals from NPP-OIC and TAPE, selects one of them and applies MUTING, BALANCE and VOLUME level regulations and passes it to NPP-OIB. 
5. Voltage amplifier board no 2 - located on board NPP-OIB. It amplifies voltage level of signal from "Various switches and regulators" part and takes MIC input. It transits signal to NPP-OIA board. 
6. Frequency filters, treble and bass regulation board - located on board NPP-OIA. It takes signal from NPP-OIB, applies low and high filters and adjusts bass and treble frequencies. Every of these functionalities can be regulated or turned on/off independently. The signal is then passed either to board NPA-OI, PRE OUT or TAPE OUT. 
7. Power amplifier board - located on board NPA-OI. It amplifies power of the signal coming from NPP-OIA or MAIN IN. Amplified signal can be transmitted either to loudspeaker, MONO OUTPUT or phones.  

Below diagram shows the forementioned components and connections between them:  


## 3. References
1. [Documentation of Monacor SA-800 at www.radiomuseum.org](https://www.radiomuseum.org/r/monacor_solid_state_stereo_ampli.html)
2. [Photo of Monacor SA-800](https://www.eserviceinfo.com/preview.php?fileid=59163&mode=direct&ext=jpg)



https://web.archive.org/web/20060427120639/http://www.schmarder.com/radios/tech/tone.htm


Für den Eingang Phono 1 kann die Eingangsimpedanz je nach verwendentem Magnetsystem mit dem danbenliegenden Schiebeschalter zwischen 30 und 50 k Ohm.

