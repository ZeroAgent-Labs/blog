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

Each file is a structured snapshot of that PC.

You can roll this out however you like:

manually (ask users to run it)

logon script

your existing software deployment tool

RMM script

The important part is: no resident agent.
The script runs, dumps JSON, and exits.

Step 2 — Store everything in one place

This part is boring but important:

Pick one share (or folder) where all JSON files live

Make sure your admin PC can read it

Treat that folder as your raw inventory data lake

That folder becomes your source of truth.
As long as you keep it, you can regenerate reports as many times as you want.

Step 3 — Turn JSON into offline HTML reports and CSV inventories

Raw JSON is great for machines, not for humans.

For reporting and audits, you usually need:

A PC list you can sort/filter in Excel (computers.csv)

Per-site or per-department breakdowns

“Which PCs are low on RAM / disk?”

“Which OS / Office versions do we have and where?”

A human-readable HTML report you can show to non-IT people

This is where I use Zero Agent Viewer — a small portable .exe designed specifically for this workflow.

Note: This is not an RMM/CMDB. It’s a run-once inventory + report generator.

The flow looks like this:

Point the Viewer at the folder with all your JSON files

(Optional) define your own groupings in a simple pc_groups.csv, like:

SiteA,PC-001

SiteB,PC-050

etc.

Click Generate Reports

You get:

A grouped offline HTML report with:

grouped PCs (by site/department/whatever you defined)

drill-down into each machine

quick filters and summaries

CSV exports:

computers.csv — one row per PC

plus other summary tables ready for Excel / Power BI / internal tooling

If you want to see what the HTML output looks like before touching your own data, here’s a live sample report with dummy data:

👉 https://zeroagent-labs.github.io/za-sample-report/

It’s static HTML + JS — open it in any browser, email it, archive it with your audit docs, etc.

Why this works well as a “yearly snapshot” workflow

This isn’t trying to replace your RMM or a full CMDB.
Instead, it’s optimized for:

Yearly (or quarterly) inventory audits

New client onboarding

Pre-migration assessments

“We haven’t had a proper inventory in years, let’s fix that” projects

Pros:

No agents to deploy/remove

No central server to maintain

All data stays in your environment (local/network share)

Outputs are boring but useful (offline HTML/CSV deliverables)

Because the Viewer is a single .exe, you can:

keep it in your admin toolbox

reuse it for future inventories without rebuilding anything

generate deliverables consistently every time

If you want to try this workflow as-is

There are many ways to DIY:

Write your own PowerShell collector

Aggregate JSON with scripts

Hack together your own HTML/CSV exports

If you enjoy that — honestly, go for it. I did that for a while.

If you’d rather skip the glue code and run the full flow, I packaged my setup as:

Zero Agent Viewer — Agentless Windows PC Inventory & Audit Reports
https://www.zeroagentlabs.com/

It includes:

the pre-compiled Viewer .exe

the free Collector script

config files (grouping etc.)

a short user guide

and a 30-day money-back guarantee

So you can test it in a real environment without committing long-term.

Quick questions (I’d love your feedback)

If you do annual inventories or audit deliverables, I’m curious:

What’s your biggest pain point with yearly inventory?

Do you need software inventory as much as hardware, or vice versa?

Would an “audit pack” deliverable (offline HTML + CSV) be useful in your workflow?

Either way, I hope this post gives you a clean mental model for:

“Once a year, collect everything, generate HTML + CSV, archive the evidence, and be done.”

  
