
HDMI is a proprietary audio/video interface for transmitting uncompressed video data and compressed or uncompressed digital audio data from an HDMI-compliant source device, such as a display controller, to a compatible computer monitor, video projector, digital television, or digital audio device. HDMI is a digital replacement for analog video standards such as [[Video Graphics Array (VGA)|VGA]] and [[Digital Visual Interface (DVI)|DVI]]. 

Several versions of HDMI have been developed and deployed since the initial release of the technology, occasionally introducing new connectors with smaller form factors, but all versions still use the same basic pinout and are compatible with all connector types and cables. Other than improved audio and video capacity, performance, resolution and color spaces, newer versions have optional advanced features such as 3D, [[802.3 (Ethernet)|Ethernet]] data connection, and CEC extensions.

## 1.0

Released in 2002, HDMI 1.0 is a single-cable digital audio/video connector interface. The link architecture is based on [[Digital Visual Interface (DVI)|DVI]], using exactly the same video transmission format but sending audio and other auxiliary data during the blanking intervals of the video stream. Therefore the bandwidth is the same between HDMI 1.0 and DVI.

## 1.1

Released in 2004, HDMI 1.1 only added support for DVD-Audio to HDMI 1.0.

## 1.2

HDMI 1.2 was released in 2005, and added the option of One Bit Audio at up to 8 channels. For better compatibility with PC devices, version 1.2 also removed the requirement that only explicitly supported formates be used. It added the ability for manufacturers to create vendor-specific formates, allowing any aribtrary resolution and refersh rate rather than being limited to a pre-defined list of supported formats. In addition, it added explicit support for several new formats including 720p at 100 and 120 Hz and relaxed the pixel format support requirements so that sources with only native RGB support would not be required to support Y′CBCR output.

## 1.3

HDMI 1.2 was released in 2006, and increased the maximum TMDS clock to 340 MHz. This allowed for a new maximum video bandwidth of 8.16 Gbit/s which is sufficient for 1080p at 144Hz. It added support for 10 bpc, 12 bpc, and 16 bpc color depth.

1.3a was released later that year and had cable sink modifications for HDMI type C, source termination recommendations, and removed undershoot and maximum rise/fall time limits. 

## 1.4

HDMI 1.4 was released in 2009,. Retaining the bandwidth of the previous version, HDMI 1.4 defined standardization for several new timings. In addition, it aslo added an HDMI Ethernet Channel that accommodates a 100 Mbit/s Ethernet connection two HDMI connected devices so they can share an Internet connection.  Many small features added as well as a new Micro HDMI Connector.

1.4a added mandatory support for two 3D formats for broadcast content, which was deferred with HDMI 1.4 pending the direction of the 3D broadcast market.

1.4b only offered minor clarifications for 1.4a.

## 2.0

HDMI 2.0, released in 2013, is often referred to by some manufacturers as HDMI UHD. 

HDMI 2.0 increases the maximum bandwidth to 18 Gbit/s. This enable HDMI 2.0 to carry 4k video at 60Hz with 24 bit/px color depth. Other added features include support for Rec. 2020 color space, up to 32 audio channels, up to 1536 KHz audio sample frequences, dual video streams to multiple users on the same screen, up to four audio streams, 4:2:0 chroma subsampling, support for 21:9 aspect ratio, dynamic synchronization of video and audio streams.

2.0a was released in 2015, and added support for [[High Dynamic Range (HDR)]] video with static metadata.

2.0b was released in 2016. It added support for HDR Video transport which extends the static metadata signaling to include hybrid log-gamma (HLG).

## 2.1

Announced in 2017, HDMI 2.1 added support for higher resolutions and higher refresh rates, including 4k 120 H and 8K 60 Hz. HDMI 2.1 also introduces a new HDMI cable category called Ultra High Speed, which certifies cables at the new higher speeds that these formats require.Ultra High Speed HDMI cables are backwads compatible with older HDMI devices, and older cables are compatible with new HDMI 2.1 devices, though the full 48 Gbit/s bandwidth is only supported with the new cables.

Due to the HDMI forum preventing its use in open source, HDMI 2.1 implementations are restricted for Linux open source drivers. This leads to users of those systems to needing to use [[DisplayPort]]instead to access high resolutions and speeds.

The increase in maximum bandwidth is achieved by increasing both the bitrate of the data channesls and the number of channels. Previous HDMI versions use three data channels, with and additional channel for the TMDS clock signal, which runs at a fraction of the data channel speed. HDMI 2.1 adopts the approach initially achieved by [[DisplayPort]] which is changing the structure of the data to use a new packet-based formate with an embedded clock signal which unlocks the fourth channel for more data throughput. Combining this advancement with the doubling of the signaling rate to 12 Gbit/s per data channel allows aggregate bandwidth to increase from 18 Gbit/s to 48 Gbit/s. In addition, the data is transmitted more efficiently by using a 16b/18b encoding scheme, which uses a larger percentage of the bandwidth for data rather than DC balancing compared to the TDMS scheme usd by previous versions.

The following features were added to HDMI 2.1 Specification: 

- Maximum supported formate is 10k at 120 Hz
- Dynamic HDR for specifying HDR metadata on a scene-by-scene or even a frame-by-frame basis
- [[Display Stream Compression (DSC)]] 1.2 is used for video formats higher than 8k with 4:2:0 chroma subsampling.
- High frame rate for 4k, 8k, and 10, which adds support for refresh rates up to 120 Hz
- Enhanced Audio Return Channel (eARC) for object-based audio formats such as Dolby Atmos and DTS:X 
- Enhanced refresh rate and laentcy reduction features
- Auto Low Latency Mode (ALLM) 

2.1a, released in 2022, added support for Source-Based Tone Mapping (SBTM).

2.1b, released in 2023, includes general clean up, clarifications to improve interoperability, and incorporation of errata.

## Relationship with DisplayPort and USB-C

When [[DisplayPort]] was introduced in 2006 the HDMI Licensing LLC was publicly dismissive in the market.

DisplayPort has since become a common feature of premium products-displays, desktop computers, and video cards; most of the companies producing DisplayPort equipment are in the computer sector. Connector high-end monitors to GPUs are the most common use case for DisplayPort. The advantages for DisplayPort in these scenarios were added to the HDMI specification later and with the introduction of HDMI 2.1, these gaps are already leveled off.

The [[Universal Serial Bus (USB)#USB 3.1|USB 3.1]] type-C connector is increasingly the standard video connector, replacing legacy video connectors such as mDP, Thunderbolt, HDMI, and VGA in mobile devices. USB-C connectors can transmit DisplayPort video to docks and displays using standard USB type-C cables or type-C to DisplayPort cables and adapters; USB-C also support HDMI adapters that actively convert from DisplayPort to HDMI 1.4 or 2.0.