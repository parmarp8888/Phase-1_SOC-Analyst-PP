# Room Walkthrough: [Network Security Essentials](https://tryhackme.com/room/networksecurityessentials)
## 🎯 Day 15 Objective

As part of my SOC Analyst L1 learning journey, this room helped me understand how security teams monitor and defend enterprise networks against external threats.
<img align="right" width="600" height="600" alt="image" src="https://github.com/user-attachments/assets/1ac979a6-1a67-42da-b5fb-636ba971e6a4" />

Rather than focusing on individual tools, this room demonstrated how analysts use network visibility, perimeter monitoring, firewall logs, IDS alerts, VPN logs, and threat detection techniques to identify malicious activity.

Most importantly, it showed how a real attack progresses from reconnaissance to compromise, command-and-control communication, and data exfiltration.

---


## Who is this for?

* SOC Analysts
* Security Engineers
* Incident Responders
* Network Administrators
* Threat Hunters
* Anyone learning network security

---

## What is Network Security?

Network Security is the process of protecting network infrastructure, systems, services, and data from unauthorized access, attacks, and misuse.

It involves:

* Monitoring traffic
* Controlling access
* Detecting threats
* Investigating suspicious activity
* Responding to security incidents

---

## Where does this apply?

Network Security is used in:

* Corporate Networks
* Data Centers
* Cloud Environments
* Government Networks
* Financial Institutions
* Healthcare Organizations

Any environment connected to a network requires security monitoring.

---

## When should it be learned?

Network Security should be learned early in a SOC career because almost every cyber incident leaves traces within network communications.

Without understanding networks, analysts struggle to investigate alerts effectively.

---

## Why is it important?

Attackers rarely appear suddenly inside an organization.

Most attacks begin with:

* Scanning
* Reconnaissance
* Credential attacks
* Initial access attempts

These activities often generate network evidence before an actual compromise occurs.

Detecting those early indicators can prevent larger incidents.

---

## How does it work?

Organizations collect data from:

* Firewalls
* IDS/IPS
* VPN Gateways
* Web Application Firewalls (WAF)
* Endpoint Systems
* DNS Logs
* Proxy Logs
* SIEM Platforms

Analysts correlate information from these sources to identify suspicious patterns and investigate threats.

---

# Task 4: Network Visibility

One of the most important lessons from this room was understanding visibility.

Security teams cannot protect what they cannot see.

Network visibility is essential in cyber security. It's the ability to monitor and understand what's happening across your network. It's a core principle for security analysts: you can't defend what you can't see. Effective visibility enables threat detection, incident investigation, and a strong security posture. It’s not about tracking every packet but having the tools to build a clear picture. Without it, you're blind to potential threats.

The network visibility comes from two primary sources of logs. You must understand the difference between them to piece together an attack timeline effectively.

---

## Host-Centric Logs
<img width="1000" height="1000" alt="image" src="https://github.com/user-attachments/assets/e8c68737-d56e-430c-84f6-592d92b4a2dc" />

Host logs provide information about activities occurring directly on systems.

Examples:

* Windows Event Logs
* Linux Syslogs
* Application Logs
* Endpoint Security Logs

### What Host Logs Reveal

* User Logins
* Process Creation
* Software Execution
* File Modifications
* Privilege Changes

Key Host-Centric Log Sources
. Operating System Logs: Windows Event Logs, Linux syslog, macOS logs. These record events like user logons, process creation, service startups, and failed login attempts.

. Application Logs: Logs from software running on the host, such as web servers (Apache, Nginx), databases (MySQL, MSSQL), and other applications.

. Security Tool Logs: Logs from antivirus software, Endpoint Detection and Response (EDR) agents, and host-based intrusion detection systems (HIDS).
---

## Network-Centric Logs
<img width="1000" height="1000" alt="image" src="https://github.com/user-attachments/assets/6b94bccd-084c-4cd9-991a-c146aaf3680e" />

Network logs provide information about communications between systems.

Examples:

* Firewall Logs
* IDS Logs
* VPN Logs
* Proxy Logs
* DNS Logs

### What Network Logs Reveal

* Source IP
* Destination IP
* Protocols
* Connection Attempts
* Traffic Patterns

Key Network-Centric Log Sources

* Firewalls: The firewall logs provide a record of every connection allowed or denied based on pre-defined security rules. Firewall logs are the first place to look for unauthorized connection attempts from the internet.

