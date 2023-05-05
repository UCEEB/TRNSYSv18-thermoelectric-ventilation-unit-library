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
This project has received funding from  the  European  Union’s Horizon 2020     research     and innovation    programme    under grant agreement No 958218
