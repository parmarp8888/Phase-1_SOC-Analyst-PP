
# Walkthrough:**[[Nmap](https://tryhackme.com/room/nmap01) / [Snort](https://tryhackme.com/room/snort)]**

# **Day-17 A | Room Walkthrough: Nmap Live Host Discovery**

> **Learning Goal:** Understand how Nmap discovers live hosts using ARP, ICMP, TCP, and UDP probes while thinking like both a penetration tester and a SOC Analyst.

---

## Who is this for?

This lab is designed for beginners, SOC analysts, blue team defenders, penetration testers, cybersecurity students, and anyone who wants to understand how computers discover each other on a network. Before scanning ports or exploiting systems, security professionals first identify which devices are alive. This process is called **Host Discovery**. Understanding host discovery helps defenders recognize reconnaissance activity and enables attackers to identify potential targets. For SOC analysts, recognizing scanning behavior is often the first indication of malicious activity.

---

## What is Host Discovery?

Host discovery is the process of identifying which systems are currently online without performing a full port scan. Instead of immediately checking every port, Nmap first determines whether a host responds to network probes such as ARP Requests, ICMP Echo Requests, TCP SYN packets, TCP ACK packets, or UDP packets. If the target replies, it is considered alive. This approach saves time, reduces unnecessary traffic, and helps security teams focus only on active systems.

---

## Why is it Important?

In enterprise environments, attackers rarely launch attacks blindly. They first map the network, identify live hosts, and prioritize valuable systems. Defensive teams use the same techniques for asset inventory and network auditing. From a SOC perspective, detecting repeated host discovery activity may indicate the reconnaissance phase of an attack, allowing defenders to respond before exploitation begins.

---

### What confused me?

Initially, I assumed that a "ping" was the only method for discovering online systems. During this study, I learned that Nmap can use ARP, ICMP, TCP, and UDP probes depending on the network environment. I was also confused about why ARP works only within the local network while ICMP can travel across routed networks. After analyzing packet flow and network layers, I understood that ARP operates at Layer 2 and cannot cross routers.

### What I learned

Host discovery is not a single technique but a collection of different probing methods. Each method has advantages and limitations depending on firewalls, routers, and operating system behavior.

### Real-world relevance

Corporate attackers often perform network reconnaissance before launching attacks. SOC analysts monitor unusual ICMP bursts, ARP sweeps, or TCP SYN probes because these activities frequently precede privilege escalation or lateral movement.

---

# Important Nmap Commands

| Command          | Purpose                            |
| ---------------- | ---------------------------------- |
| `nmap -h`        | Show all available Nmap options    |
| `nmap -sn`       | Host discovery only (no port scan) |
| `nmap -PR`       | ARP Ping Scan                      |
| `nmap -PE`       | ICMP Echo Request                  |
| `nmap -PP`       | ICMP Timestamp Request             |
| `nmap -PM`       | ICMP Address Mask Request          |
| `nmap -PS80,443` | TCP SYN Ping                       |
| `nmap -PA80`     | TCP ACK Ping                       |
| `nmap -PU53`     | UDP Ping                           |
| `nmap -R`        | Reverse DNS Lookup                 |
| `nmap -n`        | Disable DNS Resolution             |

---

# Task 1 – Understanding Host Discovery

### Question

**What is Host Discovery?**

### Answer

Host Discovery is the process of identifying which systems on a network are currently online before performing detailed scanning.

### Explanation

Think of Host Discovery like knocking on every door in a neighborhood. If someone answers, you know the house is occupied. Nmap does the same by sending different types of packets and waiting for responses. This process is efficient because it avoids scanning devices that are offline.

---

### SOC Analyst Note

Repeated host discovery attempts across multiple IP addresses may indicate reconnaissance.

**Possible IOC**

* Large number of ICMP Echo Requests
* Sequential ARP Requests
* Multiple TCP SYN packets to many hosts

**MITRE ATT&CK**

* T1595 Active Scanning

---

# Task 2 – Understanding ARP Discovery

### Question

**Why does ARP only work inside the local network?**

### Answer

Because routers do not forward ARP broadcast packets.

### Explanation

ARP (Address Resolution Protocol) maps IP addresses to MAC addresses. When a device wants to communicate with another device on the same subnet, it sends a broadcast asking, "Who has this IP address?" Only devices within the same broadcast domain receive the request. Routers stop these broadcasts, which is why ARP cannot discover hosts on remote networks.

