# PLAN — NSM Resource Hub

A kit of running tools for the Norwegian Singles Method: pace math, weather-adjusted
pacing, and a full marathon build. Static, free, no accounts.

## Phases

- [x] **1 — Dew Point Pace Converter** *(live on `master`)*
      Live + historical weather → adjusted pace. Global city search, geolocation,
      editable adjustment scale, dew point + air temp model.
- [x] **2 — Multi-tool hub** *(on `hub-redesign`)*
      Hub landing page, tools split into their own pages, shared Everforest theme,
      responsive pass for desktop widths.
- [x] **3 — Sub-T Pace Converter** — 5K → easy + 3/6/10-min rep paces.
- [x] **4 — Marathon Plan Builder** — 15-week build from the book's own schedule,
      per-week volume/intensity stats, drag-to-reorder day cards.
- [x] **5 — Exports + intervals.icu sync** — JSON, styled Excel, intervals.icu text,
      and verified direct calendar push.
- [ ] **6 — Standalone site on `nsmtools.run`** ← *in progress*
      Move the hub to its own domain so `master` keeps serving existing Dew Point users
      undisturbed. `CNAME` + separate GoatCounter site are in; the second GitHub repo and
      the registrar DNS records are still outstanding. See HANDOFF.md.
- [ ] **7 — Backlog** (not started, no commitment)
      - Distance-based "carbon approach" block for ~2:20–2:30 marathoners (the book
        describes it in prose only; would need interpretation, deliberately deferred)
      - Per-session time-of-day input for the dew point historical lookup (currently
        three fixed buckets: 7:00 / 12:00 / 18:00 local)

## Decisions log

- **2026-06-30 — Global city search instead of US ZIP lookup.** Zippopotam.us only
  covered US-style postal codes, making the tool unusable abroad. Open-Meteo's geocoding
  API is global, free, keyless, and we already depend on Open-Meteo for weather — so this
  removed a dependency rather than adding one.
- **2026-06-30 — Reverse-geocode the geolocation button via OpenStreetMap Nominatim.**
  Open-Meteo has no reverse endpoint. "Current location" as a bare label gave the user no
  way to confirm the tool had the right place.
- **2026-07-01 — Air temperature as an additive on top of the dew point scale**
  (+1% at 85–90°F, +2% at 90–95°F, +4% at 95°F+), thresholds anchored in Fahrenheit and
  converted for °C users.
- **2026-07-01 — Today/yesterday route to the forecast API, not the archive.** The ERA5
  archive lags ~5 days; the forecast API's `past_days=1` covers the gap, so a runner can
  check this morning's run after the dew point has shifted by evening.
- **2026-07-02 — Keep `master` untouched; all hub work on `hub-redesign`.** People are
  already using the Dew Point converter at the existing URL. Restructuring the repo root
  would break that bookmark.
- **2026-07-02 — Unify pace formulas across tools.** Sub-T and Marathon Builder were using
  two different sources (book-table regressions vs. spreadsheet ratios) and disagreed by a
  few sec/mile for the same 5K. Standardised on the book-table regressions; goal marathon
  pace is the sole exception (no equivalent exists in that table) and is user-overridable.
- **2026-07-03 — Rebuilt the marathon data model from a hand-transcribed copy of the book
  schedule.** Previously derived from a third-party spreadsheet, which required a pile of
  reconciliation hacks (dropping an anomalous week 1, per-day "atypical" flags) and still
  encoded at least two errors. Hand transcription reduced the data to 7 short codes per
  week parsed generically, and surfaced a real correction: repeated 15-min sessions run at
  **101% of goal marathon pace**, not 101% of the Sub-T 10-min pace.
- **2026-07-03 — Pin ExcelJS by exact version + SRI hash.** The page invites users to paste
  an intervals.icu API key; a compromised CDN script was the one realistic path to stealing
  it. SRI closes that off for free.
- **2026-07-25 — Hub gets its own domain (`nsmtools.run`) and its own GoatCounter site.**
  A second GitHub repo is required because Pages serves one site per repo. Analytics are
  split rather than pooled so hub traffic is measurable separately from the existing
  standalone Dew Point converter, which keeps `rikkar69.goatcounter.com` on `master`.
- **2026-07-25 — intervals.icu sync promoted out of beta** after the user verified a real
  push against their own account. Warning banners and "untested" copy removed.
