---
layout: post
title:  "Development Journal for universal washing machine control unit"
date:   2025-03-06 22:30:29 +0200
categories: jekyll update
---
_**fair**devices_ wants to become an association, listed as a German _"eingetragener Verein"_.

# Development Journal
## Universal Washing Machine Conrtol

### Introduction
This is the development jounal for an open source/hardware washing machine control. It shall help to repair an old washing machine for which there are no spare parts available or update a more current modell to more connectivity functions to as to use cheap electricity tarrifs with an WIFI internet connection.

### General technical facts

+ Input: 230 V, 16 A
+ universal sockets with screws or clamps
+ common Mikrocontroller with open software documentation (ESP32?!)
+ fault identification techniques
+ full repair instructions
+ machine model related firmware in regards to sensor characteristics, valve control, etc.
+ the control will work for machines with so-called universal motors only. It will not serve for drive-fed motor
+ ...

### Components
#### power supply
Convential washing machines without a converter drive have the following three power levels:
+ 230-V-Level for Motor, Heating and Valves directly connected to the power plug (+electric filters and fuses)
+ 12-V-Level for the relais control elememts
+ 3.3-V-Level for the microcontroller
  
#### 230 V-Level
Fuse, Filter to reduce EMC levels of the universal motor

#### 12-V-Level
The 12 V will be provided by a separat power supply unit. This will make the control more robust overall. The power functionalty of the power supply unit can be checked easily by a multimeter. It can be purchased or taken from an old PC. The latter one would also provide 5 V which is used for the 3.3 V. Alternatively almost every smartphone charger can provide the 5V and 500 mA easily.

#### 3.3-V-Level
The stabilized power supply of 3.3 V for the microcontroller is realized via a linear regulator from 5V.

### Zero current switching
Switching causes trouble and wear. When using a single relais an arc at opening contacts will wear the contacts eventually. Small arcs may even occure while closing when the moving contacts of the relais bounce. The wear and possible weling of the contacts is minimized by putting a thyristor parallel to the relais. This has the following advantages:

+ no switching arcs for ON- and OFF-state within the relais
+ minimal thermal losses on the thyristor, because of only one halfwave of current

Every switch event shall be synchronised with net voltage in such a way, that the high loads like motor and heating will be switched on just after the zero crossing of the voltage and switch off at zero current by the thyristor. The reaction time of a relais is usually less than 15 ms so it can easily open during on half cycle of the AC net volatage.
