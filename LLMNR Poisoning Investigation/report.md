# 🧪 Noxious — LLMNR Poisoning Investigation

## 📌 Case Overview

An IDS alert reported unusual LLMNR traffic inside the internal Active Directory VLAN. This typically indicates a rogue device attempting to intercept name‑resolution requests and capture credentials.

A packet capture from the time of the alert was analyzed to confirm whether an LLMNR poisoning attack occurred and whether any credentials were exposed.

**Case Type:** Network Forensics
**Attack Type:** LLMNR Poisoning
**Data Source:** PCAP
**Victim Host:** Forela‑WKstn002 (172.17.79.136)

---

## 🎯 Objectives

* Identify rogue system
* Confirm LLMNR poisoning
* Determine if credentials were captured
* Reconstruct authentication attempt
* Identify targeted share
* Assess impact

---

## 🧭 Key Findings

* Rogue Responder machine present in network
* LLMNR poisoning confirmed
* NTLM authentication attempt captured
* Username exposed
* Password not sent in plaintext
* Attempt targeted internal file share

---

## 🌐 Environment Details

| Item              | Value           |
| ----------------- | --------------- |
| Attacker IP       | 172.17.79.135   |
| Attacker Hostname | kali            |
| Victim IP         | 172.17.79.136   |
| Victim Host       | Forela‑WKstn002 |
| User              | john.deacon     |

---

## 1️⃣ Rogue Device Identification

Analysis of LLMNR traffic showed responses coming from a non‑domain host.

The device at **172.17.79.135** responded to LLMNR queries and impersonated the requested host.

DHCP traffic confirmed hostname:

* Hostname: **kali**

This indicates a rogue system running a poisoning tool (commonly Responder).

### Evidence

<img width="940" height="266" alt="image" src="https://github.com/user-attachments/assets/b9e46a6a-5c14-401a-b03a-586ae242879d" />


---

## 2️⃣ LLMNR Poisoning Trigger

The victim attempted to access a network share but mistyped the hostname:

```
DCC01
```

Windows attempted to resolve this via LLMNR broadcast.
The rogue system replied first and forced authentication.

### Evidence

<img width="1886" height="779" alt="image" src="https://github.com/user-attachments/assets/1f3b0375-cc8a-48e7-a1d3-ffd8ba924ca5" />

---

## 3️⃣ Credential Exposure

NTLM authentication traffic shows the victim attempting to authenticate to the rogue system.

Captured username:

* **john.deacon**

First capture time:

* **2024‑06‑24 11:18:30**

Important:
The password itself is **not transmitted in plaintext**.
NTLM uses a challenge‑response mechanism. The attacker captures a hash, not the raw password.

### Evidence

<img width="1533" height="615" alt="image" src="https://github.com/user-attachments/assets/be2e9384-1b63-4244-8189-1986a92309f9" />

---

## 4️⃣ NTLM Negotiation Data

From NTLM packets, the following values were extracted:

* NTLM Server Challenge:
  `601019d191f054f1`

* NTProofStr:
  `c0cc803a6d9fb5a9082253a04dbd4cd4`

These values can be used for offline cracking.

### Evidence

<img width="940" height="259" alt="image" src="https://github.com/user-attachments/assets/fb5f626a-4fa6-4138-8a91-ecc5778bcd3d" />

<img width="1782" height="625" alt="image" src="https://github.com/user-attachments/assets/a0f73e2b-ff8b-4df1-bf57-30ee246e8f4d" />


---

## 5️⃣ Authentication Context

The authentication attempt was triggered when the victim tried to access:

```
\\DC01\\DC‑Confidential
```

Because of the typo **DCC01**, the system broadcast an LLMNR request instead of reaching the real server.

The rogue device intercepted the request and captured NTLM authentication data.

No plaintext password is visible in network traffic because NTLM does not send the password directly.

---

## ⏱️ Timeline

| Time     | Event                                |
| -------- | ------------------------------------ |
| 11:18:30 | Victim mistypes share                |
| 11:18:30 | LLMNR broadcast sent                 |
| 11:18:30 | Rogue host responds                  |
| 11:18:30 | NTLM authentication attempt captured |

---

## 📍 Indicators of Compromise

**IP:** 172.17.79.135
**Hostname:** kali
**User:** john.deacon
**Technique:** LLMNR Poisoning

---

## 🗺️ MITRE ATT&CK Mapping

| Technique               | ID        |
| ----------------------- | --------- |
| Adversary‑in‑the‑Middle | T1557     |
| LLMNR Poisoning         | T1557.001 |
| Credential Capture      | T1003     |
| Valid Accounts          | T1078     |

---

## 🚨 Impact

* Username exposed
* NTLM authentication captured
* Potential offline password cracking
* Possible lateral movement risk

---

## 🛡️ Recommendations

* Disable LLMNR via Group Policy
* Disable NetBIOS over TCP/IP
* Enforce strong passwords
* Enable SMB signing
* Monitor for Responder activity
* Use EDR detection rules for LLMNR poisoning

---

## 🧾 Conclusion

The investigation confirmed an LLMNR poisoning attack inside the internal network.
A rogue system responded to name‑resolution requests and captured NTLM authentication attempts after a user mistyped a file share path.
Although the plaintext password was not visible, the captured NTLM data could be used for offline cracking, creating a risk of credential compromise and lateral movement inside the domain.

