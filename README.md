# SOC Analyst Investigation Portfolio

## Overview
This repository contains structured SOC-style investigations based on packet captures, network traffic, and log analysis.  
Each case is written as a professional incident report that focuses on identifying suspicious activity, tracking attacker behavior, and building a clear timeline of events.

The purpose of this repository is to demonstrate practical investigation and reporting skills used by Security Operations Center (SOC) analysts.

---

## Skills Demonstrated
- PCAP analysis using Wireshark  
- Network traffic investigation  
- Suspicious IP/domain identification  
- DNS and HTTP traffic analysis  
- File and stream reconstruction  
- Attack timeline creation  
- IOC identification  
- MITRE ATT&CK mapping  
- Incident reporting  
- Basic threat hunting workflow  

---

## Tools Used
- Wireshark  
- TCPDump  
- Windows Event Logs  
- Sysmon (case dependent)  
- VirusTotal  
- Public threat intelligence sources  

---

## Investigation Methodology
Each investigation in this portfolio follows a consistent process:

1. Initial triage of traffic or logs  
2. Identification of anomalies  
3. Pivoting on suspicious IPs, domains, or files  
4. Reconstruction of attacker actions  
5. Timeline creation  
6. IOC identification  
7. Mapping activity to MITRE ATT&CK  
8. Writing a structured incident report  

---

## Investigation Cases

### Network Traffic Investigations
- **Packet Puzzle – Network Traffic Analysis**  
  Investigation of suspicious traffic within a packet capture to identify attacker activity and potential compromise.  
  [View Report](packet-puzzle-htb/report.md)

(Additional investigations will be added over time.)

---

## Report Structure
Each case folder contains:
- `report.md` → Full investigation report  
- `screenshots/` → Supporting evidence images  

Every report includes:
- Case overview  
- Investigation steps  
- Key findings  
- Timeline of events  
- MITRE ATT&CK mapping  
- Evidence screenshots  
- Conclusion  

---

## Purpose
This repository demonstrates practical SOC investigation skills including traffic analysis, incident detection, and structured reporting.  
It is intended to reflect real-world analyst workflows rather than simple lab walkthroughs.