---

### Enterprise Example

A SOC analyst notices one workstation sending ARP Requests to every IP address in the subnet. This behavior may indicate that an attacker is performing local network reconnaissance.

---

### Detection Opportunities

* ARP Sweep Detection
* Network Discovery Alert
* Excessive Broadcast Traffic

---

### MITRE ATT&CK

* T1016 System Network Configuration Discovery
* T1595 Active Scanning

---

# Interview Question

### Scenario

A SIEM generates an alert indicating that one workstation sent ARP Requests to every IP address within five minutes.

### Sample Answer

I would first verify whether the activity originated from an authorized vulnerability scanner. If not, I would investigate the source endpoint, identify the logged-in user, review recent processes, correlate firewall and endpoint logs, and determine whether the scan is part of broader reconnaissance. If suspicious, I would escalate the incident to the Incident Response team with supporting evidence.

---

# **Day-17 B | Room Walkthrough: Snort – Snort "Network Intrusion Detection & Packet Analysis"

> **Learning Goal:** Build a strong understanding of how Snort monitors network traffic, detects suspicious activities using rules, and helps SOC analysts investigate potential security incidents in real-world enterprise environments.

---

## **Who is this for?**

This room is designed for cybersecurity beginners, SOC Analysts (L1/L2), Blue Team defenders, Network Security Engineers, and anyone interested in learning how Intrusion Detection Systems (IDS) and Intrusion Prevention Systems (IPS) protect enterprise networks. Before responding to attacks, defenders must first detect malicious behavior. Snort provides this visibility by inspecting network packets in real time and generating alerts based on predefined or custom detection rules.

---

## **What is Snort?**

Snort is an open-source Network Intrusion Detection and Prevention System (NIDS/NIPS) developed to analyze network traffic and detect suspicious or malicious activity. It captures packets, compares them against detection rules, and generates alerts when traffic matches known attack signatures or suspicious behavior. Unlike a firewall that mainly controls traffic flow, Snort deeply inspects packets to identify attacks such as port scans, malware communication, brute-force attempts, web attacks, and protocol anomalies.

---

## **Why is it Important?**

Most enterprise cyberattacks begin with network communication. Malware contacts command-and-control servers, attackers scan ports, and compromised hosts communicate with internal systems. Snort helps security teams identify these activities before they cause significant damage. It is widely used with Security Information and Event Management (SIEM) platforms, where Snort alerts are correlated with endpoint logs, firewall logs, and authentication events to provide complete visibility of an attack.

---


### **What confused me?**

Initially, I believed Snort only captured packets like Wireshark. After completing the exercises, I realized Snort does much more than packet capture. It analyzes packets using detection rules, generates alerts, and can even block malicious traffic when configured in IPS mode. Understanding the rule syntax (`action`, `protocol`, `source`, `destination`, `options`) was initially challenging, but practicing simple rules made the logic much clearer.

### **What I learned**

I learned that effective detection depends on well-written rules. A poorly written rule may miss attacks or create excessive false positives. I also understood how packet inspection helps identify suspicious HTTP requests, unusual TCP flags, and protocol misuse.

### **Real-world relevance**

Enterprise SOC teams rely on IDS alerts to detect reconnaissance, malware downloads, exploit attempts, and lateral movement. Snort provides valuable evidence that helps analysts investigate incidents faster.

---

# **Task 1 – Introduction**

### **Question**

**What is the primary purpose of Snort?**

### **Answer**

To monitor network traffic, detect suspicious activity, and generate security alerts using rule-based inspection.

### **Explanation**

Snort continuously analyzes network packets and compares them with configured detection rules. When traffic matches a rule, Snort creates an alert containing details such as source IP, destination IP, protocol, timestamp, and the rule that triggered the event. These alerts help analysts quickly identify potential threats without manually inspecting every packet.

---

# **Task 2 – IDS vs IPS**

### **Question**

**What is the difference between IDS and IPS?**

### **Answer**

* **IDS (Intrusion Detection System)** detects malicious activity and generates alerts.
* **IPS (Intrusion Prevention System)** detects malicious activity and actively blocks or drops the traffic.

### **Explanation**

An IDS acts like a security camera—it observes and reports suspicious activity. An IPS acts like a security guard—it observes and immediately stops malicious actions. Organizations often deploy IDS in monitoring environments and IPS at network gateways where automatic prevention is required.

