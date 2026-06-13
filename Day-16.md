# Room Walkthrough:[ Secure Network Architecture &](https://tryhackme.com/room/introtosecurityarchitecture) [Discovery Detection](https://tryhackme.com/room/networkdiscoverydetection)

## 🎯 Day 16 Objective
<img width="50%" height="50%" align="right" alt="image" src="https://github.com/user-attachments/assets/c18ef5cf-4691-4506-91b0-4ff159a01b42" />

As part of my SOC Analyst L1 learning journey, these rooms helped me understand two important security concepts:

1. How organizations design secure networks.
2. How attackers discover assets and services inside those networks.

Before defenders can detect attacks, they must build secure environments. Likewise, before attackers can compromise systems, they must first identify what exists on the network.

These rooms demonstrated both perspectives and showed how security architecture and detection work together.

---

## Who is this for?

* SOC Analysts
* Network Security Engineers
* Security Architects
* Incident Responders
* Threat Hunters
* Students learning network defense

---

## What is Secure Network Architecture?

Secure Network Architecture is the process of designing networks to reduce risk, limit attack paths, and improve visibility.

It includes:

* VLAN Segmentation
* Network Zoning
* Access Control Lists (ACLs)
* Firewalls
* SSL Inspection
* Network Monitoring

The goal is not simply preventing attacks but limiting attacker movement after compromise.

---

## What is Network Discovery Detection?

Network Discovery Detection focuses on identifying attacker reconnaissance activities.

Before attackers exploit systems, they typically perform:

* Host Discovery
* Port Scanning
* Service Enumeration
* OS Fingerprinting
* Network Mapping

Detecting these activities early can stop attacks before compromise occurs.

---

## Where does this apply?

* Enterprise Networks
* Data Centers
* Cloud Environments
* SOC Operations
* Incident Response Teams

---

## Why is it important?

Most successful attacks begin with reconnaissance.

If defenders can detect scanning activity early:

* Vulnerabilities can be patched.
* Exposure can be reduced.
* Attackers can be blocked.
* Investigations can begin before compromise.

---

## How does it work?

Organizations use:

* VLANs
* Firewalls
* IDS/IPS
* ACLs
* SSL Inspection
* Traffic Monitoring
* SIEM Platforms

to secure and monitor network environments.

---

# Task 2: Network Segmentation
Subnets seem to solve all problems a network may face; why would we use another solution? To answer this question, let’s consider a scenario where a client brings in their own device, a common practice known as BYOD (Bring Your Own Device). 
The client’s device was infected with a Remote Access Trojan (RAT) that will attempt to traverse the network the device is connected to and exfiltrate any sensitive information. With subnetting in place, there is no restriction in place as to where the infected device could connect as long as the proper routes are in place, leaving sensitive information and servers open to the unknown device. How do we fix this problem? VLANs! 

VLANs (Virtual LAN) are used to segment portions of a network at layer two and differentiate devices. 
<img width="721" height="341" alt="image" src="https://github.com/user-attachments/assets/b703562b-a577-4ea2-8dd0-bc2a2c7c1018" />

VLANs are configured on a switch by adding a "tag" to a frame. The 802.1q or dot1q tag will designate the VLAN that the traffic originated from.
Network segmentation divides networks into smaller logical groups.

This reduces attack surface and limits lateral movement.
Analyzing a VLAN Configuration

Apply what you learned to analyze a VLAN configuration given the output below.

Interface Configuration Snippet 

