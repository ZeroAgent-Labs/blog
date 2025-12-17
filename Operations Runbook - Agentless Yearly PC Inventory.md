# Operations Runbook: Agentless Yearly Windows PC Inventory (No Agents → Offline HTML/CSV)

This is a follow-up to the basic workflow (Collector → JSON folder → Viewer → HTML/CSV).
If you already understand the flow, this runbook focuses on **running it safely in the real world**:
permissions, folder structure, grouping, and deliverables.

Links:
- Live sample report (dummy data): https://zeroagent-labs.github.io/za-sample-report/
- Collector (free): https://github.com/ZeroAgent-Labs/ZeroAgent-Inventory
- Viewer / product page ($59): https://www.zeroagentlabs.com/

---

## 0) Decide the folder structure first (prevents 80% of “inventory chaos”)

Create a folder per inventory run (or per client/site).

Example:
\\SERVER\PC_Inventory\
  2025-12_ACME-OfficeA\
    json\
    output\
    pc_groups.csv
    RUNBOOK.txt

- `json/`   = collector outputs (one JSON per PC)
- `output/` = generated HTML + CSV
- `pc_groups.csv` = optional grouping (site/department)
- `RUNBOOK.txt`   = what you did, when, scope, notes

---

## 1) Permissions (most common failure point)

Minimum requirements:
- PCs (collector) must be able to **write** to `json/` (or write locally and later copy)
- Admin PC (viewer) must be able to **read** `json/` and **write** `output/`

Tip: do a 2-PC test before rolling out.

---

## 2) “2-PC preflight” (always do this)

1) Run collector on PC #1 → confirm JSON is created
2) Run collector on PC #2 → confirm JSON is created
3) Confirm both JSON files are in the same `json/` folder
4) Run Viewer once → confirm:
   - `output/index.html` opens
   - `computers.csv` exists
   - basic fields look correct

Only after this, scale to the rest.

---

## 3) Grouping with pc_groups.csv (keep it simple)

Start small:
- Site (OfficeA, OfficeB)
- Department (Sales, Admin)
- Managed/Unmanaged

Don’t try to perfectly classify everything on day 1.
Unknown → “Unknown” is fine.

---

## 4) Deliverables (audit pack)

If you’re producing annual inventory/audit deliverables, this is a clean minimum set:

- `output/index.html`
- `output/<per-PC pages>`
- `output/computers.csv`
- `pc_groups.csv` (how you grouped)
- `RUNBOOK.txt` (date, scope, notes)

This makes it easy to archive “what the environment looked like on that date”.

---

## 5) Common pitfalls (quick)

- ExecutionPolicy blocks the script → align with your org policy
- Network share permission issues → fix write access early (2-PC preflight)
- AV/EDR blocks the Viewer EXE → allow-list the folder if needed

---

## Summary

This workflow is optimized for **run-once inventory + reporting**, not continuous monitoring.
Treat it like a repeatable yearly process:
folder template → 2-PC preflight → full run → deliverable pack → archive.
