# Solving IT Misery – Workflow Reference  
*Based on the webinar aired **May&nbsp;28, 2025***

> **Watch the recording:** _https://www.youtube.com/watch?v=3_beMYz_538_  

| Resource | Purpose |
| -------- | ------- |
| **Spreadsheet – Conditions Utilized** | Reference list of all script-result and device-property conditions mentioned in the session. _https://app.ninjarmm.com/ws/knowledgebase/public/resource/503/view/BYTITuk1978HYFU5WpDosjVHa8XfyQvmWg8V_ |
| **Spreadsheet – On-Boarding Automations** | Example automations used for hands-free device provisioning. _https://app.ninjarmm.com/ws/knowledgebase/public/resource/441/view/cwywNu0uZrbB4sZ3L0liYpPln0Iyn2fUiyvR_ |

---

## 1. Unauthorized Local-Administrator Workflow  

This workflow detects — and can optionally remediate — accounts that hold local-administrator rights without approval.

### 1.1  Custom Fields  

| Location | Field | Type | Automation Access |
| -------- | ----- | ---- | ----------------- |
| **Organization Custom Field** | Authorized&nbsp;Global&nbsp;Admins | Multi-line | **Read / Write** |
| **Device Custom Fields** | Local&nbsp;Admins | Multi-line | **Read / Write** |
|  | Authorized&nbsp;Admins | Multi-line | **Read / Write** |
|  | Unauthorized&nbsp;Admins | Multi-line | **Read / Write** |

### 1.2  Detection Scripts  

| Script | Source | Schedule |
| ------ | ------ | -------- |
| Report Local Admins | NinjaOne *Template Library* | Same cadence as **Check for Unauthorized Admins** |
| Check for Unauthorized Admins | <https://pastebin.com/vwetEwui> | Script-result condition (align with above) |

### 1.3  Remediation Scripts (optional)  

| Script | Purpose |
| ------ | ------- |
| Disable Bad User – <https://pastebin.com/vjw95bkg> | Disables an unauthorized account. |
| Force Bad User Logoff – <https://pastebin.com/08gMv2T8> | Immediately signs the user out. |

> **Tip:** Keep remediation disabled at first; review detections for accuracy before enforcing.

---

## 2. Endpoint-Security Compliance Report (HardeningKitty)  

Evaluates devices against a security benchmark and stores results in documentation for easy reporting.

### 2.1  Custom Fields  

| Field | Type | Automation Access | API Visibility |
| ----- | ---- | ----------------- | -------------- |
| Endpoint Hardening Score | Text | **Read / Write** | **Read-only** |
| Endpoint Hardening JSON | WYSIWYG | **Read / Write** | **Read-only** (technician view = None) |

### 2.2  Prerequisite: Automated Documentation Server  

Follow the **“Getting Started”** section of the MSPP guide to spin up a small Windows server that runs documentation-only policies:  
<https://docs.mspp.io/ninjaone/getting-started>

### 2.3  Scripts  

| Script | Runs On | Interval | Notes |
| ------ | ------- | -------- | ----- |
| HardeningKitty Retrieve Score | All endpoints | Weekly / Monthly | <https://pastebin.com/hNBMzMDc> |
| HardeningKitty Documentation Report | *Documentation server only* | Match above | <https://pastebin.com/MmYxfNwz>  <br>▪ Ensure the custom-field name on **line 329** matches exactly (case-sensitive). |

---

## 4. Important Checklist  

1. **Test in a lab** (or on a single client) before wide deployment.  
2. Confirm custom-field names and IDs match your environment.  
3. Start with *detection-only*; enable remediation after review.  

---