```
/ # ovs-vsctl show
87c2a3ee-5374-435a-81c2-e8aafa96e3b9
    Bridge br2
        datapath_type: netdev
        Port br2
            Interface br2
                type: internal
    Bridge br1
        datapath_type: netdev
        Port br1
            Interface br1
                type: internal
    Bridge br0
        datapath_type: netdev
        Port eth9
            tag: 30
            Interface eth9
        Port br0
            Interface br0
                type: internal
        Port eth13
            tag: 30
            Interface eth13
        Port eth14
            tag: 30
            Interface eth14
        Port eth15
            tag: 30
            Interface eth15
        Port eth2
            tag: 10
            Interface eth2
        Port eth8
            tag: 30
            Interface eth8
        Port eth3
            tag: 20
            Interface eth3
        Port eth7
            tag: 30
            Interface eth7
        Port eth4
            tag: 20
            Interface eth4
        Port eth10
            tag: 30
            Interface eth10
        Port eth5
            tag: 20
            Interface eth5
        Port eth6
            tag: 30
            Interface eth6
        Port eth12
            tag: 30
            Interface eth12
        Port eth0
            Interface eth0
        Port eth11
            tag: 30
            Interface eth11
        Port eth1
            tag: 10
            Interface eth1
    Bridge br3
        datapath_type: netdev
        Port br3
            Interface br3
                type: internal
  ```

---

## Question
How many trunks are present in this configuration?


**Answer:** 4
### Why It Matters

Trunk links carry traffic from multiple VLANs between switches.

Without trunks, VLAN communication cannot scale across a network.

---

## Question
What is the VLAN tag ID for interface eth12?

**Answer:** 30
### Deep Dive

VLAN tagging uses IEEE 802.1Q.

The VLAN tag tells switches which VLAN traffic belongs to.

---

### IOC Perspective

Unexpected VLAN traffic can indicate:

* VLAN hopping attacks
* Misconfigurations
* Unauthorized devices

---

# Task 3: Common Secure Network Architecture

Network zones create security boundaries.
<img width="681" height="481" alt="image" src="https://github.com/user-attachments/assets/16d89075-ec07-4db7-814c-38c4ff532239" />
Security zones and access controls will physically direct how and where traffic goes. But how is it decided what resources users or devices have access to? Traffic rules are often governed by company security policy or compliance as equally as security controls that determine access permissions. 

---

## Question
From the above table, what zone would a user connecting to a public web server be in?

**Answer:** External Zone

---

## Question
Which zone would a public web server be in?

**Answer:** DMZ Zone

---

## Question
From the above table, what zone would a core domain controller be placed in?

**Answer:** Restricted Zone

---

## Security Architecture Model

### External Zone

Untrusted networks.

Examples:

* Internet
* Third-party connections

---

### DMZ Zone

Public-facing systems.

Examples:

* Web Servers
* Mail Servers
* Reverse Proxies

---

### Internal Zone

Employee systems and business applications.

---

### Restricted Zone

Highly sensitive assets.

Examples:

* Domain Controllers
* PKI Servers
* Database Servers

---

## SOC Analyst Perspective

Attackers typically move:

External → DMZ → Internal → Restricted

Detection opportunities increase at each stage.

---

# Task 4: Network Security Policies & Controls

## Question
According to the corresponding ACL policy, will the first packet result in a drop or accept? 

**Answer:** Accept

## Question
According to the corresponding ACL policy, will the second packet result in a drop or accept?

**Answer:** Drop

---

## Why ACLs Matter

ACLs determine:

* Who can communicate
* What services are allowed
* Which traffic should be blocked

---

### Detection Opportunity

Repeated ACL drops often indicate:

* Reconnaissance
* Misconfiguration
* Unauthorized access attempts

---

# Task 5: Zone-Pair Policies
## Zone-Pairs
Zone-pairs are a direction-based and stateful policy that will enforce the traffic in single directions per each VLAN, hence, zone-pair. For example, DMZ → LAN or LAN → DMZ.

Each zone in a given topology must have a different zone-pair for each other in the topology and every possible direction. This approach provides the most visibility from a firewall and drastically improves the filtering capabilities.

In this task, we will use VyOS as an example of configuring a firewall for zone-pairing.

Before defining each pair's zoning policy, we must add a common name and default action for each VLAN.

