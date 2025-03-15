---
layout: post
title:  "Development Journal for universal washing machine control unit"
date:   2025-03-15 18:30:00 +0200
categories: jekyll update
---

# Development Journal
# Universal Washing Machine Conrtol

## 1 Introduction 
This is the development jounal for an open source/hardware washing machine control. It shall help to repair an old washing machine for which there are no spare parts available or update a more current modell to more connectivity functions to as to use cheap electricity tarrifs with an WIFI internet connection.



## 2 General technical facts

+ Input: 230 V, 16 A
+ universal sockets with screws or clamps
+ common Mikrocontroller with open software documentation (ESP32?!)
+ fault identification techniques
+ full repair instructions
+ machine model related firmware in regards to sensor characteristics, valve control, etc.
+ the control will work for machines with so-called universal motors only. It will not serve for drive-fed motor
+ ...

## 3 Components
### 3.1 power supply
Convential washing machines without a converter drive have the following three power levels:
- 230-V-Level for Motor, Heating and Valves directly connected to the power plug (+electric filters and fuses)
- 12-V-Level for the relais control elememts
- 3.3-V-Level for the microcontroller
  
#### 3.1.1 230 V-Level
Fuse, Filter to reduce EMC levels of the universal motor

### 3.1.2 12-V-Level
The 12 V will be provided by a separat power supply unit. This will make the control more robust overall. The power functionalty of the power supply unit can be checked easily by a multimeter. It can be purchased or taken from an old PC. The latter one would also provide 5 V which is used for the 3.3 V. Alternatively almost every smartphone charger can provide the 5V and 500 mA easily.

#### 3.1.3 3.3-V-Level
The stabilized power supply of 3.3 V for the microcontroller is realized via a linear regulator from 5V.

#### 3.1.4 Zero current switching
Switching causes trouble and wear. When using a single relais an arc at opening contacts will wear the contacts eventually. Small arcs may even occure while closing when the moving contacts of the relais bounce. The wear and possible weling of the contacts is minimized by putting a thyristor parallel to the relais. This has the following advantages:

- no switching arcs for ON- and OFF-state within the relais
- minimal thermal losses on the thyristor, because of only one halfwave of current

Every switch event shall be synchronised with net voltage in such a way, that the high loads like motor and heating will be switched on just after the zero crossing of the voltage and switch off at zero current by the thyristor. The reaction time of a relais is usually less than 15 ms so it can easily open during on half cycle of the AC net volatage.

![frequency detection](/assets/images/frequency-detection.svg)

#### 3.1.5 Frequency detection

The frequency detection serves the time dependent relais switching for minimised wear at the relais contacts. The voltage of an ohmic devider at one end of the secondary winding of the power supply transformer is lead via a diode to an input of the Mikrocontroller. A 2.7...3 V-Zener-Diode protects the input against possible overvoltages.

The ESP has to measure the time difference use it for the determination of the time trigger for the relais. The high load of motor and heat resistor need to be switch on alternative in positiv an negativ halfwave to symmetric loads to the feeding electric network.

### 3.2 Heating - temperature control
#### 3.3.1 General

The water temperature is conventionally controlled by a resistance heater (230V, 2kW, 10A). The temperature is measured via an NTC element. Most washing machines use NTC sensors with a resistance of around 4.8 kΩ at 20°C. If a different model is used in a particular WaMA, this must be stored accordingly in the software for the µC.
The central µC takes over the control. The electrical switching element is a relay (12 V/230).
Relays as switching elements have the following advantages and disadvantages:
+ galvanic isolation
+ low electrical resistance → low thermal losses, no cooling required
+ favorable costs
- Burn-off at the switching electrodes and possible contact sticking – failure / life
Triacs/thyristors as switching elements
+ No mechanically moving parts → long life
+ no arcing → long life
- high thermal loss 1W/A → heat sink required
- no galvanic isolation → optocoupler required
Due to the development goal of achieving the longest possible service life, the decision was made to use a triac. The 10W losses at a current of 10 A correspond to a reasonable 10W/2000W = 0.5 % power loss. A sufficiently large heat sink is determined by tests.

#### 3.3.2 Physical boundary conditions for heating control

- Water volumen, aprrox.: 10-25 L 
- Heat capacity, water: 4,19 kJ/(kg K)
- Worst case: 90-°C-program, dT=80 K, 10..25 kg → 30...60 min heat period with thermal losses over vat and housing
- Control with relais: 2-point-control with +/- 1.5 °C sufficient
- Control via thyristor: Switch-on duration function-related 2 sine half-waves = 20 ms, the thyristors would have to be retriggered every period.

#### 3.4 Motor control
RPM, direction, tumbling, spinning

#### 3.5 magnetic valves for cold and hot water
Electric parameters for valves: 230 V, xxx? A

## 4 Fault identification strathegies

Similar to cars, which tell you that i.e. the rear right brake light is defective, the control system should be able to check itself or detect faults in the controlled components.

### 4.1 dampers
- The washing drum is a spring-mass oscillator → natural frequency → if a corresponding current harmonic in the motor current is too high, then the damper is worn out.

### 4.2 main bearing
- accustic signal via piezzo microphone.

### 4.3 heating
- no current, broken fuse, earth fault

### 4.4 main motor
- no current, then defec carbon brushes 

### 4.5 drain pump

## 5 Question lists
- 12V vs 24 V bei den Relais? → 12 V is suppost to be cheaper because more common through automotiv.
- Relais vs Thyristoren vs SSR?

## 6 2Do list
- Frequency detection with capacitive devider
- Washing programma, times, motor speed for tumbling
- ECO-Modes: temperature reduction and time extension
- determination of switch-off times of relais
