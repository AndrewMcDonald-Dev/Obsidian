
The OSI Model is a reference model created by the [[International Organization for Standardization (ISO)]] for the intent of creating a common basis for the coordination of standards development for the purpose of systems interconnection.

The model is split into seven abstraction layers that explain the flow of data in a communication systems from the physical implementation of transmitting bits all the way to the highest-level representation of data of a distributed application.

Each layer has a well-defined purpose and interacts directly with layers below and above as appropriate. The [[Internet Protocol Suite (TCP IP)]] is a model developed for a similar goal but instead focuses purely on the software layers of communication instead of the rigorous structure of the OSI model.

The OSI Model has become the most popular model for describing networking between one or more parties, due in large part to its commonly accepted user-friendly framework. OSI has two major components: an abstract model of networking and a set of specific protocols. The reference model was a major advance in the standardization of network concepts. Having a consistent model for protocol layers helped defining interoperability of new devices and protocols added to the market.

The OSI protocol suite that was specified as part of the OSI model was considered by many as too complicated and inefficient, and to a large extent unimplementable. It specified eliminating all existing networking protocols and replacing them at all layers of the stack. Due to this the [[Internet Protocol Suite (TCP IP)]] has become the standard for networking while just the OSI model is referenced.

# Layers

The OSI Model defines a generalized set of rules each layer within the model must follow. To understand these rules two terms must be defined. Protocol data units (PDUs) are used to communicate between two devices at the same layer N by means of a layer N protocol. In each PDU is a payload, called the service data unit (SDU), along with protocol related headers or footers. WIth these two definitions, describing the steps of communicating between two OSI-compatible devices proceeds as follows:

1. The data to be transmitted is composed at the topmost layer of the transmitting device (layer N) into a PDU.
2. The PDU is passed to layer N-1, where it then becomes known as a SDU.
3. At layer N-1 the SDU has a header, a footer, or both added producing a layer N-1 PDU. It is then passed to layer N-2.
4. This process continues until reaching the bottommost layer, from which the data is transmitted to the receiving device.
5. At the receiving device the data is passed from the lowest to the highest layer as a series of SDUs while being successively stripped of each layer's header or footer until reaching the topmost layer, where the last of the data is consumed.

## Physical

The physical layer deals with the transmission and reception of unstructured raw data between a device, such as a [[Network Interface Card (NIC)]], [[Ethernet Hub]], or [[Network Switch]], and a physical transmission medium. It converts digital bits into eletrical, radio, or optical signals. Layer specifications define characteristics such as voltage levels, the timing of voltage changes, physical data rates, maximum transmission distances, modulation scheme, [[Channel Access Method|channel access method]], and physical connectors. 

Bit rate control is done at the physical layer and may define transmission mode as [[simplex]], [[half duplex]], and [[full duplex]]. The components of a physical layer can be described in terms of the [[Physical Topology|physical network topology]]. Physical layer specifications are included in the specifications for the ubiquitous [[Bluetooth]], [[802.3 (Ethernet)|Ethernet]], and [[Universal Serial Bus (USB)|USB]] standards. 

The physical layer also specifies how encoding occurs over a physical signal, such as eletrical voltage or a light pulse. As a result, common problems occurring at the physical layer are often related to the incorrect media termination, EMI or noise scrambling, and [[Network Interface Card (NIC)|NICs]] and [[Ethernet Hub|hubs]] that are misconfigured or do not work correctly.

Some examples of transmission mediums would be [[Twisted Pair]], [[Coaxial Cable]], [[Optical Fiber]], [[802.11 Specification (Wi-Fi)|wireless]], etc. 


## Data Link