Recall from task two that VLANs are routed through a trunk and divided through sub-interfaces. The syntax is <interface>.<VLAN #> e.g. eth0.30

Below is an example of setting a default action for the DMZ and assigning the VLAN to the common zone name.

* set zone-policy zone dmz default-action drop

* set zone-policy zone dmz interface eth0.30

## Flag

THM{M05tly_53cure}
<img width="1100" height="1209" alt="image" src="https://github.com/user-attachments/assets/76e0d5b9-11ad-49f1-bbe4-71e2f683da60" />

---

## Key Learning

Security is not created by a firewall alone.

Security requires:

* Zones
* Policies
* Traffic Validation
* Monitoring

Misconfigured zone policies often become attack paths.

---

# Task 6: Validating Network Traffic

This was one of the most valuable topics in the room.
<img width="906" height="416" alt="image" src="https://github.com/user-attachments/assets/3e4d8a21-c4d4-4757-98df-0caed95cfd5d" />
## Zone-Pairs
Zone-pairs are a direction-based and stateful policy that will enforce the traffic in single directions per each VLAN, hence, zone-pair. For example, DMZ → LAN or LAN → DMZ.
|   |   |   |   |
|---|---|---|---|
|**Zone A**|**Zone B**|**Protocol**|**Action**|
|LAN|WAN|ICMP|Drop|
|LAN|LOCAL|-|-|
|LAN|DMZ|-|-|
|WAN|LAN|ICMP|Accept|
|WAN|LOCAL|-|-|
|WAN|DMZ|-|-|
|LOCAL|LAN|-|-|
|LOCAL|WAN|-|-|
|LOCAL|DMZ|-|-|
|DMZ|LAN|HTTP|Accept|
|DMZ|WAN|-|-|
|DMZ|LOCAL|-|-|

---

## SSL/TLS Inspection

Organizations increasingly rely on encrypted traffic.

Unfortunately, attackers do too.
SSL/TLS Inspection

SSL/TLS inspection uses an SSL proxy to intercept protocols, including HTTP, POP3, SMTP, or other SSL/TLS encrypted traffic. Once intercepted, the proxy will decrypt the traffic and send it to be processed by a UTM (Unified Threat Management) platform. UTM solutions will employ deep SSL inspection, feeding the decrypted traffic from the proxy into other UTM services, including but not limited to web filters or IPS (Intrusion Prevention System), to process the information.
This solution may seem ideal, but what are the downsides? Some of you may have already noted that this requires an SSL proxy or MitM (Man-in-the-Middle). Even if a firewall or vendor has already implemented the solution, it will still act as a MiTM between your devices and the outside world; what if it intercepts potentially plain-text passwords? A corporation must assess the pros and cons of this solution, dependent on its calculated risk. You could allow all applications that you know are safer to prevent potential cons, but this solution will still have disadvantages. For example, an advanced threat actor could route their traffic through a cloud provider or a trusted domain.

---

## Question
Does SSL inspection require a MiTM proxy?

**Answer:** Yes

---

## Question
What platform processes data from the SSL Proxy?

**Answer:** Unified Threat Management (UTM)

---

## SOC Analyst Perspective

Without SSL inspection:

* Malware C2 traffic may appear legitimate.
* Data exfiltration may go unnoticed.
* Threat hunting becomes difficult.

---

## IOC Examples

Potential indicators:

* Unusual TLS Certificates
* Rare Domains
* Suspicious User Agents
* Beaconing Traffic

---

## ATT&CK Mapping

* T1071.001 Application Layer Protocol
* T1041 Exfiltration Over C2 Channel

---

# Task 7: Addressing Common Attacks
DHCP Snooping
Cisco(opens in new tab) defines DHCP snooping as "a security feature that acts like a firewall between untrusted hosts and trusted DHCP servers."

