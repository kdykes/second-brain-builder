# Self-heal (lint) checklist

Run one or two checks per brief, rotating through the full list over days. Can also be triggered on demand ("run a lint"). Fix structural issues immediately. Flag content issues in the brief for the human to judge. Append every action to `log.md`.

**The one hard rule: never delete from the wiki.** It is a permanent curation archive. Age is not a removal trigger.

## Structural checks (fix on the spot)

1. **Orphaned pages**: wiki pages with no inbound `[[wikilinks]]`. Fix by adding links from related pages, or merging.
2. **Dead links**: `[[wikilinks]]` pointing to pages that do not exist. Fix by creating the page or removing the link.
3. **Index drift**: pages present in `wiki/` but missing from `index.md`, or index entries pointing to pages that no longer exist.
4. **Routing table drift**: new pages not reflected in the Routing Table.

## Content checks (flag for the human, do not auto-apply)

5. **Stale claims**: when an entry first crosses 90 days without reconfirmation, note it once in the brief as an optional FYI. Do not re-surface it on later days, and never remove it. Stale entries simply fall off the daily surface and stay in the wiki.
6. **Contradictions**: conflicting information across pages. Flag with the specific quotes from both pages.
7. **Merge candidates**: pages with significant overlap that should be combined.
8. **Split candidates**: pages covering multiple distinct topics that should be separated.
9. **Missing pages**: topics referenced across several pages but lacking their own dedicated page.

## Connection checks (flag)

10. **Weak clusters**: groups of related pages with few cross-links between them.
11. **Source gaps**: wiki pages with no `## Sources` section, or citing raw files that no longer exist.

## Rotation

Keep a simple pointer (in `log.md` or a small state note) of which checks ran last, and advance through the list so every check runs periodically rather than the same one every day.
