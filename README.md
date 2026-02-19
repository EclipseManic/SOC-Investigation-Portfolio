# 🛡️ SOC Analyst Investigation Portfolio

## 📌 Overview
This repository contains structured SOC-style investigations based on packet captures, network traffic, and log analysis.  
Each case is written as a professional incident report focused on identifying suspicious activity, tracking attacker behavior, and building a clear timeline of events.

The goal is to demonstrate practical investigation and reporting skills used in real Security Operations Center (SOC) environments.

---

## 🧠 Skills Demonstrated
- 🧪 PCAP analysis using Wireshark  
- 🌐 Network traffic investigation  
- 🚨 Suspicious IP/domain identification  
- 🔎 DNS and HTTP traffic analysis  
- 📂 File and stream reconstruction  
- 🕒 Attack timeline creation  
- 📍 IOC identification  
- 🗺️ MITRE ATT&CK mapping  
- 📝 Incident reporting  
- 🕵️ Basic threat hunting workflow  

---

## 🛠️ Tools Used
- Wireshark  
- TCPDump  
- Windows Event Logs  
- Sysmon (case dependent)  
- VirusTotal  
- Public threat intelligence sources  

---

## 🔬 Investigation Methodology
Each investigation follows a consistent SOC workflow:

1. 📥 Initial triage of traffic or logs  
2. ❗ Identify anomalies  
3. 🔁 Pivot on suspicious IPs/domains/files  
4. 🧩 Reconstruct attacker actions  
5. 🕒 Build timeline  
6. 📍 Identify indicators of compromise  
7. 🗺️ Map activity to MITRE ATT&CK  
8. 📝 Write structured incident report  

---

## 📂 Investigation Cases

### • Phishing Email Incident — PhishNet
SOC investigation of a vendor-impersonation phishing email delivering a malicious attachment.  
Includes manual analysis and tool-assisted detection workflow.
Report link → [View Report](https://github.com/EclipseManic/SOC-Investigation-Portfolio/blob/main/Phishing%20Email%20Investigation/report.md)


### • Backup Server Compromise — Telly
SOC/DFIR investigation of a compromised backup server where an attacker exploited a Telnet vulnerability to gain root access, establish persistence, and exfiltrate a sensitive customer database.
Includes PCAP network analysis, command reconstruction, persistence tracking, and data-exfiltration validation workflow.
Report link → [View Report](https://github.com/EclipseManic/SOC-Investigation-Portfolio/blob/main/Linux%20Server%20Investigation/report.md)


### • LLMNR Poisoning Attack — Noxious
Network forensics investigation of a rogue device performing LLMNR poisoning inside an Active Directory environment to capture NTLM authentication attempts after a mistyped file-share request.  
Includes PCAP analysis, rogue host identification, NTLM challenge/response reconstruction, credential-exposure validation, and lateral-movement risk assessment workflow.  
Report link → [View Report](https://github.com/EclipseManic/SOC-Investigation-Portfolio/blob/main/LLMNR%20Poisoning%20Investigation/report.md)


### • Web Exploitation & Privilege Escalation — Packet Puzzle
SOC investigation of an internal compromise where an attacker performed reconnaissance, exploited a vulnerable PHP service to gain RCE, deployed payloads, and attempted privilege escalation using GodPotato.  
Includes PCAP analysis, exploit reconstruction, command tracking, payload analysis, and MITRE ATT&CK mapping workflow.  
Report → [View Report](https://github.com/EclipseManic/SOC-Investigation-Portfolio/blob/main/Network%20Intrusion%20Investigation/report.md)



### • BonitaSoft Exploitation — Meerkat
SOC investigation of a Business Management Platform compromise. The attacker performed credential stuffing to gain initial access, exploited CVE-2022-25237 (Authorization Bypass) to achieve RCE, and established persistence via SSH key injection using a text-sharing site.
Report link → [View Report](https://github.com/EclipseManic/SOC-Investigation-Portfolio/blob/main/Bonitasoft%20Compromise%20Investigation/report.md)

(Additional investigations will be added over time.)

---

## 🧾 Report Structure
Each investigation folder contains:

- `report.md` → Full investigation report

Every report includes:
- Case overview  
- Investigation steps  
- Key findings  
- Timeline  
- MITRE ATT&CK mapping  
- Evidence screenshots  
- Conclusion  

---

## 🎯 Purpose
This repository showcases hands-on SOC investigation skills including traffic analysis, threat detection, and incident reporting.  
It is designed to reflect real-world analyst workflows and investigation thinking.

