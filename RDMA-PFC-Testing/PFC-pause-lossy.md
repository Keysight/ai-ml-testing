# Validating PFC pause for lossy queue
## Introduction
PFC pause is the mechanish used by PFC to perform flow control during a congestion. When a switch determines there is a congestion happening in the downstream network it starts sending a PFC pause packet to let its upstream neighbor know about the congestion and the high priority queue which is configured to be lossless to slow down or pause the transmit. This test plan will validate performance of a lossless traffic flow under congestion. 

PFC pause frames can prevent a switch queue from sending any data in a time interval. The PFC frame has a 8-bit field called 'class-enable vector' or 'priority enable ventor' where the bit N tells which priority N it pauses. if this bit is set to 1, it means pause. The PFC frame has 8 pause durations fields, one for each priority. For a priority to pause, the PFC frame also needs to specify its pause duration.  The pause duration for each priority is a 2-byte value that expresses time as a number of quanta, where each quanta represents the time needed to transmit 512 bits at the current network speed. For example, the value of 65535 at a 40 Gbps link means 65535 * 512bits / 40Gbps = 838.84 microseconds. Therefore, to fully block a switch queue, we need to generate PFC pause frames fast enough. For example, to block 40G link, we need to generate a PFC pause frame with 65535 quantas in every 838.84 microseconds. A pause duration of zero quanta has the special meaning of unpausing a priority.

In this test plan we will work with two PFC lossless priorities: 3 and 4. Note that only the lossless priorities can react to or generate PFC frames. In other words, PFC frames should not have any impact on traffic on lossy priorities. Packets with Differentiated Services Code Point (DSCP) 3 and 4 are mapped to priority 3 and 4, respectively.

## Setup

The testbed consists of two IXIA ports and a SONiC device under test (DUT) as follows. Both IXIA ports should have the same bandwidth capacity. To reduce the configuration complexity, we recommond configuring the switch as a Top of Rack (ToR) / Tier 0 (T0) switch and binding two switch interfaces to the Vlan.
```
                     _________
                    |         |
IXIA tx port ------ |   DUT   |------ IXIA rx port
                    |_________|
```
## Test Steps
In this experiment, we need to create three traffic items:

Test data traffic: Data packets sent from the IXIA tx port to the IXIA rx port at the lossy priorities (e.g., 0-2 and 5-7) that we want to test. Note that the packets should be marked with the correct DSCP value. The traffic demand should be 50% of the line rate.

Background data traffic: Data packets sent from the IXIA tx port to the IXIA rx port at the other priorities (e.g., 3 and 4). The traffic demand should be 50% of the line rate.

PFC pause storm: Persistent PFC pause frames from the IXIA rx port to the IXIA tx port. The priorities of PFC pause frames should be same as those of test data traffic. And the inter-frame transmission interval should be smaller than per-frame pause duration.

This experiment needs the following five steps:
- Start PFC pause storm.
- After a fixed duration (e.g., 1 second), start test data traffic and background data traffic simultaneously.
- After a fixed duration (e.g., 5 seconds), stop test data traffic and background data traffic.
- Check if the IXIA rx port receives all the sent frames of test data traffic and background data traffic.
- Stop PFC pause storm
