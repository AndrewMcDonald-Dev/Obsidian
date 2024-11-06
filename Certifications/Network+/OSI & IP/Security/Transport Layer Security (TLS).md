TLS is a cryptographic protocol designed to provide communications security over a computer network, such as the Internet. The protocol is widely used in applications such as email, instant messaging, and [[Voice over Internet Protocol (VoIP)]], but its use in securing [[Hypertext Transfer Protocol (HTTP)#HTTPS|HTTPS]] remains the most publicly visible.

THe TLS protocol aims primarily to provide security, including privacy, integrity, and authenticity through the use of cryptography, such as the use of certificates, between two or more communicating computer applications. It runs in the [[OSI Model#Presentation|Presentation Layer]] and is itself composed of two layers: the TLS record and the TLS handshake protocols.
## TLS Record
The TLS Record Protocol secures application data using the keys created during the Handshake. The Record Protocol is responsible for securing application data and verifying its integrity and origin. It manages the following:
- Dividing outgoing messages into manageable blocks, and reassembling incoming messages. 
- Compressing outgoing blocks and decompressing incoming blocks (optional)
- Applying [[Message Authentication Code (MAC)]] to outgoing messages, and verifying incoming messages using the MAC
- Encrypting outgoing messages and decrypting incoming messages.
When the Record Protocol is complete, the outgoing encrypted data is passed down to the [[Internet Protocol Suite (TCP IP)#Transmission Control Protocol (TCP)|Transmission Control Protocol (TCP)]] layer for transport.
## TLS Handshake Protcols
The TLS Handshake protocol is a process that establishes a secure communication channel between a client and a server. It involves several steps, including the client sending a "ClientHello" message to propose supported TLS versions and cipher suites, the server responding with a "ServerHello" message to confirm the chosen settings, and the exchange of digital certificates for authentication, utimately leading to the generation of session keys for encrypted communication.