# NSM Resource Hub

A small kit of free running tools built around the **Norwegian Singles Method** — the
high-frequency, sub-threshold approach to endurance training. Pace maths, weather-adjusted
pacing, and a full marathon build, in one place.

**Live:** https://nsmtools.run

No accounts, no tracking beyond a page counter, nothing to install. Every page is a single
self-contained HTML file served straight off GitHub Pages.

---

## The tools

### 🌡 Dew Point Pace Converter — [`dew-point.html`](dew-point.html)

High dew point stops sweat evaporating, so the same effort costs you speed. Enter a target
pace and the tool tells you what your body will actually hold.

- Live conditions anywhere in the world, by city search or geolocation
- Historical mode: check what a past run *should* have cost you
- Air temperature applied as an additive on top of the dew point penalty
- Rounds up when you're within 2° of the next band, so you don't sit on a boundary
- Adjustment scale is fully editable and saved in your browser

### 📈 Sub-T Pace Converter — [`sub-t.html`](sub-t.html)

Enter a recent 5K time, get your easy pace and your 3, 6 and 10-minute sub-threshold rep
paces. That's the whole tool.

### 📅 Marathon Plan Builder — [`marathon-builder.html`](marathon-builder.html)

A 15-week, time-based marathon build with a pace on every session.

- Per-week volume and easy/Sub-T intensity split
- Drag to reorder sessions within a week
- Configurable Sub-T warm-up/cool-down and long-run ramp
- Goal marathon pace auto-estimated from your 5K, and overridable
- Exports: JSON, a colour-coded Excel workbook, intervals.icu workout text — or push all
  15 weeks straight onto your intervals.icu calendar

### 📊 Training Tracker — *in development, not yet deployed*

Syncs your intervals.icu history and trends what the method actually cares about: rep pace
by session length, heart rate at that pace, whether sessions land between LT1 and LT2, and
weekly volume by session type.

---

## Privacy

- **Your intervals.icu API key is never stored.** It lives in the input field for the
  current page session, is sent only to intervals.icu, and there's a "Clear key" button.
  Nothing is written to `localStorage` but your own display preferences.
- No backend. Nothing you enter is sent anywhere except the public APIs listed below.
- Page views are counted with [GoatCounter](https://www.goatcounter.com/), which sets no
  cookies and collects no personal data.

## Tech

Static HTML — no build step, no bundler, no framework, no `npm install`. Each tool is one
file with inline CSS and JS. To run it locally:

```bash
python -m http.server 4173
```

Then open `http://localhost:4173`.

The only third-party runtime dependency is [ExcelJS](https://github.com/exceljs/exceljs)
for the Excel export, loaded from a CDN pinned to an exact version with an SRI hash.

**External APIs:** [Open-Meteo](https://open-meteo.com) for weather, historical weather and
geocoding (free, keyless); [OpenStreetMap Nominatim](https://nominatim.openstreetmap.org)
for reverse geocoding; [intervals.icu](https://intervals.icu) for calendar push and history
sync (your own API key).

## Credit

The method is not mine. If these tools are useful to you, the books are worth owning:

- **[Norwegian Singles Method: Subthreshold Running Kept Simple](https://a.co/d/05AZoAVo)**
  — James Copeland
- **[The Norwegian Method Applied: Threshold Training and Intensity Control](https://a.co/d/0bQ85uec)**
  — Marius Bakken

Also drawn on:

- Dew point adjustment scale adapted from
  [RunnersConnect](https://runnersconnect.net/dew-point-effect-running/)
- Heat-stress research:
  [Mantzios et al., 2022, *Med Sci Sports Exerc*](https://pmc.ncbi.nlm.nih.gov/articles/PMC8677617/)

## A note on the numbers

The pace equations are **reverse-fitted from the pace table in Copeland's book** — they are
estimates that reproduce that table closely, not official formulas published by the author.
The marathon plan is likewise an adaptation of the book's schedule, not a reproduction of
it. Treat every number here as a starting point.

A guide, not gospel — trust how you feel out there.

## Licence

No licence has been chosen yet, so default copyright applies to the code. Whatever gets
picked, it covers only what's in this repo — the training method, the book schedules and
the pace tables belong to their authors.
