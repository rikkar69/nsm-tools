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
- **Phase 6 started.** Hostname is **`nsmtools.run`**. `CNAME` file added at the repo root,
  and GoatCounter repointed from `rikkar69.goatcounter.com` to `nsmtools.goatcounter.com`
  in all four pages. Hub + marathon builder verified loading with a clean console.

## In progress

Phase 6. The in-repo file changes are done; everything remaining is outside the repo
(GitHub repo creation, registrar DNS) or blocked on the site being live.

## Next steps

1. **Confirm the GoatCounter site code.** The four pages now assume the site was registered
   as `nsmtools`. If it was registered under a different code, or not created yet, the
   counts silently go nowhere — fix with a find/replace on `nsmtools.goatcounter.com`.
2. **Create a second GitHub repo for the hub.** GitHub Pages serves one site per repo, so
   two live sites means two repos. Name not yet decided (`nsmtools` / `nsm-tools` would
   match the domain). Push the current `hub-redesign` tree as the new repo's default
   branch. Leave this repo and its `master` completely alone.
3. **Point the domain at it:** at the registrar, apex `A` records to `185.199.108.153`,
   `185.199.109.153`, `185.199.110.153`, `185.199.111.153`, plus a `CNAME` for `www` →
   `rikkar69.github.io`. Then enable Pages + "Enforce HTTPS" in the new repo settings.
   The `CNAME` file is already committed, so Pages will pick the domain up on first build.
4. **Once `nsmtools.run` resolves**, add a banner to `master`'s `index.html` pointing at
   the hub, with an eventual redirect to `nsmtools.run/dew-point.html` planned. The user
   has approved this in principle — but show the diff before pushing, `master` is live.
   Deliberately NOT done yet: linking live users at a domain that doesn't resolve is worse
   than no banner.

## Gotchas / dead ends

- **Do not merge `hub-redesign` into `master`.** `master` is live and serving real users
  the standalone Dew Point converter at the repo root. Merging changes what `index.html`
  means and breaks their bookmark. The whole point of Phase 6 is avoiding this.
- GitHub Pages will not serve two branches of one repo at two domains — that's why this
  needs a second repo rather than clever branch/CNAME configuration.
- Relative links (`dew-point.html`, `sub-t.html`, `marathon-builder.html`, `index.html`)
  are already domain-agnostic, so no link rewriting is needed when the domain changes.
- GoatCounter is hardcoded at the bottom of all four pages — now `nsmtools.goatcounter.com`
  on `hub-redesign`, still `rikkar69.goatcounter.com` on `master`. Keep them split.
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

- **What should the new repo be called?** Blocks step 2.
- Was the GoatCounter site actually registered as `nsmtools`? (See step 1.)

_Answered 2026-07-25: hostname is `nsmtools.run`; GoatCounter tracks separately from
`master`; `master` gets a banner and an eventual redirect, once the new site is live._
