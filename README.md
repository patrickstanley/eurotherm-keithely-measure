# eurotherm-keithely-measure
The purpose of this LabVIEW program is to periodically measure the temperature from a Eurotherm process controller and calculate p<sub>O<sub>2</sub></sub> based on voltage from a Keithley multimeter for a TGA system.
This work is the continuation and development of programs previously written by others.

## Hardware and other libraries
Temperature measurements are taken directly from a Eurotherm 2408 controller with optional serial communications module.
A [driver package](http://sine.ni.com/apps/utf8/niid_web_display.download_page?p_id_guid=E3B19B3E971F659CE034080020E74861) for all 2400 series Eurotherm controllers is available form the NI website.
Serial communications again occur by VISA and require the [NI VISA drivers](https://www.ni.com/nisearch/app/main/p/bot/no/ap/tech/lang/en/pg/1/sn/catnav:du,n8:3.25.123.1640,ssnav:ndr/).

p<sub>O<sub>2</sub></sub> was calculated from voltage measurements taken of a YSZ sensor by a Keithley 2000 digital multimeter.
Communications were established with the Keithley via GPIB (IEEE-488) and thus require the [NI-488 driver from NI](http://www.ni.com/nisearch/app/main/p/bot/no/ap/tech/lang/en/pg/1/sn/catnav:du,n8:3.25.123.785,ssnav:ndr/).
Additionally, [drivers for the Keithley 2000](http://sine.ni.com/apps/utf8/niid_web_display.download_page?p_id_guid=014E6EF883B9743DE0440003BA7CCD71) are available online from NI and are required for the VI.

## Program Overview
A copy of the block diagram is given below.
When started, the program asks for the location to save the measurements.
The program then creates the csv file time in seconds, the voltage read, the calculated log p<sub>O<sub>2</sub></sub>, and the temperature from the controller.
Communications variables, such as baud rate and channels, are sent the appropriate sub-VIs to begin communications to the device and a timer is started.
The program backs up the Keithely's current settings, resets the device and prepares it to measure voltage on one channel, while displaying "Running" on the front panel display.
After the program is complete, the Keithely is reset once again and its original setting are restored before communications are closed to it and the Eurotherm.

The rest of the program occurs in a while loop.
The loop is delayed a fixed amount of time, set by the Measurement Period.
If the timer functionality is turned on, the loop repeats for a fixed amount of time based on the Length of Run variable, after which a stop signal is sent.
During each loop, the process value (temperature) of the Eurotherm is measured and any alarms or flags are checked for.
If any occur, they are passed to the error handler and the program stops.
The voltage of the specified channel of the Keithley is measured and the log(p<sub>O<sub>2</sub></sub>) is calculated from it.
These values are then displayed, plotted, saved to the csv file.

## Block Diagram
![Block Diagram](https://github.com/patrickstanley/eurotherm-keithley-measure/raw/master/Documentation%20Images/Temp_%26_pO2_Measurementsd.png)

## Front panel
![Front Panel](https://github.com/patrickstanley/eurotherm-keithley-measure/raw/master/Documentation%20Images/Temp_%26_pO2_Measurementsp.png)
