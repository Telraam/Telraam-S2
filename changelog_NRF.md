# Change log for the NRF firmware of the Telraam S2 devices
## 1.19 (07/03/2025)
- support for LED-PCB on new outdoor device
- support for 1NCE Brasil ICCID
- various improvements and bug-fixes

## 1.18 (14/01/2025)
- update Thingstream library
- Improved handling of K210 data and K210 communication, in case of K210-reset less data will be lost
- various improvements and bug-fixes

## 1.17.1 (18/09/2024)
- merged indoor, outdoor and 1NCE functionality in 1 FW
- various improvements and bug-fixes

## 1.16 (06/06/2024)
- FW specific to devices with a 1NCE sim card (functionality will be merged to general fleet-wide FW in 1.17)

## 1.15.1-rc4 (14/05/2024)
- new outdoor specific FW version to facilitaty easier testing during assembly (not released for normal fleet, functionality will be merged in 1.17)

## 1.15.1-rc3 (13/03/2024)
- compatibility updates for the night counts
- other bugfixes
- fleet wide release date 27/05/2024

## 1.15.0 (7/11/2023)
- first release with support for outdoor sensor

## 1.14.1 (3/10/2023)
- support for 2 K210 models (for day and night counts)

## 1.14 (17/04/2023)
- bug fixed where setting roi=666 triggered a reboot, but roi=666 was not saved before rebooting.  In this fw, roi is saved before rebooting.

## 1.13 (28/03/2023)
- further fine-tuning of manually and automatically selected ROI storage in flash memory (only manually set is persistent) and communication between devices and backend

## 1.12 (27/03/2023 - only for internal testing)
- ROI setting stability fixed (manual setting was sometimes not applied after a reboot cycle and the automated selection was applied instead)

## 1.11 (06/03/2023 - only for internal testing)
- show skippable message when firmware update is ongoing
- small bug-fixes (total counter handling on boot, FW update time limit adjustment, line spacing adjusted on error message screen)
- NB-IoT fallback

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
