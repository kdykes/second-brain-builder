# Morning brief and content extraction routine

Two routines that bookend a working day. The brief runs at the start (and/or on cron before the person wakes). The extraction runs at the end of a working session.

## Morning brief

Run at the start of every session, and optionally on a cron that fires after the overnight fetches (see `architecture-and-automation.md`).

1. **Groom the backlog first.** Auto-dismiss stale open items past a day threshold, and dedup. (Reference implementation: a small script; see the architecture notes.) Commit the result before generating.
2. **Ingest new inputs.** Check `raw/` for material not yet in `log.md`. Ingest each per the schema's Ingest operation (route, update pages, cite, link, log).
3. **Self-heal.** Run one or two rotating checks from `self-heal-checklist.md`. Fix structural issues now; hold content issues for the brief.
4. **Collect open action items.** Scan `content/*.md` from the last 14 days for unchecked `- [ ]` lines not yet marked done (`- [x]`) or dismissed (`- [~]`). Take up to the 10 oldest.
5. **Write the brief** to `content/YYYY-MM-DD.md`:
   - **New inputs.** Each item carries a clickable source URL on its title, so it is one click to act. Never strip the source link.
   - **New calls** (if applicable). One-line summary, a wikilink to the raw file, and the call's action items surfaced as checkboxes so they enter the carryover loop.
   - **Open action items.** Carried from prior briefs, link preserved verbatim, grouped by original date, with a one-line "still relevant?" nudge. If more than 10 remain, show the oldest 10 and note the overflow. The person marks each `- [x]` or `- [~]`.
   - **Topics for review.** Two or three wiki pages not recently revisited, or connected to today's new inputs.
   - **Health.** Any self-heal findings: structural fixes applied, content issues flagged.

Keep it operational and tight. The brief is an interface, not an essay.

### The carryover contract
Unchecked `- [ ]` items ride forward across briefs until marked `- [x]` (done) or `- [~]` (dismissed). This is what stops the brief becoming an ever-growing wall. The groom step (1) enforces the threshold so nothing lingers forever unactioned.

## End-of-session content extraction (optional, if the person publishes)

At the end of a working session, append to today's `content/YYYY-MM-DD.md` under a `## Session: [context]` heading:

1. **Post drafts.** Insights from the session, each mapped to one of the person's content themes. Write them ready to post, not as outlines. Match their voice.
2. **Long-form candidate.** If something is substantial enough for a longer piece, flag it with a working title and a three-bullet outline. Skip if nothing qualifies.
3. **Link roundup.** Links discovered or referenced this session that are worth sharing, each with a one-line context. Skip if none.

These drafts are the raw material for the content pipeline (draft, accept gate, polish, atomize, schedule). The extraction produces candidates; a human still accepts before anything expensive or public happens.

## Voice note

The extraction and the brief both write in the person's voice, from a single stated voice-and-style reference. Keep one such reference file and have every generating step read it, so the brief, the drafts, and any polish cannot disagree about how the person sounds.
