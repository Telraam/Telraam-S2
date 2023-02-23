# Telraam S2 count performance validation

We are continuously working on improving the AI which is responsible for the traffic counts. This is a never-ending process, but to stay completely transparent, we provide updates on the current (and past) performance of the AI chip. We will update this article regularly to keep you up to date with the performance of Telraam S2.

In order to evaluate the count performance of any K210 firmware of the AI chip (this one is responsible for counting objects) we use a debugging tool, which can emulate the performance of the firmware on simple video files (recorded with, e.g., a GoPro camera). As input we have assembled a set of 12 short videos (2-6 minutes) covering various streets across Leuven, Brussels, and London; ranging from cycling and pedestrian areas to 2×3 lane express roads, and representing various traffic and weather conditions. We derived the true number of road users from these videos by manual counting, and we used the debugging tool to get the totals that would be counted by our Telraam S2 devices.

