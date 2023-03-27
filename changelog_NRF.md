# Change log for the NRF firmware of the Telraam S2 devices

## 1.12 (27/03/2023)
- ROI setting stability fixed (manual setting was sometimes not applied after a reboot cycle and the automated selection was applied instead)

## 1.11 (06/03/2023 - only for internal testing)
- show skippable message when firmware update is ongoing
- small bug-fixes (total counter handling on boot, FW update time limit adjustment, line spacing adjusted on error message screen)

## 1.10 (31/01/2023)
- fixed bug in time handling where the time skipped from 28/01/2023 to 01/03/2023
- added sim-iccid in the hello message

## 1.9 (26/01/2023)
- changed time state (modem or config) behaviour to not be persistent and reset with power cycle

## 1.8 (4/01/2023)
- fixed issue with network operator not providing the time

## 1.7.1 (12/12/2022)
- print network time -> check issue with network operator not providing the time

## 1.7 (no date, internal bagaar fw)
- changes for production testing

## 1.6 (21/10/2022)
- modify the QR code that it also contains the 2 letter characters, not only the numeric part

## 1.5 (21/10/2022)
- memory optimization of 38KB RAM in the image array
- fixing the issue that a picture request did not always complete
- messages preferably appear vertically centred on the screen and not on top as they appear now
- put the serial number under the QR code in one line

## 1.4 (12/10/2022)
- Finalized screen anti-aliasing
- manually set ROI sent to K210 chip (this is currently not being transferred by the NRF to the K210)
- QoS to 0 -> no more puback messages so number of messages is now half of the original
- spread out the data transmission a bit from 0 to a max where max is a duration in seconds that is customisable via a config update message (for load balancing in the future) - so transfer would happen at a random moment between 0 and max.
