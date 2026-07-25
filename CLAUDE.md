# NSM Resource Hub — working notes

Static site: a small kit of running tools built around the Norwegian Singles Method.
No build step, no backend, no dependencies to install. Every page is one self-contained
HTML file (inline CSS + JS) served straight off GitHub Pages.

## Branch strategy — read this before any git operation

- **`master` is LIVE and has real users.** It serves the standalone Dew Point Pace
  Converter at `rikkar69.github.io/Dew-Point-Pace-Converter/` (root `index.html` =
  the converter). Do not merge, rebase, or force-push onto it without explicit say-so.
- **`hub-redesign`** is the multi-tool hub (`index.html` = hub landing page, tools moved
  to `dew-point.html` / `sub-t.html` / `marathon-builder.html`). All feature work lands here.
- Merging `hub-redesign` → `master` would change the root URL's meaning and break the
  bookmark existing users have. The plan is a **separate site on a purchased hostname**
  instead, leaving `master` untouched. See PLAN.md.

## Files

| File | What |
|---|---|
| `index.html` | Hub landing page (on `hub-redesign`) / dew point converter (on `master`) |
| `dew-point.html` | Dew Point Pace Converter — live + historical weather |
| `sub-t.html` | 5K → sub-threshold rep paces |
| `marathon-builder.html` | 15-week marathon plan + JSON/Excel/intervals.icu exports |

## Conventions (match these when adding a tool)

- Everforest palette via CSS custom properties, `[data-theme="dark"|"light"]` on `<html>`.
- Dark/light toggle per tool with its own localStorage key (`dp_`, `st_`, `mb_`, `nsm_`).
  Deliberate — each tool remembers its own preference.
- `.wrap` is `max-width:440px`, widening at `@media (min-width:700px)`. Mobile-first.
- `escapeHtml()` around **every** interpolated value that reaches `innerHTML`.
- `store` get/set wrapper swallows localStorage failures (private browsing safe).
- GoatCounter snippet at the bottom of each page.
- Hub back-link (`.hub-back`) at the top of every tool page.

## Source material is gitignored — keep it that way

`*.jpg` (photographed book pages), `NSA 42K CALCULATOR.xlsx`, `Hand Copy of Book
Schedule.xlsx`, `nsm_marathon_plan.md`, `norwegian_singles_sub_t_pace_calculator.md`.
These are copyrighted/reference material that stays local. **`nsm_marathon_plan.md` is the
written provenance for every marathon-builder formula and session** — read it before
touching that data model.

## Pace formulas are shared across tools — don't let them drift

`sub-t.html` and `marathon-builder.html` must produce identical easy and rep paces for a
given 5K time. Both use the book-table regressions (sec/mile):

```
easy          0.458979*T + 2.344
rep <=3min    0.321496*T + 22.44
rep <=6min    0.328865*T + 24.18
rep  else     0.338341*T + 25.98
```

Goal marathon pace is the one exception, sourced from the marathon calculator's ratio
formula `(T/5) * 1.1275` (sec/km), and is user-overridable in the UI.

## Gotchas

- **ExcelJS is pinned with an SRI hash.** Bumping the version means recomputing it:
  `curl -s <url> | openssl dgst -sha384 -binary | openssl base64 -A`. A stale hash
  silently breaks the Excel export (script refuses to execute).
- intervals.icu's API **does** allow cross-origin browser `fetch()` — no proxy needed.
  Auth is Basic with username `API_KEY`, password = the user's personal key.
- The user's API key is held only in the input's DOM value, never persisted. Keep it
  that way; there's a "Clear key" button for the paste → push → clear flow.
- Verify UI changes in the browser preview before committing — this project has no tests.
