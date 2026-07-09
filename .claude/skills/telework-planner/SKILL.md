---
name: telework-planner
description: Domain knowledge and algorithm for this annual telework planner — the 40% effective-days rule, Madrid holiday data, mandatory-telework weeks (Christmas & Holy Week), summer 4-month block, start-date handling, data model and how to verify. Read this before changing the calculation, the calendar, the histogram, the holiday/mandatory-week config, or the PDF report.
---

# Telework Planner — algorithm & domain knowledge

Single-page, dependency-free web app that lets a worker plan on-site vs telework
days across a year and check compliance with a mandatory minimum of on-site
("presencial") days. Built for a Spanish public-sector telework rule; the UI is
de-branded (no organisation name in the product).

## Files
- `index.html` — the planner (calendar + histogram + PDF report). All CSS/JS inline.
- `configuracion.html` — editor for Madrid holidays and mandatory-telework week ranges. Shares data via `localStorage`.
- `favicon.svg`, `robots.txt` (site is `Disallow: /` + `noindex` — must NOT be indexed).

No build step, no external libraries/CDNs (a hard requirement — everything must be self-contained so it works offline and on GitHub Pages). Vanilla ES5-ish JS.

## The core rule (the algorithm)
For each **computation period**:

```
effectiveDays = workdays − absences − mandatoryTelework
requiredOnsite = floor(0.40 × effectiveDays)      // 40% minimum, rounded DOWN
teleworkPlanned = effectiveDays − onsitePlanned
compliant  = onsitePlanned >= requiredOnsite
```

- **workdays** = Mon–Fri that are not Madrid-capital holidays (and not before the optional start date).
- **absences** = days the user marked as vacation/sick/leave. Removed from the denominator.
- **mandatoryTelework** = Christmas + Holy Week workdays (see below). Removed from the denominator AND cannot be marked on-site.
- **Rounding always favours telework** → the on-site requirement uses `Math.floor`, so the worker never owes a fractional day rounded up.

The 40% comes from "2 mandatory on-site days per week" ≈ 40% of a 5-day week, but the requirement is on **effective worked days**, not calendar weeks — which is the whole point of the tool (holidays and absences shrink the denominator, so 2 days/week can fall short, e.g. October or December after a holiday).

## Computation windows (periods)
Nine periods per year:
- Each month **Jan, Feb, Mar, Apr, May** on its own.
- **Summer = Jun+Jul+Aug+Sep as ONE combined 4-month block** (holidays/vacation flexibility over the summer). Do not split it.
- Each month **Oct, Nov, Dec** on its own.

`PERIODS` array in `index.html` encodes this; the summer entry has `months:[5,6,7,8], summer:true`.

## Day categories (priority order)
On a workday the state is resolved in this order (`dayState` / `computePeriods`):
`absence > mandatoryTelework > on-site(presencial) > telework`.
- Absence wins (you can take leave during a mandatory-telework week).
- Mandatory telework beats a user on-site mark (you cannot be on-site those weeks; the paint tool ignores on-site on those days).
- Everything left over defaults to telework (shown automatically in green).

Colours: telework `#2fa36b` green · presencial `#e8813a` orange · absence `#6d7c93` slate · mandatory telework `#0ea5b7` cyan · holiday `#8b5cf6` violet · weekend muted. Brand accent (UI) is teal `#0f6e78`.

## Mandatory-telework weeks (Christmas & Holy Week)
Excluded from the computation (do not raise the minimum, cannot be on-site). Default ranges per year, editable in `configuracion.html` (stored per year in `localStorage`, else computed):
- **Holy Week (Semana Santa):** Monday–Friday of Holy Week, computed from Easter. Easter via the anonymous Gregorian (Meeus) algorithm — see `easter(y)`, duplicated in both HTML files. Range = `Easter−6` to `Easter−2`; the workdays inside are Mon/Tue/Wed (Maundy Thu & Good Fri are holidays anyway).
- **Christmas:** default `Dec 24–31` plus `Jan 2–5` (the Reyes tail; Jan 1 and Jan 6 are holidays). These are ANECA-style defaults and are user-adjustable — confirm with the user if exact dates matter.

## Madrid-capital holidays
Seeded for 2025 and 2026 from the official Ayuntamiento de Madrid calendar; editable/extendable in `configuracion.html`. Key gotchas (verified):
- **2025:** San Isidro May 15; **La Almudena moved to Mon Nov 10** (Nov 9 is Sunday); Santiago Jul 25 included.
- **2026:** San Isidro May 15; La Almudena Mon Nov 9; **Todos los Santos → Mon Nov 2** and **Constitución → Mon Dec 7** (both moved off Sunday).
- General transfer rule: national holidays on Sunday move to the following Monday.
- Weekday of any date is computed in JS from the stored `YYYY-MM-DD`; only the correct **set of dates** must be maintained.
- Future years are NOT preloaded — the app warns and lets the user enter them.

## Optional start date
If set (per year), workdays before it are excluded from everything (shown hatched, not paintable). Periods entirely before the start are "not applicable" (—) and excluded from the compliance count (`applicable = work>0`; compliance shown as `ok/appCount`, not `ok/9`).

## Weekly template
User picks weekdays (L–V) that are on-site; "Rellenar el año" fills every workday of the selected year: on-site on chosen weekdays, telework otherwise, **preserving absences and mandatory-telework weeks**, skipping before-start days.

## Data model (localStorage)
Keys (renamed from `aneca-*`; a one-time migration copies old→new on load — keep it):
- `tw-holidays-v1` — `{ year: [[ "YYYY-MM-DD", "name" ], …] }`
- `tw-obligatorio-v1` — `{ year: [[ from, to, label ], …] }` (overrides default mandatory ranges)
- `tw-presencial-v1` / `tw-absences-v1` — arrays of `YYYY-MM-DD`
- `tw-inicio-v1` — `{ year: "YYYY-MM-DD" }`
- `tw-nombre-v1` — report name string
All dates are handled in **local time** (`parseYMD`/`ymd`), never `new Date("YYYY-MM-DD")` (which is UTC and shifts days).

## PDF report
"Descargar informe (PDF)" opens a print-optimised modal (`#report`) and uses the browser's Save-as-PDF — **no PDF library** (deliberate). Contains: summary cards, colour year mini-calendar, per-period compliance table, absence ranges (consecutive workdays merged across weekends), month-by-month day lists, methodology note + author credit. A `@media print` block hides the app and prints only `#report`. Wide tables must wrap/scroll (`td.daylist` wraps, tables in `.rscroll`) so the report never overflows.

## How to verify a change
Drive the page headless with the pre-installed Chromium + Playwright:
```
node -e 'const {chromium}=require("/opt/node22/lib/node_modules/playwright/index.js"); …'
// launch({executablePath:"/opt/pw-browsers/chromium-1194/chrome-linux/chrome"})
// goto("file://"+cwd+"/index.html"), drive selects/#applyTmpl, drag day cells via mouse, read #totals/#periodTable, assert numbers, screenshot.
```
Always: check the compliance table numbers by hand for one scenario, emulate `media:print` to confirm the report fits, and assert **no page-error/console-error**.

## Deployment
GitHub Pages serves from **`master`** at `https://elbenxo.github.io/CalculadoraTeletrabajoAneca/`. Dev branch is `claude/aneca-telework-calculator-rm4tss`; keep both in sync (fast-forward `master`). Push to master to deploy.
