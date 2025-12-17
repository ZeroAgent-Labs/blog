# How to Run a Yearly Windows PC Inventory with No Agents (PowerShell → JSON → Offline HTML/CSV)

If you work in a small IT team or MSP, you probably know this feeling:

> “We *should* know what’s inside all these Windows PCs,
> but the last ‘full inventory’ is buried in a spreadsheet from two years ago.”

Most inventory solutions want you to:

- deploy agents everywhere, or
- spin up a server + database, or
- commit to a full-blown CMDB/RMM platform.

Sometimes that’s the right answer.  
But sometimes you just want a **clean, reliable snapshot once or twice a year** without changing your entire tooling.

This post is for that second case.

**TL;DR:** Run a free PowerShell collector on each Windows PC → gather JSON snapshots in one folder → generate **offline HTML reports + CSV inventories** in minutes.  
**No agents. No cloud. No installs.**

---

## The constraints (a realistic small-IT scenario)

This is the environment I had in mind:

- 50–300 Windows 10/11 PCs
- A small IT team or one-person MSP
- Maybe you already have AD / Intune / RMM / “something”
- But you still want a **human-readable snapshot** you can hand to management, auditors, or clients

Requirements:

- **No agents**: you don’t want to install and later clean up yet another agent
- **No central server**: you want to run from an admin PC + (optional) network share
- **Portable outputs**: HTML for humans, CSV for Excel/BI/tools
- **Run it once (or a few times a year)** and you’re done

If that sounds like your life, keep reading.

---

## Step 1 — Collect data with a simple PowerShell script

First, you need a way to ask each PC:

> “Who are you? What hardware do you have? What software is installed?”

You can do this with a single PowerShell script that:

- queries WMI / registry / basic system info
- writes everything into a **single JSON file per PC**
- saves it to a folder (local or a network share)

A typical run looks like this:

```powershell
.\ZeroAgent-Collector.ps1 `
  -OutputPath "\\YOURSERVER\PC_Inventory\%COMPUTERNAME%.json"

  After you’ve run this on all target PCs, you end up with:

  \\YOURSERVER\PC_Inventory\
  PC-001.json
  PC-002.json
  PC-003.json
  ...

  