* Intrusion Detection/Prevention Systems (IDS/IPS): They monitor network traffic for patterns that match known malicious attacks (signatures) or for unusual behavior (anomalies). Their logs are critical for detecting active attacks in real time.

* Routers and Switches: While not logging in the traditional sense, devices like routers can generate flow data. This data summarizes traffic conversations, telling who talked to whom, for how long, and how much data was sent. It’s excellent for getting a high-level overview of network activity.

* Web Proxies: These logs are a goldmine for organizations that route their web traffic through proxies. They record every website user's visit, providing visibility into web-based threats, policy violations, or data exfiltration attempts.

* VPN: These devices manage remote access for employees. Their logs track who is connecting to the corporate network, from where, and at what time, which is essential for monitoring the security of remote connections.


---

## SOC Analyst Perspective

Host logs answer:

> What happened on the device?

Network logs answer:

> How did communication occur?

Effective investigations require both perspectives.

---

# Task 5: Network Perimeter

The network perimeter is often the first location where attackers become visible.

Attackers frequently probe external-facing systems searching for weaknesses.
<img width="600" height="600" alt="image" src="https://github.com/user-attachments/assets/3e8a1d24-e8db-4917-b9e9-01173dbc711e" />

---

## Common Perimeter Threats

### Port Scanning

Attackers identify exposed services.

Example:

* SSH (22)
* RDP (3389)
* SMB (445)

### Brute Force Attacks

Attackers attempt multiple username-password combinations.

### Web Exploitation Attempts

Attackers target:

* SQL Injection
* Cross-Site Scripting
* Directory Traversal

### Malware Delivery

Compromised websites or phishing infrastructure may be used to deliver malware.

---

# Task 6: Monitoring and Protecting the Perimeter

This task simulated several common attack scenarios.

---

## Scenario 1: Probing for Ports (Port Scanning)
An attacker is testing your network to see which ports are open or closed, while the firewall is doing its job and blocking them.

Firewall Log

2025-09-22 08:30:04 ALLOW TCP 198.51.100.45:49876 -> 10.0.0.51:80
2025-09-22 08:30:05 BLOCK TCP 203.0.113.10:50001 -> 10.0.0.20:21
2025-09-22 08:30:06 BLOCK TCP 203.0.113.10:50002 -> 10.0.0.20:22
2025-09-22 08:30:07 ALLOW TCP 192.0.2.115:51235 -> 10.0.0.50:443
2025-09-22 08:30:08 BLOCK TCP 203.0.113.10:50003 -> 10.0.0.20:23
2025-09-22 08:30:09 BLOCK TCP 203.0.113.10:50004 -> 10.0.0.20:25
2025-09-22 08:30:10 ALLOW TCP 198.51.100.92:51111 -> 10.0.0.50:443
2025-09-22 08:30:11 BLOCK TCP 203.0.113.10:50005 -> 10.0.0.20:53

Log Breakdown

The same external IP (203.0.113.10) is trying to connect to multiple ports on the same internal machine quickly.
Analyst's Verdict: This is a classic port scan. The attacker is looking for an open service to target.

### Question
Which IP performed the port scan?

### Answer
203.0.113.10

### What Happened?

One source repeatedly attempted connections against multiple ports.
This behavior is commonly associated with reconnaissance activity.

---

### IOC

* Source IP: 203.0.113.10
* Multiple Port Probes
* High Connection Volume

### Detection Opportunities

Monitor:
* Sequential Port Access
* High Connection Rates
* Multiple Destination Ports


---

### MITRE ATT&CK

* T1595 – Active Scanning

---

## Scenario 2: Attacking the Web Server (SQL Injection)
The company website is being attacked. An intrusion detection system (IDS) provides more details than a firewall, identifying the type of attack.

WAF (Web Application Firewall) Logs
timestamp=2025-09-22T09:14:44Z src_ip=192.0.2.130 action=ALLOW request="GET /index.html"
timestamp=2025-09-22T09:14:45Z src_ip=198.51.100.92 action=ALLOW request="GET /products.php?id=9"
timestamp=2025-09-22T09:14:46Z src_ip=[REDACTED] action=BLOCK request="GET /search.php?q=<script>alert('XSS')</script>" rule_id=941100 attack_type="XSS"
timestamp=2025-09-22T09:14:47Z src_ip=192.0.2.140 action=ALLOW request="GET /css/style.css"
timestamp=2025-09-22T09:15:42Z src_ip=[REDACTED] action=BLOCK request="GET /../../../../etc/passwd" rule_id=930120 attack_type="Directory Traversal"
...
.....
Log Breakdown

