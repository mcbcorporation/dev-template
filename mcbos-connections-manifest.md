# McB OS — Connections Manifest Standard

This file is the canonical reference for `mcbos.manifest.json`, the file
every McB OS tool repo carries from day one. Load it as a rule before
editing that file or building anything that reads it.

## Why this exists

Today, when a new tool gets connected to McB OS, an admin manually
retypes its catalog entry (name, company, category, owner, description)
and hand-reconstructs what it connects to — information the tool's own
developer already knows while building it, and that goes stale the
moment nobody keeps re-typing it. Per DAMA-DMBOK2 (Ch. 3.2), governance
works best embedded in how things get built, not bolted on afterward as
someone else's cleanup task. `mcbos.manifest.json` is that embedding,
applied to McB OS onboarding specifically: the tool declares its own
metadata and connections as it's built, instead of an admin
reconstructing them later from memory or a Slack thread.

**This is a starting declaration, not a live feed.** Filling this file in
does not automatically register anything in McB OS — an admin still
reviews and imports it by hand when the tool is actually connected (the
same confirm-before-it's-live discipline McB OS applies everywhere else).
Treat it as handing the admin accurate values to import, not as a push
API.

## The file

Every repo built from this template includes a blank
`mcbos.manifest.json` at its root. Fill in real values as you build —
don't leave placeholders in, and don't guess at a value you're not sure
of; leave it blank and flag it for the McB OS team instead. See
`mcbos.manifest.example.json` for a filled-in reference.

```json
{
  "tool": {
    "name": "",
    "companies": [],
    "category": "",
    "description": "",
    "owner": { "name": "", "role": "" },
    "url": "",
    "status": "in-development",
    "progressPercent": 0
  },
  "connectsTo": [
    {
      "targetType": "tool",
      "targetName": "",
      "label": "",
      "status": "planned"
    }
  ]
}
```

## Field reference

| Field | Required | Notes |
|---|---|---|
| `tool.name` | Yes | The name McB OS should display. |
| `tool.companies` | Yes, at least one | Which McB operating companies this tool serves — an array, since a tool can serve two sister companies without serving the whole portfolio. Must match McB OS's existing Company list exactly (currently: OMW, MCB, CEC, BCC, MCI, LMM) — **this is a different list from the SharePoint business glossary** this template already governs. If your tool needs a company not on that list yet, flag it to the McB OS team rather than inventing a new value. |
| `tool.category` | Yes | Must match McB OS's existing Category list (check Tool Manager, or ask). McB OS assigns the tool's icon from this automatically — don't specify an icon separately. |
| `tool.description` | Yes | One or two sentences, what it actually does. |
| `tool.owner` | Yes | A real name and role — not a team alias. |
| `tool.url` | Yes once deployed | The live app URL. Leave blank until it exists; don't put a placeholder domain here. |
| `tool.status` / `tool.progressPercent` | Yes | Starting values only (McB OS's Roadmap view is entirely computed from these two fields on the Tool, refreshed independently later — see "What this doesn't do" below). `status` is one of `in-development`, `active`, `at-risk`. |
| `connectsTo` | No — omit or leave empty if nothing to declare yet | Everything this tool sends data to or receives data from, whether that's another McB OS tool or a backend system with no UI of its own (a pipeline, a database, a vendor platform). |
| `connectsTo[].targetType` | Yes, if the entry exists | `"tool"` if the target is a McB OS Tool (has its own URL/catalog entry), `"system"` if it's anything else. The axis is *does it have a UI to open*, not who built it — an internal pipeline you built yourself is still `"system"`. |
| `connectsTo[].targetName` | Yes, if the entry exists | The target's exact name as it exists (or will exist) in McB OS — resolved by name at import time, not an internal ID your repo can't know. If the target doesn't exist in McB OS yet, use the name it should be created under and say so when handing this off. |
| `connectsTo[].label` | Yes, if the entry exists | What actually flows over the connection (e.g. "Nightly CSV export", "OAuth token exchange") — not a restatement of the target's name. |
| `connectsTo[].status` | Yes, if the entry exists | `"live"` if this connection is already working, `"planned"` if it's designed but not built yet. |

## What this does NOT do (open gaps)

Same spirit as this template's other open gaps — tracked, not resolved,
don't treat the absence as "not needed":

- **No automated import yet.** As of this writing, an admin still copies
  values from this file into McB OS's Tool Manager by hand. A paste/URL
  import feature is planned but not built — check with the McB OS team
  before assuming otherwise.
- **No live sync.** Once imported, `status`/`progressPercent` and
  anything else about the tool are managed in McB OS directly. Updating
  this file later does not update McB OS — there is no round-trip yet.
- **No enforcement.** Nothing currently checks that a repo built from
  this template actually filled this file in, or that its values are
  accurate. Don't assume a manifest existing means it's been reviewed.

## Getting help

If a company, category, or connection target you need doesn't exist yet
in McB OS's lists, or you're unsure how to describe a connection —
raise it with the McB OS team (Ryan Swanson / Michael Johnson) rather
than guessing and moving on, same as every other open question this
template flags.
