# Telraam S2 count performance validation

We are continuously working on improving the AI which is responsible for the traffic counts. This is a never-ending process, but to stay completely transparent, we provide updates on the current (and past) performance of the AI chip. We will update this documentation regularly to keep you up to date with the performance of Telraam S2.

In order to evaluate the count performance of any K210 firmware of the AI chip (this one is responsible for counting objects) we use a debugging tool, which can emulate the performance of the firmware on simple video files (recorded with, e.g., a GoPro camera). As input we have assembled a set of 12 short videos (2-6 minutes) covering various streets across Leuven, Brussels, and London; ranging from cycling and pedestrian areas to 2×3 lane express roads, and representing various traffic and weather conditions. We derived the true number of road users from these videos by manual counting, and we used the debugging tool to get the totals that would be counted by our Telraam S2 devices.

The validation of this method itself, and the evaluation of the device-to-device scatter (a.k.a. the precision of Telraam S2) is discussed in the [debug tool validation and evaluation of the precision of Telraam S2 documentation](https://github.com/Telraam/Telraam-S2/blob/main/count-consistency-validation.md). If you are interested in the speed measurements of Telraam S2, then please consult the [relevant FAQ page](https://telraam.helpspace-docs.io/article/41/speed-measurements-with-telraam-s2).

## K210 FW version 393476

No changes on the count performance.

## K210 FW version 327940 (evaluation done on 31/01/2023)

![FW 327940 count evaluation](/assets/327940_counts.png)

The improvement compared to FW 262404 is 7% when performance is described by the sum of full sample and segment by segment accuracy percentages of the 4 classical Telraam modes. The highest this sum could reach is 800 (four times 100% per mode, for both full sample and segment by segment evaluations). It is 650 for FW 262404, and 695 for the new one. Note: if in all categories we would reach 95% that would mean a sum of 760, 90% would translate to 720, while 695 represents a grand average of 87%. Our total accuracy concerning total object count is 96%, and our weighted average accuracy over the individual segments is 91%. Individual mode by mode accuracy numbers are shown in the table and figure below.

| Accuracy       | Full sample [%] | Segment-by-segment (traffic weighted average) [%] |
|----------------|-----------------|---------------------------------------------------|
| Heavy vehicles |              90 |                                                74 |
| Cars           |              96 |                                                93 |
| Two wheelers   |              96 |                                                92 |
| Pedestrians    |              96 |                                                58 |

![FW 327940 accuracy values](/assets/327940_accuracy.png)

The improvement compared to the earlier FW is actually even larger than what the numbers suggest, as now cars and light trucks are counted separately (and light trucks historically belong with heavy vehicles for Telraam, which means that we need this separation to be able to consistently aggregate data coming from both Telraam V1 and Telraam S2 devices to he classical 4 Telraam road user classes).

Unfortunately, pedestrian performance is still lacking, albeit the main reason for that is that our testing videos are biassed towards difficult situations for pedestrians, which is rather atypical for normal Telraam streets. In these more simple streets the performance is expected to be even better for every class, especially for bikes and pedestrians.

While the amount of non-favourable streets and locations is much less in the eye of Telraam S2 compared to Telraam V1, there are still some special situations that might cause sub-par performance. 

## K210 FW version 262404 (evaluation done on 20/12/2022)

![FW 262404 count evaluation](/assets/262404_counts.png)

When looking at the segments together (in the figure above), we can see that the performance is already very good. 874 objects were counted by Telraam S2, while in reality there were 870 objects, meaning that the error on the total number of objects is half percent (or that the accuracy is 99.5%). When looking at the accuracy on a road user type basis (in the figure below, orange graph of full sample), we see accuracy values of 90% for heavy vehicles, 95% for cars and light trucks, 90% for bicycles and motorcycles, and 85% for pedestrians.

As some segments perform better than others, and various conditions can result in overcounts or undercounts, we also calculated the accuracy on a segment-by-segment basis where the absolute errors are averaged over the 12 segments and weighted by traffic volume. This results in accuracy values at or above 90% for all modes except for heavy vehicles and pedestrians, but the total number of these objects is relatively low in our sample, and heavy vehicles suffer from a still-to-be-corrected detection bias, while pedestrians were only present in large numbers in two of the 12 videos, both cases in very challenging situations (groups of pedestrians on crossing trajectories), where we don’t expect the AI to be fully capable of detecting every object.

![FW 262404 accuracy values](/assets/262404_accuracy.png)
