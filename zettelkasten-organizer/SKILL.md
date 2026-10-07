---
name: zettelkasten-organizer
description: "Audit and maintain Zettelkasten slip-boxes in Obsidian."
version: 3.2.0
author: absolute-duckdev, Hermes Agent
license: MIT
platforms: [linux, macos, windows]
metadata:
  author: absolute-duckdev
  version: "3.2.0"
  hermes:
    tags: [obsidian, zettelkasten, notes, knowledge-base, markdown]
    category: note-taking
    related_skills: [obsidian]
---

# Zettelkasten Organizer

## THE GOLDEN RULE: PLAN FIRST, ACT SECOND

This skill touches the user's real notes — their thinking, their work. Trust is everything. The rule that governs every batch operation in this skill:

**Do not create, move, rename, split, or delete any note until you have presented a complete written plan and the user has explicitly approved it.**

The ceremony scales with the operation:

- **Structural changes** (reorganizing a vault, moving/renaming/splitting notes, archiving in bulk) → present the full phased plan, wait for explicit approval.
- **Obvious triage** (an unprocessed fleeting note that is clearly archival) → a one-line confirmation is enough: "This fleeting note is a dead-end reminder — archive it? OK?"
- **Single note** (one new permanent note, one bib note) → draft it, show it, get a quick confirmation before saving.

At the very start of any batch operation, say something like: "I'll start by looking at your vault and asking a few questions, then I'll put together a complete plan showing every note I'd create, move, split, or link. You'll review the whole plan before I touch anything."

Say this early and unprompted. The user needs to hear it before they start worrying about it.

---

This skill helps someone implement and maintain the Zettelkasten method — the slip-box system developed by sociologist Niklas Luhmann (90,000 hand-written cards) and popularized by Sönke Ahrens in *How to Take Smart Notes* (2017) — **inside Obsidian**. Its core insight: **the value of a note system is not in collecting notes but in the network of connections between them.** Folders organize; links think.

Obsidian is a natural Zettelkasten home: `[[wikilinks]]` and backlinks are native, the graph view *is* the structure, and properties carry the metadata. The skill speaks Obsidian's conventions — flat layout, statement titles as filenames, `type` properties, Dataview for health checks.

## When to Use

Use this skill when the user wants to:

- Audit the health of an Obsidian vault (orphaned notes, inbox debt, atomicity violations, bare links)
- Process an inbox of fleeting notes into permanent notes (Daily Notes journal or Inbox folder)
- Create an atomic permanent note from an idea, with genuine links
- Set up a Zettelkasten from scratch in a vault, or rebuild an existing one
- Organize existing Obsidian notes with links instead of folders
- Split notes that cover multiple ideas, or find and connect orphaned notes

Also trigger on: "Zettelkasten", "slip-box", "smart notes", "atomic notes", "permanent notes", "literature notes", "fleeting notes", "Luhmann", "Ahrens", or "How to Take Smart Notes".

**Don't use for:** task management, project planning, or any operational material — this skill manages knowledge only, and never touches notes without an approved plan.

The skill is multi-agent: the `zettelkasten-organizer/` folder installs as-is into Claude Code, Hermes Agent, OpenCode, OpenAI Codex, or Gemini CLI (see the repo README for the exact commands). The frontmatter carries both the Anthropic agent-skills convention (`metadata.author` / `metadata.version`) and the Hermes convention (`version`, `author`, `platforms`, `metadata.hermes`); every loader reads the fields it knows and ignores the rest.

Before classifying, splitting, or restructuring any notes, read `references/zettelkasten-framework.md` (the method, source of truth) and `references/obsidian.md` (the Obsidian mechanics) in this skill's directory.

## The three note types

`Archive` is a storage destination, not a note type, and `status` describes a note's lifecycle without changing its type:

- **Fleeting notes** — quick captures of ideas, reminders, quotes. Raw material. They live in the inbox and are processed or discarded within days.
- **Bib notes** — brief, paraphrased records of ideas from external sources (books, articles, talks), with a full reference to the source. Also called literature notes.
- **Permanent notes** — the heart of the system. **Atomic** (one idea each), written in full sentences in the user's own words, titled with a **statement** (the idea itself, not a topic), and densely **linked** to related notes. These live in the slip-box forever.

