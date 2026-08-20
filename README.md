# dev-template
Template setup for future devs
McB OS Dev Template
Start here. This template is the paved road for building tools and
integrations across McCarthy Bush Corporation (MCB) subsidiaries — BCC,
OMW, MCI, MCT, CEC, and others. Building from this template means you
inherit McB OS's data governance, naming, and pipeline standards by
default, instead of picking them up later in a review.
Why this exists
Per DAMA-DMBOK2 (Chapter 3, Data Governance), data governance works
best when it is embedded rather than bolted on:
> "Data Governance is not an add-on process. Data Governance activities
> need to be incorporated into development methods for software, use of
> data for analytics, management of Master Data, and risk management,
> among other activities." (DAMA-DMBOK2, Ch. 3.2)
This template is that embedding, applied to how McB OS tools get built.
Standards live in the scaffolding, not in a checklist someone reviews
after the fact.
Before you write any code
Read these two files first. They are symlinked into `.claude/rules/`
and are loaded automatically if you're using Claude Code:
File	Covers
`.claude/rules/azure-naming-standard.md`	How every Azure resource (resource groups, Data Factory, SQL, storage, Key Vault) must be named
`.claude/rules/mcbos-pipeline-standards.md`	How data pipelines are structured, source-to-target mapping conventions, write behavior rules
`mcbos-connections-manifest.md`	The `mcbos.manifest.json` schema every repo carries — your tool's McB OS catalog entry and what it connects to, filled in as you build
If you're not using Claude Code, read the files directly at the paths
above — they're the same standard either way.

Also fill in `mcbos.manifest.json` at the repo root as you build (see
`mcbos-connections-manifest.md` for the full field reference, and
`mcbos.manifest.example.json` for a filled-in illustration). This is what
lets McB OS pick up your tool's catalog entry and connections without an
admin retyping them from scratch when it's time to connect your tool.
Ground rules this template enforces
Naming convention is not optional. Every Azure resource follows
`<type>-mcbos-<component>-<env>`. See the naming standard file for
the full prefix table and the one documented exception
(`mcb-keyvault`).
Secrets never live in code. Every credential — API tokens,
connection strings, client secrets — goes in Azure Key Vault.
Nothing hardcoded, ever.
New fields and mappings anchor to the business glossary, not to
source system field names. ERP and CRM systems across MCB
subsidiaries use inconsistent field names for the same concept.
Don't rename source schemas to force alignment — map source fields
to glossary term IDs (e.g. BG-001) in your connector/transform
layer instead. This is DAMA-DMBOK2's guidance under Data
Integration and Interoperability (Ch. 8) and Reference & Master
Data (Ch. 9): consistent meaning, not conformed naming.
Staging data is schema-per-source, not database-per-source.
If your tool lands data in the shared staging environment, use a
schema named for your source system (e.g. `spectrum.*`) inside the
existing staging database — don't provision a new database unless
you've confirmed resource contention with the McB OS team first.
McB OS reflects data, it doesn't enforce process. Tools built
here should represent what's true about the business's data — they
should not silently impose a workflow or business rule that a
subsidiary hasn't agreed to. If your tool needs to enforce a
process, that's a governance conversation, not a code decision.
New glossary terms go through the glossary, not around it. If
your tool introduces a new business concept (or a term that
collides with an existing one — see BG-022 as a precedent), add it
to the SharePoint glossary list before building against it.
Per DAMA-DMBOK2 Ch. 3.2.14, a maintained business glossary is a
named, foundational Data Governance activity, not an optional
artifact.
What this template does NOT do yet (open gaps)
Per DAMA-DMBOK2, these are foundational governance elements that McB OS
has not fully closed out. Don't treat their absence as "not required" —
they're tracked, not resolved:
A named, accountable owner for governance guidance content
A formal data governance roles/council structure
Explicit definition of the governance advisor agent as advisory
(not authoritative)
If your tool surfaces a new instance of one of these gaps, flag it —
don't quietly resolve it your own way.
Getting help
Naming or pipeline standards questions → check the rule files first;
they're written to be Claude-Code-readable.
Glossary or governance questions → check the SharePoint glossary
list and RACI matrix before asking.
Anything not covered here → raise it with the McB OS team
(Ryan Swanson / Michael Johnson) rather than guessing and moving on.
---
This template's standards are grounded in DAMA-DMBOK2. Where McB OS
practice and DAMA-DMBOK2 diverge, DAMA-DMBOK2 wins — raise the
divergence rather than building around it.
