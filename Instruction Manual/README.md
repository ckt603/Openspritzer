﻿<p align="right"><img src="https://github.com/BadenLab/Zebrafish-visual-space-model/blob/master/Images/Logo.png" width="300"/>
<h1 align="center">Instruction Manual</h1></p>
<h1 align="center">OpenSpritzer v1.3</h1>

***

## Overview

<img align="right" width="650" height="500" src="https://github.com/BadenLab/Openspritzer/blob/master/Images/components.png">

This document contains detailed assembly instructions, a software guideline and includes a parts list.

 The Arduino code and 3D printing files (SCAD and STL) can be downloaded [here](https://github.com/BadenLab/Openspritzer/tree/master/3D%20Designs/v1.3%20-%20Customed%20PCB), and further modified to fit customise purposes. The purpose of the device is to regulate the pressure and duration of a puff of compressed air. Typically, the output port is connected to a glass puffer pipette which has been drawn into a sharp point with a narrow (2-3μm) diameter pore.

The device consists of a circuit board, a solenoid valve, a pressure regulator with a gauge and various interface, off-the-shelf, components.

***

- [Assembling OpenSpritzer](#Assembly)
- [Programming the Stimulator](#Programming-the-ESP32)
- [Operating the Stimulator](#Operating-the-Stimulator)
- [Calibrating the Stimulator](#Calibrating-the-Stimulator)

***

## Assembly

<img align="right" width="350" height="225" src="https://github.com/BadenLab/Openspritzer/blob/master/Images/Schematics2.png">

<p align="center"><h4 align="left">1 – Obtaining the custom-designed PCB</h4></p>

From the GitHub repository, one can find the [gerber.zip folder]https://github.com/BadenLab/Openspritzer/tree/master/PCB) needed to order the PCB to any manufacturer company (i.e. https://jlcpcb.com).

Ordering a minimum of 5 units should not cost more than £10. Gerber files were designed with [KiCad 5.0](http://kicad-pcb.org/).


Schematics and PCB footprint can be downloaded and modified from the same repository if need be.


****

<p align="center"><h4 align="left">2 – Soldering the custom-designed PCB</h4></p>
<img align="left" width="375" height="225" src="https://github.com/MaxZimmer/Multi-Chromatic-Stimulator/blob/master/Images/PCB01.png">

The board is self-explanatory. On the left, two options are available, one for the Arduino (close rows) the other for the ESP (spread rows). There is no need to solder more JST pins that the number of LED required for the desired stimulator. The two resistors at the bottom are a voltage divider for the ESP32 only (There’s no need to solder any resistor here if the Arduino option is chosen).

ESP32 unlike Arduino Nano, works on a 3.3V logic; no higher tension should be sent to this board. Since most TTL deliver 5V pulses, we selected a 220/470Ω divider to bring a 5V blanking signal into a 3.3V input. Depending on the blanking signal generator used, this divider can be modified to fit one’s personal design or bypassed by only bridging the 220Ω resistor.

(Supplementary Figure S3 TESSA PICTURE)

<img align="right" width="500" height="200" src="https://github.com/MaxZimmer/Multi-Chromatic-Stimulator/blob/master/Images/reference%20resistor%20vs%20output%20current.png">

The Adafruit TLC5947 LED driver is a constant current driver configured by default to set the current level at 15mA per channel, which is virtually safe for any LED. However, one can operate at different current by replacing the on-board reference resistor with a through hole resistor. The driver is capable to deliver up to 30mA, the graph below shows the relationship between resistance and output current.



****

<p align="center"><h4 align="left">3 – Mounting the potentiometer</h4></p>
<img align="right" width="300" height="192" src="https://github.com/MaxZimmer/Multi-Chromatic-Stimulator/blob/master/Images/Potentiometer%20PCB.png">

In order to finely adjust each LED power, we added multiple-turn trimmer potentiometers to our design. A simple solution is to manufacture the appropriate PCB board (We provide multiple options on the [GitHub repository](https://github.com/MaxZimmer/Multi-Chromatic-Stimulator/tree/master/PCB/Potentiometer%20Mounts)).

Otherwise, one can make its own little PCB by using a solderable board.
Each potentiometer connects its ClockWise (pin 3) to the LED (+) stimulator JST pin; and its Base (pin 2) to the LED (+) leg. The LED (-) stimulator JST pin connects the LED (-) leg directly.

***

<p align="center"><h4 align="left">4 – Printing the Stimulator box</h4></p>
<img align="left" width="300" height="250" src="https://github.com/MaxZimmer/Multi-Chromatic-Stimulator/blob/master/Images/Box.png">

[STL files](https://github.com/MaxZimmer/Multi-Chromatic-Stimulator/tree/master/3D%20Designs/Stimulator/STL%20Files) can be found on the GitHub repository and print directly if the user wishes to go for the default design (4 stimulation LEDs + 4 proxy LEDs) and [BoM](https://github.com/MaxZimmer/Multi-Chromatic-Stimulator/blob/master/Bills%20of%20Materials/BOM%20-%20Stimulator.ods).

However, [SCAD files](https://github.com/MaxZimmer/Multi-Chromatic-Stimulator/blob/master/3D%20Designs/Optical%20Components/Optical%20Components.scad) are also available and easily adjustable for personalised design.

We used [OpenSCAD](www.openscad.com) to design the stimulator box. The tolerance of the printer can be adjusted in the “USER Parameters” section of the script (tol =0.1; by default, this value is used for Prusa MK3 and Ultimaker 2). Each component can be displayed/design individually in the “Switches” section. Variables such as LED number (4 by default) and the potentiometer board dimensions can be adjusted in the “Component Parameters” section.

The PCB is screwed to the “Bottom” part of the box by using M3 screws and nuts. The potentiometer board adjusts itself with the “Back” part of the box, and the trimmers should adapt to their respective holes exactly.
All part should fit tightly together and are maintained together by 4 M3*50mm socket screws.


***

<p align="center"><h4 align="left">5 – Adjusting the proxy LED</h4></p>
The proxy LED are markers for the experimenter to have a ready visualisation of the stimulus being displayed under the objective. If this option is selected, 3mm LED are to be mounted at the back of the LED holder using 3mm LED mount.

(Tessa picture)

Finally, any translucid material could be placed in the LED holder slot in order to diffuse the proxy LED light (In our example, we used a piece of thin Teflon)

(TESSA PICTURE OF THE LED HOLDER Supplementary Figure S7)

***

<p align="center"><h4 align="left">6 – Connecting the Stimulator</h4></p>
The stimulator can be powered from 5 to 30v. The Adafruit TLC5947 being a constant current LED driver, the voltage selection is not critical, however it has to be slightly higher than the LED forward voltage (LED datasheet). Multiple LEDs can be connected to the same channel; however, the voltage supply has to be adjusted in consequence. For example, if an LED has 3.2v forward voltage, then a power supply => 3.2v will be enough, however if 5 of those LEDs are connected in series to the same channel, then the power supply has to be => 3.2v * 5 => 16v.

Blanking signal BNC has to be connected to a TTL (pulse generator), that delivers 5V pulses (if voltage is different, refer yourself to the soldering paragraph). Here we called blanking signal the pulse synchronised with the scanning mirror(scanning retrace), the laser pocket cell and the PMTs. On our setup, when blanking signal is HIGH, the scanning mirror starts a new scanning line, the pocket cell let the laser beam through, the PMT records the emitting fluorescence and the stimulating LEDs are turned off. When the blanking signal is LOW, the opposite occurs.

The trigger channel is an output signal generated by the stimulator that by default sends a 3.3v pulse (if ESP32 is used, 5v for Arduino nano) at the start of the stimulus and then at every 1.00000s. This trigger is used during the analysis to correlate the displayed stimulus to the fluorescence trace recordings.

Finally, the board is connected to a computer via micro USB cable (for ESP32, mini USB for Arduino Nano), and ready to be used.

(Final picture)

(Experiment Picture)

***

## Programming the ESP32


1-	Download and install [Arduino environment](www.arduino.org) on the computer.

2-	To use the ESP32, a couple of steps are necessary
Install the latest SiLabs [CP2104 driver](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers).
Follow the installation instructions from the [Espressif repository](https://github.com/espressif/arduino-esp32/blob/master/docs/arduino-ide/boards_manager.md).

3-	Install the TLC5947 library:	Start Arduino and from the “Sketch” tab, select “Include Library” and open “ Manage Libraries”.	From the search bar enter “TLC5947”. Select and Install the library

4-	Close Arduino and open the Multi-Chromatic-Stimulator arduino file from the [Arduino Code folder](https://github.com/MaxZimmer/Multi-Chromatic-Stimulator/tree/master/Arduino%20code/2-Photon_Mutli_Chromatic_Stimulator).


5-	From the “Tools” tab: Select from “Boards” the “Adafruit ESP32 Feather”.	From “Upload Speed”, select 921600.	From “Flash Frequency”, select 80Hz. From “Port”, select the computer port to which the ESP is connected (if doubt, unplug, replug and observe the choices differences). If the ESP is not recognised, check the driver installation (2), then check the micro USB cable.

6-	Compile and Upload the code by clicking on the arrow button on the top left.

7-	The stimulator is ready to be used.

***

## Operating the Stimulator
<p align="center"><h4 align="left">The code is organised in five distinct part:</h4></p>

  Stimulus Parameters

Here, the user determines the sequence of his looping stimulus.

–	The array_LED# corresponds to the sequence of each LED#. The value corresponds to the LED power and CAN ONLY TAKE A VALUE FROM 0 TO 100, where 0 corresponds to no light and 100 to max light intensity

–	The array_Time corresponds to the duration of each entry in ms; the first entry being the pre-adaptation that will only be played at the start of the stimulus, the sequence will then loop starting at the second position.

–	The number of Loops is determined by nLoops. The stimulus will stop after finishing the n Loop.

–	IMPORTANT, for the code to work flawlessly, the number of entries within the arrays has to be the same and manually entered in nArrayEntries (including the pre-adaptation at position 1).

***

  Microcontroller Board Selection

This is where the user defines if he is using an ESP32 or an Arduino Nano

***

  Internal Definitions

This is the main definition part of the code which can be modified to:

-	Add more LEDs than the 4 main and 4 proxy defined by default. (Global variables, the LED pins correspond to the pin number on the TLC5947).

-	Adjust the trigger duration (25ms by default).

-	Adjust the Trigger interval (1000 ms by default)

***

  Internal Methods

This is the main core of the code and should not be structurally changed apart from adding more LEDs (straight forward)

***

  Main Loop

This is where the stimuli controls are defined. By default, when compile window is open (magnifying glass on the top right corner) and the baud rate at the bottom right of the window has be changed to 115200, a manual command will trigger a stimulus:

By default:

-	'a' –> Play stimulus at max power

-	'b' –> Play stimulus at min power

-	'0' –> Turn off all LEDs stop any stimulus being played and reset the loop

-	'+' –> Turn on all LED to max power

-	'-' –> Turn on all LED to min power

-	'1' –> Turn First LED to max power

-	'2' –> Turn First LED to min power

-	'3' –> Turn Second LED to min power

-	'4' –> Turn Second LED to min power

-	'5' –> Turn Third LED to min power

-	'6' –> Turn Third LED to min power

-	'7' –> Turn Fourth LED to min power

-	'8' –> Turn Fourth LED to min power

Important to note, obviously the stimulation will only be played if a blanking signal is sent to the board.

***

## Calibrating the Stimulator

The TLC5947 is 12 bits PWM grayscale driver, meaning that it offers a 4096 degree of freedom to adjust each LED power.
On the arduino code there is a second tab called “LED_values” which hard-codes the maximum power an LED can get. Those values range from 0 (no current) to 4095 (max current, 15mA by default).

On the default script we proposed 2 distinct max values (oddly named here max and min power) that can be called individually. The purpose here is to have the opportunity to use the same stimulus sequence at two different light intensities. Off course more can be added manually by the user.

For the calibration, we suggest setting the max_LED# value at 4095 (Full power) and use successively a spectrometer and a powermeter. To adjust the LED brightness, the user should first use the trimmer potentiometer at the back of the stimulator then finely tune the desired max power in the code. When the LED value (0-100) will be entered in the stimulus sequence, it will automatically be mapped to 0-max_LED#

***

This project is licensed under the [GNU General Public License v3.0](https://github.com/MaxZimmer/Multi-Chromatic-Stimulator/blob/master/LICENSE)

***