# Word Speed Trainer — progress

## START HERE
Last updated 2026-10-16. Read this file, then `CLAUDE.md` (file map, data keys, traps).

## Goal right now
Nothing in progress. The app is finished and published; only fixes when Tim asks.

## Status
| Area | State |
|---|---|
| Site icon (tab + iPhone home screen) | Added 2026-10-17: red lightning bolt on a dark key; `icon.svg` + `apple-touch-icon.png`, linked from both html pages |
| App (`country_training.html`) | Working, live on GitHub Pages |
| Showcase README + screenshots | Done (2026-09-27) |
| Bug sweep | Done 2026-10-16 (see Decisions) |

## Authorized next steps
None.

## Standing instructions
- No project-specific instructions from Tim recorded yet. The global rules in `~/.claude/CLAUDE.md` apply.

## Traps
- Data is only in each browser's localStorage. Tim's real data is on his own devices; the
  `word-speed-trainer-data.json` export in the main checkout is personal and git-ignored.
- Test with a `claude-test` user inside the headless browser only.
- See `CLAUDE.md` for code traps.

## Decisions
- 2026-09-27: Published as public repo `word-speed-trainer` with a showcase README.
- 2026-10-16 sweep: fixed stale Data-screen numbers when a filter has too little data; a pending
  wrong-letter timer no longer wipes the next run after Back/Retry; Auto Play drag listeners no
  longer pile up on each visit; very long words no longer cut off at phone width; added aria-labels,
  meta description and an empty favicon (no visual change). Report:
  `laptop-ops/coding-and-claude/reports/sweep-2026-10-16/word-speed-trainer.md`.
- 2026-10-17: the 7 browser alert/confirm/prompt pop-ups are now an in-app dialog (`appAlert/appConfirm/appPrompt`
  in country_training.html); while it's open a capture-phase key listener swallows all keys, so Enter/Tab/typing
  never reach the trainer, and focus returns to where it was on close.

## NOT verified
- On Tim's real iPhone (only Safari's engine at iPhone size via phone-view).
- "Download Data" on iPhone Safari (the link's blob URL is revoked right after the click).

## Rejected / parked
- (none)
