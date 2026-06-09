# Room Walkthrough: [Introductory](https://tryhackme.com/room/introtonetworking) [Networking](https://tryhackme.com/room/whatisnetworking) [Concepts](https://tryhackme.com/room/networkingconcepts)

## 🎯 Day XX Objective
<img width="1406" height="703" alt="image" src="https://github.com/user-attachments/assets/4bc8650d-203b-41f9-9922-ea6054953f5a" />

As part of my SOC Analyst L1 learning journey, this room helped me understand one of the most important foundations in cybersecurity:

> Networks are the environment where almost every cyber attack and security event occurs.

Before learning SIEMs, IDS/IPS, malware analysis, threat hunting, or incident response, I wanted to understand how devices communicate, how data travels between systems, and how defenders monitor that communication.

This room introduced the OSI Model, TCP/IP Model, IP Addressing, Encapsulation, Common Protocols, and basic networking tools that security analysts use during investigations.

---

## Who is this for?

* SOC Analysts
* Security Engineers
* Network Administrators
* Incident Responders
* Threat Hunters
* Anyone starting cybersecurity

## What is it?

An introduction to networking theory and practical networking tools.

The room explains:

* OSI Model
* TCP/IP Model
* Encapsulation
* IP Addressing
* TCP vs UDP
* DNS
* Common Networking Tools

## Where does this apply?

Networking concepts appear everywhere:

* SOC monitoring
* Firewall investigations
* IDS/IPS alerts
* Malware investigations
* Threat Hunting
* Incident Response

## When should it be learned?

Very early.

Before learning:

* SIEM Platforms
* Threat Hunting
* Malware Analysis
* Digital Forensics
* Detection Engineering

A strong networking foundation makes every future topic easier.

## Why is it important?

Every cyber attack generates network activity.

Without networking knowledge, it becomes difficult to understand:

* Where traffic originated
* Where traffic is going
* Which protocol is involved
* Whether communication is normal or suspicious

## How does it work?

Networking works through layers.

Each layer has specific responsibilities.

Data moves down the sender's stack through encapsulation and moves up the receiver's stack through de-encapsulation.

Understanding these layers helps analysts determine where communication problems or malicious activity may exist.

---

## What confused me?

Initially, I struggled with:

* Why there are two networking models (OSI and TCP/IP)
* Why the same communication process is divided into multiple layers
* The difference between Frames, Packets, Segments, and Datagrams
* How encapsulation actually works
* Why TCP and UDP behave differently

The layer structure felt theoretical until I began connecting protocols and real network traffic to specific layers.

## What I learned?

I learned that networking is essentially a structured communication process.

### Core Concepts Learned

#### OSI Model (7 Layers)
The OSI (Open Systems Interconnection) model is a conceptual model developed by the International Organization for Standardization (ISO) 
<img width="800" height="251" alt="image" src="https://github.com/user-attachments/assets/fb7ead5c-990c-49d2-828e-9cb65c264e96" />

|Layer Number | Layer Name|	Main Function	|Example Protocols and Standards|
|-------------|-----------|---------------|-------------------------------|
|Layer 7|	Application layer|	Providing services and interfaces to applications	|HTTP, FTP, DNS, POP3, SMTP, IMAP
|Layer 6|	Presentation layer|	Data encoding, encryption, and compression	|Unicode, MIME, JPEG, PNG, MPEG
|Layer 5|	Session layer	|Establishing, maintaining, and synchronising sessions	|NFS, RPC
|Layer 4|	Transport layer|	End-to-end communication and data segmentation	|UDP, TCP
|Layer 3|	Network layer|	Logical addressing and routing between networks	|IP, ICMP, IPSec
|Layer 2|	Data link layer|	Reliable data transfer between adjacent nodes	|Ethernet (802.3), WiFi (802.11)
|Layer 1|	Physical layer|	Physical data transmission media	|Electrical, optical, and wireless signals

#### TCP/IP Model (4 Layers)
 TCP/IP stands for Transmission Control Protocol/Internet Protocol and was developed in the 1970s by the Department of Defense (DoD).
<img width="390" height="341" alt="image" src="https://github.com/user-attachments/assets/ba41add9-2841-48e1-9eda-dfdd752b5732" />

