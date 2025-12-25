Overview

This project implements a PLC-based control system for an automated warehouse retrieval process. The system retrieves totes from storage bays and delivers them to an output conveyor using a rail-guided cart, following industrial automation principles.

The control logic is developed using Structured Text (ST) in CODESYS 3.5, based on IEC 61131-3 standards and state-machine design.

System Description

Two storage bays (A and B), each with:

Main conveyor and output conveyor

Proximity switch for tote detection

Tower light and acknowledgment pushbutton for empty-bay handling

A motor-driven cart with:

Direction-controlled motor

Optical sensor for incremental position tracking

Roller conveyor and proximity switch

Continuous output conveyor at the end of the track

Communication with a Warehouse Management System (WMS) via digital and numeric I/O

Key Features

State-machine–based PLC control for deterministic and safe operation

Incremental position tracking using rising-edge counting from an optical sensor

Automatic empty-bay detection with timeout monitoring and operator acknowledgment

SKU-based tote retrieval logic with validation and rejection of invalid requests

Synchronized conveyor and cart control to prevent unsafe material transfers

Ready / Invalid signaling for reliable WMS communication

Technologies Used

PLC Programming: Structured Text (ST)

Standard: IEC 61131-3

Environment: CODESYS 3.5.18.20

Control Concepts:

State machines

Edge detection

Timers

Sensor–actuator integration

Program Structure

Main program handles:

WMS request processing

Cart movement and positioning

Storage bay control implemented using reusable logic:

Conveyor coordination

Empty detection and recovery

All logic executed in a freewheeling PLC task with defined priority

Simulation & Testing

Tested using the provided CODESYS simulation environment and HMI

Verified scenarios include:

Valid and invalid SKU requests

Empty storage bays

Correct cart positioning and transfer sequences

Safety checks prevent:

Tote transfer at incorrect cart positions

Cart movement beyond track limits

Learning Outcomes

Practical experience with industrial PLC programming

Application of state-machine control in automation systems

Understanding of warehouse automation and material handling logic

Improved skills in debugging and validating PLC control software
