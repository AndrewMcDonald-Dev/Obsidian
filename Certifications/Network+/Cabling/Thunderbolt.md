Thunderbolt is the brand name of a hardware interface for the connection of external peripherals to a computer. It was developed by Intel in collaboration with Apple. Thunderbolt combinse [[Peripheral Component Interconnect (PCI)#PCIe|PCI Express (PCIe)]] and [[DisplayPort]] into two serial signal, and additionally provides DC power via a single cable. Up to six peripherals may be supported by one connector through various [[Physical Topology|topologies]]. Thunderbolt 1 and 2 use the same connector as Mini DisplayPort, whereas Thunderbolt 3, 4, and 5 use the USB-C connector, and support USB devices. 

Thunderbolt can be implemented on PCIe graphics cards, which have access to DisplayPort data and PCIe connectivity, or on the motherboard of computers with onboard video, such as the MacBook Air. Thunderbolt was initially designed to use optical fiber cabling but it was found that copper wiring could furnish the desired 10 Gbit/s per channel at lower cost.

## Thunderbolt 1

Thunderbolt 1, released in 2011, was a connection interface based on Mini-DisplayPort which ran at 10 Gbit/s making it faster than USB at the time. The DisplayPort standard was partially compatible with Thunderbolt 1.

## Thunderbolt 2

In 2013, Intel announced the next version of Thunderbolt which support 20 Gbit/s transfer rates. This is made possible by joining the two existing 10 Gbit/s channels, which does not change the maximum bandwidth, but makes using it more flexible.

Thunderbolt 2 incorporates [[DisplayPort#1.2| DisplayPort 1.2]] support, which allows for video streaming to a single 4k video mointor or dual QHD monitors. 

## Thunderbolt 3

Thunderbolt 3 is a hardware interface developed by Intel. It shares USB-C connectors with USB, supports [[Universal Serial Bus (USB)#USB 3.1|USB 3.1 Gen 2]], and can require special "active" cables for maximum performance for cable lengths over 0.5 meters. Compared to last version, Thunderbolt 3 doubles the bandwidth to 40 Gbit/s. It allows up to 4 lanes of [[Peripheral Component Interconnect (PCI)#PCIe|PCI Express 3.0]] for general-purpose data transfer, and 4 lanes of DisplayPort 1.4 HBR3 for video, but the maximum combined data rate cannot exceed 40 Gbit/s.

Thunderbolt 3 has up to 15 watts of power delivery on copper cables and no power delivery capability on optical cables. Using USB-C on copper cables, it can incorporate [[Universal Serial Bus (USB)#USB-PD|USB Power Delivery]], allowing the ports to source or sink up to 100 watts of power. This eliminates the need for a separate power supply from some devices. Thunderbolt 3 allows backwards compatibility with the first two versions by the use of adapters or transitional cables.

#### USB4 Implementation

The [[Universal Serial Bus (USB)#USB4|USB4]] specification is based on the Thunderbolt 3 protocol specification. It supports 40 Gbit/s throughput, is optionally compatible with Thunderbolt 3, and is backwards compatible with USB 3.2 and USB 2.0. USB4 supports [[DisplayPort#2.0|DisplayPort 2.0]] over its alternative mode.

## Thunderbolt 4

Thunderbolt 4 was released in 2020. The key differences between this version and last are a minimum bandwidth requirement of 32 Gbit/s for PCIe link, support for dual 4K displays, and Intel VT-d-based direct memory access protection to prevent physical [[Direst Memory Access|DMA attacks]].

Another majory advancement is that Thunderbolt 4 supports Thunderbolt Alternate Mode USB hubs, and not just daisy chaining. These hubs are backwards compatible with Thunderbolt 3. The maximum bandwidth remains the same at 40 Gbit/s

## Thunderbolt 5

Previewed in 2023, Thunderbolt 5 aligned with USB Implementers Forum's [[Universal Serial Bus (USB)#USB4|USB4 2.0]] specification. It provides symmetric bandwidth of 80 Gbit/s, for mass-storage devices, double that of Thunderbolt 4, and unidirectional bandwidth of 120 Gbit/s for displays, supporting dual 8K Displays at 60 Hz. 

The full specifications cover:

- Supporting the latest version of USB4 2.0 80 Gbit/s specification
- Two times the total bandwidth of Thunderbolt 4 to 80 Gbit/s, while providing up to three times the bandwidth to 120 Gbit/s for video-intensive uses
- Support for [DisplayPort](https://en.wikipedia.org/wiki/DisplayPort "DisplayPort") 2.1
- Two times (64 G[bit](https://en.wikipedia.org/wiki/Bit "Bit")/s) the PCI Express data-throughput using [PCI Express Gen. 4 x4](https://en.wikipedia.org/wiki/PCI_Express#PCI_Express_4.0 "PCI Express"), for faster storage and external graphics
- Up to 240W of charging power downstream
- Works with existing passive cables up to 1 m (3.3 ft) via PAM-3
- Compatible with previous versions of Thunderbolt, USB, and DisplayPort
- Supported by Intel's enabling and certification programs