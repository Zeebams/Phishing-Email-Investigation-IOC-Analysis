#  Phishing Email Investigation & IOC Analysis

##  Project Overview

This project demonstrates the investigation of a suspected phishing email reported by an employee.

The investigation simulates a Security Operations Center (SOC) analyst responding to a phishing alert, with a focus on identifying suspicious email characteristics, extracting Indicators of Compromise (IOCs), analyzing potential threats, mapping observed techniques to the MITRE ATT&CK framework, assessing risk, and recommending appropriate containment and remediation actions.



## Disclaimer

This is a controlled cybersecurity lab using **simulated phishing artifacts and simulated threat-intelligence results** for educational and portfolio purposes.

No real malicious infrastructure was accessed or interacted with during this investigation.



## Investigation Objectives

The objectives of this investigation were to:

* Determine whether the reported email was malicious.
* Identify suspicious characteristics within the email.
* Analyze sender information and URLs.
* Extract Indicators of Compromise (IOCs).
* Assess the potential threat associated with the identified IOCs.
* Map observed attacker techniques to the MITRE ATT&CK framework.
* Assess the potential impact of the phishing attempt.
* Recommend containment and remediation actions.
* Document the investigation and findings.



## Tools & Techniques Used

* Email analysis
* Manual IOC extraction
* URL and domain analysis
* Threat intelligence analysis
* CyberChef
* VirusTotal-style IOC assessment
* MITRE ATT&CK
* Browser-based investigation
* Markdown/GitHub documentation



# 1. Incident Scenario

An employee reported receiving a suspicious email claiming to be from the organization's Microsoft 365 security team.

The email stated that unusual activity had been detected on the employee's account and that the account would be suspended within 24 hours unless the user completed an account verification process.

The email contained a link directing the employee to an external login page.

The employee noticed that the sender's address appeared suspicious and reported the email before clicking the link.

As the SOC analyst, I was tasked with determining whether the email was legitimate or part of a phishing campaign.


# 2. Phishing Email Analysis

### Reported Email

**From:** `security@micr0soft-support.com`

**To:** `employee@company.local`

**Subject:** `URGENT: Your Microsoft 365 Account Will Be Suspended`

**Date:** October 8, 2026, 09:14 AM

### Email Content

> Dear User,
>
> We detected unusual activity on your Microsoft 365 account.
>
> For your security, your account will be suspended within 24 hours unless you complete the verification process.
>
> Please verify your account immediately using the link below:
>
> **Verify Your Microsoft 365 Account**
>
> Failure to complete this verification may result in permanent account suspension.
>
> Microsoft 365 Security Team
>
> This is an automated security notification. Please do not reply to this email.

### Extracted URL

`https://micr0soft-account-verification[.]com/login`



## Initial Observations

Several suspicious characteristics were identified during the initial review:

* The sender domain does not match Microsoft's legitimate domain.
* The domain uses `micr0soft` instead of `microsoft`.
* The email creates urgency by threatening account suspension.
* The recipient is instructed to verify their account through an external link.
* The destination domain does not belong to Microsoft's legitimate infrastructure.
* The email attempts to direct the user to a login page where credentials could potentially be collected.


### Initial Assessment

The combination of impersonation, urgency, a lookalike domain, and a suspicious login link strongly suggests a **credential-phishing attempt**.


# 3. Indicators of Compromise (IOCs)

The following indicators were extracted from the suspicious email.

| IOC                                                  | Type          | Description                                                 | Risk     |
| ---------------------------------------------------- | ------------- | ----------------------------------------------------------- | -------- |
| `security@micr0soft-support.com`                     | Email Address | Suspicious sender impersonating Microsoft security services | High     |
| `micr0soft-support.com`                              | Domain        | Lookalike domain using a typo in the Microsoft name         | High     |
| `https://micr0soft-account-verification[.]com/login` | URL           | Suspicious account verification URL                         | Critical |
| `micr0soft-account-verification.com`                 | Domain        | Domain associated with the suspected phishing login page    | Critical |