DHCP snooping was introduced to combat rogue DHCP servers; it will validate and rate-limit DHCP traffic as necessary. If a host is untrusted, its traffic will be filtered and rate-limited.

Although DHCP is a layer three protocol, DHCP snooping operates on the switch at layer two. The switch will store untrusted hosts with leased IP addresses in a DHCP Binding Database. The database is used to validate traffic and can be used by other protocols, such as dynamic ARP inspection, which we will cover later in this task.

We know how DHCP snooping will gather and store addresses, but how does it determine what to do with traffic? Below is a list of conditions the protocol will inspect to determine if a DHCP packet should be dropped.

Any DHCP packet is received from outside of the network.
The source MAC address and DHCP client hardware address do not match.
A DHCPRELEASE or DHCPDECLINE packet is received on an untrusted interface that does not match an interface that the source address already has registered.
A DHCP packet that includes a relay agent address that is not 0.0.0.0
Although recognized by the IEEE in several research papers, there is no standardization of DHCP snooping. That said, it generally does not change between vendors, unlike other protocols.

Dynamic ARP Inspection
Cisco(opens in new tab) defines ARP inspection as "a security feature that validates Address Resolution Protocol (ARP) packets in a network."

ARP inspection will validate and rate-limit ARP packets as necessary; if an ARP packet's MAC and IP address do not match, the protocol will intercept, log, and discard the packet.

ARP inspection uses the DHCP binding database filled from DHCP snooping as its list of binding IP addresses.

```Address Resolution Protocol (request)
    Hardware type: Ethernet (1)
    Protocol type: IPv4 (0x0800)
    Hardware size: 6
    Protocol size: 4
    Opcode: request (1)
    Sender MAC address: 02:c8:85:b5:5a:aa (02:c8:85:b5:5a:aa)
    Sender IP address: 10.10.0.1
    Target MAC address: 00:00:00_00:00:00 (00:00:00:00:00:00)
    Target IP address: 10.10.239.154
                |
                |
                ⇓
Router# show ip dhcp snoop bind
MacAddress         IpAddress       Lease(sec) Type          VLAN Interface
------------------ --------------- ---------- ------------- ---- --------------------
02:c8:85:b5:5a:aa  10.10.0.1       23453      dhcp-snooping 10   GigabitEthernet1/1
01:02:03:04:05:06  2.2.2.2         69445      dhcp-snooping 20   GigabitEthernet2/1

```
Below is a diagram showing an invalid ARP request that does not match the binding database and requests information (note the Sender MAC address has been spoofed).
```
Address Resolution Protocol (request)
    Hardware type: Ethernet (1)
    Protocol type: IPv4 (0x0800)
    Hardware size: 6
    Protocol size: 4
    Opcode: request (1)
    Sender MAC address: 02:c8:85:bb:bb:bb (02:c8:85:bb:bb:bb)
    Sender IP address: 10.10.0.1
    Target MAC address: 00:00:00_00:00:00 (00:00:00:00:00:00)
    Target IP address: 10.10.239.154
                |
                X
                |
                ⇓
Router# show ip dhcp snoop bind
MacAddress         IpAddress       Lease(sec) Type          VLAN Interface
------------------ --------------- ---------- ------------- ---- --------------------
02:c8:85:b5:5a:aa  10.10.0.1       23453      dhcp-snooping 10   GigabitEthernet1/1
01:02:03:04:05:06  2.2.2.2         69445      dhcp-snooping 20   GigabitEthernet2/1
```

---

## DHCP Snooping

### Question
Where are leased IP addresses stored?

**Answer:** DHCP Binding Database

---

### Purpose

Prevents rogue DHCP servers.
the DHCP binding database provides the expected MAC and IP address pair of untrusted hosts; ARP inspection will compare the source IP address and MAC address to the binding pair; if they are mismatched, it will drop the packet.

---

### Question
Will a switch drop or accept a DHCPRELEASE packet?

**Answer:** Drop

---

## Dynamic ARP Inspection

