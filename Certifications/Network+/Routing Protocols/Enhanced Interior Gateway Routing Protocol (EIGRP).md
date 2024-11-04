EIGRP is an advanced distance-vector routing protocol that is used on a computer network for automating routing decisions and configuration. EIGRP is used on a router to share routes with other routers within the same autonomous system. Unlike other well known routing protocols, such as [[Routing Information Protocol (RIP)|RIP]], EIGRP only sends incremental updates, reducing the workload on the router and the amount of data that needs to be transmitted. EIGRP replaced [[Interior Gateway Routing Protocol (IGRP)]] in 1993. One of the major reasons for this was the change to classless [[Internet Protocol Suite (TCP IP)#IPv4|IPv4]] addresses in the Internet Protocol, which IGRP could not support.

## Features
EIGRP supports the follwoing features:
- Support for [[Load Balancer|load balancing]] on parallel links between sites
- The ability to use different authentication passwords at different times.
- [[MD5]] and [[Secure Hash Algorith 2 (SHA-2)|SHA-2]] authenticaiton between two routers.
- Sends topology changes, rather than sending the entire routing table when a route is changed.
- Periodically checks if a route is available, and propagates routing changes to neighboring routers if any changes have occurred.
- Runs separate routing processes for [[Internet Protocol Suite (TCP IP)#Internet Protocol (IP)|Internet Protocol (IP)]], [[Internet Protocol Suite (TCP IP)#IPv6|IPv6]], [[Internet Protocol Suite (TCP IP)#IPX|IPX]] and AppleTalk, through the use of [[Protocol-Dependent Modules (PDM)]].
- Backwards compatibility with the IGRP routing protocols.