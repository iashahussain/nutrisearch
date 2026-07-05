# Kaana (ކާނާ)

Clinical nutrition suite — single-file HTML PWA. Live at **https://iashahussain.github.io/nutrisearch/** (URL kept from its NutriSearch days so installed apps and data survive).

Dual-use: **patient meal planning** (side-by-side multi-day planner, preferences → auto-fill with swap, food records with intake-vs-requirements reporting) and **hospital food service** (recipe bank incl. the 86 menu-card recipes, menu cycles checked live against editable diet-code standards). Nutrition engine spans IFCT 2017, AFCD R3, AUSNUT 2023, NZFCD, USDA FDC (live API) and custom packaged foods, with cooking yield + USDA R6 retention factors.

## Structure

- `index.html` — the entire app (embedded food databases + baked recipe bank, ~2.6 MB)
- `manifest.json` + `sw.js` + `icon-*.png` — PWA install/offline support
- `label-extraction.md` — Claude instructions for packaged-food nutrition labels (`label-import` skill)
- `recipe-extraction.md` — Claude instructions for batch recipe import (`recipe-import` skill)
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