### Question
Does DAI dynamic ARP inspection use the DHCP Binding Database?

**Answer:** Yes

---

### Question
Dynamic ARP inspection will match an IP address and what other packet detail?

**Answer:** MAC Address

---

## Why This Matters

Prevents:

* ARP Spoofing
* Man-in-the-Middle Attacks
* Network Impersonation

---

## ATT&CK Mapping

* T1557 Adversary-in-the-Middle

---

# Network Discovery Detection

This room focused on attacker reconnaissance.
Set up your virtual environment <img align="right" width="50%" height="50%" alt="image" src="https://github.com/user-attachments/assets/a3c4aab9-6045-4074-819c-f033170e07c3" />

To successfully complete this room, you'll need to set up your virtual environment. This involves starting the Target Machine, ensuring you're equipped with the necessary tools and access to tackle the challenges ahead.

---

# Task 2: Network Discovery

## Attackers and Network Discovery
What assets can be accessed by the attacker? 

What are the IP addresses, ports, OS, and services running on these assets?

What versions of services are running? Does any service have a vulnerability that can be exploited?
<img width="752" height="759" alt="image" src="https://github.com/user-attachments/assets/784d0f29-971b-43f8-9045-aada6c35f689" />

## Defenders and Network Discovery
On the other hand, defenders also sometimes run software that performs network discovery activity. During this activity, defenders want to achieve the following goals:

Inventory the organisation's assets and ensure all assets are documented.

Ensure no unnecessary IP, port, or service is open, and whatever is running is necessary.

Ensure vulnerabilities are patched, or at least the exploitable vulnerabilities are patched.
In short, defenders attempt to reduce the attack surface as much as possible.

## The Challenge in Detecting Network Discovery
As you might have noticed, both attackers and defenders perform discovery actions. Multiple research organisations, web crawlers, search engines, etc., also perform similar discovery actions to map the resources present on the Internet. SOC teams must differentiate between good and bad discovery. To mitigate this challenge, SOC teams often use the following techniques.

Allowlist known internal and benign external scanners, ensuring no alerts are triggered on those sources.

Integrate Threat Intelligence with detection use cases and flag scanning activity only from known malicious or suspicious sources.

Since the previous point has a chance of missing some malicious scanning activity, some teams use the Threat Intelligence to raise the severity of the alerts instead of only triggering alerts on them.

## Question
What do attackers scan, other than, IP addresses, ports, and OS version, in order to identify vulnerabilities in a network?

**Answer:** Services

---

## Why Services Matter

Attackers often care more about:

* Apache
* IIS
* SMB
* SSH
* RDP

than the operating system itself.

Services reveal attack opportunities.

---

# Task 3: External vs Internal Scanning
## External Scanning Activity
A SOC analyst might encounter scanning activity from outside their organisation's network and scan the machines inside the network, mainly the public-facing assets on the perimeter. The SOC analyst will observe that the source IP in this type of attack is an external IP and the destination is an IP address belonging to the organisation. This type of scan indicates that the attack is still in the Reconnaissance(opens in new tab) phase of the MITRE ATT&CK lifecycle. The attacker does not have a foothold inside the network and is doing initial reconnaissance to identify opportunities to gain initial access to the network.<img width="5000" height="5000" alt="image" src="https://github.com/user-attachments/assets/1d5a1c06-e680-4a49-8f11-76747415aac7" />

## Internal Scanning Activity
The other type of scanning that a SOC analyst might observe is an internal-to-internal scanning activity. The SOC analyst will observe that the source and destination IP addresses are private IP addresses inside the organisation's network. This type of scan initiates inside the organisation's network and scans assets from the same network. This type of scan indicates that the attack has progressed to the Discovery(opens in new tab) phase of the MITRE ATT&CK lifecycle. In some other frameworks, it might also be called internal reconnaissance, indicating that the attacker has a foothold in the network and is now gearing up for lateral movement. <img width="5000" height="5000" alt="image" src="https://github.com/user-attachments/assets/27f36adc-3e9f-4783-a0b4-e984a2178127" />


