# Task5
Task 5 for Elevate Labs
# Network Traffic Analysis Report – `192.168.1.5`
**Date:** June 9, 2025  
**Tool Used:** Wireshark  
**Focus:** TCP, TLS, and HTTP Application-layer Analysis  
**Analyst:** Anamika  

---

##  Executive Summary

The network traffic associated with host `192.168.1.5` exhibits multiple anomalies across TCP, TLS, and HTTP layers. The combination of abrupt TCP resets, suspicious TLS negotiations, and a burst of repetitive HTTP GET requests suggests potential reconnaissance activity, command and control (C2) behavior, or a misconfigured/malicious application generating noise.

---

## Key Findings

### 1.  TCP Layer Anomalies

####  Suspicious RST Behavior
- Numerous `RST, ACK` packets with `Win=0` shortly after connection establishment.
- Abnormal session teardowns and incomplete 3-way handshakes.

**Interpretation:**  
- Possible **stealth TCP scanning**, **firewall filtering**, or **malware probing behavior**.

#### TCP Retransmissions & Zero Window
- Multiple segments retransmitted with little payload exchange.
- Zero window size and incomplete data transfer.

**Interpretation:**  
- **Network degradation**, or **intentional disruption** by middleboxes or malware.

####  PSH/ACK Data Bursts
- Repetitive `[PSH, ACK]` streams from external IPs (notably `44.228.249.3`) with full-sized TCP segments.

**Interpretation:**  
- Indicative of **C2 traffic**, beaconing behavior, or **data exfiltration attempts**.

---

### 2.  TLS/SSL Layer Anomalies

#### Ignored or Malformed TLS Records
- Multiple records marked as "Ignored Unknown Record" or malformed during TLSv1.2 negotiation.

####  TLS Handshake Failures
- Handshakes reset prematurely or fail due to abrupt RST/FIN from the server or client.

**Interpretation:**  
- Attempted **encrypted communication** possibly blocked or poorly implemented (e.g., malware using self-signed certs or fake TLS handshakes).

---

### 3.  Application Layer (HTTP)

#### Repetitive GET Requests
- High frequency of repeated `HTTP GET /` requests.
- Host: `192.168.1.5`, targeting standard web servers.
- Payload content is minimal and repetitive.

**Interpretation:**  
- Could be:
  -  **Beaconing behavior** for periodic check-ins
  -  A **scripted client or bot**
  -  **Fuzzing activity** trying to map HTTP behaviors

#### 404 Not Found and 200 OK Loop
- Some responses return 404, while others are HTTP 200 OK with negligible content.

**Interpretation:**  
- May indicate:
  - **Directory brute-force attempts**
  - **Unstructured scraping tools**
  - Or, **malware C2 misconfigured endpoints**

---
