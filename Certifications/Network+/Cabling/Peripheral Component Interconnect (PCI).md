PCI is a local computer bus, created in 1992, used for attaching hardware devices in a computer and is part of the PCI Local Bus standard.Devices connected to the PCI bus appear to a bus master to be connected directly to its own bus and are assigned addresses in the processor's address space. It is a parallel bus, synchronous to a single bus clock. Attached devices can take either the form of an integrated circuit fitted onto the motherboard or an expansion card that fits into the slot.

The PCI standard has had many revisions over the years PCI-X and later PCIe in 2004.

| Spec    | Year | Change summary                                                                                                                     |
| ------- | ---- | ---------------------------------------------------------------------------------------------------------------------------------- |
| PCI 1.0 | 1992 | Original issue                                                                                                                     |
| PCI 2.0 | 1993 | Incorporated connector and add-in card specification                                                                               |
| PCI 2.1 | 1995 | Incorporated clarifications and added 66 MHz chapter                                                                               |
| PCI 2.2 | 1998 | Incorporated [ECNs](https://en.wikipedia.org/wiki/Engineering_change_orders "Engineering change orders"), and improved readability |
| PCI 2.3 | 2002 | Incorporated ECNs, errata, and deleted 5 volt only keyed add-in cards                                                              |
| PCI 3.0 | 2004 | Removed support for 5.0 volt keyed system board connector                                                                          |

## PCI-X

PCI-X, short for Peripheral Component Interconnect eXtended, is a computer bus and expansion card standard that enhanced the 32-bit PCI local bus for higher bandwidth demanded mostly by servers and workstations. It has beeen replaced in modern designs by the similar-sounding PCI Express with a completely different physical connector and a very different electrical design.

## PCIe

PCI Express is a high-speed serial computer expansion bus standard, designed to replace older PCI, PCI-X and [[Accelerated Graphics Port (AGP)|AGP]] bus standards. It is the commn motherboard interface for personal computers' graphics cards, capture cards, sound cards, hard disk drive host adapters, SSDs, Wi-Fi, and Ethernet hardware connections. PCIe has numerous improvements over the older standard, including higher maximum system bus thorughput, lower I/O pin count and smaller physical footprint, better performance scaling for bus devices, a more detailed error detection and reporting mechanism, and native hot-swap functionality. More recent revisions of the PCIe standard provide hardware support for I/O virtualization.

![[Pasted image 20241002141444.png]]

Different form factors for PCIe exist including PCI Express M.2. Computer bus interfaces provided through the M.2 connector are PCI Express 3.0, Serial ATA 3.0, and USB 3.0. It is up to the manufacturer of the M.2 ohst or device to choose which interfaces to support, depending on the desired level of host support and device type.

The PCIe transaction-layer protocol can also be used over some ohther interconnects, which are not electrically PCIe including Thunderbolt and USB4.