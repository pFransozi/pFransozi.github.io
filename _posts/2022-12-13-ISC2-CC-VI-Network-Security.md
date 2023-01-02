---
layout: post
title:  ISC² CC VI Network Security
date:   2022-12-13 10:00:00
description: analysis of fourth chapter of the self-paced “Certified in Cybersecurity” training, focusing on Network Security.
tags: ['network-concepts', 'network-cyber-threats-attacks', 'network-sec-infra']
category: ['isc²-cc-content']
---
## Introduction

Network security is the fourth part of ISC² CC. This is a sequence of a story started [in this link]({% post_url 2022-09-21-ISC2-CC-I-Foundational %}){:target="_blank"}. It's an entry-level content about network.

## Understand Computer Network

For secure data communications, it requires to explore all of the technologies involved in computer communications. Some of them are:

* **Local area network (LAN)** and **Wide area network (WAN)** are two basic types of networks. LAN is commonly limited geographical area, spanning a single floor or buinding; WAN is a long-distance connection between geographically remote networks:
* **Network devices** are the backbone of networks, between them:
  * **switch** is the next generation of an old technology called hub. Switch is known as intelligent hub because hub connects multiples devices in way that it sends messages to all devices connected in it. Consider ten computes connected in a hub, of C1 sends a message to C8, all ten computers will receive the message. That way to send message to all in a network is known as **broadcast**. A switch does not work in broadcast mode by default, rather than the message is sent to a specific receiver, for switch knows the addresses of the devices connected and then it can route traffic to a specific port, which is equivalent to the device.  Offering greater efficiency for traffic delivery and improving the overall throughput of data, switches are smarter than hubs, but not as smart as routers. Switches can also create separate broadcast domains when used to create VLANs, which will be discussed later;
  * **router** is used to control traffic flow on networks and are often used to connect similar networks and control traffic flow between them. Smarter than hubs and switches, routers determine the most efficient “route” for the traffic to flow across the network;
  * **server** is a computer that provides information to other computers on a network;
  * **endpoint** is the end of a network communication link, which is often a server where a resource resides, and the other end is often a client making a request to use a network resource.
  * **firewall** is a essential tool in managing and controlling network traffic and protecting the network, which is used to filter traffic and it is typically deployed between a private network and the internet;
  * **wifi** is wireless networking;
* **Protocols and Addresses** are as important as network devices:
  * **ethernet (ieee 802.3)** is a standard that defines wired connections of networked devices. This standard defines the way data is formatted over the wire to ensure disparate devices can communicate over the same cables.
  * **media access control (mac) address** is assigned to every network device and it is a sequence of 48 bits, that is represented as 00-13-02-1F-58-F5 hexadecimal number. The first 3 bytes (24 bits) of the address denote the vendor or manufacturer of the physical network interface. No two devices can have the same MAC address in the same local network; otherwise an address conflict occurs.
  * **internet protocol and (ip) address** associates that physical address with a unique logical address. This logical IP address represents the network interface within the network and can be useful to maintain communications when a physical device is swapped with new hardware. Examples are 192.168.1.1 and 2001:db8::ffff:0:1.
* **Networking Models** are architectures and standards exist that provide ways to interconnect different hardware and software systems with each other for the purposes of sharing information, coordinating their activities and accomplishing joint or shared tasks. In general, those models follow **some goals in therms of security**, such as:
  * provide reliable, managed communications between hosts (and users);
  * isolate functions in layers;
  * use packets as the basis of communication;
  * standardize routing, addressing and control;
  * allow layers beyond internetworking to add functionality;
  * be vendor-agnostic, scalable and resilient;
In the most basic form, a network model has two layers:
  * **upper layer**, which is also known as the application layer and it is responsible for managing the integrity of a connection and for controlling the session as well as establishing, maintaining and terminating communication sessions between two computers. It is also responsible for transforming data received from the Application Layer into a format that any system can understand. And finally, it allows applications to communicate and determines whether a remote communication partner is available and accessible.
  * **lower layer**, which is often referred to as the media or transport layer and is responsible for receiving bits from the physical connection medium and converting them into a frame. Frames are grouped into standardized sizes. Think of frames as a bucket and the bits as water. If the buckets are sized similarly and the water is contained within the buckets, the data can be transported in a controlled manner. Route data is added to the frames of data to create packets. In other words, a destination address is added to the bucket. Once we have the buckets sorted and ready to go, the host layer takes over.

