# How to Run a Yearly Windows PC Inventory with No Agents (PowerShell + HTML Reports)

If you work in a small IT team or MSP, you probably know this feeling:

> “We *should* know what’s inside all these Windows PCs,
> but the last ‘full inventory’ is buried in a spreadsheet from two years ago.”

Most inventory tools want you to:

* deploy agents everywhere, or
* spin up a server + database, or
* commit to a full-blown CMDB/RMM platform

Sometimes that’s the right answer.
But sometimes you just want a **clean, reliable snapshot once or twice a year** without changing your entire tooling.

This post is about that second case.

I’ll walk through a simple, agentless workflow:

1. Run a free PowerShell collector on each Windows PC
2. Drop all the JSON into a single folder
3. Turn that into **HTML reports + CSV inventories** in a few minutes

No agents, no cloud, no installs.

---

## The constraints (a realistic small-IT scenario)

This is the environment I had in mind when building this:

* 50–300 Windows 10/11 PCs
* A small IT team or one-person MSP
* Maybe you already have:

  * AD / Intune / RMM / something
  * but you *still* want a human-readable snapshot you can hand to management, auditors, or clients

Requirements:

* **No agents**: you don’t want to install and later clean up yet another agent
* **No central server**: you want to run from your admin PC and a network share
* **Portable outputs**: HTML for humans, CSV for Excel/BI/tools
* **Run it once (or a few times a year)** and you’re done

If that sounds like your life, keep reading.

---

## Step 1 – Collect data with a simple PowerShell script

First, you need a way to ask each PC:

> “Who are you? What hardware do you have? What software is installed?”

You can do this with a single PowerShell script that:

* queries WMI / registry / basic system info
* writes everything into a **single JSON file per PC**
* saves it to a folder (local or a network share)

A typical run looks like this (from the user or via your deployment tool):

```powershell
.\ZeroAgent-Collector.ps1 `
  -OutputPath "\\YOURSERVER\PC_Inventory\%COMPUTERNAME%.json"
```

After you’ve run this on all target PCs, you end up with something like:

```text
\\YOURSERVER\PC_Inventory\
  PC-001.json
  PC-002.json
  PC-003.json
  ...
```

Each file is a structured snapshot of that PC.

You can roll this out however you like:

* manually (ask users to double-click)
* logon script
* your existing software deployment tool
* RMM script

The important part is: no resident agent.
The script runs, dumps JSON, and exits.

---

## Step 2 – Store everything in one place

This part is boring but important:

* Pick one share (or folder) where all JSON files live
* Make sure your admin PC can read it
* Treat that folder as your **raw inventory data lake**

That’s your source of truth.
As long as you have that folder, you can regenerate reports as many times as you want.

---

## Step 3 – Turn JSON into HTML reports and CSV inventories

Raw JSON is great for machines, not for humans.

For reporting and audits, you usually need:

* A PC list you can sort/filter in Excel (`computers.csv`)
* Per-site or per-department breakdowns
* “Which PCs are low on RAM / disk?”
* “Which OS / Office versions do we have and where?”
* A human-readable HTML report you can show to non-IT people

This is where I use **Zero Agent Viewer**, a small portable `.exe` designed specifically for this workflow.

The flow looks like this:

1. Point the Viewer at the folder with all your JSON files
2. (Optionally) define your own groupings in a simple `pc_groups.csv`, like:

   * `SiteA,PC-001`
   * `SiteB,PC-050`
   * etc.
3. Click **Generate Reports**

You get:

* A live HTML report with:

  * grouped PCs (by site / department / whatever you defined)
  * drill-down into each machine
  * quick filters and summaries
* A set of CSV files:

  * `computers.csv` – one row per PC
  * plus other summary tables, ready for Excel / Power BI / import into your ticketing system

If you want to see what the HTML output looks like *before* you even touch your own data, here’s a live sample report with dummy data:

👉 [https://zeroagent-labs.github.io/za-sample-report/](https://zeroagent-labs.github.io/za-sample-report/)

It’s just static HTML + JS – you can open it in any browser, email it, archive it with your audit docs, etc.

---

## Why this works well as a “yearly snapshot” workflow

This isn’t trying to replace your RMM or full CMDB.

Instead, it’s optimized for:

* Yearly (or quarterly) inventory audits
* New client onboarding
* Pre-migration assessments
* “We haven’t had a proper inventory in years, let’s fix that” projects

Pros:

* No agents to deploy/remove
* No central server to maintain
* All data stays in your environment (local/network share)
* Outputs are boring but useful (HTML/CSV)

And because the Viewer is just a single `.exe`, you can:

* Keep it in your admin toolbox
* Re-use it for future inventories without re-building anything

---

## If you want to try this workflow as-is

There are many ways to DIY:

* Write your own PowerShell collector
* Build some scripts to aggregate JSON
* Hack together your own HTML/CSV exports

If you enjoy that, honestly, go for it.
I did exactly that for a while.

If you’d rather skip the glue code and just run the whole flow, I packaged my setup as:

**Zero Agent Viewer – Windows PC Inventory & Audit Tool**
[https://www.zeroagentlabs.com/](https://www.zeroagentlabs.com/)

It includes:

* the pre-compiled Viewer `.exe`
* the free Collector script
* config files (grouping etc.)
* a short user guide
* and a 30-day money-back guarantee

…so you can test it in your real environment without committing long-term.

Either way, I hope this post gives you a clean, agentless mental model for:

> “Once a year, collect everything, get HTML + CSV, and be done.”

If you end up trying this approach (with or without my tool),
I’d genuinely love to hear how you’re running your inventories and what surprised you.

---