|Layer Number|	ISO OSI Model|	TCP/IP Model (RFC 1122)|	Protocols|
|------------|---------------|-------------------------|-----------|
|7	|Application Layer	|Application Layer	|HTTP, HTTPS, FTP, POP3, SMTP, IMAP, Telnet, SSH,
|6	|Presentation Layer|
|5	|Session Layer|
|4	|Transport Layer|	Transport Layer	|TCP, UDP|
|3	|Network Layer	|Internet Layer	|IP, ICMP, IPSec|
|2	|Data Link Layer|	Link Layer	|Ethernet 802.3, WiFi 802.11|
|1	|Physical Layer|		

Many modern networking textbooks show the TCP/IP model as five layers instead of four. For example, in Computer Networking: A Top-Down Approach 8th Edition, Kurose and Ross(opens in new tab) describe the following five-layer Internet protocol stack by including the physical layer:

Application
Transport
Network
Link
Physical
#### Encapsulation
The process of wrapping data with layer-specific headers and trailers as it travels down a protocol stack
Data changes names as it moves through layers:
<img width="1279" height="720" alt="image" src="https://github.com/user-attachments/assets/3121bb51-816f-4938-b9db-284ca89b154f" />

Application Data

↓

TCP = Segment
<img align="right" width="50%" height="523" alt="image" src="https://github.com/user-attachments/assets/fe8b648d-8408-47a3-9ef8-7094b6469a81" />

UDP = Datagram

↓

Network = Packet

↓

Data Link = Frame

#### TCP

* Connection-based
* Reliable
* Uses Three-Way Handshake
* Error checking
* Packet ordering

<img width="1056" height="484" alt="image" src="https://github.com/user-attachments/assets/3a0e5515-d545-4d01-a500-354f71fb5c27" />

#### UDP

* Connectionless
* Faster
* Less overhead
* Common for streaming and VoIP

<img width="759" height="357" alt="image" src="https://github.com/user-attachments/assets/27e4bb45-4c2f-4ecf-ab17-595d9d0369cc" />

#### DNS
The Domain Name System (DNS) is the internet's phonebook. Its core function is to translate human-readable domain names 
<img width="2485" height="1912" alt="image" src="https://github.com/user-attachments/assets/0f4722f3-ff89-4448-927f-12e724b91891" />

Converts domain names into IP addresses.

Example:

google.com

↓

142.250.68.238

---

## How I improved?

Before this room, I looked at network traffic as random technical information.

Now I understand:

* Which protocol is being used
* Which layer is responsible
* Why traffic behaves a certain way
* How attackers may abuse networking protocols

Instead of seeing only IP addresses and ports, I now understand the communication process behind them.

## Real-World Relevance

Inside a SOC environment:

* DNS requests reveal destinations.
* TCP connections reveal communications.
* Failed connections may indicate scanning.
* Unusual ports may indicate malware.
* Large outbound transfers may indicate exfiltration.

Almost every investigation begins with networking evidence.

Understanding networking is therefore one of the most valuable skills for a SOC Analyst.

---

## Network Types
Computer networks are primarily classified by their geographic range, scale, and organizational purpose.

<img width="318" height="159" alt="image" src="https://github.com/user-attachments/assets/6594cd33-f6c4-44e3-88a5-1ce3b06b5554" />

### LAN

Local Area Network

Example:

Office network inside a building.

### WAN

Wide Area Network

Example:

The Internet.

### WLAN

Wireless Local Area Network

Example:

Corporate Wi-Fi.

### VPN

Virtual Private Network

Encrypted connection between systems.

Very common in enterprises.

---

## Network Topologies
Network topology is the arrangement of nodes and connections in a computer network. It defines both the physical layout of devices (hardware and cables) and the logical paths that data travels. Choosing the right topology optimizes traffic flow, fault tolerance, and scalability.

<img width="957" height="671" alt="image" src="https://github.com/user-attachments/assets/5f300c9c-5811-452a-9b7e-ff3c7efdbda5" />

### Star Topology

Most common today.

All devices connect to a central switch.

### Bus Topology

Single communication cable.

Rare in modern environments.

### Ring Topology

Devices form a circular connection.

### Mesh Topology

Multiple redundant connections.

Common in critical infrastructure.

---

## IP Address Classes
An IP (Internet Protocol) address is a unique numerical label assigned to every device connected to a computer network. It acts like a digital mailing address, allowing your devices to send, receive, and route data across the internet and local networks
<img width="211" height="239" alt="image" src="https://github.com/user-attachments/assets/5feb68a9-fa40-4cb7-9701-64d16a973894" />

