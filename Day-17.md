# Room Walkthrough: [Nmap](https://tryhackme.com/room/nmap01)  & [Snort](https://tryhackme.com/room/snort)

## 🎯 Day 17 Objective

As part of my SOC Analyst L1 learning journey, these rooms helped me understand two closely related areas of network security:

1. How hosts are discovered on a network using Nmap.
2. How network traffic can be monitored and analyzed using Snort.

Before these rooms, I knew that Nmap was used for scanning and Snort was used for detection. However, I did not fully understand how these tools relate to each other during real-world security operations.

By completing both rooms, I learned how reconnaissance activity appears on a network and how security monitoring tools can detect and investigate that activity.

---

## Who is this for?

* SOC Analysts
* Security Engineers
* Threat Hunters
* Incident Responders
* Penetration Testers
* Anyone learning network security

---

## What are Nmap and Snort?

### Nmap

Nmap (Network Mapper) is a network discovery and reconnaissance tool used to identify:

* Live Hosts
* Open Ports
* Running Services
* Network Topology

---

### Snort

Snort is an Intrusion Detection and Prevention System (IDS/IPS) used to:

* Monitor Network Traffic
* Detect Malicious Activity
* Generate Security Alerts
* Analyze PCAP Files
* Identify Attack Patterns

---

## Where does this apply?

These tools are commonly used in:

* Enterprise Networks
* SOC Environments
* Security Monitoring Platforms
* Threat Hunting Operations
* Incident Response Investigations
* Penetration Testing Engagements

---

## When should it be learned?

These topics should be learned early because network visibility is one of the foundations of security monitoring.

Without understanding network traffic, it becomes difficult to investigate alerts or understand attacker behavior.

---

## Why is it important?

Most cyber attacks generate network evidence before compromise occurs.

Examples include:

* Host Discovery
* Port Scanning
* Service Enumeration
* Malware Communication
* Data Exfiltration

Understanding these behaviors helps analysts identify attacks early.

---

## How does it work?

### Nmap

Nmap sends different types of packets:

* ARP Requests
* ICMP Echo Requests
* TCP SYN Packets
* TCP ACK Packets
* UDP Probes

Any response indicates a potentially live host.

---

### Snort

Snort analyzes traffic and compares it against:

* Rules
* Signatures
* Protocol Decoders
* Anomaly Detection Logic

When suspicious activity is detected, alerts are generated.

---

# Room 1: Nmap Live Host Discovery

---

## Task 2: Understanding ARP and Subnetworks

One important lesson was understanding broadcast domains.

### Observation

Computer1 sent an ARP request for Computer6.

Result:

* Four devices observed the request.
* Computer6 never received it.

### Why?

ARP broadcasts do not cross routers.

They remain inside the local broadcast domain.

---

### SOC Relevance

Understanding ARP traffic helps identify:

* ARP Spoofing
* Rogue Devices
* Network Discovery Activity
* Internal Reconnaissance

---

### IOC Examples

* Excessive ARP Requests
* Duplicate MAC Addresses
* Unexpected ARP Replies

---

## Task 3: Host Discovery Through TCP/IP

Before sending a ping request, systems must first identify the destination MAC address.

### Key Discovery

Ping does not happen immediately.

The system performs:

1. ARP Request
2. ARP Response
3. ICMP Echo Request
4. ICMP Echo Reply

---

### Analyst Perspective

Many analysts focus only on ICMP.

However, ARP activity often provides the first indication of internal discovery activity.

---

### ATT&CK Mapping

* T1018 – Remote System Discovery

---

## Task 4: Enumerating Targets

### Example

Target:

10.10.12.13/29

First Scanned Address:

10.10.12.8

---

### Key Lesson

Before scanning, analysts must understand:

* CIDR Notation
* Network Addresses
* Broadcast Addresses
* Usable Host Ranges

Incorrect scope can lead to missed systems or unnecessary scanning.

---

## Task 5–8: Nmap Host Discovery Techniques

### ARP Discovery

```bash
sudo nmap -PR -sn 10.200.6.0/24
```

Best for local networks.

---

### ICMP Echo Discovery

```bash
sudo nmap -PE -sn 10.200.6.0/24
```

Uses traditional ping requests.

---

### ICMP Timestamp Discovery

```bash
sudo nmap -PP -sn 10.200.6.0/24
```

Useful when echo requests are filtered.

---

### ICMP Address Mask Discovery

```bash
sudo nmap -PM -sn 10.200.6.0/24
```

Older technique but still important historically.

---

### TCP SYN Discovery

```bash
sudo nmap -PS22,80,443 -sn target
```

Useful when ICMP is blocked.

---

### TCP ACK Discovery

```bash
sudo nmap -PA22,80,443 -sn target
```

Can bypass certain firewall configurations.

---

### UDP Discovery

```bash
sudo nmap -PU53,161,162 -sn target
```

Useful for discovering systems through UDP services.

---

## Detection Opportunities

SOC teams may identify Nmap activity through:

### IOC