### OSI Model

It was developed to establish a common way to describe the communication structure for interconnected computer systems, and it serves as an abstract framework for how protocols should function in an ideal world on ideal hardware.

It divides networking tasks into seven distinct layers, and each layer is responsible for performing specific tasks or operations with the goal of supporting data exchange between two computers. The layers are ordered specifically to indicate how information flows through the various levels of communication. Each layer communicates directly with the layer above and the layer below it.

Encapsulation occurs as the data moves down the OSI model from Application (7) to Physical (1). As data is encapsulated at each descending layer, the previous layer’s header, payload and footer are all treated as the next layer’s payload. The data unit size increases as we move down the conceptual model and the contents continue to encapsulate.  

The inverse action occurs as data moves up the OSI model layers from Physical to Application. This process is known as de-encapsulation  (or decapsulation). The header and footer are used to properly interpret the data payload and are then discarded. As we move up the OSI model, the data unit becomes smaller. The encapsulation/de-encapsulation process is best depicted visually below: 

|------------|-------------|--------------|-------------------------------|----------------|
| 7          | Application |              |        DATA                     |                |
| 6          | Presentation| Header -->   |      \|..\| DATA                |                |
| 5          | Session     |              |     \|..\|..\|DATA              |                |
| 4          | Transport   |              |       \|..\|..\|..\|DATA           |                |
| 3          | Network     |              |   \|..\|..\|..\|..\|DATA        |                |
| 2          | Data Link   |              |  \|..\|..\|..\|..\|..\|DATA\|..\|         |   <--Footer   |
| 1          | Physical    |              | \|..\|..\|..\|..\|..\|..\|DATA\|..\|..\|    |                |

### Transmission Control Protocol/Internet Protocol (TCP/IP)

TCP/IP is the most widely used protocol today, and its stack is focuses on the core functions of networking.

||TCP/IP Protocol Architecture Layers| |
|-|-----------------------------------|-|
|Application Layer |Defines the protocols for the transport layer||
|Transport Layer   |Permits data to move among devices||
|Internet Layer    |Creates/inserts packets||
|Network Interface Layer 	|How data moves through the network||

The most widely used protocol suite is TCP/IP, but it is not just a single protocol; rather, it is a protocol stack comprising dozens of individual protocols. TCP/IP is a platform-independent protocol based on open standards. However, this is both a benefit and a drawback. TCP/IP can be found in just about every available operating system, but it consumes a significant amount of resources and is relatively easy to hack into because it was designed for ease of use rather than for security. 

At the Application Layer, TCP/IP protocols include **Telnet**, File Transfer Protocol (**FTP**), Simple Mail Transport Protocol (**SMTP**), and Domain Name Service (**DNS**). The two primary Transport Layer protocols of TCP/IP are **TCP and UDP**. **TCP is a full-duplex connection-oriented protocol, whereas UDP is a simplex connectionless protocol**. In the Internet Layer, **Internet Control Message Protocol (ICMP)** is used to determine the health of a network or a specific link. **ICMP is utilized by ping, traceroute and other network management tools**. The ping utility employs ICMP echo packets and bounces them off remote systems. Thus, you can use ping to determine whether the remote system is online, whether the remote system is responding promptly, whether the intermediary systems are supporting communications, and the level of performance efficiency at which the intermediary systems are communicating.

* Application, Presentation and Session layers at OSI model is equivalent to Application Layer at TCP/IP, and the protocol suite is: FTP, Telnet, SNMP, LPD, TFPT, SMTP, NFS, X Window.
* Transport layer are the same between OSI model and TCP/IP model, protocol suite: TCP, UDP
* Network layer at OSI model is equivalent to Internet layer at TCP/IP model, and protocol suite is: IGMP, IP, ICMP
* Data link and Physical layer at OSI model is equivalent at Network Interface layer at TCP/IP, and protocol suite is: Ethernet, Fast Ethernet, Token Ring, FDDI

### Internet Protocol (IPv4 and IPv6)

IPv4 provides a 32-bit address space. IPv6 provides a 128-bit address space. The first one is exhausted nowadays, but it is still used because of the NAT technology. 32 bits means 4 octets of 8 bits, which is represented in a dotted decimal notation such as 192.168.0.1, which means in binary notation 11000000 10101000 00000000 00000001