The Two Main Types:
### Private IP Ranges
Assigned by your home or office router to individual devices (like your phone or smart TV) so they can communicate on your secure local network without being visible to the outside world.
10.0.0.0 – 10.255.255.255

172.16.0.0 – 172.31.255.255

192.168.0.0 – 192.168.255.255

### Public IP
 Assigned by your Internet Service Provider (ISP), this is your primary identity on the wider internet. It can be Dynamic (changes periodically) or Static (stays the same).
Accessible from the Internet.

Example:

So, what makes an IP address? An IP address comprises four octets, i.e., 32 bits. Being 8 bits, an octet allows us to represent a decimal number between 0 and 255. An IP address is shown in the image below.
<img width="1140" height="487" alt="image" src="https://github.com/user-attachments/assets/a9893f45-aae6-4a43-acc7-e9165963c2dc" />
---

## Common Ports Every SOC Analyst Should Know
In computer networking, a port number is a 16-bit integer (0 to 65,535) that directs network traffic to the correct application or service on a device. While an IP address finds the device, the port number ensures the data reaches the specific program (like a web browser or email server).
| Layer Number | ISO OSI Model      | TCP/IP Model (RFC 1122) | Protocols                                                                                          |
| ------------ | ------------------ | ----------------------- | -------------------------------------------------------------------------------------------------- |
| 7            | Application Layer  | Application Layer       | HTTP, HTTPS, FTP, POP3, SMTP, IMAP, Telnet, SSH,                                                   |
| 6            | Presentation Layer |                         |                                                                                                    |
| 5            | Session Layer      |                         |                                                                                                    |
| 4            | Transport Layer    | Transport Layer         | TCP, UDP                                                                                           |
| 3            | Network Layer      | Internet Layer          | IP, ICMP, IPSec                                                                                    |
| 2            | Data Link Layer    | Link Layer              | Ethernet 802.3, WiFi 802.11                                                                        |
| 1            | Physical Layer     |                         |                                                                                                    |
| Port         | Protocol           | Service                 | How It Works                                                                                       |
| **20/21**    | TCP                | FTP                     | Transfers files between a client and a server. Port 21 sends commands, port 20 moves the data.     |
| **22**       | TCP                | SSH                     | Creates a secure, encrypted tunnel to remotely manage servers and execute commands.                |
| **23**       | TCP                | Telnet                  | Connects to remote text-based systems. It sends text in plain text, making it insecure.            |
| **25**       | TCP                | SMTP                    | Acts as the postal service for email, routing outgoing messages from one mail server to another.   |
| **53**       | TCP/UDP            | DNS                     | Translates human website names (like website.com) into computer IP addresses.                      |
| **67/68**    | UDP                | DHCP                    | Automatically assigns IP addresses and network settings to new devices when they connect.          |
| **80**       | TCP                | HTTP                    | Transmits unencrypted web pages from a web server to your browser.                                 |
| **110**      | TCP                | POP3                    | Downloads email messages from a server to your device and typically deletes them from the server.  |
| **143**      | TCP                | IMAP                    | Syncs and stores emails on a server so you can view them from multiple devices simultaneously.     |
| **123**      | UDP                | NTP                     | Synchronizes the internal clocks of computers and devices across a network to the exact same time. |
| **135**      | TCP                | RPC                     | Helps Windows programs on different computers talk to each other and trigger actions remotely.     |
| **137-139**  | TCP/UDP            | NetBIOS                 | Helps older Windows devices find each other by name and share basic data on a local network.       |
| **389**      | TCP/UDP            | LDAP                    | Searches corporate directories to verify user permissions, usernames, and passwords.               |
| **443**      | TCP                | HTTPS                   | Transmits web pages securely by encrypting the traffic to protect passwords and personal data.     |
| **445**      | TCP                | SMB                     | Allows modern Windows computers to share files, folders, and printers over a local network.        |
| **3389**     | TCP                | RDP                     | Transmits a live graphical desktop screen so you can control another PC from far away.             |
### SOC Analyst Tip

When you see unusual communication over these ports, ask:

> Is this expected behavior for this device?

That question often starts an investigation.

---

## Task 2 – OSI Model

### Key Questions

| Question                   | Answer |
| -------------------------- | ------ |
| TCP or UDP selection layer | 4      |
| Error checking             | 2      |
| Data formatting            | 2      |
| Transmission layer         | 1      |
| Encryption/Compression     | 6      |
| Session tracking           | 5      |
| Application requests       | 7      |
| Logical addressing         | 3      |

