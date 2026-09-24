# Architecture and automation (reference pattern)

The transferable core of this skill is the operating contract and the routines. The pieces below are **reference patterns to implement against the person's own tools**, not code to copy blind. They are thin and source-specific on purpose.

## The overnight pipeline

Three scheduled jobs, each after the last with buffer for cron lag. GitHub Actions is a clean, free host: the vault is a git repo, and the jobs commit back to it.

| Job | Does | Suggested time |
|---|---|---|
| Input fetch A (e.g. read-later app) | New saves into `raw/` | early, e.g. 02:30 UTC |
| Input fetch B (e.g. meeting recorder) | New calls into `raw/` | early, e.g. 02:00 UTC |
| Daily brief | Groom, ingest, self-heal, write brief | after the fetches, e.g. 03:30 UTC |

Pick times so the brief is waiting before the person sits down, with roughly an hour of buffer after the fetches to absorb cron scheduling lag.

### The brief job shape
1. Checkout the vault, pull latest.
2. Run the groom step (below) and commit if it changed anything.
3. Run the LLM headless against the brief prompt (the routine in `morning-brief-routine.md`, encoded as a prompt file in the repo).
4. On failure, open a tracking issue so a silent break surfaces instead of rotting.

Auth for the headless LLM run comes from a repo secret. Keep it in secrets, never in the repo.

## Fetchers (reference)

Each fetcher is a thin script that:
- Calls the source API for items newer than a stored high-water mark.
- Writes each new item as one immutable markdown file in `raw/`, with frontmatter.
- Updates a small state file (`.<source>_state.json`) with the new high-water mark, used as a health heartbeat.

Keep fetchers dumb: fetch, write, record. All intelligence lives in the ingest routine, not the fetcher. Two fetchers are enough to start; add sources only when the person actually uses them.

For a meeting recorder, storing the full transcript is optional. A summary plus a link back to the source keeps `raw/` light and is usually enough, since the transcript is re-fetchable if ever needed.

## The groom step (reference)

A small script that runs before each brief and keeps the action backlog honest:
- **Carryover:** unchecked `- [ ]` items ride forward until marked done or dismissed.
- **Dedup:** collapse repeated items so the same link does not appear twice.
- **Toggle fan-out:** when the person marks an item done or dismissed in one place, reflect it everywhere it appears.
- **Auto-dismiss:** items past a day threshold (e.g. 30) are auto-dismissed so nothing lingers unactioned forever.

Encode these as deterministic text transforms over the `content/*.md` files. Keep it a plain script, not an LLM call, so it is fast and free and cannot hallucinate.

## The control surface (optional)

A localhost dashboard is a strong addition once the vault is producing daily. It reads the vault (action items, drafts), the fetch heartbeats, and job health, and writes back only on a button click (then commits and syncs). It is where the content pipeline lives: draft, an explicit accept gate, polish, atomize, then a one-click unscheduled draft into the publishing tool.

Two design choices worth copying:
- **Ask for the outcome at the moment of closing.** When an item is marked done, offer one optional line: what happened? Write it as `- **Outcome:** ...` under the newest copy of the item (or into today's file if that copy is older than the brief's scan window). This is what feeds the promote-outcomes loop; without the prompt, nobody writes the line and the loop never fires.
- **Derive state, do not store it.** Compute a card's stage from files on disk at read time so the UI can never drift from the truth in the vault.
- **Generation on a flat-rate login, not a metered key.** If the platform allows a headless CLI authenticated by a subscription login, drafting and rewriting cost no per-token billing, which is what makes it sane to expand and polish freely.

The dashboard is a convenience, not a requirement. The brief in the vault is the minimum viable interface, and everything works without a UI.

## Privacy and secrets

Half of what this system touches is private: names in transcripts, tokens, internal status. Anything that leaves the vault is scrubbed by a human first. Keep all tokens in a gitignored `.env` or in cron secrets, never in the repo, and never in `raw/` or `wiki/`.
