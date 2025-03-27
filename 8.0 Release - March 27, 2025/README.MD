# 🚀 NinjaOne 8.0 Release

## 🛠 Patching Automations
  There are now native automations for initiating OS patch scans and apply cycles across Windows, MacOS, and Linux. Software patch scan and apply cycles can also be executed across Windows and MacOS.

  Examples of how these automations can be utilized:

### Conditions
- **No Patch in 30 Days**  
  Run a patch install cycle if a device has gone more than 30 days without a patch install.
- **High CVSS Score Patch**  
  Run a patch install cycle if a patch with a high CVSS score is available.

### Tasks & Groups
- **Patch Scan Cycle**  
  Run a patch scan cycle on devices that haven’t had one in a certain number of days.
- **Patch Install on Approval**  
  Run a patch install cycle when an approved patch becomes available.

---

## 📁 Template Library
  There are several new automations added to the template library, including one for upgrading a Windows 11 device to the latest version, and one to upgrade a Windows 10 device to Windows 11.

### Compound Condition: Upgrade Windows 10 to Windows 11
- **Custom Field:** `Windows 11 Compatibility`  
  Populated by the template library script.
- **Scripts Used:**
  - `Check-WindowsVersion` – Detects if the system is Windows 10 or 11. https://github.com/jeffhunterninja/NinjaOne-Scripts/blob/main/Check-NinjaWindowsVersion/Check-NinjaWindowsVersion.ps1
  - `Check-NinjaTime` – Defines an approved upgrade window. https://github.com/jeffhunterninja/NinjaOne-Scripts/tree/main/Check-NinjaTime
- **Automation:**
  - `Allow Windows 10 to 11 Upgrade` script from template library to remove previous registry blocks on Windows 11 Upgrade
  - `Update Windows 10 to Windows 11` script from template library to perform upgrade

---

## 🛡 Patch + Vulnerability Management

- Vulnerabilities now identified via **missing patches**.
- **API endpoints** added to query vulnerability and patch data.

---

## 🏷 Tags

### Overview
Use tags to drive automation and software deployment across devices.

### Compound Condition: Upgrade Windows 10 to Windows 11
Just like the example above, but with the addition of a tag that identifies authorized devices that are allowed to upgrade

- **Custom Field:** `Windows 11 Compatibility`  
  Populated by the template library script.
- **Scripts Used:**
  - `Check-WindowsVersion` – Detects if the system is Windows 10 or 11. https://github.com/jeffhunterninja/NinjaOne-Scripts/blob/main/Check-NinjaWindowsVersion/Check-NinjaWindowsVersion.ps1
  - `Check-NinjaTime` – Defines an approved upgrade window. https://github.com/jeffhunterninja/NinjaOne-Scripts/tree/main/Check-NinjaTime
  - `Check-NinjaTag` – Triggers when one or more tags are present. https://github.com/jeffhunterninja/NinjaOne-Scripts/tree/main/Check-NinjaTag

### Tasks
- **Deploy Software via Tags**  
  Automate software deployments based on assigned tags - i.e. devices tagged with `Accounting` would receive accounting related software that is missing from their devices.

### API
- Tags can now be **set via API**, enabling programmatic tag management.

---

## 🖥 NinjaOne Remote Background Mode

---

## 📱 Mobile Device Management (MDM)

### Android (7.0 and 8.0)
- MDM enrollment info in **NinjaOne Assist**
- **Always-On VPN** configuration
- **Factory Reset Protection**
- Control over **Permitted Accessibility Services**
- Support for **Delegated Scope**
- Custom **Lock Screen and Support Messages**
- **Input Method Management**
- Support for **Hidden SSIDs**
- Redesigned **Managed App Configurations**
- Full **Android Remote Control**

### iOS
- Support for **Custom Payloads**
- **Managed App Configurations**
- **Variable Support** in profiles

---

## 🔐 Agent Tokenization *(Early Access)*

- Token-based agent identity support for secure deployments and registration. Request access from your account manager

---

## 📋 NinjaOne PSA *(Early Access)*

- Integrated PSA solution now available in early access.

---