IP hosts/devices associate an address with a unique logical address. An IPv4 address is expressed as four octets separated by a dot (.), for example, 216.12.146.140. Each octet may have a value between 0 and 255. However, **0 is the network itself (not a device on that network), and 255 is generally reserved for broadcast purposes**. Each address is subdivided into two parts: **the network number and the host**. The network number assigned by an external organization, such as the Internet Corporation for Assigned Names and Numbers (ICANN), represents the organization’s network. The host represents the network interface within the network.  

**To ease network administration, networks are typically divided into subnets**. Because subnets cannot be distinguished with the addressing scheme discussed so far, a separate mechanism, **the subnet mask**, is used to define the part of the address used for the subnet. The mask is usually converted to decimal notation like 255.255.255.0. **With the ever-increasing number of computers and networked devices, it is clear that IPv4 does not provide enough addresses for our needs.** To overcome this shortcoming, **IPv4 was sub-divided into public and private address ranges.** Public addresses are limited with IPv4, but this issue was addressed in part with private addressing. Private addresses can be shared by anyone, and it is highly likely that everyone on your street is using the same address scheme.  

The nature of the addressing scheme established by IPv4 meant that network designers had to start thinking in terms of IP address reuse. IPv4 facilitated this in several ways, such as its creation of the private address groups; this allows every LAN in every SOHO (small office, home office) situation to use addresses such as 192.168.2.xxx for its internal network addresses, without fear that some other system can intercept traffic on their LAN. This table shows the private addresses available for anyone to use:

| RANGE |
|-------|
|10.0.0.0 to 10.255.255.254|
|172.16.0.0 to 172.31.255.254|
|192.168.0.0 to 192.168.255.254|

The first octet of **127 is reserved for a computer’s loopback address**. Usually, the address 127.0.0.1 is used. **The loopback address is used to provide a mechanism for self-diagnosis and troubleshooting at the machine level**. This mechanism allows a network administrator to treat a local machine as if it were a remote machine and ping the network interface to establish whether it is operational.

IPv6 is a modernization of IPv4, which addressed a number of weaknesses in the IPv4 environment:

* A much larger address field: IPv6 addresses are **128 bits**, which supports 2128 or 340,282,366,920,938,463,463,374,607,431,768,211,456 hosts. **This ensures that we will not run out of addresses**.
* Improved security:** IPsec is an optional part of IPv4 networks, but a mandatory component of IPv6 networks**. This will help ensure the integrity and confidentiality of IP packets and allow communicating partners **to authenticate with each other**.
* Improved quality of service (QoS): This will help services obtain an appropriate share of a network’s bandwidth.

An IPv6 address is shown as **8 groups of four digits**. Instead of numeric (0-9) digits like IPv4, **IPv6 addresses use the hexadecimal range (0000-ffff) and are separated by colons (:)** rather than periods (.). An example IPv6 address is **2001:0db8:0000:0000:0000:ffff:0000:0001**. To make it easier for humans to read and type, it can belower layer shortened by removing the leading zeros at the beginning of each field and substituting two colons (::) for the longest consecutive zero fields. All fields must retain at least one digit. After shortening, the example address above is rendered as 2001:db8::ffff:0:1, which is much easier to type. As in IPv4, there are some addresses and ranges that are reserved for special uses:

* ::1 is the local loopback address, used the same as 127.0.0.1 in IPv4.
* The range 2001:db8:: to 2001:db8:ffff:ffff:ffff:ffff:ffff:ffff is reserved for documentation use, just like in the examples above.
* **fc00**:: to **fdff**:ffff:ffff:ffff:ffff:ffff:ffff:ffff are addresses reserved for internal network use and are not routable on the internet.

### Ports and Protocols (Applications/Services)