# 4. IOC Reputation & Threat Analysis

The extracted indicators were assessed using simulated threat-intelligence results as part of this controlled lab.

## 4.1 Sender Domain

**IOC:** `micr0soft-support.com`

The sender domain was identified as a lookalike domain impersonating Microsoft.

The domain replaces the letter **"o"** in "Microsoft" with the number **"0"**, a technique commonly used to make fraudulent domains appear legitimate.

**Assessment:** Malicious



## 4.2 Phishing Domain

**IOC:** `micr0soft-account-verification.com`

The domain was identified as suspicious because it does not belong to Microsoft's legitimate infrastructure and is structured around account verification.

The naming convention, combined with the content of the email, suggests that the domain was intended to deceive users into believing they were accessing a legitimate Microsoft account security page.

**Assessment:** Malicious



## 4.3 Phishing URL

**IOC:** `https://micr0soft-account-verification[.]com/login`

The URL points to a login endpoint hosted on the suspicious domain.

The use of a login page alongside an urgent account-suspension message creates a strong indication of credential harvesting.

The URL was therefore classified as a **high-risk phishing indicator**.

**Assessment:** Malicious



# 5. MITRE ATT&CK Mapping

The observed phishing activity was mapped to the MITRE ATT&CK framework.

| Tactic            | Technique                         | ID        | Evidence                                                                     |
| ----------------- | --------------------------------- | --------- | ---------------------------------------------------------------------------- |
| Initial Access    | Phishing: Spearphishing Link      | T1566.002 | Email contains a link directing the victim to a fraudulent login page        |
| Credential Access | Input Capture: Web Portal Capture | T1056.003 | Fraudulent login page could capture credentials entered by the victim        |
| Credential Access | Phishing: Spearphishing Link      | T1566.002 | Phishing link is used to direct the victim to the credential-harvesting page |
| Persistence       | Valid Accounts                    | T1078     | Stolen credentials could potentially be used to access legitimate services   |

### Technique Analysis

### T1566.002 — Phishing: Spearphishing Link

The attacker used an email containing a malicious link that directs the recipient to a fraudulent login page.

The email impersonates Microsoft and uses urgency to encourage the recipient to click the link and verify their account.

This represents the primary initial access technique observed during the investigation.


### T1056.003 — Input Capture: Web Portal Capture

The fraudulent login page is designed to imitate a legitimate Microsoft authentication page.

If a victim enters their username and password, the attacker could potentially capture the submitted credentials.

This represents the potential credential theft mechanism associated with the phishing campaign.


### T1078 — Valid Accounts

If credentials were successfully captured, an attacker could potentially use those credentials to authenticate to legitimate services.

However, there is **no evidence that credentials were actually compromised during this investigation**.

Therefore, Valid Accounts is documented as a potential follow-on technique rather than a confirmed technique.


# 6. Impact & Risk Assessment

## Confidentiality

If credentials were successfully captured, an attacker could potentially gain unauthorized access to the employee's Microsoft 365 account, exposing sensitive emails, documents, and business information.

## Integrity

A compromised account could potentially be used to modify emails, account settings, or other resources.

## Availability

An attacker could potentially change account credentials or settings and temporarily prevent the legitimate user from accessing the account.

## Account Takeover

Successful credential theft could allow an attacker to attempt unauthorized access to the victim's Microsoft 365 account and potentially use the compromised account for further phishing activity.


## Risk Rating

**Overall Risk: HIGH**

The incident was classified as High Risk because the phishing attempt was designed to obtain authentication credentials.

However, the actual impact was limited because the employee reported the email **before interacting with the phishing link**.

### Incident Status

