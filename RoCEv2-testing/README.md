This section describes test consideration for validating RoCEv2 congestion control implementing of switch and switch fabric in order to support lossless Ethernet for AI/ML use cases. 

# Background
The large AI model training involves many compute nodes of Servers with GPUs doing parallel computing and requires collective communication operations among many devices. The network connecting these devices need to provide high bandwidth throughput, lossless traffic, balanced latency between a pair of devices, reduce tail latency of worst case, and fast convergence during failure. 
The AI training network design can be 2-tiers or 3-tiers depends on required scale and design choice. Care needs to be taken for ECMP hashing, PFC deadlock and end-to-end communication latency. To validate the effectiveness of network fabric for large AI training network, switch fabric needs to exercise RoCE Congestion Control and Priority Flow Control and optimize buffer management for AI/ML workload. 
Below shows a scaled down 2-tier switch fabric with 4 leaf switches and 2 spine switches. Test consideration includes validating individual switch, as well as switch fabric. For simplicity, we start with testing individual switch for buffer management and PFC handling. The test methodology can be expanded to test switch fabric efficiency of forwarding AI/ML workload.

* [RoCEv2 Congestion Control](RoCEv2.md)
* [Priority flow control](PFC.md)
