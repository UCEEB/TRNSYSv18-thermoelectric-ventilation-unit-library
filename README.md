# Getting started with TRNSYSv18 thermoelectric ventilation unit library
## Description
This repository contains codes for types representing thermoelectric ventilation unit and its controllers. These types are add-on extensions to be installed to TRNSYSv18 64bit simulation environment [https://www.trnsys.com/]. The types are written in fortran language and compiled to *.dll via TRNSYS TypeStudio, embedded toolkit of TRNSYSv18 simulation environment.  

## Install
1) Download released zip file [https://github.com/UCEEB/TRNSYSv18-thermoelectric-ventilation-unit-library/blob/680482849760598bc2d79baa0faa043579759942/TRNSYSv18-thermoelectric-ventilation-unit-library.zip]
2) Exteract all files into TRNSYS18 root folder i.e. C:\TRNSYS18 as default
3) New files such as proformas, fortran codes, executable dynamic libraries, documentation and example files will be unzipped into standard TRNSYS reposistories
4) Open TRNSYS Simulation Studio and find "Thermoelectric Units" library in the library tree at the right side of the Simulation Studio

Note: The funcionality can be quickly tested via accompanied  example file in folder C:\TRNSYS18\Examples\Thermoelectric ventilation unit as default  

## Features
The library contains 
* Type 221 - Active thermoelectric heat excanger
* Type 230 - Rule-based controller of the active thermoelectric heat exchanger unit

The assembly of these components can simulate energy behaviour of  ventilation unit with "pasive" and "active" heat exchangers for precise conditioning of the supply air. The type 230 controller in assembly with Type 23 PID controller provide rule-based control regimes of the unit such as 
* operation without active heat exchanger incl. bypass control
* operation with active heat exchanger incl. bypass control
* operation with active heat exchanger in overheating mitigation mode incl. bypass control
* operation with active heat exchanger in underheating mitigation mode incl. bypass control

