# Mini Zettelkasten — a tiny working example

This folder is a complete, functioning Zettelkasten with **6 notes**, produced by following the `zettelkasten-organizer` skill. It exists to show what the method looks like in practice — not as theory, but as a small network you can explore in two minutes.

**Best way to explore it:** open this folder as a vault in Obsidian and follow the links — the graph view shows exactly what the method is about.

## The scenario

Someone heard about Luhmann's slip-box, read Sönke Ahrens' *How to Take Smart Notes*, and started building their Zettelkasten around the topic of **writing and thinking**. You are looking at their system one week in.

## What's here (the pipeline, end to end)

| Folder | Contents | What it demonstrates |
|---|---|---|
| `Inbox/` | 1 fleeting note | Raw capture, not yet processed — no polish, no links |
| `Notes/` | 1 bib note + 3 permanent notes | One source distilled; atomic notes with statement titles, full sentences, links with stated reasons |
| `Archive/` | 1 archived fleeting note | Nothing is deleted — processed notes rest here, marked with where they went |

The layout is **flat**: all knowledge in one folder, note kind marked by the `type` property. Inbox and Archive keep their own folders — capture and disposal need boundaries. There is no index or map note: navigation comes from the links themselves, Obsidian's graph, and search.

## The network

Every permanent note searches for related notes before saving, and **every genuine link carries a one-line reason**. A first note in a new territory may remain temporarily unlinked rather than inventing a connection.

- **Writing externalizes thought** → links to the other two permanent notes + the source
- **Emergent structure beats imposed taxonomy** → links to *Writing externalizes thought* + the source
- **Notes are conversations with a future self** → links to *Writing externalizes thought* + the source
- The bib note points to all three permanent notes it was distilled into

## Conventions shown

- **Statement titles as filenames** ("Writing externalizes thought.md", never "Writing.md") — no timestamp IDs, since Obsidian links resolve by name
- **Frontmatter properties:** `type`, `created`, `status`, `tags`, `source`
- **Flat layout:** one `Notes/` folder; the `type` property separates bib from permanent
- **The archived fleeting note** records what it was distilled into — the audit trail that makes people brave enough to process aggressively
- **Nothing is deleted** — the archive holds what was processed

## Try it

Run the skill's audit mode against this folder: the orphan query and the inbox-debt query (see `references/obsidian.md` in the skill) both work against this vault as-is.
