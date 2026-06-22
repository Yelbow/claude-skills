---
name: work-setup-sheet
description: >
  Use this skill any time you need to work with Jelle's "work setup" Google Sheet, or when
  using the Google Sheets MCP tools (mcp__google-sheets__*). This includes: adding new projects
  or tasks, reading or updating existing tasks, understanding the sheet's formula structure,
  working with the Projecten tab, creating new project tabs, and troubleshooting the STATUS
  formula or checkbox validation. Also use when the user says things like "zet dit in mijn
  work setup", "voeg een taak toe", "maak een nieuw project aan", or any variation of managing
  their work or admin tasks in Google Sheets. Trigger also when any Google Sheets MCP tool is
  about to be used, to load the correct context before starting.
---

# Work Setup Sheet — Context & How-To

## Spreadsheet basics

- **Name**: work setup
- **Spreadsheet ID**: `1qbezaQ8lQEuoHnqeceinZaN3DXmvz1d1psir87zEpAU`
- **Direct link**: https://docs.google.com/spreadsheets/d/1qbezaQ8lQEuoHnqeceinZaN3DXmvz1d1psir87zEpAU/

---

## Sheet tabs overview

| Tab | Purpose |
|---|---|
| Projecten | Master list of all projects with IDs and metadata |
| Vandaag | Today's focus (daily planning) |
| Ritme | Recurring rhythm / habits |
| [Project tabs] | One tab per project, e.g. "Admin", "Wijnhuis", "Karin EIGNWS TKKNTJE" |
| [Archive tabs] | Archived completed project tabs, e.g. "Admin Archive" |

---

## Projecten sheet

**Columns**: `ID | Naam | Type | Status | Afhankelijk van (ID) | Skilltree tab | Notitie`

- **ID**: 3-letter uppercase abbreviation (e.g. WIJ, ADM, KAR)
- **Type**: `Werk` or `Privé`
- **Status**: `Actief` (or `Inactief` when paused)
- **Afhankelijk van**: comma-separated IDs of projects that must finish first
- **Skilltree tab**: exact name of the project's tab in this spreadsheet
- **Notitie**: short description

**Example row**:
```
WIJ | Online Wijnhuis | Werk | Actief | | Wijnhuis | WooCommerce webshop
```

---

## Project task sheets

Each active project has its own tab. Tab name must match the "Skilltree tab" field in Projecten.

**Columns**: `Klaar | Status | Subproject | Taak | Vereist | Sub-ID | Dep1 | Dep2 | Archiveren? | Vandaag?`

| Column | Type | Description |
|---|---|---|
| A: Klaar | Checkbox (bool) | Check when task is done |
| B: Status | Formula | Auto-calculated: ACTIEF / LOCKED / KLAAR |
| C: Subproject | String | Group label, usually emoji + name (e.g. `⚖️ Herroepingswet`) |
| D: Taak | String | Task description |
| E: Vereist | String | Extra requirements or context |
| F: Sub-ID | String | Unique ID for this task (e.g. HW1, HW2) |
| G: Dep1 | String | Sub-ID of first dependency (must be KLAAR before this task unlocks) |
| H: Dep2 | String | Sub-ID of second dependency |
| I: Archiveren? | Checkbox (bool) | Mark to archive |
| J: Vandaag? | Checkbox (bool) | Mark for today's focus |

### STATUS formula (column B)

The exact formula for row N (replace `{N}` with the actual row number):

```
=IF(D{N}="","",IF(A{N}=TRUE,"KLAAR",IF(OR(AND(G{N}<>"",COUNTIFS($F$2:$F$301,G{N},$A$2:$A$301,FALSE)>0),AND(H{N}<>"",COUNTIFS($F$2:$F$301,H{N},$A$2:$A$301,FALSE)>0)),"LOCKED","ACTIEF")))
```

**Real example for row 2:**
```
=IF(D2="","",IF(A2=TRUE,"KLAAR",IF(OR(AND(G2<>"",COUNTIFS($F$2:$F$301,G2,$A$2:$A$301,FALSE)>0),AND(H2<>"",COUNTIFS($F$2:$F$301,H2,$A$2:$A$301,FALSE)>0)),"LOCKED","ACTIEF")))
```

**Logic:**
- Empty task row → `""` (blank, not shown)
- Klaar (A) = TRUE → `"KLAAR"`
- Dep1 (G) or Dep2 (H) references a Sub-ID that still has an incomplete task → `"LOCKED"`
- Otherwise → `"ACTIEF"`

**Note on range:** `$F$2:$F$301` and `$A$2:$A$301` cover rows 2–301 (300 task rows). Tasks in rows 302+ exist in the sheet but fall outside the dependency check range. This is the sheet's current design.

### Sub-ID convention

Format: `[ABBREVIATION][NUMBER]` — e.g. HW1, HW2, HW3 for the Wijnhuis Herroepingswet subproject. Tasks sharing a Sub-ID have no sequential dependency between them. Use different numbers to enforce order (HW2 depends on HW1).

---

## How to add a new project (full flow)

1. **Add row to Projecten** via `batch_update_cells`.
2. **Copy the Admin sheet** via `copy_sheet` — give it a temp name like "ProjectTemp". The Admin tab has 800 pre-filled formula rows and checkbox validation on columns A, I, J already set up.
3. **Clear Admin's task data** from the copy: overwrite rows 2–17, columns A and C–H with empty strings (Admin typically has ~15 data rows).
4. **Write the project tasks** to rows 2–N via `batch_update_cells`, including formula strings in column B.
5. **Delete any old placeholder tab** if one exists, via `batch_update` with `deleteSheet`.
6. **Rename the temp tab** to the correct name via `batch_update` with `updateSheetProperties`.

