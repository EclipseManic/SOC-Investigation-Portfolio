# 📧 Phishing Email Incident Investigation — Vendor Impersonation Campaign

## 🧾 Case Overview

An accounting team received an urgent payment request appearing to originate from a known vendor. The message contained a payment link and a compressed attachment disguised as an invoice. The objective of this investigation was to determine whether the email was malicious, identify attacker infrastructure, analyze the attachment, and confirm the attacker’s intent.

This investigation was conducted using manual email analysis techniques supported by a custom-built automation tool to accelerate detection and validation.

---

## 🎯 Investigation Objectives

* Identify true sender origin
* Validate email authentication and routing
* Inspect phishing link and domain
* Analyze attachment and payload
* Extract indicators of compromise (IOCs)
* Confirm attacker technique and intent

---

## 🛠️ Tools Used

* Manual email header analysis
* Base64 decoding using certutil
* Hash inspection
* URL inspection
* **Advanced Phishing Mail Detector (custom tool)**

Custom tool repository:
[https://github.com/EclipseManic/Advance_Phishing_Mail_Detector](https://github.com/EclipseManic/Advance_Phishing_Mail_Detector)

---

## 🤖 Tool-Assisted Detection

A custom automation tool, **Advanced Phishing Mail Detector**, was used to assist with triage and validation of phishing indicators during the investigation.

The tool performed:

* Header anomaly detection
* Sender vs reply-to comparison
* Domain and IP reputation checks
* Attachment risk detection
* Keyword pressure analysis
* Final phishing risk scoring

The automated analysis flagged:

* Reply-to mismatch
* Suspicious infrastructure
* Malicious attachment
* Urgent social-engineering language
* Final verdict: **UNSAFE**

Evidence:  ![1770791819040](https://github.com/user-attachments/assets/431a0b29-8f7b-48d3-893d-0b736906f0a7)


---

## ⏱️ Automation Impact on Workflow

Manual analysis of all email indicators requires checking headers, domains, attachments, and message content individually. The custom tool automated these checks and produced a structured summary of risks.

This improved investigation efficiency by:

* Highlighting high-risk indicators immediately
* Reducing repetitive manual checks
* Supporting faster triage
* Providing validation for manual findings

Automation did not replace analysis but accelerated detection and improved investigation workflow.

---

## 🔍 Investigation Process

### 1. Header Analysis

Header review identified the following infrastructure:

* Originating IP: **45.67.89.10**
* Relay server: **203.0.113.25**
* Sender: [finance@business-finance.com](mailto:finance@business-finance.com)
* Reply-To: [support@business-finance.com](mailto:support@business-finance.com)
* SPF: Pass
* DKIM: Pass
* DMARC: Pass

Although authentication passed, the reply-to mismatch and attachment raised suspicion.

Evidence:   ![1770791819040](https://github.com/user-attachments/assets/2a941dcb-8795-4ac8-af60-743f76b3e210)



---

### 2. Phishing Link Analysis

The email contained a payment link pointing to:

```
https://secure.business-finance.com
```

The domain impersonated a legitimate vendor domain and was intended to appear trustworthy.

---

### 3. Attachment Analysis

Attachment name:

```
Invoice_2025_Payment.zip
```

SHA-256:

```
8379C41239E9AF845B2AB6C27A7509AE8804D7D73E455C800A551B22BA25BB4A
```

Contents:

```
invoice_document.pdf.bat
```

The file was disguised as a PDF but was actually an executable batch file designed to run on the victim system.

---

### 4. Payload Recovery

Encoded content was decoded using:

```
certutil -decode base64_content.txt Invoice_2025_Payment.zip
```

Recovered archive confirmed presence of disguised executable.

Evidence:    ![1770791819382](https://github.com/user-attachments/assets/d613e7b9-402d-4a48-9c28-67d543df9204)


---

## 📍 Key Findings

* Vendor impersonation phishing email
* Malicious ZIP attachment
* Disguised executable payload
* Reply-to mismatch
* Suspicious sender infrastructure
* Tool-assisted detection confirmed risk

---

## 🕒 Attack Flow

| Step | Activity                         |
| ---- | -------------------------------- |
| 1    | Phishing email delivered         |
| 2    | User prompted for urgent payment |
| 3    | Attachment opened                |
| 4    | Malicious script execution       |
| 5    | Potential compromise             |

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

The email represents a vendor payment phishing attack delivering a malicious attachment disguised as an invoice. The attacker used domain impersonation and urgency to pressure the recipient into opening the file.

Manual analysis confirmed suspicious infrastructure and payload delivery. The custom detection tool accelerated the identification of key indicators and validated the findings, improving investigation efficiency while maintaining manual verification.

---

## 🛡️ Recommended Response

* Block sender IP and domain
* Block phishing domain
* Quarantine attachment hash
* Alert users
* Add detection rules
* Reset potentially exposed credentials

---

## 📌 Conclusion

The investigation confirmed a phishing campaign delivering malware through a disguised invoice attachment. The case demonstrates practical SOC workflow including email header analysis, payload recovery, indicator extraction, and tool-assisted detection.

The integration of automation improved triage speed, supported accurate findings, and strengthened the overall investigation process.

