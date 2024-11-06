VNC is a graphical desktop-sharing system that uses the [[Virtual Cloud Network (VNC)#Remote Frame Buffer protocol (RFB)]] to remotely control another computer. It transmits the keyboard and mouse input from one computer to another, relaying the graphical-screen updates, over a network.

Multiple clients may connectd to a VNC server at the same time.  VNC mab be tunneled over [[Secure Shell (SSH)]] or [[Virtual Private Network (VPN)]] connectiond which would add an extra security layer with stronger encryption.

## Remote Frame Buffer protocol (RFB)
By default, RFB is not a secure protocol. While passwords are not send in plain-text (as in [[Telnet]]), cracking could prove successful if both the encryption key and encoded password were sniffed from a network. For this reason it is recommended that a password of at least 8 characters be used.

UltraVNC support the use of an open-source encryption plugin which encrypts the entire VNC session including password authentication and data transfer. There are [[Advanced Encryption Standard (AES)]] encryption patches for VNC.