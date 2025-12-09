## A Very Low Dropout Regulator Based on G4COL / ND6T

Implemented for use with the [QMX+ HF transceiver](https://qrp-labs.com/qmxp.html).  
PCB designed in KiCAD v9.


References:  
https://www.gqrp.com/limiter.jpg (
via https://groups.io/g/QRPLabs/topic/qmx_battery/111670594)  
http://www.nd6t.com/qrp/VLDO.htm

PCB, top  
<img src="images/VLDO_PCB_overview_top.jpg" alt="photo, top" height="50%" >  

PCB, bottom  
<img src="images/VLDO_PCB_overview_bottom.jpg" alt="photo, bottom" height="50%" >  

Schematic  
<img src="VLDO_schematic.png" alt="VLDO schematic" height="50%" >  

PCB layout  
<img src="VLDO_PCB.png" alt="VLDO PCB" height="50%" >  

LTSpice schematic for static simulation  
<img src="design/VLDO_LTSpice_schematic.png" alt="LTSpice (static) simulation" height="50%" >  

### Output Voltage with a 5V LDO 
The schematic uses a 5V LDO as the reference voltage.  The output voltage is given by: 
```  
Vldo = Vout * (R4 / (R3 + R4))
Vout = Vldo / (R4 / (R3 + R4))
12.5V = 5V / ( 20k / (30k + 20k))
11.8V = 5V / ( 22k / (30k + 22k))
8.85V = 5V / (39k / (30k + 39k))
```  
In practice, depending on the quality of transistors used, you may need to tweak the value of R3 or R4.

### Parts Tested in Prototype  
The PCB has alternate footprints for the active components so that alternative parts may be tested, or that the PCB may be built mostly surface-mount or through-hole.  

| RefDes | Part | Source | Comments           |   
| ------ | ----------- | -------------------- | ------------------------------------ |  
| M1 | IRF4905S | https://www.aliexpress.us/item/3256806571411100.html | (TO-263) Works, but runs hotter than expected; Rds_on likely much higher than spec
| M1 | IRF4905 | https://www.aliexpress.us/item/3256806880410046.html | (TO-220 alternate) Part from this supplier does not work well  
| U3 | ME6203A50M3G  | https://www.aliexpress.us/item/3256806655687105.html | (SOT23-3) Works well  
| Q3/Q4 | MMBT2222A  | https://www.aliexpress.us/item/3256806768261444.html | (SOT23-3) Works well  


### Power In/Out Connectors  
The prototype used Anderson PowerPole [PP15/30/45 housings](https://powerwerx.com/anderson-powerpole-colored-housings) with [PP45 crimp pins](https://powerwerx.com/anderson-261g2-powerpole-contact-pp45) as these were on hand -- not the [special PCB-mount pins](https://powerwerx.com/printed-circuit-board-pcb-contact-25amp).  
The PCB is designed for 14 AWG solid wire. To fit 14 AWG solid wire into a PP45 pin (which is designed for 10 AWG wire), it is folded over at the pin end, then soldered into the PP45 pin.  PP30 pins are deisgned for 12-14 AWG wire and should solder in directly.

14 AWG Wire folded over for PowerPole crimp pin  
<img src="images/PP15_with_14AWG_folded_wire.jpg" alt="14 AWG Wire folded over for PowerPole crimp pin" height="25%" >  

14 AWG Wire soldered into PowerPole crimp pin  
<img src="images/PP15_with_14AWG_wire.jpg" alt="14 AWG Wire soldered into PowerPole crimp pin" height="50%" >  

14 AWG Wire soldered into PCB (be sure to cut excess lead length off)  
<img src="images/PP15_with_14AWG_wire_soldered_to_PCB.jpg" alt="14 AWG Wire soldered into PCB" height="50%" >  

### Thermal Test  
Although the [QMX+](https://qrp-labs.com/qmxp.html) is specified to consume slightly less than 1 amp at 12 volts, the VLDO prototype was tested with a [WEL-3005 30W constant-current load tester](https://www.hackster.io/john-bradnam/30w-constant-current-load-65ecdd) at 2.5A.  
When using parts from AliExpress or of unknown pedigree, it is recommended that VLDO operating conditions do not exceed those normally expected for the QMX+; i.e., 1A@2 minutes (TX) / 0.1A@2 minutes (RX).   

Vin=13.24, Vout=12V (nominal)  
Iout=2.5A  
Q3/Q4=MMBT2222A  
U3=ME6203-5.0  
M1=IRF4905S (TO-263), __no heatsink__   

Temperature measured with a Cen-Tech 69465 infrared pyrometer aimed at M1's black marked surface, dist= 0.75", or at the bottom of the PCB directly under M1 at a distance of 0.75".  

| Time, seconds | T (deg C), M1 top | T (deg C) PCB bottom |
| ------------- | ----------------- | --------------- |  
| 0 | 22 | 20.6 |  
| 30 | 41.8 | 37.8 |  
| 60 | 52.8 | 47.4 |  
| 90 | 59.1 | 53.4 |  
| 120 | 62.5 | 56.8 |  
| 180 | 62.6 | 58.8 |  
| 240 | 64.4 | --- |  
| 300 | 65.4 | 60.3 |  
| 360 | 66.6 | 62.6 |  

### Rds(on) Measurement  
Rds(on) Measurement  
The IRF4905S's "ON" resistance, Rds(on), was measured with a 
[WEL-3005](https://www.hackster.io/john-bradnam/30w-constant-current-load-65ecdd) load tester 
and a UT161E multimeter.  For each constant-current test of Id below, current was only turned 
on for 3-4 seconds-- long enough to measure one parameter, but short enough to avoid significantly
heating the device. 

Device: IRF4905S from [this AliExpress vendor](https://www.aliexpress.us/item/3256806571411100.html).  
Rds(on) = Vds / Id  

| Vin, V |  Id, A | Vds, V | Vout, V | Rds(on)†, Ω |  
| --- | --- | --- | ---- | --- |  
| 13.33 | 0.00 | 1.13 | 12.16 | --- |  
| 13.28 | 0.50 | 1.12 | 12.13 | 2.24 |  
| 13.29 | 1.00 | 1.10 | 12.13 | 1.10 |  
| 13.20 | 1.50 | 1.06 | 12.12 | 0.71 |  
| 13.16 | 2.00 | 1.01 | 12.12 | 0.51 |  
| 13.11 | 2.50 | 0.96 | 12.11 | 0.38 |  

†Rds(on) is calculated; other table values are measured.

At 2 amps, the LTSpice [simulation](design/vldo_nd6t1.asc) reports an Rds(on) value of ~21.6mΩ.  The OEM's IRF4905S [datasheet](datasheets/MOSFET_P-Channel-irf4905s-datasheet-en_Infineon.pdf) says it should be ~20mΩ @Vgs = -10V, Id = -42A.
<img src="design/VLDO_LTSpice_simulation.png" alt="LTSpice simulation" height="50%" >  