* Sequential Connections
* Multiple Ports Probed
* Large Numbers of Hosts Scanned
* High ICMP Volumes
* Unusual ARP Activity

---

### ATT&CK Mapping

* T1595 – Active Scanning
* T1046 – Network Service Discovery
* T1018 – Remote System Discovery

---

# Room 2: Snort

---

## Task 3: IDS and IPS Concepts

### Important Distinction

| Mode | Purpose                       |
| ---- | ----------------------------- |
| HIDS | Detect activity on a host     |
| HIPS | Prevent activity on a host    |
| NIDS | Detect activity on a network  |
| NIPS | Prevent activity on a network |

---

### What Confused Me?

Initially, I mixed up detection and prevention.

I assumed IDS and IPS were identical.

After completing the room, I understood:

* IDS observes.
* IPS can actively block.

---

## Task 4: First Interaction with Snort

### Key Learning

Configuration files directly affect:

* Rules Loaded
* Detection Coverage
* Alert Quality

Example:

```bash
snort -T -c /etc/snort/snort.conf
```

---

### SOC Relevance

Poor configurations can result in:

* Missed Attacks
* False Positives
* Reduced Visibility

---

## Task 5 & 6: Sniffer and Packet Logger Modes

### What I Learned

Snort can operate as:

1. Packet Sniffer
2. Packet Logger
3. IDS
4. IPS

---

### Investigation Skills Practiced

* Source Port Analysis
* TCP Session Analysis
* HTTP Request Analysis
* Packet Header Analysis

---

### Real-World SOC Use Cases

* Malware Investigation
* PCAP Analysis
* Threat Hunting
* Incident Response

---

## Task 7 & 8: IDS and PCAP Investigation

### Key Findings

Detected:

* HTTP GET Requests
* TCP Sessions
* Response Headers
* Security Alerts

Generated Alerts:

* 68
* 340
* 1020
* 170

depending on rule configuration and dataset.

---

### SOC Analyst Lesson

Alert quantity alone does not determine severity.

Analysts must investigate:

* Context
* Source
* Destination
* Protocol
* Business Impact

---

## Task 9: Snort Rule Creation

This was the most valuable section.

I learned how detection rules are built.

Example Rule Structure:

```snort
alert icmp any any -> any any (msg:"ICMP Alert"; sid:1000001; rev:1;)
```

---

### Detection Logic Learned

Detect:

* Specific IP IDs
* SYN Packets
* PUSH-ACK Packets
* Loopback Traffic
* Protocol Anomalies

---

### Analyst Mindset

Every SIEM alert originates from some form of detection logic.

Understanding Snort rules helps analysts understand:

* Why an alert fired
* What triggered detection
* How false positives occur

---

## IOC Examples From These Rooms

### Reconnaissance

* ARP Scans
* ICMP Sweeps
* Host Discovery

### Enumeration

* Multiple Port Probes
* Service Discovery

### Suspicious Traffic

* Repeated SYN Packets
* Unusual TCP Flags
* High Connection Rates

### Detection Indicators

* Snort Alerts
* Signature Matches
* Protocol Violations

---

## 🛡️ SOC Analyst L1: Tricky Connection

The most important connection between these rooms is:

### Nmap Creates Activity

Examples:

* ICMP Requests
* SYN Packets
* ARP Requests

### Snort Detects Activity

Examples:

* Scan Detection
* Signature Matches
* Alert Generation

In a real SOC:

An attacker may use Nmap.

A defender may detect it using Snort.

Understanding both sides helps analysts recognize suspicious network behavior more effectively.

---

## Next Learning Steps

After these rooms, I should continue learning:

1. Nmap Basic Port Scans
2. TCP Three-Way Handshake
3. Wireshark
4. Zeek
5. Suricata
6. IDS Tuning
7. Detection Engineering
8. Network Traffic Analysis
9. Threat Hunting
10. MITRE ATT&CK

---

## 💼 Critical Thinking Question

### Scenario

A Snort sensor generates alerts indicating multiple TCP SYN packets targeting ports 22, 80, 443, 445, and 3389 across dozens of hosts within a few minutes.

### Question

As a SOC Analyst L1:

* What activity might be occurring?
* Which logs would you review?
* What evidence would support your conclusion?

### Answer Concept

This activity strongly resembles network reconnaissance or host discovery.

Investigation:

1. Review Snort alerts.
2. Check firewall logs.
3. Identify source IP.
4. Count unique destination hosts.
5. Identify targeted ports.
6. Search for follow-on activity.
7. Escalate if scanning is unauthorized.

### ATT&CK Mapping

* T1595 – Active Scanning
* T1046 – Network Service Discovery

---

## Final Reflection

These rooms taught me that network attacks begin long before exploitation occurs.

Nmap showed me how attackers discover systems.

Snort showed me how defenders identify and investigate that activity.

As an aspiring SOC Analyst L1, understanding both perspectives helps me connect reconnaissance, detection, traffic analysis, and incident investigation into a complete security workflow.