The workflow: **capture → review → permanent → network**. Two streams enter at capture: your own ideas and thoughts arrive as **fleeting notes**; external sources (books, articles, podcasts, videos) arrive as **bib notes**. Both converge in **Review**, the triage step that decides what earns a permanent note.

**Operational material lives outside the vault.** This skill is about knowledge, not task management. Drafts, task lists, decisions, and working files for a deliverable live wherever the user works (a separate folder, Notion, Trello — their choice); the Zettelkasten never manages them. Only the durable ideas that emerge from that work enter the slip-box as permanent notes.

## Vault naming conventions (house style)

The vanilla method prefers plain statement filenames with no IDs. **The veridis-quo vault runs its own established naming convention**, and this skill honors it. When capturing into that vault (or any vault that adopts this style), follow these rules exactly — they override the generic defaults below:

- **Bib notes** — the filename is the descriptive title of the subject, and *only* that: `Adaptación de modelos.md`, `Kubernetes — Pods, Deployments y manifiestos.md`. **No date serial, no `Bib` prefix.** The title is what tells you what the note is about and what links resolve to.
- **Permanent notes** — the filename starts with the section ID, then the readable statement, then the date hash **at the very end of the title**: `1a - El alcance debe definirse antes de pedirle a un agente que ejecute 202613081728.md`. The 12-digit date hash (`yyyymmddhhmm`) always lands **last**, never at the front of the filename. Keep the leading section ID (`1a`, `2b1`, …); only the date hash position changes.

This is a deliberate, local override of Core rule 9 (the generic "no-IDs" default). It keeps each section sortable and the statement at the front so the title reads naturally. Whatever convention a vault uses, apply it consistently and never mix styles.

## The four universal primitives (as Obsidian expresses them)

Every Zettelkasten implementation needs exactly four things. In Obsidian they map like this:

1. **Containers** → folders, or none: a `type` property (fleeting / bib / permanent) distinguishes note kinds. The graph is the structure.
2. **Links** → `[[wikilinks]]` between notes; backlinks are automatic. Every link carries a line explaining *why*.
3. **Unique IDs** → **not needed.** Obsidian links resolve by name, so the statement title is the identity. No timestamp prefixes.
4. **Tags** → a small set of stable, cross-cutting keywords in properties (always secondary; never a substitute for links).

## Step 0 — Locate the vault and confirm access (ALWAYS DO THIS FIRST)

Before anything else:

1. **Locate the vault.** Ask where it lives, or infer from the working directory / the files at hand. An Obsidian vault is just a folder of markdown files.
2. **Determine access.** Can you read and write the vault directly (filesystem access)? Or can you only deliver artifacts? If direct, work in place — that is the normal case.
3. **Confirm the mapping in one short message** before proceeding:
   > "I'll organize your Zettelkasten like this: notes in one folder with a `type` property, statement titles as filenames, links → `[[wikilinks]]`, no IDs. Sound right?"

If you cannot read/write the vault directly, deliver standard markdown files plus brief import instructions — Obsidian reads plain markdown natively. Do not invent workarounds beyond that.

## The five modes of use

Zettelkasten is a living system, not a one-time setup. The modes are ordered by how often a user needs them.

### Mode 1 — Audit & maintenance (the primary mode)

Scan the vault and report on its health. Read-only findings first, then propose fixes as a plan. Run the concrete checks in `references/obsidian.md` (Dataview queries, Unlinked mentions, Graph view filtered by `type`) instead of reading notes one by one:

- **Orphaned notes** — permanent notes with no links in or out. For each, suggest specific existing notes to connect (with the "why"), or mark it as a legitimate first note in a new territory. Never create an artificial link.
- **Dense clusters** — topics with many mutually linked permanent notes. Report the cluster and suggest new links *between* its notes. Do not create index or map notes.
- **Atomicity violations** — notes covering 2+ distinct ideas. Propose a split: the new notes, their statement titles, and how links redistribute.
- **Context-free notes** — permanent notes missing their source or the "why this matters" context.
- **Inbox debt** — unprocessed fleeting notes older than a week.
- **Link quality** — bare `[[link]]` lists with no stated reason for the connection.

Present findings as a short report, then a plan of proposed fixes. Execute only after approval.

### Mode 2 — Review (process the inbox)

The core habit of the method — the Review step of the pipeline, where fleeting and bib notes converge. For each fleeting note in the inbox, triage:

