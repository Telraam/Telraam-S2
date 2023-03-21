# Debug tool validation and evaluation of the precision of Telraam S2

As mentioned in the [count performance validation documentation](https://github.com/Telraam/Telraam-S2/blob/main/count-performance-validation.md), to evaluate the count performance of any K210 firmware of the AI chip (this one is responsible for counting objects) we use a debugging tool, which can emulate the performance of the firmware on simple video files (recorded with, e.g., a GoPro camera). This way we can test the performance of a new firmware on a set of videos recorded at various locations, instead of having to deploy the firmware on test devices and having to collect parallel ground truth counts to evaluate the performance. Of course one wants to control the validity of this setup by comparing the results of the debug tool and the actual Telraam S2 device in a controlled experiment, hoping that these would be reasonably similar to each other.

We set up our control experiment (using the K210 FW version 393476) at the Telraam offices. We hang 8 Telraam S2 devices in a 4×2 pattern on one of our windows with a clear, unobstructed view of a three-lane road which is sandwiched by a segregated bike lane and a sidewalk on each side. We have fixed the region of interest of these devices to the same optimal setting, which means that - barring some minor differences originating from field rotation (simulating not perfectly vertical unit installation by the users) and the slightly different installation positions (in the 4×2 pattern) - they all have the same view of the street.

Parallel with the counting devices, we have made a GoPro recording of the street covering an exact counting period of the Telraam S2 devices, from 2023-03-15 14:30Z to 2023-03-15 14:45Z.

The main goal of the experiment is to compare the counts from the 8 Telraam S2 devices from this quarter of an hour, to the counts that are derived by running the debug tool on the GoPro recording (which is cut to match the region of interest set on the Telraam S2 cameras). A secondary result is sampling the device-to-device scatter of Telraam S2 devices installed at the same location and using the same settings, which gives an indication of the precision of our count measurements - as optimally one wants high measurement accuracy and high precision from any kind of data collecting sensor. 

| Class       | Ground truth | 8 Telraam S2 devices | Debug script |
|-------------|-------------:|---------------------:|-------------:|
| Bicycle     |  25          |  27 ± 3              |  33          |
| Bus         |   6          |   6 ± 1              |   6          |
| Car         | 200          | 204 ± 5              | 202          |
| Light truck |  24          |  23 ± 3              |  31          |
| Motorcycle  |   3          |   1 ± 0              |   2          |
| Pedestrian  |  13          |  22 ± 3              |  25          |
| Stroller    |   0          |   2 ± 2              |   0          |
| Tractor     |   0          |   0 ± 0              |   0          |
| Trailer     |   1          |   1 ± 1              |   1          |
| Truck       |   5          |   5 ± 1              |   5          |
