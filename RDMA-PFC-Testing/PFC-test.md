# Testing PFC for AI/ML
This testplan describes the key use cases of PFC and its relevance to AI/ML networks and also covers how to test PFC functionality in AI/ML switch fabrics
## What is PFC
Priority Flow Control (PFC) is an important feature for AI/ML switch fabric. PFC is ethernet standards methodology to provide essential functionalities of prioritizing trafic and making sure data is transimitted in lossless manner.The main feature of PFC that help in it functionality are 
1. Priority class: Determines the priority of a flow of traffic and is marked by assigning it to a priority queue
2. Flow control: During a congestion PFC can determine which traffic to pause and which to transmit to maintain lossless and lossy flows
3. Traffic pause funtionality: Switches and network devices with PFC capability can generate "Pause" frames with a particular quanta (time) to pause traffic from a particular queue. 
   
In case of congestion PFC mechanism can manage the data flows in lossless and lossy quesues. It will pause the lossless queue in order not to drop any packets from this flow and will let the lossy flow transmit even if loses some poackets

## Relevance to AI/ML
PFC is very relevant to AI/ML usecases. With PFC one can make sure the AI?ML switch fabric delivers on the KPIs like
1. Prioritizing traffic
2. Congestion handling

## PFC testing area
Testing PFC functionality is one of the key area of validation of AI/ML switch fabric. This test plan convers comprehensive testing of PFC and draws inspiration from established test methodologies used by SONiC community. Here are the different areas of PFC that we will explore in this test plan.
* PFC pause lossless
* PFC pause lossy
* PFC pause response headroom
* PFC congestion oversubscription