This log shows a mix of ALLOW and BLOCK actions. An analyst can filter for action=BLOCK to immediately find threats. The WAF does the hard work by blocking the request and telling us why.

The attack_type="SQL Injection" alert shows the attacker trying to dump database information.

The attack_type="XSS" alert is an attempt to inject malicious scripts.

The attack_type="Directory Traversal" alert shows an attempt to read sensitive server files.

Analyst's Verdict: The WAF successfully identifies and blocks multiple web attack types from suspicious IP. This is a high-confidence alert that an attacker is actively targeting the website.

---

### Question
Which source IP generated all blocked web attacks?

### Answer

198.51.100.12

### What Happened?
The Web Application Firewall detected repeated malicious requests targeting web services.

---

### IOC

* Repeated WAF Blocks
* Suspicious HTTP Requests
* Malicious Query Strings

### Detection Opportunities

Monitor:

* Repeated WAF Alerts
* Excessive HTTP Errors
* Suspicious URL Patterns

---

### MITRE ATT&CK

* T1190 – Exploit Public-Facing Application

---

## Scenario 3:  Guessing the Password (VPN Brute-Force)
An attacker is trying to guess a user's password to gain remote access to the network. This creates a lot of noise in the authentication logs.

VPN Gateway Log
2025-09-22 10:12:11 FAILED_AUTH TCP [REDACTED]:31245 -> 10.0.0.1:443 (user 'admin')
2025-09-22 10:12:15 FAILED_AUTH TCP [REDACTED]:31248 -> 10.0.0.1:443 (user 'admin')
2025-09-22 10:12:21 SUCCESS_AUTH TCP 198.51.100.88:41233 -> 10.0.0.1:443 (user 'b.jones')
2025-09-22 10:12:08 FAILED_AUTH TCP [REDACTED]:31249 -> 10.0.0.1:443 (user 'guest')
2025-09-22 10:12:09 FAILED_AUTH TCP [REDACTED]:31250 -> 10.0.0.1:443 (user 'user')
Log Breakdown

The log is flooded with FAILED_AUTH and SUCCESS_AUTH events.

A few successful logins from various IPs are normal.

The problem is the massive volume of failures.

To find the attack, an analyst would filter or group the logs by source IP.
Doing so would immediately reveal that a suspicious user is responsible for hundreds of failed login attempts in a very short period.

Analyst's Verdict: One suspicious IP is observed conducting a brute-force attack against the VPN gateway. The attacker is attempting to use a list of common usernames ('admin', 'root', 'test', etc.) to find a valid account to compromise. The scattered successful logins are benign traffic from legitimate employees.

Key Takeaways

An analyst's job is to separate normal traffic from suspicious activity.
Look for suspicious patterns.
Repetition from one source to many destinations = Scanning.
Repetition from one source to one destination = Brute-forcing.
Traffic at perfect, regular intervals = Malware beaconing.
Context is key. An IDS alert telling you why something was flagged is more valuable than a simple firewall block.
Monitoring the network perimeter is the first step in detecting attacks.
### Question
How many brute-force attempts failed?

### Answer
90

### Question
Which IP performed the attack?

### Answer
45.137.22.13

---

### What Happened?

The attacker repeatedly attempted authentication against the VPN gateway.

---

### IOC

* Multiple Failed Logins
* Authentication Failures
* Repeated Attempts From One IP

### Detection Opportunities

Monitor:

* Excessive Login Failures
* Geographic Anomalies
* Impossible Travel Events
* Failed-to-Successful Login Patterns

---

### MITRE ATT&CK

* T1110 – Brute Force

---

# Task 7: Investigating the Breach

This was the most valuable section because it showed how multiple logs can be combined into a complete attack timeline.
You have been given three sets of logs from the time of the incident. The logs can be found in the Perimeter_logs/challenge directory on the Desktop.

Firewall Logs:firewall.log
WAF Logs:ids_alerts.log 
VPN Logs:vpn_auth.log 
<img width="677" height="281" alt="image" src="https://github.com/user-attachments/assets/a2f83ee2-82da-4e43-ab7e-68ae315e97ec" />

---
## Analyzing Logs via Splunk:

