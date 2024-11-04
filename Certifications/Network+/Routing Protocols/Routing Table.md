In computer networking, a routing table is a data table stored in a [[Router]] or a network host that lists the routes to particular network destinations, and in some cases, metrics associated with those routes. The routing table contains information about the topology of the network immediately around it.

The construction of routing tables is the primary goal of routing protocols. Static routes are entries that are fixed, rather than resulting from routing protocols and network topology discovery procedures.

The primary function of a router is to forward a [[Packet]] toward its destination network, which is the destination [[Internet Protocol Suite (TCP IP)#Internet Protocol (IP)|IP]] address of the packet. To do this, a router needs to search the routing information stored in its routing table. The routing table contains network/next hop associations. These associations tell a router that a particular destination can be optimally reached by sending the packet to a specific router  that represents the next hop on the way to the final destination.

With hop-by-hop routing, each routing table lists, for all reachable destinations, the address of the next device along the path to that destination: the next hop. Assuming that the routing tables are consistent, the simple algorithm of relaying packets to their destination's next hop thus suffices to deliver data anywhere in a network. Hop-by-hop is the fundamental characteristc of the IP [[Internet Protocol Model (IP Model)#Internet|Internet Layer]] and the OSI [[OSI Model#Network|Network Layer]].

The need to record routes to large numbers of devices using limited storage space represents a major challenge in routing table construction. In the internet, the currently dominant address aggregation technology is a bitwise prefix matching scheme called [[Classless Inter-Domain Routing (CIDR)]]. 

## Contents
The routing table consists of at least three information fields:
1. Network Identifies: The destination subnet and netmask
2. Metric: The routing metric of the path through which the packet is to be send. The route will go in the direction of the gateway with the lowest metric.
3. Next Hop: The next hop, or gateway, is the address of the next station to which the packet is to be send on the way to its final destination.

Depending on the application and implementation, it can also contain additional values that refine path selection: 
1. [[Quality of Service (QoS)]] associated with the route. For example, the U flag indicates that an IP route is up.
2. Filtering Criteria: [[Access Control List (ACL)]] associated with the route
3. Interface: Such as eth0 for the first Ethernet card, eth1 for the second Ethernet card, etc.

