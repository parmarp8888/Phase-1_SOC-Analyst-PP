
# Room Walkthrough: [DNS in Detail](https://tryhackme.com/room/dnsindetail)

## 🎯 Day 14 Objective

As part of my SOC Analyst L1 learning journey, this room helped me understand one of the most important services on the internet: DNS (Domain Name System).
<img width="224" height="225" alt="image" src="https://github.com/user-attachments/assets/4d7b2d43-8586-4da5-a308-10b50b36cecb" />

Before studying traffic analysis, threat hunting, or malware investigations, I wanted to understand how systems locate websites and services across the internet.

This room explained how DNS translates human-readable names into IP addresses and how DNS infrastructure supports internet communication.

More importantly, it helped me understand why DNS is frequently monitored during security investigations.

---
## Who is this for?

* SOC Analysts
* Network Administrators
* Security Engineers
* Incident Responders
* Threat Hunters
* Anyone learning networking fundamentals

## What is DNS?
[<img width="930" height="572" alt="image" src="https://github.com/user-attachments/assets/22f38f94-dd09-4542-8384-f2b2cd6daef2" />
](https://www.ibm.com/think/topics/dns)
DNS (Domain Name System) is the internet's directory service.

Instead of remembering IP addresses such as:

192.168.1.1

Users can access resources using names like:

example.com

DNS translates domain names into IP addresses so systems can communicate.

## Where does DNS apply?

DNS is used in:

* Web Browsing
* Email Services
* Cloud Platforms
* Internal Corporate Networks
* Mobile Applications
* Security Monitoring Systems

Almost every internet-based service relies on DNS.

## When is DNS used?

DNS requests occur whenever:

* Visiting a website
* Opening an application
* Sending email
* Accessing cloud resources
* Connecting to remote services

Most users generate hundreds or thousands of DNS requests daily without realizing it.

## Why is DNS important?

Without DNS:

* Websites would require IP addresses.
* Email routing would become difficult.
* Cloud services would be harder to manage.
* Internet usability would significantly decrease.

From a security perspective, DNS visibility helps detect:

* Malware communications
* Phishing domains
* Command and Control (C2) activity
* Data exfiltration
* Domain Generation Algorithms (DGA)

## How does DNS work?

A simplified process:
[<img width="1978" height="1294" alt="image" src="https://github.com/user-attachments/assets/72948e52-e83f-427f-8e2f-f1e6c9e5eaf8" />
](https://www.fortinet.com/resources/cyberglossary/dns-security)
1. User enters a domain.
2. Device checks local cache.
3. Recursive DNS server receives request.
4. Root server provides TLD information.
5. TLD server points to authoritative server.
6. Authoritative server returns the requested record.
7. Result is cached according to TTL.
8. Connection is established.

---

# Task 1: What is DNS/DHCP?
DNS and DHCP are two very critical services over a network. DNS refers to the Domain Name System, whereas DHCP refers to the Dynamic Host Configuration Protocol.
Domain Name System (DNS) is an Internet service that translates human-readable domain names (e.g., www.geeksfosgeeks.org ) into IP addresses (like 192.160.0.1 ), whereas Dynamic Host Configuration Protocol (DHCP) is a protocol for automatically assigning IP addresses and other configurations to devices when they connect to a network. These are two different but complementary services in the management of communication across a network.

## Question
What does DNS stand for?

### Answer
Domain Name System

### Explanation
DNS acts like a phonebook for the internet.
Humans remember names.
Computers communicate using IP addresses.
DNS connects the two.

**Advantages**
User-Friendly: Translates difficult IP addresses into easy-to-remember domain names.
DNS eliminates the need for users to memorize complex strings of numbers.
Load Balancing: DNS can distribute incoming traffic across multiple servers.
Efficient: Provides fast resolution of frequently accessed websites by implementing caching.
**Disadvantages**
Vulnerable: Attacks target it through DNS spoofing or DDoS, aiming to hamper access to websites.
Complex: Requires proper configuration and maintenance of DNS records; otherwise, issues will arise during resolution.

## Difference Between DNS and DHCP
|DNS|DHCP|
|---|---|
|DNS stands for Domain Name System.| DHCP stands for Dynamic Host Configuration Protocol.|
|It works in 53 port number.|It works in 67 and 68 port number.|
|The protocol supported by DNS are: UDP and TCP.|In this only UDP protocol is used.|
|DNS is decentralized system.|DHCP is centralized system.|
|In DNS, with the help of DNS server, domain names are translated into IP addresses and IP addresses are translated into domain names.|While in DHCP, DHCP server is used to configures the hosts mechanically.|
|DNS management involves configuring and maintaining DNS servers and records.|DHCP management involves configuring and maintaining DHCP servers, setting lease times, and managing IP address pools.|
|With the help of DNS, we don't need to remember the IP address.|It is reliable IP configuration.|
<img width="740" height="635" alt="image" src="https://github.com/user-attachments/assets/6b1548cd-cd7c-4c24-bad8-01ababf847a7" />

---

# Task 2: Domain Hierarchy

Understanding domain hierarchy is critical because many attacks abuse subdomains and newly created domains.
<img width="1140" height="800" alt="image" src="https://github.com/user-attachments/assets/8df6a8ff-00c3-4160-81f5-495340887f9a" />
1. Root DomainDescription: The topmost layer of the DNS hierarchy, represented by a dot (.) at the end of a fully qualified domain name.Function: It acts as the starting point for all DNS lookups. In practice, this trailing dot is usually invisible in modern web browsers.

2. Top-Level Domain ([TLD](https://data.iana.org/TLD/tlds-alpha-by-domain.txt))Description: The first level below the root domain, defining the domain's extension.Function: They categorize domains by purpose or geographic region. Common examples include Generic TLDs (gTLDs) like .com, .org, and .net, as well as Country-Code TLDs (ccTLDs) like .in (India), .uk (United Kingdom), and .us (United States).

3. Second-Level Domain (SLD)Description: The portion of the domain that appears immediately to the left of the TLD.Function: This is the unique name you actually register (e.g., "google" in google.com) to represent your specific brand, organization, or identity.

4. Subdomains (Third-Level Domains & Beyond)Description: Any segment placed to the left of the SLD, separated by a period.Function: They allow you to divide your website into distinct sections or host different services without registering a new domain. For example, in ://google.com, "mail" is the subdomain. Common uses include shop, blog, or dev.

5. HostnameDescription: The leftmost part of a fully qualified domain name (FQDN).Function: It identifies a specific device or server within a network (e.g., "www" in www.example.com).
---

## Question
What is the maximum length of a subdomain?

### Answer
63

### Why it Matters

Threat actors often create:

* Long subdomains
* Randomized subdomains
* DGA-generated domains

These can become useful detection indicators.

---

## Question
Which character cannot be used in a subdomain( 3 b _ - )?

### Answer _  Underscore

### Explanation

Valid domain names can contain:

* Letters b
* Numbers 3
* Hyphens (-)

Underscores are generally not allowed in hostnames.

---

## Question
What is the maximum length of a domain name?

### Answer
253

### SOC Perspective

Attackers sometimes create unusually long domains to:

* Evade detection
* Hide malicious infrastructure
* Encode information

Long domains may deserve additional review.

---

## Question
What type of TLD is .co.uk?

### Answer
ccTLD

### Explanation

ccTLD stands for:

Country Code Top-Level Domain

Examples:

* .uk
* .in
* .jp
* .de

Understanding geographic domains helps during investigations.

---

# Deep Dive: Domain Hierarchy

Example:
[
<img width="272" height="185" alt="image" src="https://github.com/user-attachments/assets/b91206ca-0388-4e71-83f9-daf27523d139" />
](https://cloudinfrastructureservices.co.uk/what-is-dns-hierarchy/)

Hierarchy:

* Root (.)
* com (TLD)
* example (Domain)
* login (Subdomain)
* support (Subdomain)

SOC analysts often investigate suspicious subdomains because attackers frequently use them for phishing campaigns.

Example:

login-office365-security-check.example.com

Looks legitimate at first glance but may be malicious.

---

# Task 3: Record Types
[<img width="456" height="339" alt="image" src="https://github.com/user-attachments/assets/df2399a0-bf59-40e0-8846-5f7c2c1aeb31" />
](https://www.google.com/imgres?q=record%20type%20in%20domain&imgurl=https%3A%2F%2Fupload.wikimedia.org%2Fwikipedia%2Fcommons%2F5%2F59%2FAll_active_dns_record_types.png&imgrefurl=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FList_of_DNS_record_types&docid=SIK46jOdJwjl_M&tbnid=bXfaQrl5KJ4n0M&vet=12ahUKEwjO6tO8nf6UAxW11DgGHQUCMwcQnPAOegQIGBAB..i&w=1448&h=1176&hcb=2&ved=2ahUKEwjO6tO8nf6UAxW11DgGHQUCMwcQnPAOegQIGBAB)
DNS records provide different types of information.

Understanding record types is essential for investigations.

---

## Question
Which record specifies where email should be sent?

### Answer
MX Record

### Explanation

MX = Mail Exchange

It tells email servers where mail should be delivered.

### SOC Relevance

Threat actors often abuse email infrastructure for:

* Phishing
* Business Email Compromise (BEC)
* Spam campaigns

MX records can help identify email providers used by attackers.

---

## Question
Which record handles IPv6 addresses?

### Answer
AAAA Record

### Explanation

A Record → IPv4

AAAA Record → IPv6

Example:

A Record:

192.168.1.1

AAAA Record:

2001:db8::1

---

# Additional DNS Record Types Every SOC Analyst Should Know
<img width="733" height="448" alt="image" src="https://github.com/user-attachments/assets/c02e865c-7daa-4997-a604-a3058f62e26b" />

## A Record

Maps:

Domain → IPv4 Address

Example:

example.com → 93.184.216.34

---

## AAAA Record

Maps:

Domain → IPv6 Address

---

## CNAME Record

Creates an alias.

Example:

shop.example.com

→ shops.myshopify.com

Useful when organizations use cloud services.

---

## MX Record

Controls email routing.

---

## TXT Record
_acme-challenge.example.com TXT "token_value_here"
@ TXT "v=spf1 ip4:192.0.2.0/24 include:_spf.google.com include:amazonses.com ~all"
_dmarc.example.com TXT "v=DMARC1; p=reject; rua=mailto:dmarc-reports@example.com; adkim=s; aspf=s; pct=100"
@ TXT "MS=ms12345678"
Stores text information.

Commonly used for:

* SPF
* DKIM
* Domain Verification

Attackers sometimes abuse TXT records for command-and-control communications.

---

## NS Record

Specifies authoritative name servers.

Useful during infrastructure investigations.

---

# Task 4: Making a DNS Request
<img width="800" height="1000" alt="image" src="https://github.com/user-attachments/assets/529af9c6-8ccd-40f2-8954-6a710599fe1b" />

Understanding the DNS resolution process is one of the most important networking concepts.

---

## Question
What field specifies how long a DNS record should be cached?

### Answer
TTL (Time To Live)

### Explanation

TTL determines how long systems store DNS responses before requesting fresh information.

### SOC Relevance

Very low TTL values may indicate:

* Fast-changing infrastructure
* Load balancing
* Potential malicious activity

---

## Question
What DNS server is usually provided by an ISP?

### Answer
Recursive DNS Server

### Explanation
The recursive resolver performs lookups on behalf of clients.
It gathers information from multiple DNS servers and returns the final answer.

---

## Question
What server contains the actual records for a domain?

### Answer
Authoritative Server

### Explanation
The authoritative server is the final source of truth for DNS records.

---

# Task 5: Practical Investigation

## Question
What is the CNAME of shop.website.thm?

### Answer
shops.myshopify.com

### Explanation
<img width="864" height="361" alt="Screenshot 2026-06-11 081717" src="https://github.com/user-attachments/assets/7d17b55f-d4dd-4e72-b522-70e32121c8ae" />
The organization is using Shopify infrastructure.

### SOC Perspective
Cloud-hosted services frequently appear in DNS investigations.
<img width="300" height="168" alt="image" src="https://github.com/user-attachments/assets/9c35ed3e-904f-4008-b252-9801d2d9c2e8" />

---

## Question
What is the TXT Record value?

### Answer
THM{7012BBA60997F35A9516C2E16D2944FF}

### SOC Perspective
<img width="921" height="195" alt="Screenshot 2026-06-11 081847" src="https://github.com/user-attachments/assets/c675c6ce-dcc6-4685-993c-aecc1b4df5bd" />

TXT records are frequently used for:
<img width="910" height="977" alt="Screenshot 2026-06-11 091724" src="https://github.com/user-attachments/assets/9757fa9b-240b-4a71-b351-833208433f43" />

* SPF
* DKIM
* Domain Verification

Attackers occasionally abuse TXT records to store instructions or encoded data.

---

## Question
What is the MX priority value?

### Answer
30

### Explanation

Lower numbers have higher priority.

Example:

10 = Primary

20 = Secondary

30 = Backup

---

## Question
What is the A Record IP address?

### Answer
10.10.10.10

### Explanation

This record maps the domain to an IPv4 address.
<img width="793" height="232" alt="Screenshot 2026-06-11 082111" src="https://github.com/user-attachments/assets/557d0518-b18c-425e-8afb-3c49f686afdb" />

---

# IOC Awareness

DNS-related Indicators of Compromise include:

* Newly registered domains
* Known malicious domains
* DGA-generated domains
* Excessive DNS requests
* Unusual TXT record usage
* DNS tunneling activity
* Suspicious external resolvers
* Rare geographic domains

---

# TTPs Related to DNS

### DNS Tunneling

Attackers hide data inside DNS requests.
[<img width="480" height="270" alt="image" src="https://github.com/user-attachments/assets/6052a824-b0e7-4735-987c-18de87584de6" />
](https://www.paloaltonetworks.com/cyberpedia/what-is-dns-tunneling)
Goal:

* Data Exfiltration
* C2 Communication

Examples:

* Iodine
* DNScat2

---

### Domain Generation Algorithms (DGA)

Malware generates thousands of domains.
[<img width="1056" height="768" alt="image" src="https://github.com/user-attachments/assets/c3a3597e-0804-41a3-850f-56fd407acaa3" />
](https://www.kaggle.com/datasets/slashtea/domain-generation-algorithm)
Goal:

* Avoid domain blocking

Examples:

* Conficker
* TrickBot

---

### Fast Flux DNS

IP addresses constantly rotate.
[<img width="1024" height="511" alt="image" src="https://github.com/user-attachments/assets/58535509-bdd4-4037-9d94-7cd9b961de76" />
](https://www.google.com/imgres?q=Fast%20Flux%20DNS&imgurl=https%3A%2F%2Funit42.paloaltonetworks.com%2Fwp-content%2Fuploads%2F2021%2F03%2Fword-image-3.png&imgrefurl=https%3A%2F%2Funit42.paloaltonetworks.com%2Ffast-flux-101%2F&docid=fcT6903OtzN8zM&tbnid=bJ06lJAqjAlLKM&vet=12ahUKEwi6oLz3pP6UAxXBcGwGHc_xA18QnPAOegQIExAB..i&w=2500&h=1084&hcb=2&ved=2ahUKEwi6oLz3pP6UAxXBcGwGHc_xA18QnPAOegQIExAB)
Goal:

* Hide malicious infrastructure

Common in:

* Botnets
* Phishing operations

---

# Detection Opportunities

As a SOC Analyst, I would monitor for:

### High DNS Query Volume

Could indicate:

* Malware
* DNS Tunneling
* Automated Scripts

### Rare Domains

Could indicate:

* Initial compromise
* C2 infrastructure

### Long Random Domains

Could indicate:

* DGA activity

### Unusual TXT Queries

Could indicate:

* Data exfiltration
* Malware communications

### Repeated Failed DNS Lookups

Could indicate:

* Malware searching for C2 servers

---

# What Confused Me?

Initially, I thought DNS was simply a service that converted names into IP addresses.

I did not understand:

* Recursive servers
* Authoritative servers
* TTL values
* Different record types

The DNS resolution process seemed complicated because several servers work together behind the scenes.

---

# What I Learned?

Key lessons:

* DNS is a critical internet service.
* DNS records provide different functions.
* DNS activity can reveal malicious behavior.
* DNS investigations are common in SOC environments.
* DNS logs provide valuable threat-hunting data.
* Attackers frequently abuse DNS infrastructure.

---

# How I Improved?

Before this room, I only knew that DNS translated names into IP addresses.

Now I understand:

* How DNS requests are processed.
* Why DNS logs matter.
* How record types affect investigations.
* How attackers abuse DNS.

This gives me a stronger foundation for traffic analysis and threat detection.

---

# Real-World Relevance

During real investigations, SOC analysts often examine:

* DNS logs
* Domain reputation
* Newly registered domains
* Phishing infrastructure
* Malware beaconing
* DNS tunneling activity

DNS evidence frequently helps identify threats before endpoint alerts appear.

---

## 🛡️ SOC Analyst L1: Tricky Connection

Many analysts focus on IP addresses.

However, attackers increasingly rely on domains.

A suspicious IP may change daily.

A suspicious domain often reveals attacker infrastructure.

Understanding DNS allows analysts to move from:

> "Which IP connected?"

to

> "What domain was contacted and why?"

That shift is critical for threat hunting.

---

## Next Learning Steps

1. DNS Monitoring
2. DNS Logs Analysis
3. Wireshark DNS Investigation
4. Zeek DNS Logs
5. HTTP/HTTPS Fundamentals
6. Threat Intelligence
7. Domain Reputation Analysis
8. Malware Traffic Analysis
9. DNS Tunneling Detection
10. Threat Hunting Fundamentals

---

## 💼 Critical Thinking Question

### Scenario

A workstation suddenly generates thousands of DNS queries for random-looking domains every hour.

Examples:

* xj83hd2.com
* pq92la8.net
* mx71ab3.org

No user activity explains this behavior.

What would you investigate first as a SOC Analyst L1?

### Answer Concept

1. Review DNS logs.
2. Check domain reputation.
3. Determine the originating process.
4. Review endpoint alerts.
5. Check for malware indicators.
6. Investigate potential DGA activity.
7. Escalate if malicious behavior is confirmed.

### Possible Threat

Domain Generation Algorithm (DGA) malware attempting to locate active Command and Control servers.

### Potential IOCs

* Random Domain Queries
* High Query Volume
* Failed DNS Resolutions
* Suspicious Process Activity

### Relevant MITRE ATT&CK Techniques

* T1071.004 – DNS
* T1568.002 – Domain Generation Algorithms
* T1048 – Exfiltration Over Alternative Protocol

---

# Final Reflection

This room showed me that DNS is much more than a name-resolution service. It is a critical component of internet communication and a valuable source of security telemetry.

As an aspiring SOC Analyst L1, understanding DNS helps me investigate suspicious domains, identify malware communications, analyze network traffic, and better understand how attackers use internet infrastructure during cyber attacks.
