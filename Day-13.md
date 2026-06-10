# Room Walkthrough: [Traffic Analysis Essentials](https://tryhackme.com/room/trafficanalysisessentials)

## 🎯 Day 13 Objective

As part of my SOC Analyst L1 learning journey, this room introduced me to the foundations of Network Security and Traffic Analysis.

Before analysts can investigate alerts, detect malicious activity, or respond to incidents, they must understand what normal network traffic looks like and how malicious traffic differs from it.

This room helped me understand how security teams monitor network communications, identify suspicious activity, and use traffic analysis as part of the detection and investigation process.

---

### Who is this for?

* SOC Analysts
* Security Operations Teams
* Network Administrators
* Incident Responders
* Threat Hunters
* Anyone learning Network Security

### What is Traffic Analysis?
<img width="1140" height="800" alt="image" src="https://github.com/user-attachments/assets/732990ec-405d-4915-b264-5d2abda16ea3" />

Traffic Analysis is the process of inspecting, monitoring, and analyzing network communications to identify normal behavior, suspicious activity, security incidents, and potential threats.

Traffic analysis is one of the essential approaches used in network security, and it is part of multiple disciplines of network security operations listed below:
Network Sniffing and Packet Analysis (Covered in Wireshark)
Network Monitoring (Covered in Zeek)
Intrusion Detection and Prevention (Covered in Snort)
Network Forensics (Covered in NetworkMiner)
Threat Hunting (Covered in Brim)

It helps defenders understand:
* Who is communicating?
* What protocols are being used?
* Which systems are involved?
* When communications occurred?
* Whether activity is legitimate or malicious?

### Where does this apply?

Traffic Analysis is used in:

* Security Operations Centers (SOC)
* Enterprise Networks
* Cloud Environments
* Data Centers
* Government Networks
* Incident Response Investigations

### When should it be used?

Traffic Analysis becomes important when:

* Investigating alerts
* Hunting threats
* Detecting malware activity
* Monitoring network performance
* Responding to security incidents
* Identifying unauthorized access

### Why is it important?

Almost every cyberattack leaves traces in network traffic.

Even if attackers delete files or clear logs, network communications often reveal:

* Initial access attempts
* Malware downloads
* Command and Control (C2) communications
* Data exfiltration
* Lateral movement activity

Understanding traffic allows analysts to identify these indicators before major damage occurs.

### How does it work?

Analysts collect and review network data from:

* Packet captures (PCAP)
* Network monitoring tools
* IDS/IPS systems
* Firewalls
* Proxy logs
* DNS logs
* SIEM platforms

The goal is to establish normal behavior and quickly identify anomalies.

---

