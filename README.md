# PicoBOB-DLX for GRBLHAL and LinuxCNC

![Logo](/readme_images/logo_sm.jpg)

Expatria Technologies PicoBOB-DLX GRBLHAL/LinuxCNC module

<img src="/readme_images/Pico_Boardpng.png" width="600">

Currently available in our online store:

[https://expatria.myshopify.com/products/picobob-dlx](https://expatria.myshopify.com/products/picobob-dlx)

Please consider buying a board to support our open-source designs. 

An extended version of the original PicoBOB, the PicoBOB-DLX allows you to use either the MCU/Sender based GRBLHAL motion control system, or else it can act as a hardware step generator when connected to a LinuxCNC system over Ethernet.  It is intended to be simple and low cost.  It uses a Raspberry Pi RP2040 MCU and the widely available 5 axis Mach3 breakout board.  It can also be used with the Gecko G540 as well as other devices that use a DB25 parallel port interface.  The complete BOM and fabrications files are in the CAM_OUTPUTS folder for upload.  The design is free to use by all parties, including commercial parties, under the CERN-OHL-S V2 license.  It is our hope that the community finds the design useful and that it may be carried forward to help advance the PrintNC and broader CNC hobby community.

The PicoBOB-DLX closely tracks the features of the Mach3/LinuxCNC BOB:

1) Up to 5 axes of step/direction control.
3) 10A Spindle relay control.
4) 0-10V analog spindle speed control.
5) 5 general purpose inputs
6) One general purpose output.

In addition, it has headers for the Expatria Jog2K real-time control pendant: 

https://github.com/Expatria-Technologies/RT_Jog_Controller

The DLX can be connected to a GRBLHAL host via Ethernet or USBC.  For LinuxCNC, the Ethernet connection is used with the Remora etherent component and the DLX acts as a hardware step generator to mitigate latency demands.

The current work-in-progress version of the Remora RP2040-W5500 firmware is posted here:  
https://github.com/Expatria-Technologies/Remora-RP2040-W5500

The most current version of the GRBLHAL RP2040 driver is posted here:  
https://github.com/Expatria-Technologies/RP2040

The default GRBLHAL builds for the PicoBOB include the following features that are implemented in the RP2040 port of GRBLHAL:

1) Backlash Compensation.
2) Stress-free Autosquaring for the ganged axis
3) Ganged axis offsets to correct for offset homing switches
4) Step rates tested up to 180 KHz on 5 simultaneous axes.

In addition, the board has a USB micro connector for the 5V that is required for the BOB.  There are also pushbuttons for BOOT and RUN for easy programming using the RP2040's built-in UF2 bootloader.

## PicoBOB Overview

<img src="/readme_images/Board_Overview.jpg" width="300">

Usage notes:

Communication with GRBLHAL is accomplished via the USBC or Ethernet connections on the PicoBOB (not connected in above images).  A short USB cable is used to provide the required 5V for the BOB.  In addition, the BOB requires an external 12-24V supply.

The Mach3 BOB shares the B axis direction signal with a spindle relay enable signal - only one can be used at a time.  Also on the PicoBOB, the stepper enable signal is modified in the GRBLHAL default map file so that it is used as the coolant output signal.  All of this is configurable by re-building the GRBLHAL firwmare.

<img src="/readme_images/B1boardpic.png" width="500">

Above shows the PicoBOB B1 connected to the Mach3/LinuxCNC parallel BOB.

## Gecko Drive G540 support Overview
By using a common "mini gender changer" the PicoBOB DLX can be adapted for use with a G540 4-AXIS Digital Step Drive.

<img src="/readme_images/Gecko_Adapter.png" width="300">
<img src="/readme_images/IMG_20230304_1730163.jpg" width="500">

The updated GRBLHAL map file has the following pinout:

| DB25 Pin | G540 Function   | GRBLHAL Function | RP2040 GPIO |
|----------|-----------------|------------------|-------------|
| Pin 1    | OUTPUT 2        | SPINDLE ENABLE   | GPIO14      |
| Pin 2    | X STEP          | X STEP           | GPIO17      |
| Pin 3    | X DIR           | X DIR            | GPIO9       |
| Pin 4    | Y SETP          | Y SETP           | GPIO18      |
| Pin 5    | Y DIR           | Y DIR            | GPIO10      |
| Pin 6    | Z STEP          | Z STEP           | GPIO19      |
| Pin 7    | Z DIR           | Z DIR            | GPIO11      |
| Pin 8    | A STEP          | A/M3 STEP        | GPIO20      |
| Pin 9    | A DIR           | A/M3 DIR         | GPIO12      |
| Pin 10   | INPUT 1         | LIMIT XYZ        | GPIO3       |
| Pin 11   | INPUT 2         | PROBE            | GPIO4       |
| Pin 12   | INPUT 3         | LIMIT A/M3       | GPIO2       |
| Pin 13   | INPUT 4         | HOLD             | GPIO1       |
| Pin 14   | VFD PWM         | SPINDLE PWM      | GPIO16      |
| Pin 15   | FAULT (INPUT 5) | HALT             | GPIO5       |
| Pin 16   | CHARGE PUMP     | N/A              | GPIO21      |
| Pin 17   | OUTPUT 1        | COOLANT          | GPIO13      |
| Pin 18-25| GND             |                  |             |
