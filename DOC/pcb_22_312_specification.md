
 <!-- ------------------------Document header ------------------------------------>
 ![logo](./IMAGES/logo.png)
 ___

 <br/><br/>
 <br/><br/>

<!-- ------------------------Document TITLE PAGE --------------------------------->
 
<center><font size =6">Gantry Hardware Specification</font></center>
<center><font size =5">Tilt Service Board Specificaiton</font></center>
<center><font size =5">PCB/22-312</font></center>
<center><font size =4">REV : 1.0</font></center>


 <br/><br/>
 <br/><br/>
 <br/><br/>
 <br/><br/>

<center>
<font size =4">

|Date |Revision | Change log| Author|
|:---| :----: | :----: |:---|
|04/01/2023|1.0|First release| M. Rispoli|

</font>
</center>

<div style="page-break-after: always;"></div>


# 1. TABLE OF CONTENTS   

- [1. TABLE OF CONTENTS](#1-table-of-contents)
- [2. SCOPE](#2-scope)
- [3. Device General Requirements](#3-device-general-requirements)
- [4. Board power supply](#4-board-power-supply)
- [5. Exposure Window Interface](#5-exposure-window-interface)
- [6. CAN BUS Communication](#6-can-bus-communication)
- [7. Motor Zero Setting interface](#7-motor-zero-setting-interface)
- [8. Rotation Brake Interface](#8-rotation-brake-interface)
- [9. Rotation Safety Monitoring](#9-rotation-safety-monitoring)
- [10. PCB and Mechanical Layout requirement](#10-pcb-and-mechanical-layout-requirement)


<div style="page-break-after: always;"></div>


# 2. SCOPE

This document provides the PCB-22-312 detailed hardware specifications.


<div style="page-break-after: always;"></div>

# 3. Device General Requirements

This board shall implement the following functions:
+ Tilt motor zero setting sensor interface;
+ Exposure Window signal interface;
+ Rotation Brake interface;
+ Rotation Safety circuit;

# 4. Board power supply

+ The board shall be powered with the system 24V DC power supply;
+ The power supply input shall be protected with a PPTC rated 750mA;

# 5. Exposure Window Interface

In order to implement the synchronization between the rotation and the Tomo scan pulses,
the motor driver shall receive the Exposure Window input signal coming from
the Detector interface.

The Exposure Window signal is provided to this board through the input connector from the PCB-22-305 board. The input signal levels are:
+ 0V: low level;
+ 12V: high level
  
The board shall route this signal from the input connector to the DIGITAL INPUT 4 of the GPIO Motor Driver connector.

# 6. CAN BUS Communication 

The Motor Driver shall be connected with the System Can Bus and related bus Power Supply.

The Board shall route the Can signals from the input connector from the PCB/22-305 board 
to the CAN Motor driver dedicated connector.

# 7. Motor Zero Setting interface

The Motor zero setting is made with a dedicated photocell that 
shall detect a mechanical discontinuity in a mechanical plate.

+ The Mechanical blade shall be configured so that the photocell during its travel can be always ON in one side of the Zero position, and always OFF in the other side.
+ The photocell shall be properly conditioned in order to guarantee the repeatibility at least of +- 0.3 degree;
+ The photocell signal shall be conditioned to be +24V ON and 0V OFF;
+ The photocell signall shall be routed to the DIGITAL INPUT 1 of the Motor Driver GPIO connector;

# 8. Rotation Brake Interface

The Tilt motor needs a Normally closed EM brake in order to keep a stable position.
During the motor activation, the brake device needs to be powered (activated) in order to be released.

+ The Board shall implement a solid state switch capable to control a 500mA @ 24VDC output brake power supply;
+ The Board shall handle the Motor Driver Digital Output-1 so that it can activate the brake at the occurence;
+ The Brake shall be deactivated in the case the cable from the motor should be disconnected;
+ The current brake status shall be read back to the DIGITAL INPUT 2 of the Motor Driver GPIO connector;
  
Other requirements shall be meet related to the rotation safety monitoring (see further section).

# 9. Rotation Safety Monitoring

The Tilt rotation system needs a dedicated circuit in order to detect a 
possible fault condition that may cause a malfunction in the motor rotation 
or a mechanical fault that may cause a free rotation drop of the Tube.

The circuit shall be an independent electronic circuit not influenced by  
the motor status. The scope of the circuit is to detect a rotation speed 
exceeding the maximum working speed of 8°/s with enough margine to prevent false positive alarms.

The circuit shall make use of photocells that shall detect the rotation speed, reading the frequence of a light/dark pattern.

+ The circuit shall detect a fault rotation speed when the speed should exceed no more than 20°/s;
+ The circuit shall detect the faulty speed in no more of 4° of rotation;
+ In case of fault condition detected, the circuit shall activate a non resettable alarm;
  + The alarm condition can be reset only switching off the board;
+ In case of fault detection, the brake device shall be disabled:
  + The Motor disable status shall not be overriden by other signals in the brake control circuit;


# 10. PCB and Mechanical Layout requirement

+ The Zero setting pattern and the Rotation Speed pattern shall be implemented in the same plate mounted on the moving axes;
+ The Photocells shall be soldered to the board so that the board can be mounted parallel with the mechanical plate;
+ The signal wires to the Motor Connectors should exit from the PCB-22-312 board  with one only dedicated connector ;

