## Priority Flow Control (PFC) 
This test exercise switch PFC implementation for traffic throttling when receive PFC pause, and resume normal transmission after the situation is alleviated. This could happen in AI/ML training network when Server/NIC runs into resource bottleneck. PFC is the last barrier for preventing traffic loss when ECN/CNP congestion control failed to prevent loss. 
Test setup:
The testbed consists of 4x100GE Tester ports forming 2 port pairs. RoCEv2 traffic is sent between both port pairs. When PFC Pause is received for a PFC queue on switch port, it should only impact the traffic mapped to that PFC queue.

![PFC testing](./PFC.png) 
      
Steps: 
* Configure Port 1 & 2 as initiator ports, port 3 & 4 as responder ports. Configure 2 Q-Pairs from each initiator port to the corresponding responder port.
* At Port 1, generate RDMA traffic (simulate RDMA Write) with buffer size 1M. Configure DSCP priority 3 for Q-Pair 1 and DSCP priority 4 for Q-Pair 2. DSCP value 3 & 4 are lossless priority
* At Port 2, generate RDMA traffic (simulate RDMA Write) with buffer size as 1M. Configure DSCP priority 3 for Q-Pair 3 and DSCP priority 4 for Q-Pair 4. 
* Each Q-Pair is setup to transmit at 45% of line rate and each initiator port sends total 90% line rate traffic. 
* Generate continue PFC Pause frames with priority 3 (map to DSCP value 3) and max Quanta (65535) 
* At switch side, configures 2 PFC queues and map them to DSCP priority 3 & 4 respectively.
* Start RoCEv2 traffic. Switch forwards the traffic to egress ports. Observe traffic throughput and latency per Q-Pair and per port.
* At port 3, start PFC pause frames to block traffic for DSCP priority 3. Observe traffic throughput and latency per Q-Pair and per port. 
* At port 3, send a X-off PFC pause frame (Quanta 0). Switch should resume forwarding traffic.
   Expected results: 
* Check traffic throughput per Q-Pair and per port. There should be no loss.
* Both Q-Pairs on same port pair should show similar average latency. There should be no abnormal latency (eg. One Q-Pair has much higher latency compare with the other one). 
* At port 3, upon sending pause frame for DSCP priority 3, Rx traffic is paused. Upon sending X-off Pause frame, traffic is then resumed to normal throughput. Check latency for Q-Pair 1, it should show increase of latency due to PFC throttling. There maybe loss if switch buffer overflow during pause period.
* At port 3, check throughput and latency of Q-Pair 2. Both should not be impacted. 
* At port 4, check throughput and latency of Q-Pair 3 and Q-Pair 4. Both should not be impacted. 