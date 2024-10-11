A network swtich is networking hardware that connects devices on a computer network by using [[Packet Switching|packet switching]] to receive and forward data to the destination device. A network switch is a multiport [[Bridge|network bridge]] that uses [[MAC Address]]s to forward data at the [[OSI Model#Data Link|data link layer]] of the [[OSI Model]]. Some switches can also forward data at the [[OSI Model#Network|network layer]] by additionally incorporating [[Router|routing]] functinality. Such switches are commonly known as layer-3 switches or [[Multilayer Switch|multilayer switches]].

Switches for [[802.3 (Ethernet)|Ethernet]] are the most common form of network switch. Ethernet was initially a [[Shared-Access Medium]], but the introduction of the [[Medium Access Control (MAC)|MAC]] bridge begain its transformation into its most-common point-to-point form without a [[Collision Domain]]. Switches also exist for other types of networks including [[Fibre Channel]], [[Asynchronous Tranfer Node]], and [[InfiniBand]].

Unlike [[Ethernet Hub|repeater hubs]], which broadcast the same data out of each port and let the devices pick out the data addressed to them, a network switch learns the Ethernet addresses of connected devices and then only forward data to the port connected to the device to which it is addressed.

Typical management features found on a switch:
- Centralized configuration management and configuration distribution
- Enable and disable ports
- Link bandwidth and [[full duplex]] settings
- [[Quality of Service (QoS)]] configuration and monitoring
- [[Mac Filtering]] and other [[Access Control List (ACL)|access control list]] features
- Configuration of [[Spanning Tree Protocol (STP)]] and [[Shortest Path Bridging (SPB)]] features
- [[Simple Network Management Protocol (SNMP)]] monitoring of device and link health
- [[Switch#Port Mirroring|Port mirroring]] for monitoring traffic and troubleshooting
- [[Link Aggregation]] configuration to set up multiple ports for the same connection to achieve higher data transfer rates and reliability
- [[Virtual Local Access Network (VLAN)]] configuration and prot assingments including [[Institute of Electrical and Electronics Engineers (IEEE)|IEEE]] 802.1Q tagging
- [[Network Time Protocol (NTP)]] synchronization
- [[Network Access Control]] features such as [[Institute of Electrical and Electronics Engineers (IEEE)|IEEE]] 802.1X
- [[Link Layer Discovery Protocol (LLDP)]]
- [[Internet Group Management Protocol (IGMP)|IGMP]] snooping for control of multicast traffic


## Port Mirroring

Because the purpose of a switch is to not forward traffic to network segments where it would be superfluous, a node attached to a switch cannot monitor traffic on other segments. Port mirroring is how this problem is addressed in switched networks: In addition to the usual behavior of forwarding frames only to ports through which they might reach their addresses, the switch forward frames received through a given monitored port to a designated monitoring port, allowing analysis of traffic that would otherwise not be visible through the switch.


## Cut-through Switching

In computer networking, cut-through switching is a method for [[Packet Switching|packet switching]] systems, wherein the switch starts forwarding a [[Frame]] (or [[Packet]]) before the whole frame has been received, normally as soon as the destination address and outgoing interface is determined. Compared to [[Switch#Store and Forward|store and forward]], this technique reduces latency throuhg the swtich and relies on the destination devices for error handling. Pure cut-through switching is only possible when the speed of the outgoing interface is at laest equal or higher than the incoming interface speed.

## Store and Forward

Store and forward is a telecommunications technique in which information is sent to an intermediate station where it is kept and sent at a later time to the final destination or to another intermediate station. The intermediate station, or node in a networking context, verifies the integrity of the message before forwarding it. In general, this technique is used in networks with intermittent connectivity, especially in the wilderness or environments requiring high mobility. 