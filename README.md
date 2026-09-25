# Network Traffic Analysis & Incident Case Management — Security Onion

Analysis of a network packet capture using Security Onion's full investigation workflow — from initial alert triage through case escalation, artifact collection, and event correlation — to confirm and document an active ZBOT (Zeus) trojan command-and-control infection.

*Completed as a team project for ISSM536 (Incident Response and Digital Forensics). Full team credited at the bottom.*

## Objective

Given an assigned PCAP file, use Security Onion's built-in tools (Alerts, Hunt, Cases, PCAP, Dashboards) to characterize the traffic, identify malicious activity, and manage the investigation the way a SOC analyst would — escalating real findings into a documented, collaborative case rather than just noting them informally.

## Investigation Workflow

### 1. Import & Dashboard Overview
Imported the assigned PCAP via the Grid interface, then reviewed the Dashboard for a high-level view of the investigation window — in this case, events spanning January 2017 to March 2024:

<img width="1732" height="1079" alt="image" src="https://github.com/user-attachments/assets/2b523d9c-fea2-403e-aaf6-e5a4f192cf2a" />

### 2. Alert Triage
Used the **Alerts** tab to review all alerts generated from the capture and identify which warranted deeper investigation:

<img width="1727" height="1076" alt="image" src="https://github.com/user-attachments/assets/7366cadc-69ea-4f10-9ba4-eecb0ad3d78e" />

### 3. Drill-Down Analysis — Zeus Bot
Drilled into the first significant alert — a **Zeus Bot** detection — surfacing 16 related alerts with associated timestamps and two destination IP addresses (`172.217.17.36` and `216.58.212.163`):

<img width="1725" height="1079" alt="image" src="https://github.com/user-attachments/assets/5d62c58f-407c-45b3-a4a6-a1b4ab6c1dc1" />

Expanded key fields (`network.data.decoded`, `network.community_id`, log paths) to identify indicators worth escalating, cross-referencing, or reviewing at the packet level.

### 4. Case Escalation
Escalated the confirmed ZBOT malware POST-to-C2 activity into a formal **Case**, attaching the relevant PCAP evidence:

<img width="2048" height="1365" alt="image" src="https://github.com/user-attachments/assets/b743277c-19c9-4f72-b577-f018bad86943" />

Set case metadata including **TLP** (Traffic Light Protocol — how widely findings can be shared) and **PAP** (Permissible Actions Protocol — whether suspected files could be submitted to VirusTotal).

### 5. Artifact Collection
Used the case's **Attachments** tab to store discovered artifacts, which Security Onion automatically hashes for integrity and easy cross-referencing against VirusTotal:

<img width="1718" height="1075" alt="image" src="https://github.com/user-attachments/assets/efe1f14a-bfd3-411e-a967-bf9b7b6c94a3" />

### 6. Observables
Logged compromised indicators (source/destination IPs, domains, file hashes) in the **Observables** tab, enabling the team to pivot and hunt for the same indicators elsewhere in the network:

<img width="2048" height="1365" alt="image" src="https://github.com/user-attachments/assets/70eb16e6-2ee3-4d63-a6f2-66c92e5ab059" />

### 7. Packet-Level Confirmation
Reviewed the raw PCAP traffic between the identified IPs to confirm the malware's behavior at the protocol level:

<img width="1737" height="1074" alt="image" src="https://github.com/user-attachments/assets/da55d031-2e5b-4c07-ba10-7d8cc95c2e06" />

The capture shows the attacker machine issuing an **HTTP POST to `/ze-pi/gate.php`** (a known ZBOT C2 check-in pattern) to a suspicious domain, with the victim machine acknowledging (ACK) and the server responding **200 OK** — confirming the trojan successfully checked in with its command-and-control server.

### 8. Event Correlation (Hunt Tab)
Used the **Hunt** tab to correlate events across the identified IP addresses and build a timeline of the attack:

<img width="1722" height="1079" alt="image" src="https://github.com/user-attachments/assets/9e7e08c6-3b77-4466-a815-1bbcaf4e8b62" />

This confirmed the full attack chain: C2 check-in detected → network trojan alert fired → malicious code successfully injected into the target environment.

## Key Findings

- Identified an active **ZBOT (Zeus) trojan** communicating with a command-and-control server via HTTP POST to `gate.php`
- Confirmed via raw packet inspection that the C2 check-in received a `200 OK` response, indicating successful communication
- Built a fully documented, collaborative case in Security Onion — comments, hashed attachments, tracked observables, and a complete audit history — rather than just an informal write-up

## Key Takeaways

- Practiced the full SOC alert-to-case lifecycle: triage → drill-down → escalation → artifact preservation → correlation, not just spotting an alert in isolation
- Learned to read raw HTTP traffic inside a packet capture to independently confirm what an automated alert (Suricata/Zeek signature) flagged
- Used case metadata (TLP/PAP) the way a real SOC would — controlling what could be shared externally and what actions were permitted on suspected malware artifacts
- Practiced multi-source correlation (Alerts + Hunt + PCAP) to build one coherent incident narrative instead of treating each tool's output independently

## Tools Used
Security Onion (Alerts, Hunt, Cases, PCAP, Dashboards), Suricata/Zeek (underlying detection engines)

## Team
Varshini Sundarraj, Ajansha Shankar, Hong An Tran, Nikul Panchal — ISSM536, Concordia University of Edmonton

*Full PDF report available on request.*