### Key Takeaway

Every network communication depends on all seven layers working together.

---

## Task 3 – Encapsulation

| Question             | Answer           |
| -------------------- | ---------------- |
| Layer 2 data         | Frame            |
| UDP Layer 4 data     | Datagram         |
| Receiving process    | De-encapsulation |
| Layer adding trailer | Data Link        |
| Security benefit     | Aye              |

### Key Takeaway

Encapsulation provides structure and organization to network communication.

---

## Task 4 – TCP/IP Model

| Question                 | Answer      |
| ------------------------ | ----------- |
| Introduced first         | TCP/IP      |
| OSI Transport equivalent | Transport   |
| Session equivalent       | Application |
| Data Link + ?            | Physical    |
| Network equivalent       | Internet    |

### Three-Way Handshake
<img width="1024" height="559" alt="image" src="https://github.com/user-attachments/assets/851a140d-5dba-451f-985f-a0964abe1301" />

1. SYN
2. SYN/ACK
3. ACK

### Key Takeaway

SOC analysts frequently observe handshake activity when investigating suspicious connections.

---

## Task 5 – Ping
The ping command is used when we want to test whether a connection to a remote resource is possible. Usually this will be a website on the internet, but it could also be for a computer on your home network if you want to check if it's configured correctly. Ping works using the ICMP protocol, which is one of the slightly less well-known TCP/IP protocols that were mentioned earlier.
<img width="507" height="38" alt="image" src="https://github.com/user-attachments/assets/54b5a9fe-af2a-40fb-880d-4304bf01ab46" />

### Commands Learned

```bash
ping bbc.co.uk
```

```bash
ping -4 domain.com
```

```bash
ping -v domain.com
```

### Key Takeaway

Ping helps verify connectivity and identify reachability issues.

---

## Task 6 – Traceroute
The logical follow-up to the ping command is traceroute. Traceroute can be used to map the path your request takes as it heads to the lab machine.
<img width="910" height="271" alt="image" src="https://github.com/user-attachments/assets/8d0a7484-bd13-4675-b2bf-f7044d9eccfb" />
You can see that it took 13 hops to get from my router (_gateway) to the Google server at 216.58.205.46

### Useful Switches

```bash
traceroute -i
```

```bash
traceroute -T
```

### Key Takeaway

Traceroute shows the path traffic takes across networks.

Very useful during connectivity investigations.

---

## Task 7 – WHOIS
Whois essentially allows you to query who a domain name is registered to. In Europe personal details are redacted; however, elsewhere you can potentially get a great deal of information from a whois search.
<img width="1051" height="489" alt="image" src="https://github.com/user-attachments/assets/a47492f1-781f-40b6-a59d-2522b012987d" />
This is comparatively a very small amount of information as can often be found. Notice that we've got the domain name, the company that registered the domain, the last renewal, and when it's next due, and a bunch of information about nameservers

### Key Findings

* Facebook postal code: 94025
* Registration date: 29/03/1997
* Microsoft city: Redmond
* Tech Email: [msnhst@microsoft.com](mailto:msnhst@microsoft.com)

### Key Takeaway

WHOIS is valuable during OSINT investigations and domain analysis.

---

## Task 8 – DIG & DNS
At the most basic level, DNS allows us to ask a special server to give us the IP address of the website we're trying to access. For example, if we made a request to www.google.com, our computer would first send a request to a special DNS server (which your computer already knows how to find). The server would then go looking for the IP address for Google and send it back to us. Our computer could then send the request to the IP of the Google server.
When you visit a website in your web browser this all happens automatically, but we can also do it manually with a tool called dig . Like ping and traceroute, dig should be installed automatically on Linux systems.
<img width="616" height="366" alt="image" src="https://github.com/user-attachments/assets/3603db9a-a4a6-4ac7-8a88-a592440c9279" />


### Key Questions

| Question                | Answer             |
| ----------------------- | ------------------ |
| DNS Full Form           | Domain Name System |
| First DNS queried       | Recursive          |
| Domain Extension Server | Top-Level Domain   |
| First lookup location   | Hosts File         |
| Secondary Google DNS    | 8.8.4.4            |
| 24 Hour TTL             | 86400              |

### Key Takeaway

DNS investigations are extremely common during phishing and malware analysis.

---

## Networking Concepts Room

### OSI Review

