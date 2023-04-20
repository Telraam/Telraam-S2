# Change log for the K210 firmware of the Telraam S2 devices

## 459012 (19/04/2023)
- minor fixes (buffer overflow handling) - no effect on count accuracy and precision

## 393476 (21/02/2023)
- speed data (for cars) is now better calibrated using actual speed measurements (calibration parameter was incorrectly left out of FW 327940)

## 327940 (03/02/2023)
- count accuracy is improved by 7% using a new set of hyper-parameters trained on a larger and wider set of videos
- speed data (for cars) is now better calibrated using actual speed measurements (speed data from before this FW version was not calibrated!)

## 262404 (initial production version)