The data link layer transfers [[Frame|frames]] from one node on a network to another. It detects and possibly corrects errors that may occur in the physical layer. It defines the protocol to establish and terminate a connection between two physically connected devices. It also defines the protocol for [[OSI Model#Flow Control|flow control]] between them.

The data link layer is divided into two sublayers:

- [[Medium Access Control (MAC)]] layer - responsible for controlling how devices in a network gain access to a medium and permission to transmit data.
- [[Logical Link Control (LLC)]] layer - responsible for identifying and encapsulating network layer protocols, and controls error checking and frame synchronization.

#### Flow Control

In data communications, flow control is the process of managing the rate of data transmission between two nodes to prevent a fast sender from overwhelming a slow receiver. Flow control is different from [[TCP Congestion Control|congestion control]] because congestion control only controls the flow of data when congestion is detected. 

## Network

The network layer provides the functional and procedural means of transferring [[Packet|packets]] from one node to another connected in "different networks". A network is a medium to which many nodes can be connected, on which every node has an address and which permits nodes connected to it to transfer messeags ot other nodes connected to it by merely providing the content of a message and the address of the destination node and letting the network find the way to deliver the message to the destination node, possibly [[Router|routing]] it through intermediate nodes. If the message is too large too be transmitted from one node to another on the data link layer between those nodes, the network may implement message delivery by splitting the message into several fragments at one node, sending the fragments independently, and reassembling the fragments at another node. It may, bud does not need to, report delivery errors.

The most common protocols used belonging to this layer are [[Internet Protocol Suite (TCP IP)#Internet Protocol (IP)|Internet Protocol (IP)]], [[Internet Protocol Suite (TCP IP)#Internet Control Message Protocol (ICMP)|Internet Control Protocol (ICMP)]], [[Internet Protocol Suite (TCP IP)#Internet Group Management Protocol (IGMP)|Internet Group Management Protocol (IGMP)]], and [[Internet Protocol Suite (TCP IP)#Internet Protocol Security (IPsec)|Internet Protocol Security (IPsed)]].

## Transport

The transport layer provides the functional and procedural means of transferring variable-length data sequences from a source host to a destination host from one application to another across a network, while maintaining the [[Quality of Service (QoS)]] functions. Transport protocols may be connection-oriented or connectionless.

This may require breaking large protocol data units or long data streams into smaller chunks called [[Segment|segments]].

The transport layer also controls the reliability of a given link between a source and destination host through [[OSI Model#Flow Control|flow control]], error control, and acknowledgments of sequence and existence. Some protocols are connection-oriented. This means that the transport layer can keep track of the segments and retransmit those that fail delivery through the acknowledgment hand-shake system. The transport layer will also provide the acknowledgement of the successful data transmission and sends the next data if no errors occurred. An example of this would be [[Internet Protocol Suite (TCP IP)#Transmission Control Protocol (TCP)|TCP]].

Reliability, however, is not a strict requirement within the transport layer. Protocols like [[Internet Protocol Suite (TCP IP)#User Datagram Model (UDP)|UDP]], for example, are used in applications that are willing to accept some packet loss, reordering, errors or duplication. Streaming media, real-time multiplayer games and [[Voice over Internet Protocol (VoIP)]] are examples of applications in which loss of packets is not usually a fatal problem.

[[Transport Layer Security (TLS)]] sorta fits in this layer but more so in the presentation layers.

## Session

The session layer creates the setup, controls the connections, and ends the teardown, between two or more computers, which is called a session. Common functions of the sessino layer include user logon and user logoff functions. Including this matter, authentication methods are also built into most client software, such as [[File Transfer Protocol (FTP)]] clients. Therefore, the session layer establishes, manages and terminates the connections between the local and remote applications. Therefore, the session layer is commonly implemented explicitly in application environments that use [[Remote Procedure Calls (RPC)]].

## Presentation

The presentation layer handles protocol conversion, data encryption, data decryption, data compression, data decompression, data formatting, and graphic commands. The presentation layer transforms data into the form that the application layer accepts, to be sent across the network. The presentation layer supports the [[Transport Layer Security (TLS)]] protocol.

## Application

The application layer is the layer of the OSI model that is closest to the end user, which means both the OSI application layer and the user interact directly with a software application that implements a component of communication between the client and serves, such as web browsers and email programs. Some applications fall outside the OSI model if they do not directly integrate the functions of communication.

Application layer functions typically include file sharing, message handling, and database access, through the most common protocols at the application layer, known as [[Hypertext Transfer Protocol (HTTP)|HTTP]], [[File Transfer Protocol (FTP)|FTP]], [[Server Message Block (SMB)|SMB]]/[[Server Message Block (SMB)#Common Internet File System (CIFS)|CIFS]], and [[Simple Mail Transfer Protocol (SMTP)|SMTP]]. When identifying communication partners, the application layer determines the identity and availability of communication partners for an application with data to transmit. The most important distinction in the application layer is the distinction between the application-entity and the application. For example, a reservation website might have two application-entities: one using [[Hypertext Transfer Protocol (HTTP)|HTTP]] to communicate with its users, and one for a remote database protocol to record reservations. Neither of these protocols have anything to do with reservations. That logic is in the application itself. The application layer has no means to determine the availability of resources in the network. 