* **Physical ports** are the ports on the routers, switches, servers, computers, etc. that you connect the wires, e.g., fiber optic cables, Cat5 cables, etc., to create a network.
* **Logical Ports** are used when a connection is established between two systems. A logical port (**also called a socket**) is little more than an address number that both ends of the communication link agree to use when transferring data. Ports allow a single IP address to be able to support multiple simultaneous communications, each using a different port number. In the Application Layer of the TCP/IP model (which includes the Session, Presentation, and Application Layers of the OSI model) reside numerous application- or service-specific protocols. Data types are mapped using port numbers associated with services. For example, web traffic (or HTTP) is port 80. Secure web traffic (or HTTPS) is port 443. Table 5.4 highlights some of these protocols and their customary or assigned ports. You’ll note that in several cases a service (or protocol) may have two ports assigned, one secure and one insecure. When in doubt, systems should be implemented using the most secure version as possible of a protocol and its services.
    * **well-known ports (0–1023)** are related to the common protocols that are at the core of the Transport Control Protocol/Internet Protocol (TCP/IP) model, Domain Name Service (DNS), Simple Mail Transfer Protocol (SMTP), etc.
    * **registered ports (1024–49151)** are often associated with proprietary applications from vendors and developers. While they are officially approved by the Internet Assigned Numbers Authority (IANA), in practice many vendors simply implement a port of their choosing. Examples include Remote Authentication Dial-In User Service (RADIUS) authentication (1812), Microsoft SQL Server (1433/1434) and the Docker REST API (2375/2376).
    * **dynamic or private ports (49152–65535)**, whenever a service is requested that is associated with well-known or registered ports, those services will respond with a dynamic port that is used for that session and then released.

### Secure Ports

Some network protocols transmit information in clear text, meaning it is not encrypted and should not be used. Clear text information is subject to network sniffing. This tactic uses software to inspect packets of data as they travel across the network and extract text such as usernames and passwords. Network sniffing could also reveal the content of documents and other files if they are sent via insecure protocols. The table below shows some of the insecure protocols along with recommended secure alternatives.

| Insecure Port | Description | Protocol | Secure Alternative Port | 
|---------------|-------------|----------|-------------------------|
| 21            | File Transfer Protocol (FTP) sends the username and password **using plaintext from the client to the server**. This could be intercepted by an attacker and later used to retrieve confidential information from the server. **The secure alternative, SFTP, on port 22 uses encryption to protect the user credentials and packets of data being transferred** | File Transfer Protocol 	|22* - SFTP (Secure File Transfer Protocol)|
| 23            | Telnet, is used by many Linux systems and any other systems **as a basic text-based terminal**. All information to and from the host on a telnet connection is sent in plaintext and **can be intercepted by an attacker**. This includes username and password as well as all information that is being presented on the screen, since this interface is all text. **Secure Shell (SSH) on port 22 uses encryption to ensure that traffic between the host and terminal is not sent in a plaintext format**| Telnet | 22* - SSH (Secure Shell)|
| 25            | Simple Mail Transfer Protocol (SMTP) is the default unencrypted port for sending email messages. Since it is unencrypted, data contained within the emails could be discovered by network sniffing. The secure alternative is to use port 587 for SMTP using Transport Layer Security (TLS) which will encrypt the data between the mail client and the mail server| Simple Mail Transfer Protocol | 587 - SMTP (SMTP with TLS)|
| 37           | Time Protocol, may be in use by legacy equipment and has mostly been replaced by using port 123 for Network Time Protocol (NTP). NTP on port 123 offers better error-handling capabilities, which reduces the likelihood of unexpected errors | Time Protocol | 123 - NTP (Network Time Protocol)|
| 53           | Domain Name Service (DNS), is still used widely. However, using DNS over TLS (DoT) on port 853 protects DNS information from being modified in transit | Domain Name Service | 853 - DoT (DNS over TLS (DoT))|
| 80           | HyperText Transfer Protocol (HTTP) is the basis of nearly all web browser traffic on the internet. Information sent via HTTP is not encrypted and is susceptible to sniffing attacks. HTTPS using TLS encryption is preferred, as it protects the data in transit between the server and the browser. Note that this is often notated as SSL/TLS. Secure Sockets Layer (SSL) has been compromised is no longer considered secure. It is now recommended for web servers and clients to use Transport Layer Security (TLS) 1.3 or higher for the best protection | HyperText Transfer Protocol | 443 - HTTPS (HyperText Transfer Protocol (SSL/TLS))|
| 143          | Internet Message Access Protocol (IMAP) is a protocol used for retrieving emails. IMAP traffic on port 143 is not encrypted and susceptible to network sniffing. The secure alternative is to use port 993 for IMAP, which adds SSL/TLS security to encrypt the data between the mail client and the mail server | Internet Message Access Protocol | 993 - IMAP (IMAP for SSL/TLS)|
| 161/162      | Simple Network Management Protocol, are commonly used to send and receive data used for managing infrastructure devices. Because sensitive information is often included in these messages, it is recommended to use SNMP version 2 or 3 (abbreviated SNMPv2 or SNMPv3) to include encryption and additional security features. Unlike many others discussed here, all versions of SNMP use the same ports, so there is not a definitive secure and insecure pairing. Additional context will be needed to determine if information on ports 161 and 162 is secured or not | Simple Network Management Protocol | 161/162 - SNMP ( SNMPv3)|
| 445          | Server Message Block (SMB), is used by many versions of Windows for accessing files over the network. Files are transmitted unencrypted, and many vulnerabilities are well-known. Therefore, it is recommended that traffic on port 445 should not be allowed to pass through a firewall at the network perimeter. A more secure alternative is port 2049, Network File System (NFS). Although NFS can use encryption, it is recommended that NFS not be allowed through firewalls either | Server Message Block | 2049 - NFS (Network File System)|
| 389          | Lightweight Directory Access Protocol (LDAP), is used to communicate directory information from servers to clients. This can be an address book for email or usernames for logins. The LDAP protocol also allows records in the directory to be updated, introducing additional risk. Since LDAP is not encrypted, it is susceptible to sniffing and manipulation attacks. Lightweight Directory Access Protocol Secure (LDAPS) adds SSL/TLS security to protect the information while it is in transit | Lightweight Directory Access Protocol | 636 - LDAPS (Lightweight Directory Access Protocol Secure)|