---

### **SOC Analyst Detection Opportunity**

Monitor repeated alerts from the same source IP. Multiple IDS alerts combined with failed logins or endpoint detections may indicate an active intrusion attempt requiring immediate investigation.

---

# **Task 3 – Sniffer Mode**

### **Question**

**Why use Sniffer Mode?**

### **Answer**

To observe live network packets for troubleshooting, learning, and traffic analysis.

### **Explanation**

Sniffer Mode displays packet information directly in the terminal without saving or analyzing it against rules. It is useful for understanding communication between hosts, verifying connectivity, and identifying abnormal protocols during incident investigations.

---

# **Task 4 – Packet Logger Mode**

### **Question**

**Why save captured packets?**

### **Answer**

To preserve network evidence for later forensic analysis.

### **Explanation**

Packet Logger Mode writes captured packets into log or PCAP files. Analysts can reopen these files using Snort or Wireshark to examine HTTP requests, DNS queries, TCP handshakes, and malware communication. Preserving packet captures is important because attackers may disconnect before analysts begin investigating.

---

# **Task 5 – IDS Rule Writing**

### **Question**

**What is a Snort rule?**

### **Answer**

A Snort rule defines conditions that identify suspicious network traffic and specify the action to perform when a match occurs.

### **Example Rule**

```text
alert tcp any any -> any 80 (msg:"HTTP Traffic Detected"; sid:1000001; rev:1;)
```

This rule generates an alert whenever TCP traffic is observed on destination port 80.

---

### **Important Rule Components**

* **Action** – alert, log, drop, reject
* **Protocol** – TCP, UDP, ICMP
* **Source/Destination IP**
* **Ports**
* **Rule Options** – message, SID, revision, content matching

---

# 🛡 IOC Examples

* Multiple TCP SYN packets
* Repeated HTTP GET requests
* DNS tunneling attempts
* ICMP scanning
* Suspicious User-Agent strings
* Unexpected outbound connections
* Large number of failed connections
* Internal host communicating with known malicious IP

---

#  Common TTPs (MITRE ATT&CK)

| Technique                  | ATT&CK ID |
| -------------------------- | --------- |
| Active Scanning            | T1595     |
| Network Service Discovery  | T1046     |
| Command and Control        | T1071     |
| Application Layer Protocol | T1071.001 |
| Data Exfiltration          | T1041     |

---

#  Detection Opportunities

A SOC analyst should investigate:

* Excessive ICMP traffic
* Large ARP broadcasts
* TCP SYN scans
* HTTP requests to suspicious domains
* Repeated DNS lookups
* Multiple alerts from the same endpoint
* Unexpected outbound connections after a successful login

Correlating Snort alerts with firewall logs, endpoint telemetry, DNS logs, and authentication records provides a more complete understanding of attacker behavior.

---

#  Scenario-Based Interview Question

### **Scenario**

Your SIEM receives 150 Snort alerts from a single workstation within ten minutes. Most alerts indicate repeated TCP SYN packets targeting multiple internal servers.

### **Sample Answer**

I would first validate whether the workstation belongs to an authorized vulnerability scanner or network administrator. If not, I would identify the user and running processes on the endpoint, correlate the alerts with firewall, DNS, and endpoint logs, determine whether the activity resembles reconnaissance or malware, isolate the affected host if necessary, and escalate the incident with supporting evidence. Proper documentation of timestamps, affected systems, and observed indicators is essential for incident response.

----
Task 1 – Introduction

Learn how Nmap identifies live hosts before performing port scanning. Host discovery is the first step of reconnaissance in penetration testing and cyber attacks.

AI SOC Professor Explanation

Nmap first checks which devices are online before scanning their ports. This reduces scanning time and avoids unnecessary traffic. SOC analysts monitor host discovery because attackers often perform reconnaissance before exploiting systems.

---

Task 2 – Subnetworks (ARP Request)

<img width="395" height="782" alt="image" src="https://github.com/user-attachments/assets/5a81e41c-985a-4f77-a247-2346ee94520a" />