| Category                     | Assessment          |
| ---------------------------- | ------------------- |
| Incident Type                | Credential Phishing |
| Severity                     | High                |
| Status                       | Contained           |
| Confirmed Credential Theft   | No                  |
| Confirmed Account Compromise | No                  |
| Primary Objective            | Credential Theft    |
| Actual Impact                | Limited             |


# 7. Containment & Remediation

Based on the investigation findings, the following actions are recommended.

## Immediate Containment

### 1. Quarantine the Phishing Email

Remove the malicious email from the reporting employee's mailbox and quarantine identical messages found within the organization.

### 2. Block Malicious Domains

Add the identified phishing domains to relevant email security, DNS filtering, web proxy, and endpoint security controls.

```text
micr0soft-support.com
micr0soft-account-verification.com
```

### 3. Block the Phishing URL

Add the identified URL to applicable web-filtering and security controls.

### 4. Search for Additional Recipients

Review email security logs to determine whether the same phishing message was delivered to other employees.

### 5. Search Security Logs

Search DNS, proxy, firewall, email gateway, and endpoint logs for activity associated with the identified IOCs.


## Credential Protection

If any employee interacted with the phishing page, recommended actions include:

* Resetting affected account passwords.
* Revoking active sessions or authentication tokens where appropriate.
* Reviewing recent authentication activity.
* Investigating unusual login locations or devices.
* Verifying that multi-factor authentication is enabled.


## User Awareness

Employees should be reminded to:

* Verify unexpected security-related emails.
* Carefully inspect sender domains.
* Avoid clicking suspicious links.
* Be cautious of messages creating unnecessary urgency.
* Report suspected phishing emails to the security team.


## Post-Incident Monitoring

Security teams should continue monitoring authentication, email, DNS, and endpoint logs for suspicious activity associated with the identified indicators.

Additional investigation should be performed if evidence of successful interaction or credential submission is discovered.


# 8. Investigation Conclusion

The investigation determined that the reported email was a **credential-phishing attempt impersonating Microsoft 365 security services**.

Multiple indicators supported this conclusion, including:

* A lookalike sender domain.
* A suspicious account verification domain.
* An external login URL.
* Urgency-based social engineering.
* An attempt to obtain authentication credentials.

### Primary IOCs Identified

```text
security@micr0soft-support.com

micr0soft-support.com

micr0soft-account-verification.com

https://micr0soft-account-verification[.]com/login
```

The activity was mapped primarily to:

**T1566.002 — Phishing: Spearphishing Link**

Potential credential capture was also mapped to:

**T1056.003 — Input Capture: Web Portal Capture**

No evidence was identified to confirm successful credential theft or account compromise because the employee reported the email before interacting with the phishing link.


# 9. Final Incident Classification

| Field                            | Result                                                      |
| -------------------------------- | ----------------------------------------------------------- |
| **Incident Type**                | Phishing                                                    |
| **Attack Category**              | Credential Phishing                                         |
| **Severity**                     | High                                                        |
| **Status**                       | Contained                                                   |
| **Confirmed Compromise**         | No                                                          |
| **Primary Objective**            | Credential Theft                                            |
| **Primary Technique**            | T1566.002 — Spearphishing Link                              |
| **Potential Credential Capture** | T1056.003                                                   |
| **Recommended Response**         | Block IOCs, quarantine email, search logs, monitor accounts |


# 10. Evidence

The phishing email used in this investigation is included as an image attachment to this project.

The attached evidence contains the suspicious sender address, subject line, phishing message, and account verification link analyzed during the investigation.

# 11. Key Takeaways

This investigation provided practical experience in:

* Phishing email analysis.
* Identifying social-engineering indicators.
* Extracting and categorizing IOCs.
* Domain and URL analysis.
* Threat assessment.
* MITRE ATT&CK mapping.
* Risk assessment.
* Incident containment.
* Security log investigation planning.
* SOC incident documentation.

The project demonstrates the ability to investigate a suspected phishing incident from **initial detection through IOC analysis, threat assessment, response recommendations, and final documentation**.
