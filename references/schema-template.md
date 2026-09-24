# Schema (operating contract template)

_Copy this into the vault root as `schema.md` and adapt the bracketed parts. This file is the system's brain. The LLM reads it to know the layers, the routing table, and every operation it may run. Change behavior by editing this file, not code._

This vault is an LLM-maintained knowledge base for [PERSON / DOMAIN]. The LLM structures and maintains the wiki. [PERSON] curates sources, directs analysis, and validates changes.

## Layers

- **raw/**: immutable inputs. Bookmarks, call transcripts, saved articles, screenshots, notes. Never modified after creation.
- **wiki/**: persistent, compounding knowledge pages. LLM-generated and LLM-maintained. One topic, entity, or theme per page. Updated across many ingests.
- **wiki/index.md**: catalog of all wiki pages with one-line summaries. (The Routing Table lives below, in this file.)
- **content/**: daily files (`YYYY-MM-DD.md`), one per day, appended if multiple sessions. Holds the daily brief and the end-of-session extraction.
- **log.md**: append-only changelog of all operations.

## Routing Table

Use this to identify which wiki pages to read for a given task, so you never read the whole index. Seed it with the person's real topics; it grows as pages are created.

| Task / Topic | Start With | Also Check |
|---|---|---|
| [topic area 1] | [[Page A]] | [[Page B]] |
| [topic area 2] | [[Page C]] | [[Page A]] |
| [call ingest] | route by topic via this table | dedicated page if the relationship recurs |

When ingesting new material, scan this table to find the one or two affected pages before reading the full index. Update the table whenever a new page is created.

## Operations

### Ingest
When new raw material lands in `raw/`:
1. Read the source fully.
2. Use the Routing Table to identify likely-affected pages.
3. Read those pages, plus scan `index.md` for others that connect.
4. Create or update pages with the new information, always citing the source.
5. Add `[[wikilinks]]` to connect related pages.
6. Update `index.md` and the Routing Table if new pages were created.
7. Append to `log.md`.

### Query
1. Use the Routing Table to find the most relevant pages.
2. Read them, following `[[wikilinks]]` as needed.
3. Answer with citations to pages and original sources.
4. If the answer produces useful new synthesis, file it as a new page.

### Call / transcript ingest (if applicable)
Each call lands as one immutable file in `raw/`. Store frontmatter, a summary, and action items. Storing the full transcript is optional; a link back to the source plus a summary is usually enough.

Use a `scope` field to control **what to extract**, not whether to ingest:
- **`scope: internal`** (own working sessions): extract decisions and language commitments. Pull verbatim quotes that capture framing.
- **`scope: external`** (customer, prospect, partner): extract market signal. Pull the external party's own words, objections, and stated intent. The external voice is the asset.

Citation style for both: `> "quote", Speaker, [[Source File]]`. Do not paraphrase quotes. Always update the page's `## Sources` section.

### Promote Outcomes (action item to wiki feedback loop)
When the person marks a brief item done (`- [x]`) AND adds an indented `- **Outcome:** ...` line beneath it:
1. Route the item to the relevant wiki page via the Routing Table.
2. Append it to that page's `## Tested Plays` section (create the section if absent):
   `- **[Title](URL)**, Outcome: <text>. Tested: YYYY-MM-DD (from [[content/YYYY-MM-DD]]).`
3. Note the promotion in the brief's health section and in `log.md`.

This separates what was saved from what was actually tried and worked. Over time the base records validated plays, not just bookmarks. Dismissed items (`- [~]`) are never promoted.

### Daily brief and content extraction
See `morning-brief-routine.md`.

### Self-heal / lint
See `self-heal-checklist.md`. Run one or two checks per brief, rotating. Fix structural issues immediately; flag content issues in the brief. Never delete.

## Conventions

- One topic per wiki page. If a page covers two distinct things, split it.
- Page titles are plain language, title case.
- Every wiki page ends with a `## Sources` section listing the raw inputs that informed it.
- Use `[[wikilinks]]` for cross-references.
- Dates in ISO format (YYYY-MM-DD).
- Tone: operator, not narrator. Concise, no fluff.
- **Never delete from the wiki.** It is a permanent curation archive.
