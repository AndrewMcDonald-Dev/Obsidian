
Universal Serial Bus is an industry standard that allows data exchange and delivery of power between many types of electronics. It specifies its architecture, in particular its physical interface, and communication protocols for data transfer and power delivery to and from hosts, such as personal computers, to and from peripheral devices, e.g. displays, keyboards, and mass storage devices, and to and from intermediate hubs, which multiply the number of a host's ports.

Originally, USB was designed to standardize the connection between peripherals to computers. USB has since evolved into a standard to replace virtually all common ports on computers, mobiles devices, peripherals, power supplies, and manifold other small electronics.

Throughout the history of USB there have been multiple connectors but, in an effort to simplify the standard, only the USB-C connector is up to date. In the current standard, the USB-C connector relpaces the many various connectors for power (up to 240W), displays (e.g. [[DisplayPort]], [[High-Definition Multimedia Interface (HDMI)]]), and many other uses, as well as all previous USB connectors.

There are four generations of USB, as of 2024, consisting of [[Universal Serial Bus (USB)#USB 1.x|USB 1.x]], [[Universal Serial Bus (USB)#USB 2.0|USB 2.0]], [[Universal Serial Bus (USB)#USB 3.x|USB 3.x]], and [[Universal Serial Bus (USB)#USB4|USB4]]]. USB4 adds to the standard the ability for tunneling of [[Thunderbolt#Thunderbolt 3|Thunderbolt 3]] protocols, namely [[Peripheral Component Interconnect (PCI)#PCIe|PCIe]] and [[DisplayPort]]. USB4 also adds host-to-host interfaces.

Each specification sub-version supports different signaling rates from 12Mbit/s total in USB 1.0 to 80 Gbit/s in USB4. USB also provides power to peripheral devices; the latest versions of the standard extend the power delivery limits for battery charging and devices requiring up to 240 watts ([[Universal Serial Bus (USB)#USB Power Delivery (USB-PD)|USB Power Delivery (USB-PD)]]). Over the years, USB-PD has been adopted as the standard power uspply and charging format for many mobile devices, such as mobile phones, reducing the need for proprietary chargers.

USB specifications provide backward capability, usually resulting in decreased signaling rates, maximal power offered, and othre capabilities. The USB 1.1 specification replaces USB 1.0. The USB 2.0 specification is backward compatible with USB 1.0/1.1. The USB 3.2 specification replaces USB 3.1 (and USB 3.0) while including the USB 2.0 specification. USB4 "functionally replaces" USB 3.2 while retaining the USB 2.0 bus operating in parallel.

## Connectors

There are three types of connectors used by the USB standard, Type-A, Type-B, and Type-C. Both Type-A and Type-B come in Standard, Mini, and Micro sizes while Type-C is intentionally only in the 'standard' size. For mass adoption reasons, a Type-C connector, also known as USB-C, is not exclusive to USB. It may be the only current standard USB and required for USB4 but, it is also required for other standards including modern [[DisplayPort]]
and [[Thunderbolt]]. It is reversible and can support various functionalities and protocols, including USB; some are mandatory, and many are optional, depending on the type of hardware: host, peripheral device, or hub.

### USB 1.x

Released in 1996, USB 1.0 specified signaling rates of 1.5 Mbit/s and 12 Mbit/s. It did not allow for extension cables, due to timing and power limitations. Few USB devices made it to the market until USB 1.1 was released in 1998. USB 1.1 was the earliest revision that was widely adopted.

Neither USB 1.0 nor 1.1 specified a design for any connector smaller than the standard type A or type B. Though many designs for a miniaturized type B connector appeared on many perihperals.

### USB 2.0

USB 2.0 was released in 2000, adding a higher maximum signaling rate of 480 Mbit/s in addition to the USB 1.x signaling rate of 12 Mbit/s. Modifications to the USB specification have been made via engineering change notices (ECNs). The most important of these ECNs are included into the USB 2.0 specification namely:

- Mini-A and Mini-B connector
- Micro-USB Cables and Connectors
- InterChip USB Supplement
- Battery Charging Specifcation 1.1
- Battery Charging Specificaiton 1.2

### USB 3.x

Released in 2008, USB 3.0 adds a new architecture and protocol named SuperSpeed, with associated backward-compatible plugs, receptacles, and cables. The architecture provides for an operation mode at a rate of 5.0 Gbit/s, in addition to the three existing operation modes. 

Low-power and high-power devices remain operational with this standard, but devices implementing SuperSpeed can provide increased current of between 150 mA and 900 mA, by discrete steps of 150 mA

#### USB 3.1

Released in 2013, USB 3.1 has 2 variants. The first one preserves USB 3.0's SuperSpeed architecture and protocol and its operation mode is newly named USB 3.1 Gen 1, and the second version introduces a distinctively new SuperSpeedPlus architecture and protocol with a second operation mode named as USB 3.1 Gen 2. SuperSpeed+ doubles the maximum signaling rate to 10 Gbit/s, while reducing line encoding overhead to just 3% by changing the encoding scheme 128b/132b.

#### USB 3.2

USB 3.2, released in 2017, preserves existing USB 3.1 SuperSpeed and SuperSpeedPlus architectures and protocols and their respective operation modes, but introduces two additional SuperSpeedPlus operation modes (USB 3.2 Gen 1x2 and USB 3.2 Gen 2x2) with the new USB-C Fabric with signaling rates of 10 and 20 Gbit/s. 

Starting with USB 3.2 specification, in the effort of making an easier naming scheme, the new branding was to call the capabilities as SuperSpeed USB 5Gbps, SuperSpeed USB 10 Gbps, and SuperSpeed USB 20 Gbps, respectively. In 2023, this was replaced with simply USB 5Gbps, USB 10Gbps, and USB 20Gbps.

### USB4

The USB4 specification was released in 2019. USB4 is based on the Thunderbolt 3 protocol. It supports 40 Gbit/s throughput, is compatible with Thunderbolt 3, and backward compatible with USB 3.2 and USB 2.0. USB4 is only defined for USB-C connectors and its Type-C specification regulates the connector, cables and also power delivery features across all uses of USB-C cables. 

The dynamic sharing of bandwidth of a USB4 connection is achieved by tunneling of other protocols. This includes tunneling of USB 3.2 Gen 2 and [[DisplayPort]]. Other optional protocols, such as [[Peripheral Component Interconnect (PCI)#PCIe|PCI Express]] and [[802.3 (Ethernet)|Ethernet]] can also be tunneled.

It is mandatory that all USB4 implementations support for Thunderbolt 3 connections. USB4 "40 Gbps" is an implementation of Thunderbolt 4. Although, Thunderbolt 4 mandates some features that are optional in USB4  and mandates minimum PCIe and DP capabilities. Thunderbolt 5 is an implementation of USB4 "80 Gbps". It mandates even higher minimum PCIe and DP capabilities. It also mandates support for asymmetric 120 / 40 Gbit/s connections from hosts to docks but does not mention the reverse.

![[Pasted image 20241001200839.png]]
![[Pasted image 20241001201033.png]]

### USB-PD

USB PD, created in 2013, defines power delivery over USB connectors. Initially this was for standard USB Type-A and Type-B connectors to deliver increased power to devices with greater power demands. USB-PD Devices can request higher currents and supply voltages from compliant hosts. 

USB-PD Specification revision 2.0 was released as part of the USB 3.1 suite. It covers the USB-C cable and connector with a separate configuration channel, which now hosts a DC coupled low-frequency BMC-coded data cahnnel that reduces the possiblities for RF intereference. As of USB-PD 2.0, version 1.2, the six fixed power profiles for power sources have been deprecated. It defines four normative voltage levels and power supplies may support any maximum source output power from 0.5 W to 100W

USB-PD specification revision 3.0 allows granular control of voltage in 20 mV steps, and a current specified in 50 mA steps, to facilitate constant-voltage and constant-current charging.

USB-PD specification revision 3.1 adds extended power range mode whihc allows higher voltages of 28, 36, and 48 V, providing up to 240 W of power,.

| Specification                                                            | Current | Voltage | Power (max.) |
| ------------------------------------------------------------------------ | ------- | ------- | ------------ |
| Low-power device                                                         | 100 mA  | 5 V     | 0.50 W       |
| Low-power SuperSpeed (USB 3.0) device                                    | 150 mA  | 5 V     | 0.75 W       |
| High-power device                                                        | 500 mA  | 5 V     | 2.5 W        |
| High-power SuperSpeed (USB 3.0) device                                   | 900 mA  | 5 V     | 4.5 W        |
| Battery Charging (BC) 1.2                                                | 1.5 A   | 5 V     | 7.5 W        |
| Single-lane SuperSpeed+ (USB 3.2 Gen2x1, and former USB 3.1 Gen2) device | 1.5 A   | 5 V     | 7.5 W        |
| Power Delivery 3.0 SPR                                                   | 3 A     | 5 V     | 15 W         |
| Power Delivery 3.0 SPR                                                   | 3 A     | 9 V     | 27 W         |
| Power Delivery 3.0 SPR                                                   | 3 A     | 15 V    | 45 W         |
| Power Delivery 3.0 SPR                                                   | 3 A     | 20 V    | 60 W         |
| Power Delivery 3.0 SPR Type-C                                            | 5 A     | 20 V    | 100 W        |
| Power Delivery 3.1 EPR Type-C                                            | 5 A     | 28 V    | 140 W        |
| Power Delivery 3.1 EPR Type-C                                            | 5 A     | 36 V    | 180 W        |
| Power Delivery 3.1 EPR Type-C                                            | 5 A     | 48 V    | 240 W        |

