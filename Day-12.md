# Room Walkthrough:[ Network Traffic Basics](https://tryhackme.com/room/networktrafficbasics)

## 🎯 Day 12 Objective

As part of my SOC Analyst L1 learning journey, this room introduced me to one of the most important skills in security operations:

> "Understanding what normal network traffic looks like and identifying what deviates from that baseline."

Before learning advanced detection engineering, threat hunting, or malware analysis, I needed to understand how devices communicate across a network and how security teams monitor those communications.

Network Traffic Analysis (NTA) provides visibility into what is happening inside and outside an organization’s network. Without this visibility, detecting malicious activity becomes significantly harder.

* Know what network traffic analysis is

* Know what can be observed

* Know how to observe network traffic

* Know typical network traffic sources and flows

---

## Who is this for?

* SOC Analysts
* Security Monitoring Teams
* Incident Responders
* Threat Hunters
* Network Administrators
* Security Engineers
* Cybersecurity Students

## What is Network Traffic Analysis?

Network Traffic Analysis (NTA) is the process of capturing, inspecting, analyzing, and monitoring network communications to understand what devices, users, applications, and services are doing.

It combines:

* Packet Analysis
* Network Flow Analysis
* Log Correlation
* Traffic Monitoring
* Security Investigation

NTA is not simply using Wireshark.

It is the practice of combining multiple data sources to gain visibility into network activity and identify suspicious behavior.

## Where does it apply?

* Enterprise Networks
* Data Centers
* Cloud Environments
* Government Networks
* Financial Institutions
* Healthcare Organizations
* Security Operations Centers (SOCs)

## When should you learn it?

Network Traffic Analysis should be learned early in a SOC career because many security alerts involve network communications.

Examples include:

* Malware infections
* Data exfiltration
* Command and Control (C2)
* Lateral Movement
* Phishing Activity
* Insider Threats

## Why is it important?

Without network visibility, security teams cannot effectively answer:

* Who communicated?
* What data was transferred?
* When did it happen?
* Where did it go?
* Was it legitimate or malicious?

Network traffic often provides the first indication of an attack.

## How does it work?

NTA collects information from various sources such as:

* Packets
* Network Flows
* DNS Logs
* Firewall Logs
* Proxy Logs
* IDS/IPS Alerts
* Endpoint Logs

Analysts review this information to identify unusual patterns and suspicious activities.

---

## What confused me?

Initially, I thought network monitoring only involved capturing packets using Wireshark.

My confusion included:

* What is the difference between packet data and flow data?
* Why do organizations collect NetFlow instead of full packets?
* How can DNS traffic reveal attacker activity?
* Why is baseline traffic important?

After studying the room, I understood that analysts rarely investigate a single source of information. Multiple data sources are correlated together to build a complete picture.

## What I learned?

Key lessons from this room:

* Network Traffic Analysis is much broader than packet capture.
* Flow data provides communication summaries.
* Packet data provides detailed visibility.
* DNS can be abused for Command and Control communication.
* Fragmentation can be used to evade detection systems.
* TCP sequence numbers help detect session hijacking attempts.
* Authentication traffic often appears before business traffic.
* TLS protects network communications through encryption.

## How I improved?

I learned to think beyond individual packets and focus on communication patterns.

Instead of asking:

> "What packet was sent?"

I started asking:

> "What behavior does this traffic represent?"

That shift is important because SOC analysts investigate behaviors rather than isolated packets.

## Real-World Relevance

In a corporate SOC, analysts regularly investigate:

* Malware beaconing
* Suspicious DNS requests
* Unauthorized file transfers
* Data exfiltration attempts
* Unusual login behavior
* Lateral movement between systems

The concepts introduced in this room are directly applicable to daily SOC operations.

---

# 📘 Task 1 – Introduction
<img width="600" height="600" alt="image" src="https://github.com/user-attachments/assets/c1916484-a448-45ae-89a1-e9e2ce250ced" />

Network Traffic Analysis (NTA) is a process that encompasses capturing, inspecting, and analyzing data as it flows in a network. Its goal is to have complete visibility and understand what is communicated inside and outside the network. It is important to stress that NTA is not a synonym for the tool Wireshark
Knowing how to analyze network traffic is an essential skill, not only for an aspiring SOC L1 analyst but also for many other blue and red team roles. As an L1 analyst, you need to be able to navigate through the sea of network information and understand what is normal and what deviates from the baseline.