## Question
Which file contains logs that showcase internal scanning activity?

**Answer:** log-session-2.csv

---

## Question
How many log entries are present for the internal IP performing internal scanning activity?

**Answer:** 2276

---

## Question
What is the external IP address that is performing external scanning activity?

**Answer:** 203.0.113.25

---

## Detection Opportunities

Look for:

* High connection counts
* Sequential scanning
* Short-lived connections
* Multiple destination systems

---

## ATT&CK Mapping

* T1595 Active Scanning

---

# Task 4: Horizontal vs Vertical Scanning

## Horizontal Scan
Sometimes, an attacker will scan for the same port across multiple destination IP addresses. This type of scan is called a horizontal scan. Horizontal scans are performed to identify which hosts expose a particular port. An attacker might perform this scan if they intend to exploit that specific port. An example can be the WannaCry ransomware, which spread through the network using an SMBv1 vulnerability and scanned for machines with port 445 (which is used for SMB) open. 

### Question
One of the log files contains evidence of a horizontal scan. Which IP range was scanned? Format X.X.X.X/X

### Answer
203.0.113.0/24

### Definition

One service across many hosts.<img width="965" height="677" alt="image" src="https://github.com/user-attachments/assets/5927ff43-668e-4b44-8b1b-b86d9b46b6af" />

Example:
Scanning port 80 on hundreds of systems.
We can detect a horizontal scan in the logs if we see the same source IP, a single destination port, but multiple destination IP addresses across multiple events.

---

## Vertical Scan
Vertical scanning occurs when a single host IP address is scanned across multiple ports. Attackers perform vertical scanning to footprint a host and identify its exposed services. This activity might be performed when an attacker is focused on identifying a vulnerability on a single machine because they consider it a valuable target based on their objectives. For example, if an organisation exposes only a single server to the internet, any attacker who wants to breach that organisation would first perform a vertical scan on that server to identify open ports and understand the services running on the machine.

### Question
In the same log file, there is one IP address on which a vertical scan is performed. Which IP address is this?

### Answer
192.168.230.145

### Definition

Many ports on one host.
<img width="894" height="701" alt="image" src="https://github.com/user-attachments/assets/577a9997-f2ec-4d5a-b5af-72bb5f65c47f" />

Example:

Scanning ports 21,22,25,80,443,445,3389.
We can detect a vertical scan in the logs if we observe the same source IP, the same destination IP, but multiple destination ports across multiple events.

Sometimes attackers might perform mixed horizontal and vertical scans to gain the advantages of both types of scans.

---

### Question
On one of the IP addresses, only a few ports are scanned which host common services. Which are the ports that are scanned on this IP address? Format: port1, port2, port3 in ascending order.

### Ports
80, 445, 3389

---

### Why These Ports?

80 → Web Services

445 → SMB

3389 → RDP

All three are common attack targets.

---

# Task 5: The Mechanics of Scanning

## Credentials
Open a browser inside the attached VM and use the following credentials.
Username                               Password
elastic                                changeme
 
IP address
http://MACHINE_IP:5601

Navigate to the hamburger menu on the top-left side, and click on Discover.
<img width="3024" height="1964" alt="image" src="https://github.com/user-attachments/assets/cd9c89ec-5326-4ddc-9fd7-759ef4be28d8" />

## Question
Which source IP performs a ping sweep attack across a whole subnet?

**Answer:** 192.168.230.127

---

### What is a Ping Sweep?
A technique used to discover live hosts.
This is one of the most basic network scanning techniques. Ping sweeps are generally used to identify hosts present (and online) on a network. This scan is run by sending an Internet Control Message Protocol (ICMP) packet to the host. If the host is online, it will reply with an ICMP packet of its own. However, it is often blocked by security controls in some organisations nowadays, making it easier to defeat this type of scanning activity.
---

