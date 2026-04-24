# Counseling Schedule Dashboard

## Project

- **Live site:** https://counseling-schedule-dashboard.netlify.app/
- **GitHub repo:** https://github.com/jajuangarrett-ctrl/counseling-schedule-dashboard
- **Local path:** `Artifacts/schedule dashboard/` (inside the FJG Vault)

## Structure

Single-file static site. No build step, no package.json.

- `index.html` — the entire dashboard (HTML + inline CSS + inline JS + schedule data).
- Deployed directly to Netlify from the `main` branch (auto-deploys on push).

When editing, treat `index.html` as the source of truth. Schedule data, styling, and behavior all live in that one file.

## Deploy flow

1. Edit `index.html`.
2. Commit and push to `main`.
3. Netlify auto-builds and publishes within ~1 minute.

There is no local dev server required — open `index.html` in a browser to preview.

## Editing conventions

- Keep edits scoped to what was requested. Don't refactor the structure of `index.html` opportunistically — the single-file layout is intentional.
- Past commits have bumped a visible version string to force cache refresh after schedule updates (see commit `9481e3a`). If you update schedule data, check whether a similar bump is needed.
- Semester schedules follow a pattern (Fall 2026, Spring 2027, Summer 2026, etc.) — match the existing shape when adding new ones.

## Out of scope

Do not expand this into a build-tooled project (Vite, React, etc.) unless Franklin explicitly asks. The static single-file approach is the design.
