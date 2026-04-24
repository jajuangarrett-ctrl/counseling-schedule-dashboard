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
3. Netlify auto-publishes within ~1 minute (no build step — the repo is served as-is).

There is no local dev server required — open `index.html` in a browser to preview.

**Deploy history gotcha (2026-04-24):** Originally the Netlify site was created via **Netlify Drop** (drag-and-drop) and was NOT linked to the GitHub repo — so every git push silently did nothing on the live site. On 2026-04-24 the GitHub repo was linked in Netlify → Project configuration → Build & deploy → Repository. Verify before assuming a push deployed: `curl -s https://counseling-schedule-dashboard.netlify.app/ | grep SLOTS_FULL` and compare to the working tree. If it's still stale after ~2 min, check Netlify → Deploys for a build with the latest commit hash.

**Cache-busting:** Netlify's edge CDN plus browser caching mean changes can take a hard refresh (Cmd+Shift+R) to appear, even after a successful deploy. Past commits have sometimes bumped a visible version string to force clients to reload (see commit `9481e3a`).

## Editing conventions

- Keep edits scoped to what was requested. Don't refactor the structure of `index.html` opportunistically — the single-file layout is intentional.
- Semester schedules follow a pattern (Fall 2026, Spring 2027, Summer 2026, etc.) — match the existing shape when adding new ones.

## Key code landmarks (inside `index.html`)

- **Grid time range:** `SLOTS_FULL` (non-Friday) and `SLOTS_FRIDAY` are the half-hour slot counts that drive every rendered grid. Slot index 0 = 8:00 AM; each +1 = +30 min. Current values: `SLOTS_FULL = 25` (last row 8:00 PM), `SLOTS_FRIDAY = 12` (last row 1:30 PM).
- **Schedule data:** seeded in `createDefaultState()` via repeated `addSlots(program, staffId, day, startSlot, endSlot, location, remote?, overload?)` calls. `endSlot` is inclusive.
- **Coverage:** `buildCoverageMap()` produces `{ "day-slotIndex": [ {staffId, staffName, ...} ] }` — used by every master/gap view.
- **Persistence:** full `state` is written to `localStorage` under `STORAGE_KEY = 'counseling-schedule-data'`. Users' in-browser edits live there, not in git.
- **Terms/Programs:** `TERMS` and `DEFAULT_PROGRAMS` constants near the top of the `<script>` block are the source of truth for tabs and location lists.

## Exports

- **Individual PDF / Master PDF:** html2canvas + jsPDF (loaded via cdnjs). Snapshots the rendered DOM.
- **Excel (.xlsx):** SheetJS (loaded via cdnjs, `xlsx.full.min.js`). `exportExcel()` builds a workbook with: one "Individual – [Staff]" sheet (current selection), one "Master – [Location]" sheet per program location, and an "All Slots" flat-list sheet.
- **Import JSON** button is still wired (button text reads "Import JSON"), paired with the legacy `exportJSON()` function which is still defined but no longer exposed in the UI. Import JSON remains useful for restoring older backups — don't delete `exportJSON` or the import flow without checking with Franklin first.

## Out of scope

Do not expand this into a build-tooled project (Vite, React, etc.) unless Franklin explicitly asks. The static single-file approach is the design.
