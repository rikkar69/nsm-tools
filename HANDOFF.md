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
- **Phase 6 mostly done.** Hostname is **`nsmtools.run`**. `CNAME` file added at the repo
  root, and GoatCounter repointed to `nsmtools.goatcounter.com` (code confirmed registered)
  in all four pages. Hub + marathon builder verified loading with a clean console.
- **Second repo created and pushed: `rikkar69/nsm-tools`** (public). `hub-redesign` was
  pushed as its `main` branch and set as the default. Pages is enabled on `main` / root and
  has picked up `nsmtools.run` from the `CNAME` file. Verified beforehand that no
  copyrighted source material exists anywhere in the history being pushed.

## In progress

Phase 6, waiting on DNS. Everything in-repo is done; the domain still points at the
registrar's parking page.

## Two repos now — know which one you're in

`origin` → `Dew-Point-Pace-Converter` (this repo; `master` is the live standalone
converter, `hub-redesign` is the hub source of truth).
`hub` → `nsm-tools` (serves `nsmtools.run` off `main`).

Hub work still lands on `hub-redesign` here, then ships with
`git push hub hub-redesign:main`. Do not develop directly in the `nsm-tools` checkout —
it has no history of its own beyond what is pushed from here.

## Next steps

1. **DNS at the registrar — the only remaining blocker.** As of 2026-07-25 `nsmtools.run`
   still resolves to registrar parking IPs (`76.223.105.230`, `13.248.243.5`). Replace with
   apex `A` records to `185.199.108.153`, `185.199.109.153`, `185.199.110.153`,
   `185.199.111.153`, plus a `CNAME` for `www` → `rikkar69.github.io`.
2. **Enable "Enforce HTTPS"** once DNS propagates and GitHub finishes issuing the cert.
   It could not be set at creation time — it requires a resolving domain first.
   `gh api -X PUT repos/rikkar69/nsm-tools/pages -F https_enforced=true`
3. **Once `nsmtools.run` resolves**, add a banner to `master`'s `index.html` pointing at
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
