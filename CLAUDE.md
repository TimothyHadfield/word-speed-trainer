# Word Speed Trainer — notes for Claude

A speed-typing drill for one word list (set up with country names and acronyms). Tim built it for
himself; it's a keyboard drill, so it's mainly used on a laptop, but it also works on his iPhone.
Each word is timed from the first key to the last; best times, list records and graphs are kept.

- **Live:** https://timothyhadfield.github.io/word-speed-trainer/ (forwards to `country_training.html`)
- **Repo:** TimothyHadfield/word-speed-trainer (public), branch `main`, GitHub Pages from the repo root.
- **Main checkout:** `OneDrive\Desktop\my-website\Code Projects\Learning & Quizzes\Word_Speed_Trainer`
- State and history: `progress.md` (read START HERE first).

## Files
- `country_training.html` — the whole app: CSS, HTML and plain JS in one file (~2,600 lines, CRLF line endings).
  Screens are `div.screen` elements switched by `showScreen(id)`: user select, menu, Set Words,
  Train, Auto Play (setup / active / result), Data (two canvas scatter plots), plus a slide-in Settings bar.
- `index.html` — tiny redirect to `country_training.html` (keeps `?query` and `#hash`).
- `README.md` + `docs/screenshots/` — the public showcase page on GitHub.
- Git-ignored, only in the main checkout: `country_training (older copy).html` and
  `word-speed-trainer-data.json` (Tim's own data export; personal, never commit or edit it).

## Data (no backend, no accounts)
- Everything lives in the browser's `localStorage`; nothing is sent anywhere.
- Keys: `wordSpeedTrainer_users` (JSON array of names), `wordSpeedTrainer_lastUser`, and per user
  `wordSpeedTrainer_<name>_{words,bestTimes,listRecords,savedLists,errorDelay}`.
- `bestTimes` is keyed by the lower-cased word (ms). `savedLists` are `{name,start,end}` index ranges
  into `words`, so editing the word list shifts what each saved list covers. `listRecords` is keyed by list name.
- `migrateOldData()` moves pre-multi-user keys into a "Default" user. Keep it working.
- "Download Data" saves the current user's data as JSON. There is no import button.

## Run locally
- `python -m http.server <port>` from the repo root (use the background option), then open
  `http://localhost:<port>/`.
- Test users: create one named `claude-test` in the headless browser only (it's localStorage, so it
  never touches Tim's data). To preload data, phone-view `--local 'wordSpeedTrainer_users=["claude-test"]'
  --local wordSpeedTrainer_lastUser=claude-test --local "wordSpeedTrainer_claude-test_bestTimes={...}"`.

## Test
- No test suite and no build step. Verify with `node ~/.claude/tools/phone-view.mjs` at phone and
  `--desktop` width, using `--do` steps (e.g. `click:#menu-screen .btn-secondary` = Train) and `--eval`
  to call the app's global functions (`startTraining()`, `showStats()`, `beginAutoPlay()` …).

## Deploy
- Push to `main`; GitHub Pages rebuilds. Check with `python ~/.claude/hooks/live-check.py "<main checkout>"`.

## Traps
- Words are matched case-insensitively; a wrong letter locks the input for `errorDelay` ms and resets
  the word timer. Pending lock timers are kept in `trainTimer` / `autoPlayTimer` and cleared when a
  run (re)starts; keep that when touching those flows.
- Keyboard shortcuts are part of the design: Enter = next word in Train, Tab = previous word (or back
  to Data when the word was opened by clicking a dot), Tab on Data = back to menu.
- Very long single words are shrunk by `fitWordDisplay()` only when they'd be cut off at the screen
  edge; normal words keep the 4rem / 3.5rem size.
- The Data screen redraws on every mouse move (hover tooltip); keep `drawScatterPlot*` cheap.
- Default word list contains Tim's own spellings ("uraguay", "luxemburg", "litchtenstein"). Best times are
  keyed by those strings, so don't "correct" them.
