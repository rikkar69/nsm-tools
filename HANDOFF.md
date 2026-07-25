# Handoff — NSM Resource Hub

_Last updated: 2026-07-25 · Phase: 6 (standalone site on purchased hostname)_

## Done

- Marathon Plan Builder built end to end: 15-week schedule, per-week volume + easy/Sub-T
  split, drag-to-reorder day cards, configurable Sub-T warm-up/cool-down, long-run ramp.
- Marathon data model **rebuilt from the hand-transcribed book schedule** (`Hand Copy of
  Book Schedule.xlsx`), replacing the third-party-spreadsheet derivation. Weeks are now 7
  short codes each, parsed by `parseCode()` in `marathon-builder.html`.
- Exports: JSON, styled Excel (ExcelJS, colour-coded by session type), intervals.icu
  plain-text, and direct calendar push. All four verified against a 20:00 5K.
- ExcelJS pinned to 4.4.0 with an SRI hash; "Clear key" button for the intervals.icu key.
- **intervals.icu sync verified working against a real account** → beta/untested copy
  removed, Hub card flipped to Live (commit `ca3f48c`).
- Wrote `CLAUDE.md` and `PLAN.md` — this project had neither before now.

## In progress

Nothing mid-flight. Working tree is clean and `hub-redesign` is pushed. Phase 6 has not
been started — the notes below are a cold start, not a resumption.

## Next steps

1. **Get the hostname from the user** (see Open questions — this blocks everything else).
2. **Create a second GitHub repo for the hub.** GitHub Pages serves one site per repo, so
   two live sites means two repos. Push the current `hub-redesign` tree as the new repo's
   default branch. Leave this repo and its `master` completely alone.
3. **Point the domain at it:** add a `CNAME` file containing the bare hostname to the new
   repo root, configure DNS at the registrar (apex `A` records to GitHub's four IPs, or a
   `CNAME` record for `www`), then enable Pages + "Enforce HTTPS" in the new repo settings.

## Gotchas / dead ends

- **Do not merge `hub-redesign` into `master`.** `master` is live and serving real users
  the standalone Dew Point converter at the repo root. Merging changes what `index.html`
  means and breaks their bookmark. The whole point of Phase 6 is avoiding this.
- GitHub Pages will not serve two branches of one repo at two domains — that's why this
  needs a second repo rather than clever branch/CNAME configuration.
- Relative links (`dew-point.html`, `sub-t.html`, `marathon-builder.html`, `index.html`)
  are already domain-agnostic, so no link rewriting is needed when the domain changes.
- GoatCounter (`rikkar69.goatcounter.com`) is hardcoded at the bottom of all four pages.
  If the new site should track separately, that's a four-file find/replace.
- `.claude/launch.json` is gitignored, so a fresh clone has no preview config. Recreate it
  or just run the server directly (see Verify with).
- Source material (`*.jpg` book photos, both `.xlsx` files, `nsm_marathon_plan.md`,
  `norwegian_singles_sub_t_pace_calculator.md`) is gitignored on purpose. It must not be
  committed to the new repo either — re-check `.gitignore` carries over.
- Bumping ExcelJS requires recomputing the SRI hash or the Excel export silently dies.

## Verify with

```bash
python -m http.server 4173
```
Then load `http://localhost:4173/index.html`, click into each of the three tools, and for
the marathon builder enter a 5K time (e.g. `20:00`) and hit "Build my plan". Check the
browser console is clean and the three export buttons produce files. There is no test
suite — browser verification is the only check this project has.

## Open questions

- **What is the purchased hostname?** Needed before anything in Phase 6 can start.
- What should the new repo be called?
- Separate GoatCounter site for the new domain, or keep pooling stats with the old one?
- Should the existing Dew Point converter on `master` get a small banner pointing at the
  new hub — and is it staying up indefinitely, or eventually redirecting?
