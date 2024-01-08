
 <!-- ------------------------Document header ------------------------------------>
 ![logo](./IMAGES/logo.png)
 ___

 <br/><br/>
 <br/><br/>

<!-- ------------------------Document TITLE PAGE --------------------------------->
 
<center><font size =6">Gantry Hardware Specification</font></center>
<center><font size =5">Tilt Service Board Implementation</font></center>
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
- [3. Board power supply](#3-board-power-supply)
- [4. Exposure Window Interface](#4-exposure-window-interface)
- [5. CAN BUS Communication](#5-can-bus-communication)
- [6. Motor Zero Setting interface](#6-motor-zero-setting-interface)
- [7. Rotation Brake Interface](#7-rotation-brake-interface)
- [8. Rotation Safety Monitoring](#8-rotation-safety-monitoring)


<div style="page-break-after: always;"></div>


# 2. SCOPE

This document provides technical notes about the board inplementation.


<div style="page-break-after: always;"></div>

# 3. Board power supply

+ The 24 V power supply has been protected with a resttable fuse rated 750mA;
+ The internal components need the 5V power supply: it has been provided with a linear regulator 
  rated 30VDC in input and 500mA output. Because of the current in the 5V line is very low, 
  the power dissipated by the regulator is below of 100mW with a minimum increment in case temperature of about 3°.   The regulator has been mounted on a 25mmx25mm square Copper area to guarantee the 30°/W of the case thermal resistance.

  
# 4. Exposure Window Interface

The exposure window signal has been properly conditioned in the PCB-22-305 board so 
it can be routed directly to the Motor output connector without other modifications.


# 5. CAN BUS Communication 

The can bus signals coming from the PCB-22-305 input connector has been directly routed to the Motor output connector without other modifications.


# 6. Motor Zero Setting interface

The Photocell input diode has been powered with 15mA DC current to guarantee the correct 
working mode. It should provide the 1mA collector current when in ON condition.

The output signal has been taken by the emitter through a 3K3 ohm that should prevent the photocell to saturate and providing enough signal level to activated a comparator circuit with histeresis 
which threshold has been fixed to 1.22V.

The signal output to the motor digital input has been controlled by a Mosfet that 
sets the output level to +24V (active) required by the motor interface.  


# 7. Rotation Brake Interface

The rotation brake circuit is implemented with a Low Side Power Mosfet,
rated for the required current of 500mA, mounted on a 25x25mm suqare copper area 
to guarantee the correct dissipation of about 20mW;

The Gate of the Mosfet is shorted to ground in case the rotation alarm signal should be activated;
The gate is kept grounded in the case the controlling input coming from the motor should be 
disconnected: the activation therefore is made with an input low level.
  > NOTE: the Motor output is an open collector circuit.

The current Mosfet activation status is monitored reading the status of the Drain voltage
and routing it to the Motor Digital input as required. 


# 8. Rotation Safety Monitoring

The principle of the rotation speed monitor is following described:
+ A light pattern is realized in a round plate of 10cm of radius, with a periodic light spots pattern with 3mm separation;

Two photocells are mounted interlaced with a distance equal to k*3mm + 1.5mm;
With this pattern, during the rotation the photocell produce two separated pulses which time distance is:
  + Tr = (1.5) / (w \* R);
  + w = angular rotation in radiant;
  + R = radius in mm;
  + 1.5 is the distance of the photocells;

Every photocell activates a monostable circuit providing a pulse with a predefined time width (Tp).
When the Tp >= Tr the signals of the two photocells overlap causing the alarm activation;
The alarm activation is then passed through a flip flop to guarantee that it cannot be further reset.

The circuit has been tuned with a Tp equal to 67.2ms, equivalent to a rotation speed limit of 12.7 °/s.
This value should guarantee enough tollerance respect of the 8°/s of the working speed (50% margine).

The 3mm of pattern spacing corresponds to a detection angle (in the worst condition at the beginning of the motion) of:
+ s\*180/(R\*pi-greco) = 1.71°.

Because the speed cumulated by the ARM in the case of mechanical failure depends by the initial angle, the circuit react differently as following described:
- W(dA) = RADQ(2 \* g \* R \* (sin(Ai) - sin(Ai-dA)));
  where:
  W(dA): is the cumulated speed depending by the variation angle;
  g: is the gravity (9800 mm/s2);
  R: is the SID 700mm;
  Ai: is the initial angle (90° is the tube in CC);
  dA: is the angle variation due to the free motion;

  With this circuit setup (speed limit = 12.7°/s) and this mechanical setup (3mm spot spacing and spot at 100m from the rotation point):
  - If the initial ARM is > 87.5° (CC) the minimum detection angle is < 3.43° and the cumulated speed is 12.8°/s;
  - For lower angles the minimum detection angle is 1.71° but the cumulated speed augment with the maximum value at 52°/s (C-ARM at 0°);
  