1. **Discard/archive** — a momentary reminder with no lasting value? Archive it (never delete).
2. **Reference** — source material worth keeping but not an idea to develop? File it as a bib note (paraphrased, with the full source reference).
3. **Develop** — an idea worth keeping? Write one or more **permanent notes** from it:
   - One idea per note (split if needed)
   - Title = a full statement of the idea
   - Written in the user's own words, full sentences
   - Search the existing permanent notes and link to related notes when a genuine relationship exists. If this is the first note in a new territory, record the justified exception rather than inventing a link.
   - Each link gets a sentence explaining *why* the notes connect
   - Archive the fleeting note once processed

For batches of more than ~5 notes, use the phased workflow (plan first). For a handful of notes, propose the triage outcome per note in one compact list and confirm before acting. The inbox may be the Daily Notes journal — process entries from there the same way.

### Mode 3 — Capture a source

When the user brings an external source (article, book, podcast, video) and wants it in the system — the other entry stream, alongside fleeting notes:

1. Identify the source and ask (or infer) which ideas were relevant to them.
2. **Check for an existing bib note for that source** — never duplicate.
3. Draft the bib note following `references/note-templates.md`: paraphrased ideas, full reference, "Distilled into permanent notes" section left open.
4. Show the draft, get confirmation, save. (Single-note rule: draft + quick confirm.)
5. Report where it landed and suggest which permanent notes could be distilled from it.

### Mode 4 — Create a new permanent note

When the user brings a single idea:

1. Ask what the idea is and where it came from (their own thinking? a source?)
2. **Search the existing notes for related ideas.** This is not optional. Create genuine contextual links when they exist; a first note in a new territory may remain temporarily unlinked.
3. Draft the note following `references/note-templates.md`:
   - Statement title (the filename)
   - `type: permanent` property
   - Body: the idea in full sentences, its context, why it matters
   - Source (if any)
   - Links section with one line of "why" per link
4. Show the draft, including where each link points and why. Get confirmation, then save.
5. Report back: where the note landed and what it connected to.

### Mode 5 — Initial setup (secondary)

Build the structure from scratch (or rebuild). Present the layout options and let the user choose:

**Default — Flat layout.** Notes in one folder (or none); note type marked by a `type` property; organization emerges purely from links. Obsidian's graph, search, and backlinks make per-type folders redundant. **Inbox and Archive still get their own folders** — capture and disposal need boundaries. This is the recommended layout in Obsidian.

**Option — Containerized layout** (one folder per note type), for users who prefer a visible pipeline:

```
Inbox/        fleeting notes, quick capture
Bib/         bibliographic notes, one per source
Permanent/    atomic permanent notes — the heart
Archive/     processed or discarded notes (nothing is deleted)
```

Recommend flat; mention containerized for users who like structure they can see. Then follow the phased workflow below.

## Phased workflow (for batch operations)

### Phase 1: Discover (READ-ONLY — no changes)

**Step 1 — Set expectations.** Deliver the golden-rule reassurance from the top of this file. Not optional.

**Step 2 — Locate the vault.** Run Step 0 if not already done.

**Step 3 — Scan the existing notes.** List what's there: counts by location, a sample of titles, apparent topics, existing structure (folders, tags, links). For small vaults, list files directly. **For vaults over ~100 notes, do not read every note** — index by properties and filenames with `search_files` (e.g., all `type: permanent`, all `type: fleeting`, files with no frontmatter), sample contents, and use the Dataview queries from `references/obsidian.md` when available. Look for signs of an existing Zettelkasten (inbox/permanent containers, `type` properties, wikilinks). If one exists, do not default to rebuild — offer three options:

