The SSH protocol is a cryptographic network protocol for operating network services securely over an unsecured network. SSH applications are based on a client-server architecture, connected an SSH client instance with an SSH server. Its most notable applications are remote login and command-line execution. SSH was designed for Unix-like operating systems as a replacement for [[Telnet]] and unsecured remote Unix shell protocols, such as the Berkeley Remote Shell (rsh).

Since mechanisms like Telnet and Remote Shell are designed to access and operate remote computers, sending the authentication tokens for this access to these computers across a public network in an unsecured way, poses a great risk of 3rd parties obtaining the password and achieving the same level of access to the remote system as the telnet user. Secure Shell mitigates this rick through the use of encryption mechanisms that are intended to hide the contents of the transmission form an observer, even if the observer has access ot the entire data stream.

The protocol specification distinguishes two major versions, referred to as SSH-1 and SSH-2. The most commonly implemented software stack is OpenSSH, released in 1999 as open-source software by the OpenBSD developers.

UDP port 22 and TCP port 22 are designated for SSH Servers.

## OpenSSH key management
On Unix-like systems, the list of authorized public keys is typically stored in the home directory of the user that is allowed to log in remotely, in the file `~/.ssh/authorized_keys`.

## Use
SSH is typically used to log into a remote computer's shell or [[Command-line Interface (CLI)]] and to execute commands on a remote server. It also supports mechanisms for tunneling, forwarding of TCP ports and X11 connections and it can be used to transfer files using the associated [[File Transfer Protocol (FTP)#SFTP|SSH File Tranfer Protocol (SFTP)]] or [[Secure Copy Protocol (SCP)]].