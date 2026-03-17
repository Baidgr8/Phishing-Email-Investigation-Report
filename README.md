# Phishing Email Investigation Report


**Incident Type:** Phishing – Malware Delivery  
**Analyst:** Babafemi A. Ikujebi  
**Role:** SOC Tier 1 Analyst (Simulation)  
**Date:** January 13, 2026  
**Environment:** Simulated Corporate SOC Investigation (TryHackMe)

---

# SOC Case Snapshot

| Field | Details |
|------|------|
| Incident Type | Phishing – Malware Delivery |
| Environment | Corporate SOC (Simulated) |
| Alert Source | User-reported suspicious email |
| Targeted Role | Finance / Business user |
| Severity | High |
| Key Tools | MXToolbox, VirusTotal |
| Attack Objective | Malware delivery & potential credential compromise |
| PrimaryTechniques | T1566.001 – Spearphishing Attachment,T1036 – Masquerading 
| Status | Confirmed True Positive |
| Escalation | Yes – Tier 2 recommended |

---

## Project Overview (Recruiter Quick View)

This project demonstrates a **SOC Tier 1 phishing investigation workflow** using a simulated corporate environment.

A suspicious email reported by a user was analyzed to determine whether it represented a security threat. The investigation included:

- Email header analysis (SPF, DKIM, DMARC)
- Social engineering detection
- Attachment malware analysis
- Threat intelligence verification using VirusTotal
- IOC extraction for detection and blocking

### Key Findings

- Spoofed sender domain with **SPF authentication failure**
- **Malicious compressed attachment** disguised as a financial document
- Malware payload confirmed via **VirusTotal detection (47/63 engines)**

### Incident Classification

**True Positive — High Severity Phishing Attack**

### Skills Demonstrated

- SOC alert triage
- Email header investigation
- Malware attachment analysis
- IOC extraction
- Threat intelligence analysis



# Executive Summary

On **13 January 2026**, a Sales Executive at **Greenholt PLC** reported a suspicious email delivered to his corporate mailbox. The message impersonated a financial institution and attempted to pressure the recipient into opening a compressed attachment.

I initiated a **SOC Tier 1 phishing investigation** to determine whether the email was legitimate or malicious.

During the investigation I analyzed:

- Email content
- Sender infrastructure
- Email authentication records
- The attached file and associated hash

The analysis revealed several indicators of compromise including:

- Failed **SPF authentication**
- Mismatched **Sender and Reply-To addresses**
- A **compressed attachment masquerading as a PDF document**

Threat-intelligence analysis confirmed that the attachment was **malicious malware payload**.

The incident was therefore classified as a **True Positive phishing attack with high severity**, and escalation to **Tier 2 security analysts** was recommended.

---

# Reported Email Evidence

### Screenshot – Reported Email