> Copying Admin is always faster than creating a new sheet from scratch — it carries over the STATUS formula and checkbox validation for all 800 rows automatically.

---

## Google Sheets MCP tools — reference

### Reading
```
mcp__google-sheets__search_spreadsheets  — find a spreadsheet by name/content
mcp__google-sheets__list_sheets          — list all tabs in a spreadsheet
mcp__google-sheets__get_sheet_data       — read computed values from a range
mcp__google-sheets__get_sheet_formulas   — read formulas (not computed values)
```

### Writing
```
mcp__google-sheets__batch_update_cells   — write values/formulas to one or more named ranges
mcp__google-sheets__update_cells         — write to a single range
mcp__google-sheets__add_rows             — insert new blank rows
```

### Sheet management
```
mcp__google-sheets__create_sheet         — create a new empty tab
mcp__google-sheets__copy_sheet           — copy a tab (within or across spreadsheets)
mcp__google-sheets__rename_sheet         — rename a tab
mcp__google-sheets__batch_update         — low-level API: delete sheets, rename, set validation, format
```

---

## Key patterns and gotchas

### Formulas work in batch_update_cells
`batch_update_cells` uses USER_ENTERED mode. Strings starting with `=` are interpreted as formulas. Quotes inside formula strings must be escaped as `\"` in JSON.

```json
{"B2:B5": [
  ["=IF(D2=\"\",\"\",IF(A2=TRUE,\"KLAAR\",IF(OR(AND(G2<>\"\",COUNTIFS($F$2:$F$301,G2,$A$2:$A$301,FALSE)>0),AND(H2<>\"\",COUNTIFS($F$2:$F$301,H2,$A$2:$A$301,FALSE)>0)),\"LOCKED\",\"ACTIEF\")))"],
  ["=IF(D3=\"\",\"\",IF(A3=TRUE,\"KLAAR\",IF(OR(AND(G3<>\"\",COUNTIFS($F$2:$F$301,G3,$A$2:$A$301,FALSE)>0),AND(H3<>\"\",COUNTIFS($F$2:$F$301,H3,$A$2:$A$301,FALSE)>0)),\"LOCKED\",\"ACTIEF\")))"]
]}
```

Always verify with `get_sheet_formulas` (not `get_sheet_data`) after writing — this confirms the formula was stored, not just its computed value.

### deleteSheet and renameSheet require batch_update
These need the numeric `sheetId`, returned when you call `create_sheet` or `copy_sheet`. Do not confuse with the tab's string name.

```json
{"deleteSheet": {"sheetId": 814131189}}
```
```json
{"updateSheetProperties": {"properties": {"sheetId": 15883389, "title": "Wijnhuis"}, "fields": "title"}}
```

### Checkbox validation (when creating from scratch)
Columns A, I, J need boolean data validation to render as checkboxes in the UI. Copying Admin carries this over automatically. To add manually via `batch_update`:

```json
{
  "setDataValidation": {
    "range": {
      "sheetId": 12345,
      "startRowIndex": 1,
      "endRowIndex": 302,
      "startColumnIndex": 0,
      "endColumnIndex": 1
    },
    "rule": {
      "condition": {"type": "BOOLEAN"},
      "showCustomUi": true
    }
  }
}
```
Column index reference: A=0, I=8, J=9.

### Generating 300 row formulas via Python in bash
Each STATUS formula references its own row number, so they can't be templated in a single value. Use a Python script in bash to generate the full list:

```python
import json

formulas = [
    [f'=IF(D{n}="","",IF(A{n}=TRUE,"KLAAR",IF(OR(AND(G{n}<>"",COUNTIFS($F$2:$F$301,G{n},$A$2:$A$301,FALSE)>0),AND(H{n}<>"",COUNTIFS($F$2:$F$301,H{n},$A$2:$A$301,FALSE)>0)),"LOCKED","ACTIEF")))']
    for n in range(6, 302)
]
print(json.dumps(formulas))
```

The output can be passed directly to `batch_update_cells` as the value for range `B6:B301`.

### list_spreadsheets requires user approval
The `mcp__google-sheets__list_spreadsheets` tool triggers a user-approval prompt. Use `search_spreadsheets` instead — it's more targeted and doesn't require approval.

---

## Self-learning instructions

This skill should stay accurate as the work setup sheet evolves. After any session where new patterns, tools, quirks, or structural changes are discovered, update this SKILL.md proactively.

**Update this file when:**
- A new project or tab is added (update the tabs overview if relevant)
- A new Google Sheets MCP tool is used and reveals useful behavior
- A gotcha or workaround is discovered that isn't documented here
- The STATUS formula or sheet structure changes
- A better approach to an existing pattern is found

**How to update:**
```bash
# File location:
/home/jelle/.config/Claude/local-agent-mode-sessions/skills-plugin/54ffc109-0f15-4d9d-aa6c-f6a39922ccb0/9c8339e4-d8d9-48c5-a194-aa7e01e46414/skills/work-setup-sheet/SKILL.md
```

Write updates via `mcp__workspace__bash` with a heredoc. After updating, briefly tell the user what was added and why.
