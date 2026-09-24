---
name: second-brain-builder
description: >-
  Stand up and operate a compounding, LLM-maintained personal knowledge base:
  an append-only raw layer feeding a self-maintained wiki, a routing table for
  cheap retrieval, a daily brief, a self-heal lint loop, and a content-extraction
  tail. Use when someone wants a "second brain", a personal knowledge base or wiki
  that maintains itself, to turn bookmarks and call transcripts into compounding
  notes instead of a dead pile, to set up a morning brief or daily digest from
  their own inputs, or to make the by-products of their work into a content
  pipeline, or to grow it into a company brain (core truth files and decision
  memory that every agent starts from). Covers the folder structure, the operating
  contract, the cron automation, and the day-to-day routines.
---

# Second Brain Builder

A blueprint and operating manual for a personal knowledge base that maintains itself. The human curates inputs and validates; the LLM does the structuring, linking, upkeep, and surfacing. This skill sets the system up and then runs its routines.

It starts from Andrej Karpathy's [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) pattern (raw sources, an LLM-maintained wiki, a schema; ingest, query, lint) and adds the operating layer that keeps it alive. It is distilled from a system that has run daily since April 2026: bookmarks and call transcripts flow in on a schedule, an LLM files them into a compounding wiki, a brief surfaces what matters each morning, and the by-products become drafted content. The value is in the wiring, not any one part, so this skill is opinionated about the wiring.

## The five rules that make it work

Do not break these. They are the difference between a living system and another abandoned note app.

1. **The LLM maintains, the human curates.** The model structures and updates the knowledge base. The human chooses what enters, directs analysis, and validates output. Do not blur these.
2. **Capture is passive.** If a step needs the human to sit down and process something, it loses to real work. Capture runs on a schedule, whether they show up or not. Notes are a by-product, never a task.
3. **Never delete.** The knowledge base is a permanent curation archive. Age is not a reason to remove anything. Stale entries fall off the daily surface and stay findable forever. This rule is what makes it safe to put everything in.
4. **Gate before spend.** Cheap operations run freely. Expensive ones (long generation, publishing) are blocked behind an explicit human accept. Never pay to polish or expand something that would be discarded.
5. **Retrieval must be cheap.** A base you must read in full to use is a base you stop using. Maintain a routing table so the model reads two pages, not fifty.

## What you are building

A single version-controlled folder (works well as an Obsidian vault, but any markdown folder in git is fine) with these layers:

- `raw/`: immutable inputs. Bookmarks, call records, saved articles, notes. Never edited after they land. This is append-only ground truth.
- `wiki/`: compounding knowledge. One topic per page, written and maintained by the LLM, updated across many ingests. Living documents, not a dump.
- `wiki/index.md`: the catalog of every page with a one-line summary.
- `content/`: one file per day: the daily brief and the end-of-session extraction.
- `log.md`: append-only changelog of every operation.
- `schema.md`: the operating contract. It defines the layers, the **routing table** (task or topic to the one or two pages to start with), and every operation the LLM may run. Change behavior by editing the spec, not code.

Copy `references/schema-template.md` into the vault as `schema.md` and adapt it. That file IS the system's brain; everything else serves it.

## Setup (first run)

1. **Create the folder** and `git init` it. Add the five layers above (empty `raw/`, `wiki/` with a stub `index.md`, empty `content/`, empty `log.md`).
2. **Write `schema.md`** from `references/schema-template.md`. Fill in the person's topics, their input sources, and the routing table seeds.
3. **Seed the wiki** with 3 to 7 topic pages that match how the person actually thinks about their work. Do not over-structure; pages emerge from ingest over time.
4. **Wire inputs.** For each source the person uses (read-later app, meeting recorder, manual saves), set up a fetch into `raw/`. Two thin fetchers are enough to start; see `references/architecture-and-automation.md` for the pattern. Fetchers are source-specific and are reference implementations, not part of this skill.
5. **Schedule the automation.** Put the fetches and the brief on cron (GitHub Actions is a clean, free option). See `references/architecture-and-automation.md` for the schedule shape (fetch, then brief, with buffer).
6. **Optional: a control surface.** A localhost dashboard is a strong addition for reviewing and acting, but it is not required to start. The brief in the vault is the minimum viable interface.

## Operating routines

Load the matching reference when you run one of these.

- **Ingest** (`references/schema-template.md`, Operations section): new `raw/` material to routing table to affected wiki pages, updated with citations and `[[wikilinks]]`, then update the index and append `log.md`.
- **Morning brief** (`references/morning-brief-routine.md`): the start-of-session and/or cron routine. Groom the action backlog, ingest new inputs, run a self-heal check or two, write the day's brief with new items, carryover items, topics for review, and health notes.
- **Self-heal / lint** (`references/self-heal-checklist.md`): one or two rotating checks per brief. Fix structural issues on the spot; flag content issues for the human. Never delete.
- **Company brain** (`references/company-brain-layers.md`): when the person runs a business, add core truth files, decision memory and working records, and wire every agent to start from the part its job needs.
- **Content extraction** (`references/morning-brief-routine.md`, end-of-session section): turn the session's by-products into drafts, mapped to the person's content themes. Optional; include only if they publish.

## What this skill ships vs what it references

- **Ships:** the operating contract (`schema-template.md`), the brief routine, the self-heal checklist, the company brain layers, and the automation pattern. This is the transferable core.
- **References only:** the source-specific fetchers and any dashboard. Those are thin and personal; treat the notes in `references/architecture-and-automation.md` as a pattern to implement against the person's own tools, not code to copy blind.

## Guardrails

- Everything the system touches may be private (names in transcripts, tokens, internal status). Anything that leaves the vault (a published post, a shared doc) is scrubbed by a human. The system does not know what is confidential.
- The accept gate is load-bearing on the human. The system will draft endlessly. Without review it produces volume, not judgment. That is by design: the machine harvests the by-products, the human decides what is worth keeping.