- From **computer1**
- To **computer1** (to indicate it is broadcast)
- Packet Type: “ARP Request”
- Data: computer6 (because we are asking for **computer6's** MAC address using ARP Request)

Question

How many devices can see the ARP Request?

Answer

4

Explanation

ARP Requests are broadcast packets, meaning every device inside the same subnet receives them. Devices outside the subnet never see these packets because routers do not forward ARP broadcasts.

Question

Did computer6 receive the ARP Request?

Answer

Nay

Explanation

Computer 6 is located on a different subnet, so the ARP Request never reached it. ARP communication only works within the local broadcast domain.

Question
 From **computer4**
- To **computer4** (to indicate it is broadcast)
- Packet Type: “ARP Request”
- Data: **computer6** (because we are asking for **computer6's** MAC address using ARP Request)

  <img width="395" height="782" alt="image" src="https://github.com/user-attachments/assets/5d79c9be-dcd0-4326-8ec2-167068b2ca95" />
  

How many devices can see the second ARP Request?

Answer

4

Explanation

Again, only devices within Computer4's subnet receive the broadcast ARP Request.

Question

Did computer6 reply?

Answer

Yea

Explanation

Since Computer4 and Computer6 are now in the same subnet, Computer6 receives the request and responds with its MAC address.

SOC Note: Excessive ARP Requests may indicate an ARP Sweep or local network reconnaissance.

----

Task 3 – Understanding Host Discovery through TCP/IP
Question

What packet was sent before Ping?

Answer

ARP Request

Explanation

Before sending an ICMP Ping, the sender must first discover the destination MAC address using ARP.

Question

What packet was received before Ping?

Answer

ARP Response

Explanation

The destination replies with its MAC address, allowing the Ping packet to be delivered successfully.

Question

How many computers replied to the Ping?

Answer

1

Explanation

Only the destination host replies with an ICMP Echo Reply.

Question

Who replied first to the first ARP Request?

Answer

Router

Explanation

When communicating with another subnet, the sender first learns the router's MAC address because the router acts as the default gateway.

Question

Who replied to the second ARP Request?

Answer

Computer5

Explanation

Once traffic reaches the destination subnet, the router performs another ARP Request to locate Computer5.

Question

Did another Ping require new ARP Requests?

Answer

Nay

Explanation

The MAC addresses were already stored in the ARP Cache, so another ARP lookup was unnecessary.

SOC Note: Frequent ARP Requests from one host may indicate reconnaissance activity.

----

Task 4 – Enumerating Targets
Question

First IP scanned for 10.10.12.13/29?

Answer

10.10.12.8

Explanation

A /29 subnet contains 8 IP addresses. Nmap begins scanning from the network address (10.10.12.8).

Question

How many IPs in 10.10.0-255.101-125?

Answer

6400

Explanation

Nmap expands the specified IP ranges and scans every generated address automatically.

----

Task 5 – Nmap Host Discovery Using ARP
Question

How many hosts were found alive?

Answer

(Lab-specific result)

Explanation

Using -PR, Nmap sends ARP Requests to every IP in the subnet. Any device replying with an ARP Response is marked as Alive.

sudo nmap -PR -sn <target>/24

SOC Detection: Large ARP Sweeps may indicate internal reconnaissance.

----

Task 6 – Nmap Host Discovery Using ICMP
Question

ICMP Timestamp option?

Answer

-PP

Explanation

Uses ICMP Timestamp Requests to determine whether a host is online.

Question

ICMP Address Mask option?

Answer

-PM

Explanation

Sends ICMP Address Mask Requests for host discovery.

Question

ICMP Echo option?

Answer

-PE

Explanation

Uses the standard ICMP Echo Request ("Ping") to discover live systems.

SOC Note: Large volumes of ICMP traffic often indicate network scanning.

----

Task 7 – TCP & UDP Host Discovery
Question

TCP Ping that doesn't require privileges?

Answer

TCP Connect Ping (-PA)

Explanation

ACK Ping can be performed without administrative privileges in many environments.

Question

TCP Ping requiring privileges?

Answer

TCP SYN Ping (-PS)

Explanation

Raw SYN packets require elevated privileges because they bypass the normal TCP stack.

Question

TCP SYN Ping on Telnet?

Answer

-PS23

Explanation

Port 23 is Telnet, so Nmap sends SYN probes specifically to that port.

----
Task 8 – Reverse DNS Lookup
Question

Which option enables Reverse DNS?

Answer

-R

Explanation

Nmap attempts to resolve every IP address into a hostname, helping identify servers and devices more easily.

nmap -R 10.10.10.0/24

---
Task 9 – Summary
| Key Nmap Commands | Command	Purpose |
|-------------------|-----------------|
|-PR|	ARP Host Discovery|
|-PE|	ICMP Echo Scan|
|-PP|	ICMP Timestamp Scan|
|-PM|	ICMP Address Mask Scan|
|-PS|	TCP SYN Ping|
|-PA|	TCP ACK Ping|
|-PU|	UDP Ping|
|-sn|	Host Discovery Only|
|-n|	Disable DNS Lookup|
|-R|	Reverse DNS Lookup |


# **Task 1 – Introduction**

Understand what Snort is, how it works, and why SOC analysts use it in enterprise environments.


Before learning detection rules, understand the purpose of Snort. It acts like a security guard that continuously watches network traffic. Every packet entering or leaving the network is inspected. If the traffic matches a suspicious pattern or attack signature, Snort generates an alert. In enterprise SOCs, these alerts are forwarded to SIEM platforms for further investigation.

---

# **Task 2 – Interactive Material and VM**

### Question

Navigate to the Task-Exercises folder and run:

```bash
./.easy.sh
```

### Answer

```
Too Easy!
```

This task simply verifies that your virtual machine and lab environment are working correctly. Running the script confirms that all required files, permissions, and dependencies are available before beginning the Snort exercises. Always verify your environment first, as incorrect configurations can cause misleading errors during security investigations.

---

# **Task 3 – Introduction to IDS/IPS**

## Question

Which Snort mode can help you stop threats on a local machine?

### Answer

**HIPS**

### Explanation

**HIPS (Host Intrusion Prevention System)** runs directly on an individual computer. It monitors local processes, files, and system activities. Unlike detection-only tools, HIPS can actively block malicious actions before they damage the system.

---

## Question

Which Snort mode can detect threats on a local network?

### Answer

**NIDS**

### Explanation

A **Network Intrusion Detection System (NIDS)** monitors network traffic across multiple devices. It analyzes packets moving through the network and generates alerts when suspicious behavior is detected. NIDS does not block traffic; it only reports it.

---

## Question

Which Snort mode can detect threats on a local machine?

### Answer

**HIDS**

### Explanation

**HIDS (Host Intrusion Detection System)** monitors logs, processes, registry changes, and file integrity on a single computer. It detects suspicious activity but does not automatically stop attacks.

---

## Question

Which Snort mode can stop threats on a local network?

### Answer

**NIPS**

### Explanation

**Network Intrusion Prevention System (NIPS)** sits inline with network traffic. It analyzes packets and can automatically drop or block malicious connections before they reach their destination.

---

## Question

Which mode works similarly to NIPS?

### Answer

**NBA**

### Explanation

**Network Behavior Analysis (NBA)** detects abnormal network behavior instead of relying only on signatures. It identifies unusual traffic patterns that may indicate malware, insider threats, or data exfiltration.

---

## Question

According to Snort, what type of NIPS is it?

### Answer

**full-blown**

### Explanation

Snort is described as a **full-blown NIPS**, meaning it provides complete intrusion prevention capabilities instead of offering only basic packet filtering.

---

## Question

NBA training period is called?

### Answer

**Baselining**

### Explanation

**Baselining** means learning what normal network traffic looks like. Once normal behavior is established, unusual traffic becomes easier to detect.

---

# **Task 4 – First Interaction with Snort**

## Question

What is the build number?

### Answer

**149**

### Explanation

The build number identifies the installed Snort version. SOC teams verify software versions to ensure compatibility with rule sets and security updates.

---

## Question

How many rules are loaded using `snort.conf`?

### Answer

**4151**

### Explanation

A large rule set means Snort can detect thousands of attack signatures, including malware, exploits, reconnaissance, and suspicious protocols.

---

## Question

How many rules are loaded using `snortv2.conf`?

### Answer

**1**

### Explanation

This configuration loads only one rule, making it useful for learning how custom rules behave without interference from thousands of default signatures.

---

# **Task 5 – Operation Mode 1: Sniffer Mode**

Sniffer Mode displays live packets without saving them. It helps analysts observe protocols, IP addresses, ports, and communication patterns in real time. This mode is useful for troubleshooting and understanding how network communication works before creating detection rules.

---

# **Task 6 – Operation Mode 2: Packet Logger Mode**

## Question

Source port connecting to port 53?

### Answer

**3009**

### Explanation

Port **53** is used by DNS services. The source port **3009** is a temporary client port chosen by the operating system. Analysts often investigate DNS traffic because malware frequently communicates through DNS.

---

## Question

IP ID of the 10th packet?

### Answer

**49313**

### Explanation

The **IP Identification (IP ID)** field helps identify fragmented IP packets. During investigations, analysts use IP IDs to correlate fragmented packets and analyze suspicious traffic.

---

## Question

Referer of the 4th packet?

### Answer

**[http://www.ethereal.com/development.html](http://www.ethereal.com/development.html)**

### Explanation

The **HTTP Referer** header indicates which webpage directed the user to another resource. Analysts use this information to reconstruct browsing activity during forensic investigations.

---

## Question

ACK number of the 8th packet?

### Answer

**0x38AFFFF3**

### Explanation

The TCP **Acknowledgment Number** confirms successful packet delivery during TCP communication. It is useful for analyzing TCP sessions and troubleshooting network issues.

---

## Question

How many TCP Port 80 packets?

### Answer

**41**

### Explanation

Port **80** represents HTTP traffic. Counting HTTP packets helps analysts estimate web activity and identify suspicious web-based attacks.

---

# **Task 7 – Operation Mode 3: IDS/IPS**

### HTTP GET Methods

**Answer:** **2**

**Explanation:** Two HTTP GET requests indicate two client requests sent to a web server.

---

### TCP Segments Queued

**Answer:** **18**

**Explanation:** Queued TCP segments indicate packets waiting to be processed or reassembled before inspection.

---

### HTTP Response Headers

**Answer:** **3**

**Explanation:** These headers contain server responses such as content type, server information, and response status.

---

### Generated Alerts

**Answers:** **68**, **340**, **1020**

**Explanation:** Increasing alert counts demonstrate how different rule sets detect different types of suspicious traffic. More rules generally produce more alerts.

---

### TCP Packets

**Answer:** **82**

**Explanation:** These TCP packets were inspected by Snort and evaluated against detection rules.

---

# **Task 8 – PCAP Investigation**

## Question

Generated alerts?

### Answer

**170**

### Explanation

Snort analyzed the PCAP file offline and generated 170 alerts based on matching detection rules. Offline PCAP analysis is common during incident response.

---

# **Task 9 – Snort Rule Structure**

## Question

Request name for IP ID **35369**?

### Answer

**TIMESTAMP REQUEST**

### Explanation

This ICMP Timestamp Request is used to query a host's system time. Although legitimate, attackers may use timestamp requests during reconnaissance.

---

## Question

Packets with SYN flag?

### Answer

**1**

### Explanation

A SYN packet begins the TCP three-way handshake. Multiple SYN packets may indicate port scanning.

---

## Question

Packets with PUSH-ACK flags?

### Answer

**216**

### Explanation

PSH-ACK packets deliver application data immediately to the receiving application. High volumes are common in active TCP sessions.

---

## Question

Packets with identical source and destination IP?

### Answer

**10**

### Explanation

Traffic where source and destination IPs are identical is uncommon and may indicate testing, loopback communication, or misconfiguration.

---

## Question

Which rule option changes after modifying a rule?

### Answer

**rev**

### Explanation

The **revision (rev)** number should be increased whenever a detection rule is modified. This helps analysts track rule versions and maintain consistent rule management.

---

# **Task 10 – Snort Operation Logic**

### AI SOC Professor Note

Always remember Snort follows a simple workflow:

**Capture → Decode → Preprocess → Match Rules → Generate Alert → Log Evidence**

Understanding this sequence helps analysts troubleshoot detection issues and optimize rule performance.

---

# **Task 11 – Conclusion**

### Final Learning Summary
Nmap (Network Mapper) is an open-source network scanning and reconnaissance tool used by penetration testers, system administrators, and SOC analysts to discover live hosts, identify open ports, detect running services, determine operating systems, and map network infrastructure. It supports multiple host discovery techniques such as ARP, ICMP, TCP, and UDP scans, helping security professionals assess network exposure while enabling defenders to detect reconnaissance activity before an attack progresses.

Snort is an open-source Network Intrusion Detection and Prevention System (NIDS/NIPS) that monitors network traffic in real time to detect malicious or suspicious activities. It analyzes packets using signature-based detection rules, generates alerts for potential threats, logs network events for forensic analysis, and can block attacks when configured in IPS mode. Widely used in Security Operations Centers (SOCs), Snort helps identify reconnaissance, malware communication, web attacks, and other network-based threats, making it a key tool for network monitoring and incident response.

-----