1. **Audit & update** (run Mode 1 on the existing system)
2. **Sort into the existing structure** (process only what's outside it)
3. **Fresh start** (archive everything, rebuild)

Framing questions are only for a fresh setup (Mode 5), and only when the notes themselves don't answer them: what the user thinks about most, what they're working on, how they capture ideas today. For an existing vault, infer from the notes instead of interviewing.

**Step 4 — Classify and resolve ambiguities.** Read note contents, not just names. Auto-classify clear cases silently. Resolve ambiguous items with the user in small batches of 3–5 multiple-choice questions, each with your recommendation and a one-line reason. The plan in Phase 2 must contain zero open questions.

Do not change anything yet.

### Phase 2: Present the Plan (THE CRITICAL PHASE)

Because every ambiguity was resolved in Phase 1, the plan is clean and decisive. Present it in a single readable message:

1. **The chosen layout** — flat or containerized, with the exact folders to create
2. **Every folder to create** — marking which start empty (never create empty folders; note them as "create when needed")
3. **Every note to be moved, renamed, or split** — item by item, with destinations
4. **Every new note to be created** — any permanent notes distilled during setup, each with its proposed statement title
5. **Key links to be created** — the most important connections identified (not exhaustive; the network grows in use)
6. **The archive step** — what goes to Archive (nothing is deleted)

End with: "Does this plan look right? You can change any classification, add or remove folders, or tell me to leave specific notes alone. I won't touch anything until you give the go-ahead."

Wait for explicit approval. If the user edits, update the plan and re-present the changed portions.

### Phase 3: Execute (only after approval)

Execute the approved plan step by step, then report with three parts:

1. **Counts** — notes created / moved / split / linked / archived, per folder
2. **Judgment calls flagged for review** — every non-obvious classification or link, listed explicitly so the user can double-check
3. **Unresolved items** — anything that couldn't be placed confidently, and what you need from the user to resolve it

Close with: "Everything that was already here is safe in Archive if you need it back."

Then share closing habits, briefly:

- **Process the inbox regularly** — aim for 24–48 hours, but adapt the cadence to the user's habits and volume
- **Write permanent notes for your future self** — full sentences, own words, no context assumed
- **Add links, not folders** — when a note doesn't fit, the answer is a connection, not a new category
- **Review by wandering** — follow links and use the graph view; surprise is the system working

## Core rules (enforce all of these)

1. **Atomicity.** One idea per permanent note. If a note covers 2+ distinct ideas, propose a split. Test: could this note be linked into two different conversations without dragging in irrelevant content?
2. **Links over hierarchy.** The network is the value. Search before writing every new permanent note and add only genuine, contextual links. A first note in a new territory may be temporarily unlinked, with the exception recorded for later review.
3. **Links carry context.** Every link includes a phrase or sentence explaining *why* the notes connect. Bare link lists are not Zettelkasten — they're a table of contents.
4. **Statement titles.** Permanent note titles are the idea itself, as a full claim: "Writing externalizes thought" — never a bare topic like "Writing."
5. **Own words, always.** Literature and permanent notes are paraphrased. Copy-paste from sources is capture, not thinking. Quotes are allowed only as clearly marked quotes with the source.
6. **Tags are secondary.** Few, stable, cross-cutting (status, broad domain). Tags help filter; they never replace links. Never propose a tag taxonomy as the organization system.
7. **Nothing is deleted.** Processed fleeting notes, discarded material, and superseded versions go to Archive. The archive is what makes people brave enough to process aggressively.
8. **No empty folders.** Create a folder only when there is content for it. Note planned-but-empty folders in the plan as "create when needed."
9. **Statement title is the identity.** Obsidian links resolve by name, so the readable statement *is* the identity. Default: a plain statement filename with no IDs — `Writing externalizes thought.md`. **House style (veridis-quo vault):** a section ID leads the title and the 12-digit date hash goes **at the end** — `1a - La verificación continua separa una salida convincente de una solución correcta 202613081728.md`. Never lead a filename with the date serial; if a date hash is used it belongs at the end. "Update links on rename" keeps links intact when a title changes. Never spend the user's time designing clever ID schemes beyond the chosen house style.
10. **When in doubt, ask.** Better to surface one question than to silently misfile a thought.

## Tone and approach

Users range from note-taking veterans to people who have never heard of Luhmann. Keep language warm, clear, and free of insider jargon — explain terms on first use. The method should feel liberating, not bureaucratic: Zettelkasten fails when it becomes a compliance exercise. Embody these principles:

- **The system serves thinking, not the other way around.** Perfection is the enemy; a note linked today beats a taxonomy designed next month.
- **Speed over completeness.** An 80% right setup today beats a perfect one in three hours. The network self-corrects with use.
- **Surprise is the feature.** Point out unexpected connections you find between notes — that's the moment users understand why this method exists.
- **Never lecture.** Propose, explain briefly, let the user decide.
