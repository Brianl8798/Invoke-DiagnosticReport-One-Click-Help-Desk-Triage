# Invoke-DiagnosticReport : One-Click Help Desk Triage

<p align="center">
  <img src="docs/banner.png" alt="Invoke-DiagnosticReport" width="600">
</p>

<p align="center">
  A simple, no-install PowerShell script that turns a vague "my computer is slow" ticket into a clean, shareable diagnostic report in seconds.
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-brightgreen.svg" alt="MIT License"></a>
  <img src="https://img.shields.io/badge/PowerShell-5.1%2B-5391FE?logo=powershell&logoColor=white" alt="PowerShell 5.1+">
  <img src="https://img.shields.io/badge/Platform-Windows%2010%20%7C%2011-0078D6?logo=windows&logoColor=white" alt="Windows 10 | 11">
  <img src="https://img.shields.io/github/last-commit/brianlam/Invoke-DiagnosticReport" alt="Last commit">
</p>

> [!TIP]
> **New to the script?** Jump straight to [How to use](#how-to-use) — you can have your first report in under a minute.
>
> - 🖱️ **One click**, no modules or installs required
> - 📄 **HTML report** you can attach straight to a ticket
> - 🔒 **Read-only** — the script never changes system settings
> - 🧪 Built and tested in a **Windows VM home lab**

<p align="center">
  <img src="docs/report-preview.png" alt="Sample diagnostic report" width="800">
</p>

## Table of Contents

- [Introduction](#introduction)
- [Features](#features)
- [Demo](#demo)
- [Why not just use Task Manager / msinfo32 ... ?](#why-not-just-use-task-manager--msinfo32--)
- [How to use](#how-to-use)
- [Does it require Administrator access ?](#does-it-require-administrator-access-)
- [What gets collected](#what-gets-collected)
- [Performance](#performance)
- [Caveats](#caveats)
- [Todo](#todo)
- [Credits](#credits)

## Introduction

**Invoke-DiagnosticReport** is a *simple*, *lightweight* and *fast* PowerShell script for Tier 1 help desk work. It gathers the system details a technician usually has to click through five different windows to find, then packages them into a **single report** that can be attached to a ticket or sent to a Tier 2 escalation.

## Features

- **One-click run** — double-click the launcher or run a single command
- Collects *hardware*, *OS*, *storage*, *network* and *event log* data in one pass
- Generates a clean **HTML report** (plus optional plain-text output)
- Flags common problems: low disk space, high uptime, pending reboots, recent critical errors
- **Read-only** — safe to run on end-user machines
- No external modules, no internet connection required
- Timestamped output files for easy ticket attachment

## Demo

<p align="center">
  <img src="docs/demo.gif" alt="Running Invoke-DiagnosticReport" width="800">
</p>

## Why not just use Task Manager / msinfo32 ... ?

Built-in tools are great, but each one only shows **part** of the picture, and none of them produce something you can hand to the next technician.

| Task | Built-in tools | Invoke-DiagnosticReport |
| --- | --- | --- |
| Check disk space | File Explorer | ✅ Included |
| Check uptime / pending reboot | Task Manager + Registry | ✅ Included |
| Network config | `ipconfig /all` | ✅ Included |
| Recent errors | Event Viewer (manual filtering) | ✅ Last 24h, pre-filtered |
| Shareable summary | ❌ Screenshots / copy-paste | ✅ One HTML file |

This cuts the "can you check X for me?" back-and-forth with the user and gets the ticket to a fix faster.

## How to use

**1. Clone or download the repository**

```powershell
git clone https://github.com/brianlam/Invoke-DiagnosticReport.git
cd Invoke-DiagnosticReport
```

**2. Allow the script to run for this session** (does not change the machine's policy permanently)

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```

**3. Run it**

```powershell
.\Invoke-DiagnosticReport.ps1
```

The report is saved to your Desktop as `DiagnosticReport_<COMPUTERNAME>_<yyyy-MM-dd_HHmm>.html` and opens automatically.

**Optional parameters**

```powershell
# Save to a specific folder
.\Invoke-DiagnosticReport.ps1 -OutputPath "C:\Tickets\INC0012345"

# Also write a plain-text copy
.\Invoke-DiagnosticReport.ps1 -Text
```

## Does it require Administrator access ?

No! Most of the report runs as a standard user.

However, a few sections (such as the **Security** event log and some pending-reboot checks) need an elevated prompt. When run without admin rights, those sections are marked as *skipped* instead of failing.

## What gets collected

- **System** — hostname, manufacturer/model, serial number, BIOS version
- **Operating system** — edition, version/build, install date, last boot, uptime
- **Hardware** — CPU, RAM (total/free), GPU
- **Storage** — every drive with size, free space and a ⚠️ warning under 15% free
- **Network** — adapters, IP/gateway/DNS, connectivity test
- **Updates** — recently installed hotfixes, pending reboot status
- **Event logs** — critical and error events from the last 24 hours
- **Top processes** — highest CPU and memory consumers

## Performance

A typical run finishes in a few seconds on a standard workstation. Example from the home lab VM:

```
PS C:\Tools> Measure-Command { .\Invoke-DiagnosticReport.ps1 }

Seconds           : 6
Milliseconds      : 412
TotalSeconds      : 6.412
```

## Caveats

- Tested on **Windows 10 and 11** with PowerShell 5.1. PowerShell 7 should work but is less tested.
- Event log queries can take longer on machines with very large logs.
- The report may contain hostnames, usernames and IP addresses — review it before sharing outside your organization.

## Todo

- Option to email the report or upload it to a ticketing system
- Remote mode (`-ComputerName`) for running against another machine
- Optional dark theme for the HTML report

## Credits

- Brian Lam — [GitHub](https://github.com/brianlam)
