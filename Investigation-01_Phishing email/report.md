# 📧 Phishing Email Incident Investigation — PhishNet (HTB Sherlock)

## 🧾 Case Overview

An accounting team received an urgent payment request from a known vendor. The email appeared legitimate but contained a suspicious payment link and a ZIP attachment. The objective of this investigation was to determine whether the email was malicious, identify attacker infrastructure, analyze the attachment, and confirm the attacker’s intent.

This case was investigated using manual email analysis techniques combined with a custom-built automation tool designed to detect phishing indicators and speed up triage.

---

## 🎯 Investigation Objectives

* Identify the true sender origin
* Analyze email routing and authentication
* Inspect phishing link and domain
* Analyze attachment and payload
* Extract indicators of compromise
* Validate findings using automated detection

---

## 🛠️ Tools Used

* Email header analysis
* Base64 decoding (certutil)
* Hash inspection
* Manual URL inspection
* **Advanced Phishing Mail Detector (custom tool)**

Custom tool repository:
[https://github.com/EclipseManic/Advance_Phishing_Mail_Detector](https://github.com/EclipseManic/Advance_Phishing_Mail_Detector)

---

## 🤖 Role of the Custom Detection Tool

The **Advanced Phishing Mail Detector** was used during the investigation to automate detection of phishing indicators and validate manual findings.

The tool performed:

* Header anomaly detection
* Sender and reply‑to comparison
* Domain reputation checks
* Attachment risk detection
* Link inspection
* Keyword pressure detection

### Why the Tool Was Important

Manual email investigations require checking multiple indicators one by one. The tool automated these checks and quickly highlighted suspicious elements, allowing faster triage.

**Impact on investigation:**

* Reduced manual checking time
* Automatically flagged malicious attachment
* Detected reply‑to mismatch
* Identified suspicious infrastructure
* Confirmed phishing risk score

Estimated time saved:
Manual investigation of all indicators would take significantly longer. Automation allowed immediate identification of high‑risk elements and reduced investigation time to a few minutes while still verifying findings manually.

The tool did not replace analysis but accelerated detection and supported conclusions.

Evidence:
![Tool Output](screenshots/tool.png)

---

## 🔍 Investigation Process

### 1. Header Analysis

Email header review revealed:

* Originating IP: **45.67.89.10**
* Relay server: **203.0.113.25**
* Sender: [finance@business-finance.com](mailto:finance@business-finance.com)
* Reply-To: [support@business-finance.com](mailto:support@business-finance.com)
* SPF: Pass
* DKIM: Pass
* DMARC: Pass

Despite authentication passing, sender behavior and attachment indicated malicious intent.

Evidence:
![Header Analysis](screenshots/header.png)

---

### 2. Phishing Link

The email contained a payment link:

```
https://secure.business-finance.com
```

The domain impersonated a legitimate finance vendor and was designed to trick recipients into trusting the email.

---

### 3. Attachment Analysis

Attachment name:

```
Invoice_2025_Payment.zip
```

SHA256 hash:

```
8379C41239E9AF845B2AB6C27A7509AE8804D7D73E455C800A551B22BA25BB4A
```

Inside archive:

```
invoice_document.pdf.bat
```

The file was disguised as a PDF but was actually an executable batch file, indicating malware delivery.

---

### 4. Payload Recovery

Encoded content was decoded using:

```
certutil -decode base64_content.txt Invoice_2025_Payment.zip
```

Recovered ZIP revealed the disguised executable.

Evidence:
![Decoding Process](screenshots/decode.png)

---

## 📍 Key Findings

* Vendor impersonation email
* Malicious ZIP attachment
* Disguised executable payload
* Phishing payment link
* Reply‑To mismatch
* Suspicious sender infrastructure
* Automated tool confirmed phishing indicators

---

## 🕒 Attack Flow

| Step | Activity                         |
| ---- | -------------------------------- |
| 1    | Phishing email delivered         |
| 2    | User prompted for urgent payment |
| 3    | Attachment opened                |
| 4    | Malicious script execution       |
| 5    | Potential system compromise      |

---

## 🗺️ MITRE ATT&CK Mapping

**T1566.001 — Phishing: Spearphishing Attachment**

---

## 🚨 Indicators of Compromise

* 45.67.89.10
* 203.0.113.25
* secure.business-finance.com
* [finance@business-finance.com](mailto:finance@business-finance.com)
* [support@business-finance.com](mailto:support@business-finance.com)
* Invoice_2025_Payment.zip
* invoice_document.pdf.bat
* SHA256 hash above

---

## 🧠 Analyst Assessment

This email represents a vendor payment phishing attack delivering a malicious attachment. The attacker used domain impersonation and urgency to pressure the recipient into opening a disguised executable.

Manual investigation confirmed suspicious infrastructure and malicious payload delivery. The custom phishing detection tool accelerated the process by automatically flagging high‑risk indicators and validating the findings.

Automation significantly reduced analysis time while still allowing full manual verification, making the investigation more efficient and structured.

---

## 🛡️ Recommended Response

* Block sender IP and domain
* Block phishing domain
* Quarantine attachment hash
* Alert users
* Add detection rules for similar emails
* Reset potentially exposed credentials

---

## 📌 Conclusion

The investigation confirmed a phishing campaign delivering malware through a disguised invoice attachment. The case demonstrates practical SOC workflow including email header analysis, payload recovery, indicator extraction, and tool‑assisted detection.

The integration of a custom phishing detection tool improved investigation speed, helped identify key indicators quickly, and supported accurate conclusions while maintaining manual verification.