## Key Concepts Learned

Network Traffic Analysis consists of:

* Capturing traffic
* Inspecting traffic
* Monitoring communications
* Detecting anomalies
* Investigating incidents

Two major data sources were introduced:

### Packet Data

Provides complete visibility.

Examples:

* Source IP
* Destination IP
* Payload Contents
* Protocol Information

Advantages:

* Deep visibility
* Detailed investigations

Limitations:

* Large storage requirements
* Higher processing overhead

### Flow Data

Provides summarized communication information.

Examples:

* Source IP
* Destination IP
* Bytes transferred
* Duration

Advantages:

* Lower storage requirements
* Long-term monitoring

Limitations:

* No payload visibility

### SOC Analyst Takeaway

Most enterprise SOCs rely heavily on flow data for visibility and use packet captures during deeper investigations.

---

# 📘 Task 2 – Purpose of Network Traffic Analysis
Monitor network performance
Check for abnormalities in the network. E.g., sudden performance peaks, slow network, etc
Inspect the content of suspicious communication internally and externally. E.g., exfiltration via DNS, download of a malicious ZIP file over HTTP, lateral movement, etc

From a SOC perspective, network traffic analysis helps:

* Detecting suspicious or malicious activity

* Reconstructing attacks during incident response

* Verifying and validating alerts
### Question

What is the name of the technique used to smuggle C2 commands via DNS?

### Answer

