
ATT&CK is a guideline for classifying and describing cyberattacks and intrusions. Rather than looking at the results of an attack, it identifies tactics that indicate an attack is in progress.

### ATT&CK Matrix for Enterprise

|Category|Description|Techniques|
|---|---|---|
|[Reconnaissance](https://en.wikipedia.org/wiki/Reconnaissance "Reconnaissance")|Gathering information about a target.|10|
|Resource Development|Identifying and acquiring resources for the attack.|8|
|Initial Access|Gaining initial access to a system or network.|10|
|[Execution](https://en.wikipedia.org/wiki/Execution_(computing) "Execution (computing)")|Running malicious code on a system.|14|
|[Persistence](https://en.wikipedia.org/wiki/Persistence_(computer_science) "Persistence (computer science)")|Maintaining access to a system or network.|20|
|[Privilege Escalation](https://en.wikipedia.org/wiki/Privilege_escalation "Privilege escalation")|Obtaining elevated privileges within a system or network.|14|
|[Defense Evasion](https://en.wikipedia.org/wiki/Evasion_(network_security) "Evasion (network security)")|Disabling or evading security measures.|43|
|Credential Access|Obtaining credentials to access systems or data.|17|
|Discovery|Identifying additional systems or information within a network.|32|
|[Lateral Movement](https://en.wikipedia.org/wiki/Network_Lateral_Movement "Network Lateral Movement")|Moving laterally within a compromised network.|9|
|Collection|Collecting data from compromised systems.|10|
|[Command and Control](https://en.wikipedia.org/wiki/Botnet#Command_and_control "Botnet")|Establishing communication with compromised systems.|17|
|[Exfiltration](https://en.wikipedia.org/wiki/Data_theft "Data theft")|Transferring stolen data from a compromised system.|9|
|Impact|Taking actions to achieve the attacker's objectives.|14|

### Reconnaissance

The initial stage of information gathering for an eventual cyberattack is called Reconnaissance.

|MITRE ID|Techniques|Summary|
|---|---|---|
|T1595|[Active Scanning](https://en.wikipedia.org/wiki/Port_scanning "Port scanning")|Active reconnaissance by scanning the target network using a port scanning tool such as [Nmap](https://en.wikipedia.org/wiki/Nmap "Nmap"), [vulnerability scanning](https://en.wikipedia.org/wiki/Vulnerability_scanner "Vulnerability scanner") tools and wordlist scanning for common file extensions and software used by the victim.|
|T1598|[Phishing for Information](https://en.wikipedia.org/wiki/Phishing "Phishing")|Using social engineering techniques to [elicit](https://en.wikipedia.org/wiki/Elicitation_technique "Elicitation technique") useful information from the target. Using a communication channel such as e-mail, including generic phishing and targeted spearphishing which has been specifically created to target an individual victim|
|T1592|Gather Victim Host Information|Discover the configuration of specific endpoints such as their [hardware](https://en.wikipedia.org/wiki/Computer_hardware "Computer hardware"), [software](https://en.wikipedia.org/wiki/Software "Software") and administrative configuration (such as [Active Directory](https://en.wikipedia.org/wiki/Active_Directory "Active Directory") domain membership). Especially security protections such as [antivirus](https://en.wikipedia.org/wiki/Antivirus "Antivirus") and locks ([biometric](https://en.wikipedia.org/wiki/Biometric "Biometric"), [smart card](https://en.wikipedia.org/wiki/Smart_card "Smart card") or even a [Kensington K-Slot](https://en.wikipedia.org/wiki/Kensington_lock "Kensington lock")).|
|T1590|Gather Victim Network Information|Discover the target network's configuration such as the [network topology](https://en.wikipedia.org/wiki/Network_topology "Network topology"), security appliances ([network firewall](https://en.wikipedia.org/wiki/Firewall_(computing) "Firewall (computing)"), [VPN](https://en.wikipedia.org/wiki/VPN "VPN")), [IP address](https://en.wikipedia.org/wiki/IP_address "IP address") ranges (either IPv4, IPv6 or both), [fully qualified domain names (FQDN)](https://en.wikipedia.org/wiki/Fully_qualified_domain_name "Fully qualified domain name") and the [Domain Name System (DNS)](https://en.wikipedia.org/wiki/Domain_Name_System "Domain Name System") configuration.|