As a SOC Analyst, analyzing logs manually can become a tedious task if the log files are large. Therefore, a Splunk instance is also provided in the VM if you decide to use it for log Analysis.
To proceed, open the link **localhost:8000** in the browser, click on the **Search & Reporting** tab on the left bar, and start analyzing the logs. Logs are pre-ingested into the **index="network_logs"**, as shown below:<img width="1046" height="641" alt="image" src="https://github.com/user-attachments/assets/d8903550-359e-4088-b21b-7a0d427a916f" />
Here must be the all step's follow and check **Time range: All time**.

# Task 7: Perimeter Logs – Investigating the Breach

This task simulated a real-world SOC investigation where multiple log sources were correlated to reconstruct an attack timeline.

The investigation involved:

* Firewall Logs
* VPN Authentication Logs
* IDS Alerts

The objective was to identify reconnaissance activity, successful compromise, lateral movement, command-and-control communication, and data exfiltration.

---

## Question 1

### What external IP performed the most reconnaissance?

**Answer:** 203.0.113.45

### SPL(Search Processing Language) Query

```spl
index="network_logs" source="firewall_logs.json" action=BLOCK
| top src_ip
```

### Explanation

This query identifies the source IP responsible for the highest number of blocked firewall events.

### IOC

* Source IP: 203.0.113.45

### ATT&CK

* T1595 – Active Scanning

---

## Question 2

### Which internal host was targeted by scans?

**Answer:** 10.0.0.20

### SPL Query

```spl
index="network_logs" sourcetype=firewall_logs action=BLOCK
| stats count by dst_ip | sort -count
```

### Explanation

After identifying the attacker IP, this query determines which internal system received the highest number of scan attempts.

### IOC

* Internal Host: 10.0.0.20

### ATT&CK

* T1595 – Active Scanning

---

## Question 3

### Which username was targeted in VPN logs?

**Answer:** svc_backup

### SPL Query

```spl
index="network_logs" source="vpn_auth.json"
result="FAIL" src_ip="203.0.113.45"
| top username
```

### Explanation

This query identifies which account received the highest number of failed authentication attempts.

### IOC

* Targeted Account: svc_backup

### ATT&CK

* T1110 – Brute Force

---

## Question 4

### What internal IP was assigned after successful VPN login?

**Answer:** 10.8.0.23

### SPL Query

```spl
index="network_logs" source="vpn_auth.json" 
result=SUCCESS
username=svc_backup src_ip="203.0.113.45" | table _time assigned_ip
| sort _time
```

### Alternative SPL

```spl
index="network_logs" source="vpn_auth.log"
status="SUCCESS" src_ip="203.0.113.45" | sort _time
| table _time username assigned_ip
```

### Explanation

The successful authentication event reveals the VPN-assigned internal address.

### IOC

* Username: svc_backup
* Assigned IP: 10.8.0.23

### ATT&CK

* T1078 – Valid Accounts

---

## Question 5

### Which port was used for lateral SMB attempts?

**Answer:** 445

### SPL Query

```spl
index="network_logs" source="ids_alert.json" alert="ET EXPLOIT Possible MS-SMB Lateral Movement" | stats count by src_ip,dst_port,dst_ip
```

### Explanation

Port 445 is associated with SMB and often appears during internal lateral movement.

### IOC

* SMB Port: 445

### ATT&CK

* T1021.002 – SMB/Windows Admin Shares

---

## Question 6

### Which host beaconed to the C2?

**Answer:** 10.0.0.60

### SPL Query

```spl
index="network_logs" source="ids_alerts.json" alert="ET TROJAN Possible C2 Beaconing"
| stats count by src_ip,dst_ip,dst_port
```

### Explanation

IDS alerts revealed periodic communications indicative of beaconing behavior.

### IOC

* Host: 10.0.0.60

### ATT&CK

* T1071 – Application Layer Protocol

---

## Question 7

### Which IP was associated with the C2 infrastructure?

**Answer:** 198.51.100.77

### SPL Query

```spl
index="network_logs" source="ids_alerts.json"
src_ip="10.0.0.60"
```

### Alternative SPL

```spl
index="network_logs" source="ids_alerts.log"
| table src_ip dest_ip signature
```

### Explanation

The destination IP repeatedly contacted by the infected host was identified as the command-and-control server.

### IOC

* C2 IP: 198.51.100.77

### ATT&CK

* T1071 – Application Layer Protocol

---

## Question 8

### Which host showed exfiltration attempts?

**Answer:** 10.0.0.51

### SPL Query

```spl
index="network_logs" source="ids_alerts.json" alert="ET INFO Possible HTTP POST Large Upload" | stats count by src_ip,dst_ip,dst_port
```

