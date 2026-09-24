# From second brain to company brain

The vault in this skill is a personal knowledge base. It becomes a **company brain** when you add the layers that every agent and every teammate should start from, instead of a blank page. Load this reference when the person runs a business (even a two-person one) and wants their agents to sound like the business and remember its decisions.

The rule that makes it work: **every agent starts from the part of the brain its job needs, and every finished job writes back.**

## The layers

| Layer | What it holds | Who writes it |
|---|---|---|
| **Core truth files** | The canonical facts of the business: positioning, ideal customer, brand voice, narrative, pricing, naming conventions. One short markdown file each. | One named owner per file. Everyone else proposes changes. |
| **Decision memory** | One short record per decision, rule or lesson, each with the reason it exists. | Written at the moment a decision is made, usually by the agent in the working session. |
| **Knowledge vault** | Everything read and heard, distilled into the wiki (the rest of this skill). | The overnight routine. |
| **Skills** | Playbooks for how specific jobs get done: writing in house voice, running a review, preparing a work order. | Whoever owns the job. |
| **Working records** | Daily briefs, weekly reports, session handoffs, work orders. | The routines and the sessions that produce them. |

## Core truth files

Start with six. Keep each under two pages.

- `positioning.md`: what you are, for whom, against what alternative, and the one-line claim.
- `icp.md`: who the ideal customer is, the situations that make them buy, and the ones that disqualify them.
- `brand-voice.md`: how the business sounds, words to use and avoid, and two or three before/after examples.
- `narrative.md`: the story arc you tell, from problem to change to proof.
- `pricing.md`: current prices, what each tier includes, and what you will not discount.
- `naming-conventions.md`: product, feature and company names, spelled exactly as they must appear.

**Ownership rule.** Each file has one owner, and the owner's version wins on any conflict. Anywhere a copy or excerpt of a core truth file exists (a deck, a page, a prompt), the canonical file overrides it. Other people and agents propose edits (a pull request, a comment, a suggestion file); they do not write to the canonical copy.

Keep core truth files in their own small repository or folder so they can be shared without sharing the rest of the vault.

## Decision memory

One fact per file, so records can be updated or retired individually. A simple format:

```markdown
---
name: short-kebab-case-slug
description: one-line summary used to decide whether this record is relevant
type: decision | rule | lesson | reference
---

The decision or rule, stated plainly.

**Why:** the reason it exists (the incident, the constraint, the preference).
**How to apply:** when it matters and what to do differently.
```

Keep a one-line-per-record index (`MEMORY.md` or similar) that agents load at the start of every session. The index stays lean: a hook per record, detail in the record body. When a record turns out to be wrong, update or retire it rather than adding a contradicting one.

## Wiring "start from the brain"

- **Working sessions.** The agent's project instructions (for example a `CLAUDE.md`) tell it to read the memory index and the latest session handoff before responding, and to read the relevant core truth file before writing any customer-facing copy.
- **Scheduled routines.** Each routine's prompt names exactly which layer it reads: the brief reads the vault, a weekly report reads its own previous reports. Do not give a routine the whole brain; give it the part its job needs.
- **Work orders for other people or agents.** Write them self-contained: the relevant core truth, the rules and do-nots, the exact changes, and a "done when" per task. The receiver should never need to ask what you meant.
- **Session end.** Update memory with any new decision, refresh the handoff note, commit. This is the write-back half of the loop.

## Sharing it with a team

When more than one person needs the brain, give them read access to the core truth and the relevant records through the AI tools they already use. A small private server (for example an MCP server) with read and search tools plus a "propose a change" tool keeps the single-owner rule intact: reads are live, and writes arrive as proposals for the owner to accept. Authenticate every client, and keep any source-control token server-side.

## Guardrails

- The core truth files are the most copied files in the business. Check copies against the canonical file before any of them ships.
- Decision memory records decisions, not conversations. If a record cannot say why, it is not ready.
- Never put secrets, tokens or private personal data in any layer. Those live in a secrets store.