* End-to-End Communication → Layer 4
* Routing → Layer 3
* Encoding → Layer 6
* Same Segment Communication → Layer 2

### TCP/IP Review

HTTP belongs to:

Application Layer

Application Layer covers:

OSI Layers 5, 6, and 7

---

## IP Addressing

### Private IP

* 10.x.x.x
* 172.16-31.x.x
* 192.168.x.x

### Public IP

49.69.147.197

### Invalid IP Example

192.168.305.19

Because an octet cannot exceed 255.

### MAC address
MAC stands for Media Access Control,They are usually expressed in hexadecimal format with a colon separating each two hexadecimal digits (one byte). The three leftmost bytes identify the vendor. 
<img width="1920" height="649" alt="image" src="https://github.com/user-attachments/assets/f06c76a4-49a4-44d9-a0c2-62d7748f542d" />

We expect to see two MAC addresses in each frame in real network communication over Ethernet or WiFi. The packet in the screenshot below shows:

. The destination data-link address (MAC address) highlighted in yellow

. The source data link address (MAC address) is highlighted in blue

. The remaining bits show the data being sent
<img width="3046" height="1691" alt="image" src="https://github.com/user-attachments/assets/d6908e1c-cef8-4c47-ae7f-c0fff444f92b" />

---

## Telnet Practical
The TELNET (Teletype Network) protocol is a network protocol for remote terminal connection. In simpler words, telnet, a TELNET client, allows you to connect to and communicate with a remote system and issue text commands. Although initially it was used for remote administration, we can use telnet to connect to any server listening on a TCP port number.
<img width="781" height="509" alt="image" src="https://github.com/user-attachments/assets/651fbb5f-e1bf-420d-be22-c64a6900ee5e" />

### HTTP Server
<img width="1408" height="768" alt="image" src="https://github.com/user-attachments/assets/67302587-45e7-4ca2-9393-4cf6621782e4" />
**Question 1:** Use telnet to connect to the web server on MACHINE_IP. What is the name and version of the HTTP server?
**Answer** lighttpd/1.4.63

### Flag
THM{TELNET_MASTER}

### Key Takeaway

Telnet can manually interact with services and is useful for troubleshooting.

---

# 🛡️ SOC Analyst L1: Mindset

## Tricky Connection

Many beginners learn networking as:

> "How do computers communicate?"

A SOC Analyst learns networking as:

> "How do attackers communicate, and how can I detect it?"

For example:

* DNS → Malware domains
* HTTP → Data exfiltration
* RDP → Unauthorized access
* SMB → Lateral movement
* SSH → Remote administration

The protocol itself is not suspicious.

The behavior is.

This is one of the biggest mindset shifts in SOC operations.

---

## Next Learning Steps

To strengthen this foundation, I should continue learning:

1. Packet Analysis with Wireshark
2. TCP Flags
3. DNS Investigation
4. HTTP Requests & Responses
5. Network Traffic Analysis
6. Firewalls
7. IDS/IPS
8. Network Logs
9. Proxy Logs
10. SIEM Network Investigations

---

# Critical Thinking

## Question

A SIEM alert shows a workstation making hundreds of DNS requests to random-looking domains every few seconds. Shortly afterward, the same host begins communicating with an external IP address over TCP port 443.

As an L1 SOC Analyst, how would you investigate?

## Answer Concept

### Step 1: Validate the Alert

Review DNS logs and identify:

* Domains queried
* Query frequency
* First seen time

### Step 2: Analyze Network Activity

Review:

* Source host
* Destination IP
* Port 443 communication

### Step 3: Investigate Reputation

Check:

* Domain reputation
* IP reputation
* Threat intelligence sources

### Step 4: Determine Possible Threat

This behavior may indicate:

* Malware beaconing
* DNS tunneling
* Command & Control communication

### Step 5: Escalate

If malicious indicators are confirmed:

* Isolate the endpoint
* Preserve logs
* Notify Incident Response
* Document findings

This demonstrates structured investigation methodology and strong networking fundamentals.

---

# Final Reflection

This room taught me that networking is not just an IT topic; it is a cybersecurity skill.

Every alert, log, packet capture, malware infection, phishing investigation, and incident response activity ultimately relies on understanding how systems communicate.

As an aspiring SOC Analyst L1, building a strong networking foundation will help me understand attacker behavior, investigate alerts more effectively, and develop stronger analytical skills throughout my cybersecurity journey.