### Explanation

The IDS detected suspicious outbound traffic consistent with data exfiltration behavior.

### IOC

* Host: 10.0.0.51

### ATT&CK

* T1041 – Exfiltration Over C2 Channel

---

## Attack Timeline Reconstruction

1. Reconnaissance

   * 203.0.113.45 scans perimeter systems.

2. Credential Attack

   * svc_backup targeted through VPN brute-force attempts.

3. Initial Access

   * Successful VPN login.
   * Internal IP assigned: 10.8.0.23.

4. Lateral Movement

   * SMB activity observed over port 445.

5. Command & Control

   * Host 10.0.0.60 begins beaconing.
   * C2 IP identified as 198.51.100.77.

6. Data Exfiltration

   * Host 10.0.0.51 generates exfiltration alerts.

This sequence represents a realistic attack chain that a SOC analyst may investigate in an enterprise environment.


---

### Analyst Observation

Exfiltration represents one of the final attack stages.

Common indicators:

* Large outbound transfers
* Unexpected destinations
* Data compression activity
* After-hours network usage

---

### ATT&CK Mapping

* T1041 – Exfiltration Over C2 Channel

---

# What Confused Me?

Initially, I viewed firewall logs, IDS logs, and VPN logs as separate pieces of information.

I struggled to understand how analysts connect these data sources together during investigations.

After completing the room, I realized that every log source contributes one piece of the attack story.

---

# What I Learned?

Key lessons:

* Network visibility is essential.
* Firewall logs reveal perimeter activity.
* VPN logs reveal authentication events.
* IDS alerts reveal suspicious behavior.
* Log correlation creates attack timelines.
* Security investigations rely on context rather than isolated events.

---

# How I Improved?

Before this room, I looked at security events individually.

Now I understand that analysts must correlate multiple events together to understand the full scope of an incident.

Instead of asking:

> What alert fired?

I started asking:

> What attack stage does this alert represent?

That change in thinking is important for SOC investigations.

---

# Real-World Relevance

In real environments, analysts regularly investigate:

* Internet Scanning
* Credential Attacks
* VPN Abuse
* Malware Beaconing
* Lateral Movement
* Data Exfiltration

The attack chain demonstrated in this room closely resembles real-world intrusions.

---

## 🛡️ SOC Analyst L1: Tricky Connection

Many analysts focus only on alerts.

Experienced analysts focus on attack progression.

A firewall alert may represent reconnaissance.

A VPN alert may represent credential compromise.

An IDS alert may represent command-and-control activity.

The real skill is understanding how these events connect together.

---

## Complete Attack Story

1. Attacker performs reconnaissance.
2. Attacker discovers exposed services.
3. Attacker brute-forces credentials.
4. VPN access is obtained.
5. Internal systems are explored.
6. SMB is used for lateral movement.
7. Malware establishes C2 communications.
8. Data is exfiltrated.

This room effectively demonstrated the full lifecycle of an intrusion.

---

## Next Learning Steps

To build on this room, I should continue learning:

1. Firewall Analysis
2. IDS/IPS Technologies
3. VPN Security Monitoring
4. Network Traffic Analysis
5. Wireshark
6. Zeek
7. MITRE ATT&CK
8. Threat Hunting
9. Incident Response

---

## 💼 Critical Thinking Question

### Scenario

A firewall begins generating alerts showing a single external IP attempting connections to hundreds of ports across multiple public-facing servers.

Shortly afterward, VPN logs show repeated failed authentication attempts from the same IP.

As a SOC Analyst L1:

* What attack stage is occurring?
* What evidence would you collect?
* What actions would you take?

### Answer Concept

The attacker is progressing from reconnaissance into credential access attempts.

Investigation steps:

1. Validate firewall activity.
2. Review VPN authentication logs.
3. Identify targeted accounts.
4. Check for successful logins.
5. Review IDS alerts.
6. Search for related endpoint activity.
7. Escalate if compromise indicators are identified.

### Relevant ATT&CK Techniques

* T1595 – Active Scanning
* T1110 – Brute Force
* T1078 – Valid Accounts

---

## Final Reflection

This room taught me that network security is not only about blocking attacks. It is about understanding attacker behavior, monitoring network visibility, correlating log sources, and recognizing attack progression.

As an aspiring SOC Analyst L1, this room strengthened my understanding of perimeter defense, attack detection, incident investigation, and the importance of connecting multiple data sources into a complete security story.

