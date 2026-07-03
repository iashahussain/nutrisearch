# NutriSearch

Clinical nutrition analysis tool — single-file HTML PWA. Live at **https://iashahussain.github.io/nutrisearch/**

Searches IFCT 2017 (India), AFCD R3 + AUSNUT 2023 (Australia), NZFCD (NZ), USDA FDC (live API) and a custom packaged-food database. Features: recipe builder with cooking yield/retention factors, multi-day patient meal planner, printable handouts, household measures, packaged-food import from label photos (via the `label-import` Claude skill).

## Structure

- `index.html` — the entire app (embedded food databases, ~2.3 MB)
- `manifest.json` + `sw.js` + `icon-*.png` — PWA install/offline support
- `label-extraction.md` — instructions for extracting nutrition labels with Claude
- `packaged-custom-foods.json` — reference snapshot of the packaged-food seed data

## Deploy checklist

1. Edit `index.html` (work in this repo clone, **not** the OneDrive copy).
2. **Bump the cache version** in `sw.js` (`nutrisearch-v1` → `-v2` …) — required for installed PWAs to pick up the update.
3. Commit + `git push` (Pages serves from `main` root).
4. On the phone: close and reopen the installed app once to activate the new version.

## Data & backup

All user data (custom foods, recipes, measures, plans) lives in the browser's localStorage — per device/browser. Use the **⬇ Backup / ⬆ Restore** buttons in the header to move data between devices or keep a safety copy.

## USDA API note

Live USDA search uses the public `DEMO_KEY`, which is rate-limited per IP. If searches start failing at the hospital, get a free personal key at https://fdc.nal.usda.gov/api-key-signup.html and replace `USDA_KEY` in `index.html`.
