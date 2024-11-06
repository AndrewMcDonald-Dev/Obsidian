Virtual Private Network (VPN) is a network architecture for virtually extending a private network across one or multiple other networks which are either untrusted or need to be isolated. 

A VPN can extend access to a private network to users who do not have direct access to it, such as an office network allowing secure access from off-site over the Internet. This is achieved by creating a link between computing devices and computer networks by the use of network tunneling protocols.

It is possible to make a VPN secure to use on top of insecure communication medium by choosing a tunneling protocol that implements encryption. This kind of VPN implementation has the benefit of reduced costs and greater flexibility, with respect to dedicated communication lines, for remote workers.
## Dynamic Multipoint VPN (DMVPN)
A dynamic multipoint virtual private network (DMVPN) is a secure network that exchanges data between sites/routers withotu passing traffic thorugh an organization's VPN server or router, located at its headquarters. A DMVPN allows organizations to build a VPN network with multiple sites, without hte need to configure devices statically.

## VPN Concentrator
VPN Concentrator, also known as a VPN Headend, is a networking device that enables multiple VPN tunnels to use a single network. It receives incoming data, de-encapsulting and decrypting the data. It encapsulates the outgoing network data into encrypted packets and then transmits the data through the VPN tunnel.
## Clientless VPN
A clientless VPN allows users to connect to a VPN service without requiring the installation of any dedicated VPN software on their devices. Instead, users can access the VPN through a standard web browser. This method typically uses [[Secure Sockets Layer (SSL)]] or [[Transport Layer Security (TLS)]] protocols to establish a secure, encrypted connection. They are generally limited to HTTP/HTTPS traffic which means they are best suited to accessing web applications.
## Client-to-Site VPN (Remote Access)
A Client-to-Site configuration is analogous to joining one or more computers to a network which cannot be directly connected. This type of extension provides that computer access to [[Local Area Network (LAN)]]  of a remote site, or any wider entreprise networks, such as an internet.
## Site-to-Site VPN
A Site-to-Site configuration connects two networks. This configuratoin expands a network across georaphically disparate locations. Tunneling is only done between two devices (like routers, firewalls, VPN gateways, servers, etc.) located at both network locations. These devices then make the tunnel available to other local network hosts that aim to reach any host on the other side.
## Client-to-Client VPN
A Client-to-Client configuration connects two computers together.