## Understand Network (Cyber) Threats and Attacks

### types of threats

* **Spoofing** is an attack with the goal of **gaining access to a target system through the use of a falsified identity**.
* **Phishing** is an attack that **attempts to misdirect legitimate users to malicious websites through** the abuse of **URLs or hyperlinks in emails could be considered phishing**.
* **DoS/DDoS** is a network resource consumption attack that has the **primary goal of preventing legitimate activity on a victimized system**. Attacks involving numerous unsuspecting secondary victim systems are known as distributed denial-of-service (DDoS) attacks.
* **Virus** is perhaps the earliest form of malicious code to plague security administrators. As with biological viruses, **computer viruses have two main functions—propagation and destruction**. A virus is a **self-replicating** piece of code that spreads without the consent of a user, but frequently with their assistance (a user has to click on a link or open a file).
* **Worm** poses a significant **risk to network security**. They contain the same destructive potential as other malicious code objects with an added twist—they propagate themselves without requiring any human intervention.
* **Trojan** is a software program **that appears benevolent but carries a malicious**, behind-the-scenes payload that has the potential to wreak havoc on a system or network. For example, ransomware often uses a Trojan to infect a target machine and then uses encryption technology to encrypt documents, spreadsheets and other files stored on the system with a key known only to the malware creator.
* **On-path attack**, attackers place themselves between two devices, often between a web browser and a web server, to intercept or modify information that is intended for one or both of the endpoints. **On-path attacks** are also known as **man-in-the-middle (MITM) attacks**.
* **Side-channel** is a **passive**, **noninvasive attack** to **observe the operation of a device**. Methods include power monitoring, timing and fault analysis attacks.
* **Advanced Persistent Threat** refers to **threats that demonstrate an unusually high level of technical and operational sophistication spanning months or even years**. APT attacks are often conducted by highly organized groups of attackers.
* **Insider Threat** arises from individuals who are trusted by the organization. These could be disgruntled employees or employees involved in espionage. Insider threats are not always willing participants. A trusted user who falls victim to a scam could be an unwilling insider threat.
* **Malware** is inserted into a system, usually covertly, **with the intent of compromising the confidentiality, integrity or availability of the victim’s data**, applications or operating system or otherwise annoying or disrupting the victim.
* **Ransomware** is used for the purpose of facilitating a ransom attack. Ransomware attacks often use cryptography to “lock” the files on an affected computer and require the payment of a ransom fee in return for the “unlock” code.

### Identify Threats and Tools Used to Prevent Them Continued

