# Microsoft Entra ID — Break-Glass Account Implementation & Monitoring

**Category:** Identity and Access Management (IAM) | Cloud Security | Security Monitoring

**Technologies:** Microsoft Entra ID, Azure Monitor, Log Analytics, KQL, Microsoft Defender

## Project Overview

Identified and addressed a critical gap in the organization's identity security posture: the absence of dedicated Microsoft Entra ID emergency access (break-glass) accounts. This was one part of a larger project to bring Privileged Identity Management to the organization.

Implemented emergency access capabilities and automated monitoring to detect sign-ins involving these highly privileged accounts.

The solution provides additional administrative resilience while ensuring that emergency account usage is detected and investigated.

---

## 1. Security Gap Identified

### Problem

Prior to this project, the organization had no dedicated Microsoft Entra ID emergency access accounts.

This introduced several risks:

- Potential administrative lockout during identity or authentication service disruptions.
- Limited recovery options if standard administrator accounts became inaccessible.
- Lack of dedicated monitoring for emergency administrative access.
- Increased operational risk during identity-related security incidents.

### Objective

Establish emergency administrative access and implement automated monitoring to notify the security team whenever an emergency access account is used.

---

## 2. Solution Implemented

### Emergency Access Accounts

Established dedicated emergency access accounts within Microsoft Entra ID utilizing FIDO2.

These accounts provide an alternative administrative access mechanism when standard authentication or administrative access methods are unavailable and are exempt from all conditional access.

### Automated Sign-In Monitoring

Configured an Azure Monitor log search alert to detect authentication activity involving the emergency access accounts.

The alert queries sign-in logs collected in a Log Analytics workspace.

#### Alert Configuration

| Setting | Configuration |
|---|---|
| Monitoring platform | Azure Monitor |
| Data source | Log Analytics |
| Detection condition | Sign-in count greater than 0 |
| Evaluation frequency | 15 minutes |
| Alert rules | 1 |
| Dimension splitting | Disabled |
| Notification method | Email |
| Incident management | Microsoft Defender |

When any monitored account signs in, the alert triggers.

![Azure Monitor Alert Rule](images/BG-SignIn-Alert.png)

*Figure 2: Azure Monitor alert rule configured to detect emergency access account sign-ins.*

![Alert Condition](images/03-alert-condition.png)

*Figure 3: Alert condition configured to trigger when sign-in activity is detected.*

### Email Notifications

Created an Azure Monitor action group to deliver email notifications when emergency access account activity is detected.

This allows the security team to respond to account usage without continuously reviewing authentication logs.

![Azure Monitor Action Group](images/04-action-group.png)

*Figure 4: Azure Monitor action group configured for email notifications.*

---

## 3. Testing and Validation

### Test Procedure

Performed an authorized sign-in using a designated emergency access account to validate the monitoring configuration.

The test verified the following:

1. The emergency access account could successfully authenticate.
2. Azure Monitor detected the sign-in.
3. Microsoft Defender generated a security incident.
4. The configured email notification was delivered to all recipients.

### Test Results

| Validation Step | Result |
|---|---|
| Emergency account authentication | Passed |
| Sign-in detection | Passed |
| Defender incident generation | Passed |
| Email notification delivery | Passed |

The authorized sign-in generated a **high-severity incident in Microsoft Defender**.

The email notification was successfully received, confirming that the detection and notification workflow operated as expected.

![Microsoft Defender Incident](images/05-defender-incident.png)

*Figure 5: Microsoft Defender incident generated following the authorized test sign-in.*

![Alert Email Notification](images/06-alert-email.png)

*Figure 6: Email notification confirming successful alert delivery.*

---

## 4. Incident Investigation and Classification

Emergency access account usage is not inherently malicious.

However, because these accounts provide highly privileged access, every sign-in requires verification.

### Investigation Workflow

1. Azure Monitor detects a sign-in involving an emergency access account.
2. An alert notification is generated.
3. The security analyst reviews the sign-in activity.
4. The responsible administrator is contacted to confirm whether the activity was authorized.
5. The incident is classified according to the investigation findings.

### Authorized Sign-Ins

When a sign-in is verified as legitimate administrative activity, the incident can be classified as:

**Benign Positive → Confirmed Activity**

### Unexplained or Unauthorized Sign-Ins

Unexpected sign-ins require further investigation, including:

- Account identity
- Source IP address
- Geographic location
- Authentication details
- Related audit events
- Administrative activity performed during the session

---

## 5. Security Outcomes

### Improved Administrative Resilience

Established emergency access capabilities where none previously existed, reducing the risk of administrative lockout.

### Increased Privileged Account Visibility

Implemented automated monitoring to detect sign-ins involving highly privileged emergency access identities.

### Automated Security Notifications

Enabled email notifications and Microsoft Defender incident generation when monitored accounts are used.

### Validated Detection Capabilities

Successfully tested the monitoring configuration through an authorized emergency account sign-in.

### Established an Investigation Process

Defined a repeatable workflow for reviewing emergency access activity and distinguishing authorized usage from suspicious behavior.

---

## 6. Technologies and Skills Demonstrated

| Technology / Skill | Application |
|---|---|
| Microsoft Entra ID | Emergency access account management |
| Azure Monitor | Automated sign-in monitoring |
| Log Analytics | Authentication log analysis |
| Kusto Query Language (KQL) | Detection query configuration |
| Azure Monitor Action Groups | Email notifications |
| Microsoft Defender | Incident investigation and classification |
| Identity and Access Management | Privileged access security |
| Detection Engineering | Security alert implementation and validation |
| Incident Response | Investigation and incident disposition |

---

## 7. Lessons Learned

### Emergency Access Is Essential for Business Continuity

Organizations require reliable administrative recovery mechanisms when standard authentication or privileged access methods become unavailable.

### Privileged Account Activity Requires Monitoring

Emergency access accounts are intended for infrequent use. Their authentication activity should therefore be investigated whenever it occurs.

### Security Controls Must Be Tested

Creating an alert rule does not guarantee that the complete detection and notification workflow functions correctly.

Performing an authorized sign-in confirmed that the monitoring rule, Defender incident generation, and email notifications operated as intended.

### Not Every Security Alert Represents Malicious Activity

An authorized emergency access account sign-in can generate a legitimate security alert without indicating a compromise.

Investigation and verification are necessary before determining the appropriate incident classification.

---

## 8. Future Improvements

Potential enhancements include:

- Periodic emergency access account sign-in testing.
- Regular review of emergency access authentication methods.
- Monitoring changes to emergency access account configurations.
- Documenting emergency access procedures and account ownership.
- Periodic review of alert effectiveness and response procedures.

---

## Security and Confidentiality

All screenshots included in this repository are intended to be sanitized before publication.

Sensitive information such as usernames, tenant identifiers, email addresses, subscription identifiers, and internal configuration details must be removed.

This project documents security engineering activities without disclosing confidential production environment information.

---

## Project Summary

Identified and addressed the absence of dedicated Microsoft Entra ID emergency access accounts.

Implemented automated monitoring for four break-glass identities using Azure Monitor and Log Analytics, configured email notifications, and successfully validated the detection workflow through an authorized sign-in.

**Key competencies demonstrated:** Identity security, privileged access management, Azure security monitoring, detection engineering, and incident response.
