
[[Internet Protocol Suite (TCP IP)#Transmission Control Protocol (TCP)|Transmission Control Protocol (TCP)]] uses a congestion control algorithm that includes various aspects of an [[TCP Congestion Control#Additive increase/multiplicative decrease (AIMD)|additive increase/multiplicative decrease]] scheme, along with other schemes including [[TCP Congestion Control#Slow Start|slow start]] and a [[TCP Congestion Control#Congestion Window (CWND)|congestion window (CWND)]], to achieve congestion avoidance. 

To avoid [[TCP Congestion Control#Congestive Collapse|congestive collapse]], TCP uses multi-faceted congestion-control strategy. For each connection, TCP maintains a CWND, limiting the total number of unacknowledeged packets that may be in transit end-to-end. This is somewhat analogous to TCP's [[Internet Protocol Suite (TCP IP)#Sliding Window|sliding window]] used for [[Internet Protocol Suite (TCP IP)#Flow Control|flow control]].

## Additive increase/multiplicative decrease (AIMD)

AIMD algorithm is a closed-loop control algorithm. AIMD combines linear gorwth of the congestion window with an exponential reduction when congestion occurs. Multiple flows using AIMD congestion control will eventuallly converge to use equal amounts of a coontended link.

## Slow Start

Slow start is part of the congestion control strategy used by TCP in conjunction with other algorithsm to avoid sending more data than the network is capable of forwarding, that is, to avoid causing network congestion.

Slow start begins initially with a small congestion window size but with every acknowledgement the window size is effectively doubled. Once packet loss is detected TCP assumes that it is due to network congestion and takes steps to reduce the offered load on the network. These measures depend on the exact TCP congestion avoidance algorithm used.

## Congestion Window (CWND)

In TCP, the congesiton window (CWND) is one of the factors that determines the number of bytes that can be sent out at any time. The congestion window is maintained by the sender and is a means of preventing a link between the sender and the receiver from becoming overloaded with too much traffic. This should not be confused with the sliding window maintained by the sender which exists to prevent the receiver from becoming overloaded. The congesiton window is calculated by estimating how much congestion there is on the link.

The main cause of variance in the congestion window is dictated by an AIMD approach. This means that if all segments are received and the acknowledgments reach the sender on time, some constant is added to the window size. 