# Task 2: Network Security and Network Data
**Network Security**[<img width="4137" height="2660" alt="image" src="https://github.com/user-attachments/assets/0c8a4a0d-76fd-40ae-8e61-b6c9d03037d1" />](https://www.fortinet.com/resources/cyberglossary/what-is-network-security)

The essential concern of Network Security focuses on two core concepts: authentication and authorisation. There are a variety of tools, technologies, and approaches to ensure and measure implementations of these two key concepts and go beyond to provide continuity and reliability. 
This task introduced foundational security controls used to protect networks and manage access.
[<img width="652" height="353" alt="image" src="https://github.com/user-attachments/assets/efe999df-b39b-4bb6-94d1-b15ef84ff1a5" />](https://www.geeksforgeeks.org/computer-networks/network-security/)
**Base Network Security Control Levels:**
|Control|                         Working                          |
|-----------|------------------------------------------------------|
|Physical|	Physical security controls prevent unauthorised physical access to networking devices, cable boards, locks, and all linked components.|
|Technical|	Data security controls prevent unauthorised access to network data, like installing tunnels and implementing security layers.|
|Administrative|	Administrative security controls provide consistency in security operations like creating policies, access levels and authentication processes.|

**The main approaches:**
|Access Control ||	Threat Control|
|------------------------|-|-----------------------|
|The starting point of Network Security. It is a set of controls to ensure authentication and authorisation.||  Detecting and preventing anomalous/malicious activities on the network. It contains both internal (trusted) and external traffic data probes.|

**The key elements of Access Control:**
|Control|                         Working                          |
|-----------|------------------------------------------------------|
|Firewall Protection|Controls incoming and outgoing network traffic with predetermined security rules. Designed to block suspicious/malicious traffic and application-layer threats while allowing legitimate and expected traffic.|
|Network Access Control (NAC)|Controls the devices' suitability before access to the network. Designed to verify device specifications and conditions are compliant with the predetermined profile before connecting to the network.|
|Identity and Access Management (IAM)	|Controls and manages the asset identities and user access to data systems and resources over the network.|
|Load Balancing	|Controls the resource usage to distribute (based on metrics) tasks over a set of resources and improve overall data processing flow.|
|Network Segmentation|Creates and controls network ranges and segmentation to isolate the users' access levels, group assets with common functionalities, and improve the protection of sensitive/internal devices/data in a safer network.|
|Virtual Private Networks (VPN)|Creates and controls encrypted communication between devices (typically for secure remote access) over the network (including communications over the internet).|
|Zero Trust Model	|Suggests configuring and implementing the access and permissions at a minimum level (providing access required to fulfil the assigned role). The mindset is focused on: "Never trust, always verify".|

**The key elements of Threat Control:**
|Control|                         Working                          |
|-----------|------------------------------------------------------|
|Intrusion Detection and Prevention (IDS/IPS)|Inspects the traffic and creates alerts (IDS) or resets the connection (IPS) when detecting an anomaly/threat.|
|Data Loss Prevention (DLP)|Inspects the traffic (performs content inspection and contextual analysis of the data on the wire) and blocks the extraction of sensitive data.|
|Endpoint Protection|Protecting all kinds of endpoints and appliances that connect to the network by using a multi-layered approach like encryption, antivirus, antimalware, DLP, and IDS/IPS.|
|Cloud Security|	Protecting cloud/online-based systems resources from threats and data leakage by applying suitable countermeasures like VPN and data encryption.|
|Security Information and Event Management (SIEM)|Technology that helps threat detection, compliance, and security incident management, through available data (logs and traffic statistics) by using event and context analysis to identify anomalies, threats, and vulnerabilities.|
|Security Orchestration Automation and Response (SOAR)|Technology that helps coordinate and automates tasks between various people, tools, and data within a single platform to identify anomalies, threats, and vulnerabilities. It also supports vulnerability management, incident response, and security operations.|
|Network Traffic Analysis & Network Detection and Response|	Inspecting network traffic or traffic capture to identify anomalies and threats.|

[<img width="1600" height="900" alt="image" src="https://github.com/user-attachments/assets/287ef793-163a-4a69-a3f7-cd9f0b025adc" />
](https://www.hpe.com/in/en/what-is/network-security-management.html)
---

## Question 1

### Which Security Control Level covers creating security policies?

**Answer:** Administrative

### Explanation

Administrative controls define how security should operate within an organization.

Examples include:

* Security Policies
* Acceptable Use Policies
* Incident Response Procedures
* Employee Security Awareness Training

These controls guide the implementation of technical and physical controls.

### SOC Analyst Perspective

Analysts often follow administrative controls when:

* Escalating incidents
* Handling evidence
* Reporting security events
* Following incident response playbooks

Without administrative controls, investigations become inconsistent.

---

## Question 2

### Which Access Control element works with data metrics to manage data flow?

**Answer:** Load Balancing

### Explanation
[<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/008edba5-f5fd-46ea-bc6d-0e55e1421f28" />
](https://www.geeksforgeeks.org/system-design/what-is-load-balancer-system-design/)
Load balancing distributes traffic across multiple systems or resources.

Benefits include:

* Improved availability
* Better performance
* Reduced bottlenecks
* Increased fault tolerance

### SOC Analyst Perspective

Unexpected load balancing behavior can indicate:

* DDoS activity
* Traffic floods
* Misconfigurations
* Infrastructure issues

Analysts may investigate unusual traffic spikes that impact load balancers.

---

## Question 3

### Which technology helps correlate different tool outputs and data sources?

**Answer:** SOAR

### Explanation

Security Orchestration, Automation and Response (SOAR) platforms combine information from multiple security tools.
[<img width="800" height="800" alt="image" src="https://github.com/user-attachments/assets/34c7b120-7107-40b0-98de-14143ad493e5" />
](https://www.dropzone.ai/blog/ai-soc-analysts-soar-automation)
Examples:

* SIEM alerts
* Endpoint alerts
* Firewall logs
* Threat Intelligence feeds

SOAR helps analysts investigate incidents faster.

### SOC Analyst Perspective

In many SOC environments:

* SIEM generates alerts.
* SOAR automates workflows.
* Analysts validate findings.

Understanding this relationship is important for enterprise operations.

---

## Advanced Exploration

### Security Control Layers
<img width="361" height="208" alt="image" src="https://github.com/user-attachments/assets/b80f08ab-4747-44f1-be2e-b26dc8f821d3" />

Most organizations implement three major control categories:

### Administrative Controls

* Policies
* Standards
* Procedures
* Training

### Technical Controls

* Firewalls
* IDS/IPS
* Antivirus
* MFA
* SIEM

### Physical Controls

* Security Guards
* Access Cards
* CCTV
* Locked Server Rooms

A security incident often involves multiple control failures rather than a single weakness.

---

# Task 3: Traffic Analysis

The room demonstrated basic traffic filtering techniques.

This is one of the most common tasks performed by SOC analysts.

The goal is to separate legitimate traffic from suspicious traffic.

**There are two main techniques used in Traffic Analysis:**
|   |   |
|---|---|
|**Flow Analysis**|**Packet Analysis**|
|Collecting data/evidence from the networking devices. This type of analysis aims to provide statistical results through the data summary without applying in-depth packet-level investigation.<br><br>- **Advantage:** Easy to collect and analyse.<br>- **Challenge:** Doesn't provide full packet details to get the root cause of a case.|Collecting all available network data. Applying in-depth packet-level investigation (often called Deep Packet Inspection (DPI) ) to detect and block anomalous and malicious packets.<br><br>- **Advantage:** Provides full packet details to get the root cause of a case.<br>- **Challenge:** Requires time and skillset to analyse.|

---

## Level 1

### Question

Identify and filter malicious IP addresses.

### Flag

THM{PACKET_MASTER}

### What I Learned

A single IP address can reveal:

* Attack sources
* Scanners
* Malware infrastructure
* Command and Control servers

Filtering helps analysts reduce noise and focus on suspicious activity.

### IOC Identified

Possible IOC Types:

* Malicious Source IP
* Known Threat Actor Infrastructure
* Blacklisted Network

### Detection Opportunities

Monitor for:

* Excessive connection attempts
* Repeated failed connections
* Connections to known malicious IPs
* Unusual geographic locations

---

## Level 2

### Question

Identify and filter malicious IP and Port combinations.

### Flag

THM{DETECTION_MASTER}

### What I Learned

An IP address alone does not always tell the full story.

Ports provide valuable context.

Examples:

* Port 21: FTP (File Transfer Protocol). Used to upload or download files between a computer and a server.

* Port 445: SMB (Server Message Block). Used by Windows for sharing files and printers over a network.

* Port 3306: MySQL. The default network port for the popular MySQL database server used by websites and apps.

* Port 3689: DAAP (Digital Audio Access Protocol). Used by Apple for sharing iTunes music libraries and AirPlay.

* Port 7777: Gaming. A very common default port for multiplayer video game servers (like Terraria or Sons of the Forest).

* Port 8080: HTTP Alternate. A common port used as an alternative for standard web traffic, usually for web servers or testing web applications

* Port 2222:A standard port for Secure Shell (SSH). SSH is a tool used to safely log into and control a computer from a remote location.

* Port 4444:A port commonly used by Metasploit, a popular ethical hacking and security testing tool.

Combining IP and port analysis improves detection accuracy.

### IOC Identified

Potential IOCs:

* Malicious IP Address
* Suspicious Port Usage
* Unusual Service Communication

### Detection Opportunities

Monitor for:

* Unexpected RDP traffic
* Unauthorized SSH access
* Rare outbound connections
* High-volume connections to unusual ports

---

# Two Main Traffic Analysis Techniques

The room introduced two major approaches.

### Packet Analysis

Examines individual packets.

Focus Areas:

* Source IP
* Destination IP
* Protocol
* Port
* Payload

Common Tools:

* Wireshark
* tcpdump

### Flow Analysis

Examines communication patterns rather than full packet contents.

Focus Areas:

* Traffic volume
* Communication frequency
* Source/Destination relationships

Common Tools:

* NetFlow
* Zeek
* Security Onion

---

# What Confused Me?

Initially, I struggled to understand why analysts spend so much time looking at IP addresses and ports.

At first glance, traffic data appears overwhelming and repetitive.

I also wondered:

* How do analysts know what is normal?
* How can one IP address be important?
* Why do ports matter during investigations?

After completing the room, I realized that traffic analysis is about identifying patterns rather than memorizing numbers.

---

# What I Learned?

Key lessons from this room:

* Network traffic contains valuable security evidence.
* IP addresses can act as indicators of compromise.
* Ports provide context about services and behavior.
* Traffic filtering reduces investigation noise.
* Security monitoring relies heavily on network visibility.
* Network analysis supports detection, hunting, and incident response.

---

# How I Improved?

Before this room, I mainly viewed network traffic as technical data generated by systems.

Now I understand that network traffic tells a story about what systems, users, and applications are doing.

Instead of looking only at alerts, I started thinking:

> What network behavior generated this alert?

That mindset is essential for effective SOC investigations.

---

# Real-World Relevance

In a real SOC environment, analysts investigate:

* Malware beaconing
* DNS anomalies
* Suspicious outbound connections
* Data exfiltration attempts
* Brute-force attacks
* Unauthorized remote access

Almost every incident generates network evidence.

Traffic analysis often becomes one of the most valuable investigation techniques.

---

## 🛡️ SOC Analyst L1: Tricky Connection

Many beginners focus on tools.

Experienced analysts focus on behavior.

For example:

* An IP address is not the investigation.
* A port number is not the investigation.
* A SIEM alert is not the investigation.

The investigation is understanding:

* Who communicated?
* What happened?
* Why did it happen?
* Is it expected behavior?

This room helped me begin developing that mindset.

---

## Next Learning Steps

After this room, I should strengthen:

1. TCP/IP Fundamentals
2. Common Network Protocols
3. DNS Analysis
4. HTTP/HTTPS Traffic
5. Wireshark
6. Zeek
7. IDS/IPS Concepts
8. Security Onion
9. Network-Based Detection
10. PCAP Analysis

---

## 💼 Critical Thinking Question

### Scenario

A workstation suddenly begins making outbound HTTPS connections every five minutes to an external IP address that has never been observed in the environment before.

The user reports no issues and no software updates were scheduled.

As a SOC Analyst L1, what would you investigate first?

### Answer Concept

1. Identify the destination IP.
2. Check threat intelligence sources.
3. Review DNS activity.
4. Determine which process initiated the connection.
5. Review endpoint alerts.
6. Look for persistence mechanisms.
7. Check whether other hosts communicate with the same IP.
8. Escalate if malicious behavior is confirmed.

### Possible Threat

This pattern could indicate malware beaconing to a Command and Control (C2) server.

### Potential IOCs

* Suspicious External IP
* Repeated Periodic Connections
* Unknown Domain
* Unusual Process Activity

### Potential MITRE ATT&CK Techniques

* T1071 – Application Layer Protocol
* T1105 – Ingress Tool Transfer
* T1571 – Non-Standard Port
* T1078 – Valid Accounts (if credentials are abused)

---

## Final Reflection

This room showed me that traffic analysis is one of the core skills required for SOC analysts.

Understanding network behavior helps transform raw traffic into meaningful security insights. Rather than focusing only on alerts, I learned to look at the underlying communications that generated them.

As an aspiring SOC Analyst L1, this room strengthened my understanding of network visibility, traffic filtering, indicators of compromise, and the investigative thinking required to detect suspicious activity in enterprise environments.

