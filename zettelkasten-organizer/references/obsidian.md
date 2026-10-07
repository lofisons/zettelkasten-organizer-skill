# Obsidian Mechanics

How the four Zettelkasten primitives — containers, links, IDs, tags — are expressed in Obsidian, plus the concrete audit checks. Use this after Step 0 (locate the vault). The method never changes; this is its Obsidian syntax.

**Access model reminder:** an Obsidian vault is just a folder of markdown files — read and write in place. If you cannot access the filesystem, deliver standard markdown files (Obsidian reads them natively) plus brief import instructions. No workarounds beyond that.

## Layout — flat by default

One folder for notes (or none at all), note kind marked by a `type:` property in frontmatter:

```yaml
---
type: permanent   # fleeting | bib | permanent
created: 2026-07-28
status: active    # active | draft | review | archived
tags: []
source: "[[Ahrens — How to Take Smart Notes (2017)]]"
---
```

Folders are optional: Obsidian's Graph view, search, and backlinks make per-type folders redundant — **the graph is the structure**. Offer folders only if the user explicitly wants a visible pipeline. Two folders always earn their place:

- **Inbox** — capture needs a boundary. (Or skip the folder entirely and use the Daily Notes journal as the inbox.)
- **Archive** — disposal needs a boundary. (Or use `status: archived`.)

## Links

- `[[Note title]]` — Obsidian resolves by name, so links survive without IDs.
- The backlinks panel shows inbound connections automatically; still write the "why" line next to each link in the body.
- **Unlinked mentions** (right sidebar) surfaces notes that mention each other without linking — the cheapest way to grow the network. Check it during Mode 1 and Mode 4.
- **Renaming or moving notes:** the agent edits files on disk, so Obsidian's "Update links on rename" setting does **not** fire automatically. Before renaming or moving any note, `search_files` for the old title across the vault and update every inbound `[[link]]` to the new title. Never leave dangling links. The same applies when a statement title changes during a split.

## IDs — skip them

Filename = statement title, no timestamp prefix. Obsidian links by name, so the title *is* the identity. Never add IDs to filenames or frontmatter.

## Tags and properties

- The `type` property is essential — it powers the audit queries below. Keep other properties to a minimum (`created`, `status`, `source`, `tags`).
- Tags: few, stable, cross-cutting (status markers, broad life/work domains). Never a substitute for links.

## The inbox

Two acceptable forms — ask which the user prefers:

1. **Daily Notes journal** (core plugin): fleeting entries land on the day's page and get processed into permanent notes. The skill reads the journal and proposes which entries to develop, reference, or archive.
2. **Inbox folder**: quick-capture notes as separate files, processed the same way.

## Audit checks (Mode 1) — concrete form

**These queries assume the vault already uses the `type` property.** Against a vault that doesn't have it yet, run Phase 1 classification first (the plan adds `type` to the notes), or drop the type filter and use location heuristics: anything in the Inbox folder is fleeting; notes with zero links are orphan candidates regardless of type.

With the **Dataview** community plugin installed, run these queries instead of reading notes one by one:

- Orphans (no links in or out):
  `LIST FROM "Notes" WHERE type = "permanent" AND length(file.inlinks) = 0 AND length(file.outlinks) = 0`
- Inbox debt (unprocessed for over a week):
  `LIST FROM "Inbox" WHERE type = "fleeting" AND date(created) < date(today) - dur(7 days)`
- Oversized notes (split candidates):
  `LIST FROM "Notes" WHERE type = "permanent" AND file.size > 5000`
- All permanent notes, for a link-quality pass:
  `LIST FROM "Notes" WHERE type = "permanent" SORT file.mtime DESC`
- Hubs and isolates in the graph: Graph view → filter by `type = permanent` → look for isolated nodes (orphans) and dense clumps (clusters needing better internal links).

Adjust the `FROM "Notes"` path to wherever the user's notes live (or drop it for vault-wide). Without Dataview, substitute the same checks with native search (`path:Inbox created:<-7days`) and the Graph view.

## Working directly

The vault is a folder of markdown — use `read_file` / `write_file` / `patch` / `search_files` on the vault path. Respect Obsidian conventions:

- Frontmatter must stay valid YAML (properties UI reads it).
- **Preserve existing frontmatter fields** (aliases, cssclasses, custom properties). Add or update only the Zettelkasten fields: `type`, `created`, `status`, `source`, `tags`. Never strip a field you didn't add.
- Filenames must match the statement titles exactly (links resolve by name).
- Never edit `.obsidian/` config files unless asked.

## Plugins worth knowing (all optional)

- **Templates** (core) — insert note templates with one command; supports `{{date}}` and `{{time}}`.
- **Templater** (community) — more powerful templates (prompts, computed values).
- **Dataview** (community) — queries over frontmatter; powers the audit checks above.
- **QuickAdd** (community) — capture shortcuts (e.g., one hotkey to create a fleeting note in the inbox).
