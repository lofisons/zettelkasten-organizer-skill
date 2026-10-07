# Note Templates

Markdown templates for the three Zettelkasten note types in Obsidian: fleeting, bib, and permanent. **No template for operational material** — drafts, tasks, and project files live outside the vault, wherever the user works.

Obsidian conventions used throughout: statement titles as filenames (no IDs), `[[wikilinks]]` by name, YAML frontmatter as properties. Drop these into your vault's Templates folder (core plugin) to insert them with one command.

---

## Permanent note

```markdown
---
type: permanent
created: 2026-07-28
status: active
tags: []
source: "[[Ahrens — How to Take Smart Notes (2017)]]"
---

# Writing externalizes thought

The act of putting an idea into full sentences forces a precision that
thinking alone never achieves: vague intuitions either survive being
written or reveal themselves as gaps. [Develop the idea: what it claims,
the context in which it holds, its implications.]

## Why it matters

[One short paragraph: why this idea earns a permanent place. What does it
change, enable, or contradict?]

## Links

- [[Notes are conversations with a future self]] — because this note gives
  the mechanism behind that claim.
- [[Emergent structure beats imposed taxonomy]] — contradicts this one on
  the role of memory; worth resolving.
```

**Filename (house style): `SectionID - Statement <datehash>.md`, date hash LAST** — `1a - Writing externalizes thought 202613081728.md`. Lead with the section ID, follow with the readable statement, and end with the 12-digit date hash (`yyyymmddhhmm`). Obsidian links resolve by the statement; "Update links on rename" keeps links intact. Never front-load the date serial — if a date hash is used it sits at the end. (Default without a house style: a plain statement filename, no IDs.)

**Checklist before saving:**

- [ ] Exactly one idea (passes the drag test and the "and" test)
- [ ] Title is a full statement, not a topic
- [ ] Body in the user's own words, full sentences, understandable alone
- [ ] Source recorded (or marked as own thinking)
- [ ] At least one link with a stated reason — or a justified exception

---

## Bib note (literature note)

```markdown
---
type: bib
created: 2026-07-28
tags: []
source: "Ahrens, S. (2017). How to Take Smart Notes. CreateSpace."
---

# Ahrens — How to Take Smart Notes (2017)

[Only the ideas relevant to the user's thinking, paraphrased and brief.
Not a summary of the whole book.]

- Writing notes is not the byproduct of reading — it is the actual work of
  understanding. (ch. 1)
- The slip-box imposes no order upfront; structure emerges bottom-up from
  clusters of notes. (ch. 2)
- Permanent notes must be written as if for publication: full sentences,
  no assumed context. (ch. 3)

> "Nobody ever starts from scratch." (p. 42) [quotes sparingly, marked,
> with page]

## Distilled into permanent notes

- [[Writing externalizes thought]]
- [[...]] — add as permanent notes are written from this source
```

**Filename (house style): the descriptive subject title alone — no date serial, no `Bib` prefix.** `Adaptación de modelos.md`, `Kubernetes — Pods, Deployments y manifiestos.md`. Say what the note is about; that title is what tells you its content and what links resolve to.

---

## Fleeting note

```markdown
---
type: fleeting
created: 2026-07-28
---

Idea while walking: maybe the reason my old notes died is that I filed
them by topic instead of by what they related to. Check against the
collection-vs-network distinction when processing.
```

No ceremony. Fleeting notes are raw capture — no links, no polish required. They are processed within days and then archived. In Obsidian, a fleeting note may simply be an entry in the Daily Notes journal — no file needed until it earns one.

---

## Obsidian setup notes

Drop the templates above into your vault's Templates folder (core plugin) and insert them with the "Insert template" command. Use the core plugin variables in place of fixed dates:

- `{{date}}` → `2026-07-28`
- `{{time}}` → `17:09`

Community plugins worth knowing (all optional): **Dataview** (queries over properties — see `obsidian.md` for the audit queries), **Templater** (more powerful templates than the core plugin), and **QuickAdd** (capture shortcuts).

## Adaptation notes

- **Flat layout:** the `type` property is what separates note kinds — no folders needed. Inbox and Archive keep their own folders (capture and disposal need boundaries).
- **Archive:** either a folder (move processed notes there) or `status: archived` (keep them in place, filter them out of views). Ask which the user prefers; never delete.