## Acknowledgment
This project has received funding from  the  European  Union’s Horizon 2020     research     and innovation    programme    under grant agreement No 958218 PLURAL [https://www.plural-renovation.eu/] 



# TRNSYS 18 – Thermoelectric Ventilation Unit Library - Mathematical Reference

## About the Manual

The information presented in this manual is intended to provide a detailed mathematical and technical reference for the Thermoelectric Ventilation Unit Library developed for TRNSYS 18. The library consists of semi-physical component models representing the thermodynamic behaviour and control of thermoelectric ventilation systems for building applications.

This manual is not intended to provide detailed reference information about the TRNSYS 18 simulation software and its utility programs. More details can be found in other parts of the TRNSYS documentation set.

### Revision History

- 2021-12 — Initial implementation of the thermoelectric ventilation unit library  
- 2022-08 — Updated operational modes and control strategies  
- 2025-05 — Mathematical reference manual preparation

---

# Table of Contents

1. Introduction  
2. Type 221 - Thermoelectric Ventilation Unit Model  
3. Type 230 – Thermoelectric Ventilation Controller  
4. TRNSYS Simulation Implementation  

---

# Introduction

This manual presents a mathematical and technical reference for the Thermoelectric Ventilation Unit Library developed for the TRNSYS 18 simulation environment. The library was created to support detailed numerical analysis of façade-integrated decentralized air handling units (AHUs) equipped with passive and active thermoelectric heat recovery systems.

The library consists of two complementary simulation components:

- **Type 221** — Semi-physical performance-map-based model of the thermoelectric ventilation unit.
- **Type 230** — Supervisory controller responsible for operational mode selection, PID-based temperature regulation, bypass management, and fan control strategies.

The implementation enables integration with standard TRNSYS components as well as external airflow and contaminant transport solvers such as CONTAM developed by the National Institute of Standards and Technology (NIST).

**Figure Placeholder:** Example design of the integrated air handling unit.

---

# Type 221 - Thermoelectric Ventilation Unit Model

Type 221 represents the principal semi-physical model of the façade-integrated thermoelectric ventilation unit. The component describes the thermodynamic behaviour of the AHU using a performance-map-based approach combining passive sensible heat recovery and active thermoelectric conditioning of the ventilation air streams.

## Passive Heat Exchanger

The passive heat exchanger is based on the constant-effectiveness minimum-capacitance method adopted from the standard TRNSYS Type 760 formulation.

The sensible heat transfer between the two air streams is calculated as:

```math
\dot{Q}_{pHX} = \varepsilon \cdot \min(C_{fr,amb}, C_{ret,act}) \cdot (T_{ret,aHX} - T_{fr})
```

The outlet enthalpies are calculated as:

```math
h_{exh} = h_{ret,aHX} - \frac{\dot{Q}_{pHX}}{\dot{m}_{ret}}
```

```math
h_{sup,pHX} = h_{fr} + \frac{\dot{Q}_{pHX}}{\dot{m}_{fr}}
```

## Active Thermoelectric Heat Exchanger

The active heat exchanger consists of thermoelectric cells connected in series and parallel arrangements.

The effective cooling rate and voltage are corrected using empirical inefficiency factors:

```math
\dot{Q}_{c,TE,eff} = \dot{Q}_{c,TE} \cdot k_{th}
```

```math
U_{TE,eff} = U_{TE} \cdot k_{el}
```

The effective heating rate is given by:

```math
\dot{Q}_{h,TE,eff} = \dot{Q}_{c,TE,eff} + U_{TE,eff} \cdot I_{TE}
```

The total cooling and heating rates are:

```math
\dot{Q}_{sup,aHX} = -n_{parallel} \sum_{i=1}^{n_{series}} \dot{Q}_{c,TE,eff,i}
```

```math
\dot{Q}_{ret,aHX} = n_{parallel} \sum_{i=1}^{n_{series}} \dot{Q}_{h,TE,eff,i}
```

The electrical power consumption is:

```math
\dot{P}_{aHX} = n_{parallel} \sum_{i=1}^{n_{series}} U_{TE,eff,i} \cdot I_{TE}
```

## Performance Indicators

Coefficient of Performance (COP):

```math
COP = \frac{\dot{Q}_{ret,aHX}}{\dot{P}_{aHX}}
```

Energy Efficiency Ratio (EER):

```math
EER = \frac{\dot{Q}_{sup,aHX}}{\dot{P}_{aHX}}
```

## Condensation Estimation

```math
\dot{m}_{cond} = \max(0, \dot{m}_{fr}(\omega_{fr} - \omega_{sup}))
```

---

# Type 230 – Thermoelectric Ventilation Controller

Type 230 represents the supervisory controller responsible for regulating operational modes, fan control, bypass operation, and PID-based thermoelectric current modulation.

## Operational Modes

### Mode 1 — Ventilation Mode

Both active and passive heat exchangers operate simultaneously to maintain the desired supply air temperature.

### Mode 2 — Discomfort Mitigation

The controller activates thermoelectric conditioning outside the defined thermal comfort range.

### Mode 3 — Overheating Mitigation

Cooling operation is prioritised when indoor temperatures exceed the upper comfort boundary.

### Mode 4 — Underheating Mitigation

Heating operation is prioritised when indoor temperatures fall below the lower comfort boundary.

### Mode 0 — Passive Heat Recovery Only

Only passive heat recovery is active without thermoelectric conditioning.

## PID-Based Current Control

The electric current supplied to the thermoelectric modules is determined as:

```math
I_{TE} = PID_{signal} \cdot I_{rated}
```

## Heating and Cooling Mode Selection

### Cooling Mode Activation

Cooling mode is activated when:

```math
T_{HR,out} > T_{set} + \Delta T_{deadband}
```

### Heating Mode Activation

Heating mode is activated when:

```math
T_{set} > T_{HR,out}
```

## Bypass Control

```math
Bypass =
\begin{cases}
1 & T_{fr} \in [T_{lower}, T_{upper}] \\
0 & otherwise
\end{cases}
```

## Fan Control Modes

### Direct Fan Control

In direct fan control mode (`Mode_Fans = 1`), the fan signal follows the predefined schedule:

```math
Fan_{signal} = Schedule_{Fans}
```

### Occupancy-Based Fan Control

```math
Fan_{signal} =
\min\left(
\frac{Schedule_{occupancy} \cdot 25 \cdot 1.2}
{Rated\ Unit\ Flowrate},
1
\right)
```

---

# TRNSYS Simulation Implementation

The thermoelectric ventilation unit library is intended to be used as a modular extension of the TRNSYS 18 simulation environment.

The recommended implementation consists of:

- Type221 — Thermoelectric ventilation unit model  
- Type230 — Supervisory controller  
- Type23 — Standard PID controller  

**Figure Placeholder:** Example implementation of the thermoelectric ventilation unit library in the TRNSYS Simulation Studio environment.

The TRNSYS–CONTAM coupling may be implemented using the NIST Type98 interface enabling airflow and contaminant transport co-simulation.

This extension is particularly suitable for CO₂-based demand-controlled ventilation applications.

## References

### NIST CONTAM Documentation

https://www.nist.gov/el/beed/nist-multizone-modeling/software/contam/contam-documentation

### NIST CONTAM Software Tools

https://www.nist.gov/el/energy-and-environment-division-73200/nist-multizone-modeling/software-tools

### NIST Type98 Documentation

https://www.nist.gov/el/beed/nist-multizone-modeling/software/type98