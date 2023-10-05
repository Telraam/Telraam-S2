# Change log for the K210 firmware of the Telraam S2 devices

## 66052 - 4.2.1 (5/10/2023)
- 2 day models -> test if flashing both models is ok

## 516 - 4.2.0 (4/10/2023)
- adjusted hysteresis limits changed from 0.13/0.23 -> 0.25/0.3 for night/day switch

## 524548 - 4.1.9 (3/10/2023)
- firmware built with script for 2 models

## 524548 - 4.1.8 (22/9/2023)
- first night counts firmware (bricked device)

## 459012 - 4.1.7 (19/04/2023)
- minor fixes (buffer overflow handling) - no effect on count accuracy and precision

## 393476 - 4.1.6 (21/02/2023)
- speed data (for cars) is now better calibrated using actual speed measurements (calibration parameter was incorrectly left out of FW 327940)

## 327940 (03/02/2023)
- count accuracy is improved by 7% using a new set of hyper-parameters trained on a larger and wider set of videos
- speed data (for cars) is now better calibrated using actual speed measurements (speed data from before this FW version was not calibrated!)

## 262404 (initial production version)

