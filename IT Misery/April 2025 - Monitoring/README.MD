# IT Misery – Monitoring and Remediation

A practical outline of monitoring conditions and response automations in NinjaOne, with examples and notes.

As discussed here: https://www.youtube.com/watch?v=WwYDO02BqDk

# Table of Contents

- [Restarting a Service](#restarting-a-service)
- [Monitoring for Firewall Status](#monitoring-for-firewall-status)
- [Monitoring for BitLocker Status](#monitoring-for-bitlocker-status)
- [Alerting for High CVSS Score Patches](#alerting-for-high-cvss-score-patches)
- [Devices with No Patch Installed in 30+ Days](#devices-with-no-patch-installed-in-30-days)
- [Checking if Windows Update Service is Disabled](#checking-if-windows-update-service-is-disabled)
- [Detecting Devices on Guest/Incorrect WiFi](#detecting-devices-on-guestincorrect-wifi)
- [Checking for Unauthorized Administrator Users](#checking-for-unauthorized-administrator-users)
- [Upgrading Devices to Windows 11](#upgrading-devices-to-windows-11)
- [Sending Messages to End Users](#sending-messages-to-end-users)
- [Folder Tree Size Report for Low Disk Space Troubleshooting](#folder-tree-size-report-for-low-disk-space-troubleshooting)
- [Running a System Performance Check](#running-a-system-performance-check)
- [Monitoring for Time Drift](#monitoring-for-time-drift)
- [Final Notes](#final-notes)


---

## Restarting a Service

Create a condition that monitors for a service — in our example, the **Print Spooler**.  
When the alert triggers, we can automatically attempt to fix it by executing a script.

In this case, I used the **"Start or Stop – Windows Service"** automation (available in all NinjaOne environments) and configured it to start the Print Spooler service.

Tip: Set the alert to auto-reset **"When no longer met"** — this way, if the automation succeeds, the alert clears without manual intervention.

---

## Monitoring for Firewall Status

Start by importing the **"Firewall Status – Audit"** script from the Template Library.

- Create a **Script Result Condition** tied to that script.
- Set it to run on a regular schedule of your choosing.
- Configure the result to trigger on **any** code, and use **"firewall profile is disabled"** as your output matching text.

For remediation, you can import the **"Enable or Disable Windows Firewall"** script and add that as the response automation if a firewall profile is detected as disabled.

---

## Monitoring for BitLocker Status

Create a monitoring condition that looks for BitLocker being **disabled** or **unlocked**.

There’s a great BitLocker enablement script available here:
- [Discord NinjaOne Server](https://discord.com/channels/676451788395642880/1268702011784691853)
- [Public GitHub mirror](https://github.com/bf-ryanalexander/Scripts/blob/main/Enable-BitLocker.ps1)

Also, if you need something more custom (like suspending/un-suspending BitLocker), check out the official PowerShell documentation:
- [BitLocker Cmdlets](https://learn.microsoft.com/en-us/powershell/module/bitlocker/?view=windowsserver2025-ps)

> **Note:** BitLocker can be tricky — I strongly recommend testing on appropriate hardware and having a solid process for encryption and user/service account access before deploying remediation automatically.

---

## Alerting for High CVSS Score Patches

You can create a condition that fires when a patch with a **high CVSS score** is available.

In response, trigger the built-in **"OS Patch Apply"** automation to run an apply cycle per policy.  
(Just make sure the correct KB has been **approved** — otherwise, nothing happens!)

---

## Devices with No Patch Installed in 30+ Days

Another condition to have: alert if no patch has been installed in 30+ days.

Some notes:
- **30 days** is an absolute minimum.
- I usually prefer **45 days** to cut down noise around long Patch Tuesday cycles.
- Windows Defender updates can muddy things — I recommend using the **"Ninja Update Engine Only"** option when setting this up.

Remediation would again be running the **OS Patch Apply** automation.

---

## Checking if Windows Update Service is Disabled

Import and save this script:  
👉 [Windows Update Service Check](https://pastebin.com/Wpa4YN9S)

Set it up as a **Script Result Condition**:
- Result Code = `1`
- Output text = `"Windows Update service is disabled"`

When detected, run the **Windows Update Diagnostic** tool as remediation — it checks a bunch of underlying Windows Update components.

---

## Detecting Devices on Guest/Incorrect WiFi

*(Placeholder – not expanded in these notes, but the workflow is very similar to checking for unauthorized users.)*

---

## Checking for Unauthorized Administrator Users

This is a bit of a bigger setup but **worth it**.

- **First**, create a NinjaOne Documentation Template:
  - Name: `User Management`
  - Add a **multi-line** field: `Authorized Global Admins` (set it to Read/Write for Automations).

- **Then**, create **three custom fields** (also multi-line, Read/Write):
  1. Local Admins
  2. Authorized Admins
  3. Unauthorized Admins

**Import these two scripts**:
- `Report Local Admins` — available in the Template Library.
- [`Check for Unauthorized Admins`](https://pastebin.com/vwetEwui)

**Important**:  
Make sure these scripts run on a similar cadence — if "Check for Unauthorized Admins" runs but the "Report Local Admins" script hasn't updated yet, you'll get bad data.

**Remediation Scripts** (optional but helpful):
- [`Disable Bad User`](https://pastebin.com/vjw95bkg)
- [`Force Bad User Logoff`](https://pastebin.com/08gMv2T8)

---

## Upgrading Devices to Windows 11

Use the **"Update Windows 10 to Windows 11"** script from the Template Library.

Some notes:
- **Important**: This can trigger an unexpected reboot if a user is active.
- You might want to deploy this via **Systray** so users can self-service, or schedule it during off-hours.

If you want to get fancy, you can chain conditions and actions together. Here's an example of how that can look:  
👉 [Compound Condition Workflow Example](https://github.com/jeffhunterninja/Webinar-Notes/tree/main/8.0%20Release%20-%20March%2027%2C%202025#-template-library)

---

## Sending Messages to End Users

Import the **"Display Popup"** script from the Template Library.

This can be used for:
- Reboot prompts (outside standard patching prompts).
- Reminders (e.g., clean up local files if low on disk space).

In this example, we prompted users to clean up their files when low disk space was detected.

---

## Folder Tree Size Report for Low Disk Space Troubleshooting

Another great option for disk space issues:  
Use a **Tree Size** script to figure out which folders are hogging space.

Options:
- NinjaOne's **"Tree Size Report"** script writes into a multi-line custom field.
- Community member Jan Scholte also has a version that writes into a **WYSIWYG field** if you prefer a nicer format.

Either way, make sure to **create the custom field first**.

---

## Running a System Performance Check

Useful when users report performance issues.

- Create a **WYSIWYG custom field** named `System Performance Check`.
- Import the **"System Performance Check"** script from the Template Library.

You can:
- Let users trigger this manually via Systray.
- Or run it automatically when high CPU/memory usage is detected (but be aware — running diagnostics adds **some** system load).

---

## Monitoring for Time Drift

Another good one to have.

- Import the **"Monitor Time Difference to NTP Server"** script from the Template Library.
- Create a **Script Result Condition**:
  - Configure acceptable drift.
  - Result Code = `1`
  - Output should **not contain** acceptable NTP sync status.

---

# Final Notes

Everything here is intended as a practical, tested workflow for improving monitoring, remediation, and reporting — with a bias toward automation and self-healing where possible.  
Test before deployment, and always adapt workflows to your environment's specific needs.

---