DNS Tunneling [<img width="480" height="270" alt="image" src="https://github.com/user-attachments/assets/edeffc9b-6a13-41a2-8564-5bd1b2791dc2" />](https://www.paloaltonetworks.com/cyberpedia/what-is-dns-tunneling)


### Why is this Important?

DNS is often allowed through security controls because organizations depend on it.

Attackers exploit this trust by embedding commands and data inside DNS requests and responses.

### Real-World Detection Ideas

Monitor for:

* Excessive DNS requests
* Long DNS query strings
* Unusual domains
* High entropy DNS traffic

### SOC Analyst Perspective

DNS traffic should never be ignored simply because it appears normal.

Many advanced threats communicate through DNS.

---

# 📘 Task 3 – What Network Traffic Can We Observe?
The best way to showcase the traffic we can observe in the network is by using the architecture implemented in nearly every device with a network interface: the TCP/IP stack. The image below shows the different layers of the TCP/IP model.<img width="1370" height="457" alt="image" src="https://github.com/user-attachments/assets/40f364b4-bcac-42eb-8d30-d721333f0c1d" />

### Question 1

What is the size of the ZIP attachment included in the HTTP response?
**HTTP/1.1 200 OK
Date: Mon, 29 Sep 2025 10:15:30 GMT
Server: nginx/1.18.0
Content-Type: application/zip
Content-Length: 10485760
Content-Disposition: attachment; filename="suspicious_package.zip"
Last-Modified: Mon, 29 Sep 2025 09:54:00 GMT
ETag: "5d8c72-9f8a1c-3a2b4c"
Accept-Ranges: bytes
Connection: close
[binary ZIP file bytes follow — 10,485,760 bytes]**

### Answer
10485760 bytes

### Why does this matter?

Large file transfers may indicate:

* Software distribution
* Backup activity
* Data exfiltration

Analysts should always understand expected file transfer patterns.

---

### Question 2

Which attack do attackers use to try to evade an IDS?

### Answer

Fragmentation

### Explanation

Fragmentation breaks data into smaller pieces.

Poorly configured IDS solutions may fail to properly reassemble traffic, allowing malicious content to evade detection.

### SOC Analyst Use Case

Monitor for:

* Excessive fragmented packets
* Fragmentation anomalies
* Known IDS evasion techniques

---

### Question 3

What field in the TCP header can we use to detect session hijacking?

### Answer

Sequence Number

### Explanation

TCP sequence numbers help maintain packet order during communication.

Unexpected sequence patterns may indicate:

* Session hijacking
* Packet injection
* Traffic manipulation

### SOC Analyst Takeaway

TCP header analysis remains an important investigation skill.

---

# 📘 Task 4 – Network Traffic Sources and Flows
In the previous task, we discussed what we can observe theoretically based on the TCP/IP stack. Practically, it is more helpful to focus on specific sources and flows. A corporate network typically has some predetermined network flows and sources.
We can group the sources into two categories:
* Intermediary
* Endpoint
The flows we can also group into two categories:
* North-South: Traffic that exits or enters the LAN and passes the firewall
* East-West: Traffic that stays within the LAN (including LAN that extends to the cloud)
  
### Question 1
Which category of devices generates the most traffic in a network?

### Answer
Endpoint

### Why?
Endpoints include:

* Workstations
* Laptops
* Mobile Devices
* Servers

These devices generate most user activity.

### SOC Relevance
Endpoints are often the initial entry point for attackers.

---

### Question 2

Before an SMB session can be established, which service needs to be contacted first for authentication?

### Answer
Kerberos

### Explanation
<img width="1060" height="510" alt="image" src="https://github.com/user-attachments/assets/2c8eb6b2-30fa-48eb-ab4e-cfc011004a67" />
Kerberos provides authentication before access is granted to network resources.

### SOC Relevance

Kerberos logs frequently contain evidence of:

* Brute Force Attempts
* Pass-the-Ticket Attacks
* Credential Abuse

---

### Question 3

What does TLS stand for?

### Answer

Transport Layer Security

### Why is TLS Important?
<img width="1210" height="212" alt="image" src="https://github.com/user-attachments/assets/44a11348-a8b9-42a2-bf34-9b0e0b321b8e" />

Transport Layer Security is a cryptographic protocol that secures communications over a network. It provides authentication through the use of certificates, integrity by detecting tampering and confidentiality by encrypting the data.
TLS protects:
* Confidentiality
* Integrity
* Authentication

It secures modern web traffic.

### SOC Relevance

Although TLS encrypts content, analysts can still investigate:

* Certificates
* Domains
* IP Addresses
* Connection Metadata

---

### Scenario 1

#### Question

What is the flag found in the HTTP traffic?

#### Answer
THM{FoundTheMalware}

### Investigation Thought Process

An analyst would examine:

* HTTP Requests
* HTTP Responses
* Downloaded Files
* User Agents
* Suspicious URLs

This mirrors malware delivery investigations.

---

### Scenario 2

#### Question

What is the flag found in the DNS traffic?

#### Answer
THM{C2CommandFound}

### Investigation Thought Process

An analyst would review:

* DNS Queries
* DNS Responses
* Domain Reputation
* Query Frequency
* Unusual DNS Patterns

This mirrors Command and Control investigations.

---

## Common Network Monitoring Use Cases

### Security Monitoring

* Malware Detection
* DNS Tunneling Detection
* C2 Communication Detection
* Data Exfiltration Detection
* Lateral Movement Detection
* Insider Threat Detection
* Brute Force Detection

### Operational Monitoring

* Bandwidth Analysis
* Service Availability Monitoring
* Performance Monitoring
* Capacity Planning

### Compliance Monitoring

* Audit Reporting
* User Activity Tracking
* Access Monitoring
* Regulatory Compliance Reviews

---

# 📊 Monitoring 

## SNMP
Simple Network Management Protocol ([SNMP](https://www.networkacademy.io/ccna/network-services/simple-network-management-protocol-snmp)) is a standardized application-layer protocol used to monitor, manage, and configure network devices—such as routers, switches, servers, and printers—across an IP network. It allows administrators to track network performance, detect faults, and prevent outages
<img width="620" height="287" alt="image" src="https://github.com/user-attachments/assets/090ca462-8fe8-4940-adc9-3c8d2ad72ed7" />
### Purpose
Collects device health metrics.Communication between the manager and the agent happens through standardized requests and responses:
.GET: The manager requests specific data from an agent (e.g., CPU utilization)
.GETBULK: Efficiently retrieves large chunks of tabular data at once
.SET: The manager configures or alters a setting on the managed device
.TRAP / INFORM: Proactive alerts pushed by the agent to the manager when specific events occur (e.g., device reboot, temperature spike)
### Examples

* CPU Usage
* Memory Usage
* Interface Status
* Device Uptime

---

## Syslog
[Syslog ](https://www.techradar.com/computing/what-is-syslog-and-why-does-it-matter)is the universal standard for logging and transmitting system and network events. It acts as a common language, allowing diverse devices—like servers, firewalls, and routers—to generate event messages and send them to a centralized server (Syslog Server) for storage, troubleshooting, and security auditing.<img width="1000" height="679" alt="image" src="https://github.com/user-attachments/assets/3732a068-2ee7-41a3-9711-5d59cfec6726" />

### Purpose

Centralized event logging.

### Common Sources

* Firewalls
* Routers
* Switches
* Linux Systems
* Security Appliances

---

## [NetFlow / IPFIX](https://docs.fortinet.com/document/fortigate/6.2.0/cookbook/728397/netflow-and-ipfix-support)
|Feature|NetFlow|IPFIX|
|-------|-------------|-----------------|
|Origin|Created by Cisco.|Standardized by the IETF based on NetFlow v9.|
|Standardization|Proprietary to Cisco hardware (though some basic versions are supported elsewhere).|Open standard; compatible with any vendor's equipment (sometimes referred to as NetFlow v10).|
|Flexibility|Rigid in older versions; newer versions like v9 use customizable templates.|Highly customizable; supports variable-length fields (e.g., HTTP URLs) and enterprise-specific data types.|

<img width="632" height="428" alt="image" src="https://github.com/user-attachments/assets/dd1c66fa-55a2-47a3-8910-6e8ceb070495" />

### Purpose

Traffic flow telemetry.
<img width="385" height="486" alt="image" src="https://github.com/user-attachments/assets/fb4b25f4-8bac-482b-832e-4b950e716b0b" />

### Provides

* Source IP
* Destination IP
* Ports
* Protocols
* Duration
* Bytes Transferred

---

## Endpoint Logs
Endpoint logs are records of activities, errors, or events generated by apps or devices connected to a network. Think of them like a plane's "black box." They track who did what on a computer or server.
<img width="451" height="361" alt="image" src="https://github.com/user-attachments/assets/6e660f0b-e627-4b54-aad9-5c1a1edea78d" />

### Examples

* Windows Event Logs
* Sysmon Logs
* EDR Alerts
* Authentication Logs

---

# 🛡️ SOC Analyst L1: Mindset 

Many beginners focus on packets.

Experienced analysts focus on behavior.

For example:

* A DNS request may indicate malware communication.
* An SMB connection may indicate lateral movement.
* A large HTTP transfer may indicate data exfiltration.

The goal is not to memorize protocols.

The goal is to understand what the traffic means from a security perspective.

---

# 🚀 Next Learning Steps

After completing this room, I should continue learning:

1. DNS Deep Dive
2. HTTP and HTTPS Analysis
3. Wireshark
4. TCPDump
5. NetworkMiner
6. Zeek
7. Suricata
8. Snort
9. NetFlow Analysis
10. Packet Analysis
11. Threat Hunting Using Network Data
12. Windows Event Logs
13. SIEM Correlation Rules

---

# 💼 Scenario-Based Interview Question

## Scenario

A workstation begins sending DNS requests every 30 seconds to an unfamiliar domain. Shortly afterward, large encrypted outbound connections are observed.

How would you investigate this as a SOC Analyst L1?

## Answer Concept

1. Validate the alert.
2. Identify the affected host.
3. Review DNS logs for query patterns.
4. Check domain reputation.
5. Review outbound connections.
6. Determine whether data transfer occurred.
7. Collect supporting evidence.
8. Escalate if malicious behavior is suspected.
9. Document findings.

Possible concerns include:

* DNS Tunneling
* Command and Control Activity
* Data Exfiltration
* Malware Infection

---

# Final Reflection

This room demonstrated that Network Traffic Analysis is one of the most valuable skills for a SOC Analyst.

I learned that security investigations are not limited to endpoint alerts. Network traffic provides a detailed record of how systems communicate and often reveals attacker behavior long before a compromise becomes obvious.

As an aspiring SOC Analyst L1, understanding network traffic helps me move beyond simply reviewing alerts and begin thinking like an investigator who can identify suspicious behavior, validate findings, and support incident response activities.