## Question
The zeek.conn.conn_state value shows the connection state. Using the information provided by this value, identify the type of scan being performed by 203.0.113.25 against 192.168.230.145

**Answer:** TCP SYN Scan
A TCP connection is initiated by a three-way handshake, following the steps SYN, SYN-ACK, ACK to establish the connection. Network scanners can sometimes use this functionality of the TCP handshake to identify online hosts and their open ports. The scanner sends a SYN request to the recipient. If a SYN-ACK response is received, it means that the host is online and the port on which the SYN connection was sent is also open. This is a stealthy scan that often blends in with the rest of the network traffic and is harder to detect.
---

## Why SYN Scans?
Benefits for attackers:

* Fast
* Efficient
* Stealthier than full TCP connections

---

## Question
Is there any UDP scanning attempt in the logs?

**Answer:** No

---

## IOC Examples

* High SYN Traffic
* Sequential Destination Ports
* Repeated Connection Attempts
* ICMP Sweep Activity

---

## ATT&CK Mapping

* T1595 Active Scanning
* T1046 Network Service Discovery

---

# What Confused Me?

Initially, I thought firewalls alone were enough to secure a network.

I also struggled to understand why attackers spend so much time scanning before exploitation.

After completing these rooms, I realized that reconnaissance is often the most important stage of an attack because it determines everything that follows.

---

# What I Learned?

Key lessons:

* VLANs limit attacker movement.
* DMZs isolate public services.
* ACLs enforce communication rules.
* SSL inspection increases visibility.
* DHCP Snooping protects network trust.
* Attackers rely heavily on reconnaissance.
* Scanning activity creates valuable detection opportunities.

---

# How I Improved?

Before these rooms, I focused primarily on attack execution.

Now I understand that strong security architecture can significantly reduce attack success even before detection occurs.

I also became better at recognizing scanning behaviors and understanding why they matter.

---

# Real-World Relevance

SOC teams investigate:

* Port Scans
* Ping Sweeps
* Service Enumeration
* Unauthorized Network Discovery
* Suspicious TLS Traffic
* Rogue DHCP Activity
* Lateral Movement Attempts

These activities appear regularly in enterprise environments.

---

## 🛡️ SOC Analyst L1: Tricky Connection

Many beginners view scanning as harmless.

Experienced analysts understand that scanning is often the first observable stage of an attack.

The sooner reconnaissance is detected, the more opportunities defenders have to stop the intrusion.

---

## Next Learning Steps

1. Nmap
2. Zeek
3. Snort
4. Suricata
5. Wireshark
6. IDS/IPS Engineering
7. Threat Hunting
8. Network Traffic Analysis
9. Detection Engineering
10. SIEM Correlation Rules

---

## 💼 Critical Thinking Question

### Scenario

Your IDS generates alerts showing a single external IP performing SYN scans against multiple public-facing systems.

Thirty minutes later, VPN logs show repeated authentication failures against several user accounts.

How would you determine whether these events are related?

### Answer Concept

1. Compare timestamps.
2. Correlate source IP addresses.
3. Review targeted services.
4. Check for successful authentication.
5. Search for lateral movement indicators.
6. Review endpoint activity.
7. Escalate if compromise indicators exist.

### Potential ATT&CK Techniques

* T1595 Active Scanning
* T1046 Network Service Discovery
* T1110 Brute Force
* T1078 Valid Accounts

---

## Final Reflection

These rooms taught me that network security begins long before malware execution or privilege escalation. Strong architecture, segmentation, monitoring, and detection controls create obstacles that force attackers to reveal themselves.

As an aspiring SOC Analyst L1, understanding both secure network design and reconnaissance detection gives me a stronger foundation for investigating alerts, identifying suspicious behavior, and understanding how attacks progress through enterprise environments.

