# Second Brain Builder

A Claude skill that stands up and runs a knowledge base that maintains itself, then grows it into a **company brain**: the core truth, decisions and knowledge every agent in your business starts from.

You curate what goes in. The LLM files it, links it, keeps it tidy, and has a brief waiting for you each morning.

**No backend. No vector database. No accounts.** A folder of markdown in git, a few scheduled jobs, and an LLM doing the upkeep. You can read, version and correct every file.

## Why this exists

Most knowledge bases die the same way. Capture is easy. Maintenance is a second job, and it loses to real work every time. So the links pile up unread and the calls evaporate by Friday.

This skill treats notes as a by-product of the work, not a task on top of it:

- **Capture is passive.** Your sources are fetched on a schedule, overnight, whether you show up or not.
- **The LLM maintains, you curate.** It structures, cross-links and self-heals. You decide what matters.
- **Retrieval stays cheap.** A routing table points the model at two pages, not fifty, so the base doesn't get slower as it grows.
- **Nothing is deleted.** Stale items fall off the daily surface and stay findable forever, which is what makes it safe to put everything in.
- **Expensive steps sit behind a human yes.** The system drafts freely. Anything costly or public waits for you.

## What's in it

| File | What it does |
|---|---|
| `SKILL.md` | The skill: the five rules, setup, and the routines an agent runs. |
| `references/schema-template.md` | The operating contract you copy into your vault as `schema.md`: layers, routing table, and every operation (ingest, query, call ingest, promote outcomes). |
| `references/morning-brief-routine.md` | The daily brief and the end-of-session content extraction. |
| `references/self-heal-checklist.md` | Eleven rotating checks. Structural issues fixed on the spot, content issues flagged for you. |
| `references/architecture-and-automation.md` | The overnight cron chain, thin fetchers, the groom step, and an optional dashboard, as patterns to build against your own tools. |
| `references/company-brain-layers.md` | Growing it into a company brain: core truth files, decision memory, working records, and wiring every agent to start from them. |

It ships the operating contract and the routines. It does **not** ship source-specific fetchers or a dashboard; those are thin and personal, so they're described as patterns instead.

## Install

Claude Code, personal (available in all projects):

```bash
git clone https://github.com/kdykes/second-brain-builder.git ~/.claude/skills/second-brain-builder
```

Or per-project: clone into `.claude/skills/` inside your repo.

Then, in Claude Code:

> Set up a second brain for me. I save articles in a read-later app and record my calls.

The skill walks through the folder, the schema, seeding the first pages, and scheduling the jobs. The files are plain markdown, so the method ports anywhere an LLM can read a file.

## The honest parts

This is distilled from a system that has run every day since April 2026. A few things it taught us:

- **Docs drift.** The schema once kept documenting old cron times long after the jobs moved. A system that writes its own docs still needs a human to catch this.
- **Scheduled jobs run late.** Free cron hosts can start jobs hours after the scheduled time. Leave buffer, and don't promise anyone a 6 a.m. brief.
- **Loops only work if the interface feeds them.** The "promote outcomes" loop turns done items into a record of what actually worked. Ours sat unused for five months, because closing an item never asked what happened. One question on the Done button fixed it. Put the ask where the action happens.
- **Privacy is a standing tax.** Transcripts and notes hold names and internal detail. Anything that leaves the vault gets scrubbed by a human first. The system does not know what is confidential.

## Credits

- The core pattern (raw sources, an LLM-maintained wiki, a schema; ingest, query and lint; `index.md` and `log.md`) is Andrej Karpathy's [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).
- The routing table and the self-heal idea came from Jens Heitmann's [ai-second-brain-skills](https://github.com/NulightJens/ai-second-brain-skills).
- Everything on top (passive fetchers, the overnight chain, call scoping, the morning brief and carryover loop, grooming, promote outcomes, never-delete, and the company brain layers) was built by [Kevin Dykes](https://kdykes.com) and runs daily at Sombra.

## Want it built for you?

This is the pattern behind how we run [Sombra](https://getsombra.com): two people, multiplied. The full teardown, with every diagram, is at [kdykes.com/notes/company-brain](https://kdykes.com/notes/company-brain/). More on how I work at [kdykes.com](https://kdykes.com).

If you'd rather have it built into your business, with your tools, your core truth and private access for your team, [book a free Client Journey Review](https://cal.com/kdykes/conversational-workflow-review).

## License

MIT. See [LICENSE](LICENSE).