* If a system doesn’t need a service or protocol, it should not be running. Attackers cannot exploit a vulnerability in a service or protocol that isn’t running on a system. 
* Firewalls can prevent many different types of attacks. Network-based firewalls protect entire networks, and host-based firewalls protect individual systems.
* Instrusion Detection System (IDS) is a form of monitoring to detect abnormal activity; it detects intrusion attempts and system failures. Identifies Threats, Do not prevent threats
* Host-based IDS (HIDS) monitors activity on a single computer. Identifies threats, Do not prevent Threats.
* Network-based IDS (NIDS) monitors and evaluates network activity to detect attacks or event anomalies. Identifies threats, Do not prevent Threats.
* SIEM gathers log data from sources across an enterprise to understand security concerns and apportion resources. Identifies threats, Do not prevent Threats.
* Anti-malware/Antivirus seeks to identify malicious software or processes. Identifies and Prevent threats.
* Scans evaluates the effectiveness of security controls. Identifies threats, Do not prevent Threats.
* Firewall filters network traffic - managers and controls network traffic and protects the network. Identifies and Prevent threats.
* Intrusion Protection System (IPS-NIPS/HIPS) is an active IDS automatically attempts to detect and block attacks before they reach target systems. Identifies and Prevent threats.

## Understand Network Security Infrastructure

Let's take a look at some strategies and technologies that are related to network security infrastructure:

* **Redundancy** is a design based on the duplication of components in a way that if a failure were to occur, there would be a backup;
* **Defense in depth** is a design based on layered approach, and it must include layers from policies, procedures and awareness to technologies that protect data, such as encryption, data leak prevention, identity and access management and data controls. In the level of network, it uses multiple types of access controls in literal or theoretical layers to help an organization to avoid a monolithic security stance.
* **Zero Trust** is often related to microsegmented network in such way that encapsulates information assets, services that apply to them and their security properties. The basic concept related to zero trust is that even the most robust access control system has its weakness, for that trust must always be verified, rather than only a perimeter defense, it adds defenses at user, asset and data level. In the extreme, **it insists that every process or action a user attempts to take must be authenticated and authorized**; **the window of trust becomes vanishingly small**.
* **Microsegmentation** aids in protecting against polymorphic adversaries in such a sense that they take advantage of traditional security models to move easily between systems within a network. Microsegmentation allows isolate workload of a network in order to limit the effect of malicious lateral movement. For that, it involves controlling traffic among networked devices.
* **DMZ** stands for Demilitarized Zone is a common practice in security architecture, which aims to separate systems that are accessible through firewall from the internal network. Application DMZs (or semi-trusted networks) are frequently used today to limit access to application servers to those networks or systems that have a legitimate need to connect.
* **VLAN** stands for Virtual Local Are Network and it allows network administrators to use switches to create software-based LAN segments, which can segregate or consolidate traffic across multiple switch ports. Devices that share a VLAN communicate through switches as if they were on the same Layer 2 network.
* **VPN** stands for **Virtual Private Network** and it's a point-to-point connection between two hosts that allows them to communicate, not necessarily through an encrypted tunnel. Secure communications can be provided by the VPN, but only if the security protocols have been selected and correctly configured to provide a trusted path over an untrusted network, such as the internet. 
* **NAC** stands for **Network Access Control** and it provides a network visibility needed for access security and may later incident response analysis. Aside from identifying connections, it should also be able to provide isolation for noncompliant devices within a quarantined network and provide a mechanism to “fix” the noncompliant elements, such as turning on endpoint protection. The goal is to ensure that all devices wishing to join the network do so only when they comply with the requirements laid out in the organization policies. This visibility will encompass internal users as well as any temporary users such as guests or contractors, etc., and any devices they may bring with them into the organization.
* **Segmentation for Embedded Systems and IoT** must be considered because those devices can be used in a malicious manner. Because an embedded system is often in control of a mechanism in the physical world, a security breach could cause harm to people and property. Since many of these devices have multiple access routes, such as ethernet, wireless, Bluetooth, etc., special care should be taken to isolate them from other devices on the network. It can be impose logical network segmentation with switches using VLANs, or through other traffic-control means, including MAC addresses, IP addresses, physical ports, protocols, or application filtering, routing, and access control management. Network segmentation can be used to isolate IoT environments.


## References

[Certified in Cybersecurity℠ — CC](https://www.isc2.org/Certifications/CC?filter=featured&searchRoot=A82B5ABE5FF04271998AE8A4B5D7DEFD){:target="_blank"}.