![Reported Email Screenshot A](https://github.com/Baidgr8/Phishing-Email-Investigation-Report/blob/1d551815c8692af6e460c7a22df4f5c662c1dcad/email%20reported.PNG)

![Reported Email Screenshot B](https://github.com/Baidgr8/Phishing-Email-Investigation-Report/blob/2f2872dc41515a7df1859bbb05f9e0492cc0e6bf/email%20reported2.PNG)

![Reported Email Screenshot C](https://github.com/Baidgr8/Phishing-Email-Investigation-Report/blob/4a7ca56a9e7c6e9f78283adf114878c2dd0c07fb/email%20reported%203.PNG)

---

# Investigation Methodology

I followed a structured SOC phishing investigation workflow:

1. Initial alert triage
2. Email content inspection
3. Sender identity validation
4. Email header authentication analysis
5. Attachment and malware analysis
6. Indicator of Compromise extraction
7. Threat classification
8. Response and escalation recommendations

---

# Alert Details & Initial Triage

During initial triage, I examined the email content and identified several phishing indicators:
- Urgent and manipulative language
- Unexpected attachment
- External sender domain
- Financial-themed lure commonly associated with malware campaigns

Based on these initial observations, I classified the alert as suspicious and proceeded with a full phishing investigation.

![inital triage Screenshot C](https://github.com/Baidgr8/Phishing-Email-Investigation-Report/blob/060d780e57a083ee03240e6e55f988e1dae6dd63/assets/AT1.PNG)
---

# Email Content Analysis

I analyzed the visible components of the email to determine whether it displayed social-engineering or impersonation techniques.

Key observations included:

- The sender’s **display name attempted to impersonate a trusted financial organization.**
- The **sender email address did not match the legitimate domain of the organization being impersonated.**
- The email used **urgency-based language** designed to pressure the recipient into opening the attachment.
- The message **lacked personalized identifiers typically used in legitimate corporate communications.**
  
These elements are consistent with phishing campaigns designed to manipulate recipients into executing malicious content.

---

## Email Metadata

| Field | Value |
|------|------|
| Display Name | Mr. James Jackson |
| Sender Address | info@mutawamarine.com |
| Reply-To Address | info.mutawamarine@mail.com |
| Subject | webmaster@redacted.org your: Transfer Reference Number:(09674321) |

---

# Email Header Analysis

To validate the authenticity of the message, I extracted the **full email headers** and analyzed them using **MXToolbox**.

### Screenshot – Header Analysis

![email header(raw file)](https://github.com/Baidgr8/Phishing-Email-Investigation-Report/blob/c09d403f7a1bc987ee30ee64470b61b85d040810/assets/email%20header%20raw%20file.PNG)
![email header analysis MXToolbox (raw file)](https://github.com/Baidgr8/Phishing-Email-Investigation-Report/blob/4debd3c8e478b7844fc2fd58010a9f614e948cc5/assets/parser%20analysis%20result2.PNG)
---

## Header Findings

| Header Field | Observation |
|------|------|
| SPF | Failed |
| DKIM | Not aligned |
| DMARC | Misconfigured |
| Reply-To | Mismatch |
| Originating IP | Suspicious infrastructure |

Key technical findings:

- **SPF hard fail** indicates the sending IP was **not authorized** to send email for the domain.
- **DKIM signature was missing or invalid**, meaning message integrity could not be verified.
- **Reply-To mismatch** suggests sender spoofing.

---

## Email Header Details

| Header Name | Header Value |
|------|------|
| Return Path | info@mutawamarine.com |
| Originating IP | 192.119.71.157 |
| SPF Result | Fail |
| DMARC Policy | Quarantine |

The originating IP address **192.119.71.157** does not appear in the domain's authorized SPF record.

This confirms the message was **spoofed and sent from unauthorized infrastructure**.

---

# Attachment & Malware Analysis

The email contained a **compressed attachment disguised as a financial document**.

### Screenshot – Hash Generation

![Hash Generation Screenshot](https://github.com/Baidgr8/Phishing-Email-Investigation-Report/blob/941f0b9a1302bef83f82685f53328d61a75e0473/assets/SHA256%20Hashing%20and%20file%20type.PNG)

---

## File Analysis

| Field | Value |
|------|------|
| File Name | SWT_#09674321_PDF_.CAB |
| Actual File Type | RAR Archive |
| File Size | 400.26 KB |
| SHA256 | 2e91c533615a9bb8929ac4bb76707b2444597ce063d84a4b33525e25074fff3f |
| VirusTotal Detection | 47/63 |

The attachment used **double extension masquerading** to appear as a legitimate PDF file.

MIME analysis confirmed the file was **actually a RAR archive containing executable malware**.

---

### Screenshot – VirusTotal Results

![VirusTotal Results](https://github.com/Baidgr8/Phishing-Email-Investigation-Report/blob/0b5b83ce6a95cf4778841fa78f82ba3d5566fa5b/assets/hash%20lookup%20on%20virustotal.PNG)

VirusTotal analysis confirmed:

- High malware detection ratio
- Classification as **Trojan/Downloader**
- Association with known phishing campaigns

---

# Indicator of Compromise (IOC) Extraction

| IOC Type | Indicator |
|------|------|
| Sender Email | info@mutawamarine.com |
| Originating IP | 192.119.71.157 |
| File Hash | 2e91c533615a9bb8929ac4bb76707b2444597ce063d84a4b33525e25074fff3f |
| Malicious Attachment | SWT_#09674321_PDF_.CAB |

These indicators were documented for **security monitoring and blocking**.

# MITRE ATT&CK Mapping

| Tactic | Technique | ID | Description |
|------|------|------|------|
| Initial Access | Spearphishing Attachment | <a href="https://attack.mitre.org/techniques/T1566/001/">T1566.001</a> | Malicious attachment used to deliver malware |
| Defense Evasion | Masquerading | <a href= "https://attack.mitre.org/techniques/T1036/">T1036</a> | File disguised as legitimate document |
| Execution | User Execution |<a href="https://attack.mitre.org/techniques/T1204/"> T1204 </a> | Malware relies on user opening attachment |
---

# Attack Flow Analysis

The phishing attack followed a typical malware delivery chain:

1. Attacker sends spoofed phishing email.
2. Email bypasses gateway due to weak DMARC policy.
3. User receives malicious attachment disguised as financial document.
4. Attachment contains compressed archive with malware payload.
5. If executed, malware installs on endpoint and may download additional payloads.

Attack Chain:

Phishing Email → Malicious Attachment → User Execution → Malware Installation → Potential Credential Theft / System Compromise
# Findings

The investigation produced the following key findings:

#### 1. Sender Spoofing
SPF authentication failure confirmed the message was not sent from authorized infrastructure.

#### 2. Malicious Attachment
The compressed attachment contained **malware payload disguised as a document**.

#### 3. Evasion Technique
The attacker used **double file extension masquerading**.

#### 4. Social Engineering
The email used **financial transaction urgency** to manipulate the recipient.

#### 5. High Risk of Compromise
If executed, the malware could have resulted in:

- Endpoint compromise
- Credential theft
- Secondary malware download

---
# Detection Opportunities

Security monitoring systems could detect this attack using the following methods:

• Email gateway alerts for SPF authentication failures  
• Detection rules for double file extensions (e.g. *.pdf.exe or disguised archives)  
• Monitoring attachments containing compressed archives from external senders  
• Threat intelligence alerts for known malicious file hashes  
• User-reported suspicious email alerts

# Final Assessment

I classified the email as a **True Positive phishing attack** with high potential impact.

Immediate containment and escalation were recommended.

---

# SOC Response & Recommendations

Recommended actions:

- Block malicious **sender domain**
- Block **originating IP address**
- Block malicious **file hash**
- Remove email from all mailboxes
- Implement **double-extension detection**
- Escalate to **Tier 2 analysts for endpoint verification**
- Conduct **phishing awareness training**

---

# Lessons Learned

- Email authentication controls (SPF, DKIM, DMARC) are critical for detecting spoofed messages.
- Threat intelligence platforms help validate malicious indicators.
- Social-engineering remains a major threat vector in phishing campaigns.
- Proper SOC documentation improves incident response effectiveness.

---

# Skills Demonstrated

- Phishing email investigation
- Email header analysis
- Malware attachment analysis
- IOC extraction and documentation
- MITRE ATT&CK mapping
- Threat intelligence analysis
- SOC incident reporting
