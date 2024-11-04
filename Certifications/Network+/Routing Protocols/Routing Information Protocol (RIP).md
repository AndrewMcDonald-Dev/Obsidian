The Routing Information Protocol is one of the oldest distance-vector routing protocols which employs the hop count as a routing metric. RIP prevents routing loops by implementing a limit on the number of hops allowed in a path from source to destination. The largest number of hops allowed for RIP is 15, which limits the size of networks that RIP can support.

In most networking environments, RIP is not the prefessed choice of routing protocol, as its time to converge and scalability are poor compared to [[Enhanced Interior Gateway Routing Protocol (EIGRP)]], [[Open Shortest Path First (OSPF)]], or [[Intermediate System to Intermediate System (IS-IS)]]. However, it is easy to configure, because RIP does not require any parameters, unlike other protocols. 

RIP uses [[Internet Protocol Suite (TCP IP)#User Datagram Model (UDP)|User Datagram Protocol (UDP)]] as its transport protocol, and is assigned the reserved port number 520.

A grevious limitation of RIP is its slow convergence and [[count to infinity]] problems.

## RIPv1
Published in 1988, the initial implementation of RIP, RIPv1, would [[Broadcast]] to `255.255.255.255` every 30 seconds through every RIPv1 enabled interface. Neighbouring routers receiving the request message respond with RIPv1 segment, containing their [[Routing Table|routing table]]. The requesting router updates its own routing table, with the reachable [[Internet Protocol Suite (TCP IP)#Internet Protocol (IP)|IP]] network address, hop count and next hop, that is the router interface IP address from which the RIPv1 response was sent. As the requesting router receives updates from different neighbouring routers it will only update the reachable networks in its routing table, if it receives information about a reachable networks in its routing table, if it receives information about a reachable network it has not yet put in its routing table or information that a networ it has in its routing table is reachable with lower hop count. Therefore, a RIPv1 router will in most cases only have one entry for a reachable network, the one with the lowest hop count. 

A small random time variable being added to the update time, to avoid routing tables synchronizing across a [[Local Area Network (LAN)|LAN]]. It was thought that due to random initialization, the routing updates would sperad out in time, but this was not true in practice.

RIPv1 uses the antiquated classful routing. The periodic routing updates do not carry [[Subnet|subnet]] information, lacking support for [[Classless Inter-Domain Routing (CIDR)|Variable Length Subnet Masks (VLSM)]]. This limitation makes it impossible to have different-sized subnets inside of the same network class. In other words, all subnets in a network class msut have the same size. There is also no support for router authentication, making RIP vulnerable to various attacks.


![[Pasted image 20241012144810.png]]

## RIPv2
Due to the deficiencies of the original RIP specification, RIPv2 was published in 1994, and declared Internet Standard 56 in 1998. It included the ability to carry subnet information, thus support [[Classless Inter-Domain Routing (CIDR)]]. To maintain backward compatibility, the hop count limit of 15 remained. RIPv2 has facilities to fully interoperate with the earlier specification if all *Must Be Zero* protocol fields in the RIPv1 messages are properly speficied. In addition, a *compatibility switch* feature allows fine-grained interoperability adjustments.

In an effort to avoid unnecessary load on hosts that do not participate in routing, RIPv2 *[[Multicast|multicasts]]* the entire routing table to all adjacent routers at the address `224.0.0.9`, as opposed to RIPv1 which uses [[Broadcast|broadcast]]. [[Unicast]] addressing is still allowed for special applications.

[[MD5]] authentication for RIP was introduced in 1997. Route tags were also added in RIP version 2. This functionality allows a distinction between routers learned from the RIP protocol and routes learned from other protocols.


![[Pasted image 20241012144836.png]]

## RIPng
RIPng is an extension of RIPv2 for support of IPv6, the next generation Internet Protocol. This main differences between RIPv2 and RIPng are:
- Support of IPv6 networking
- While RIPv2 supports RIPv1 updates authentication, RIPng does not. [[Internet Protocol Suite (TCP IP)#IPv6|IPv6]] routers were, at the time, uspposed to use [[Internet Protocol Suite (TCP IP)#IPsec|IPsec]] for authenication.
- RIPv2 encodes the next-hop into each route entry, RIPng requires specific encoding of the next hop for a set of route entries.
RIPng sends updates on UDP port 521 using the multicate group `ff02::9`.