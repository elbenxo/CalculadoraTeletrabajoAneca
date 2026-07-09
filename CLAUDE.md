# CLAUDE.md

Project guidance for Claude Code working in this repo.

## What this is
A single-page, dependency-free **annual telework planner**: a worker marks on-site
("presencial"), absence and telework days on a year calendar and the app checks a
mandatory **40% of effective worked days must be on-site** rule, with a year
histogram and a downloadable PDF report. Static site deployed on GitHub Pages.

The product is **de-branded** — do not add any organisation name (e.g. "ANECA") to
the UI, titles, report or metadata. The site must stay **non-indexable**
(`robots.txt` = `Disallow: /` and `<meta name="robots" content="noindex,…">` on
every page); keep it that way.

## Files
- `index.html` — the planner (calendar, histogram, PDF report). All CSS/JS inline.
- `configuracion.html` — config page for Madrid holidays and mandatory-telework weeks. Shares data via `localStorage`.
- `favicon.svg` — calendar logo (teal `#0f6e78` + orange accent). `robots.txt` — blocks crawlers.
- `README.md` — user-facing description.

No build step, no external dependencies/CDNs (**hard rule**: everything self-contained
so it works offline and on GitHub Pages). Vanilla JS.

## The algorithm & domain knowledge → read the skill
Before touching the calculation, calendar, histogram, holiday/mandatory-week config,
start-date logic or the PDF report, read the **`telework-planner` skill**
(`.claude/skills/telework-planner/SKILL.md`). It documents the 40% floor rule,
effective-days formula, the Jun–Sep summer block, mandatory-telework weeks (Christmas
+ Holy Week, Holy Week from Easter), Madrid holiday data and transfer gotchas, the
start-date handling, the localStorage data model, and the report structure.

## Conventions
- Dates are always local (`parseYMD`/`ymd`); never `new Date("YYYY-MM-DD")` (UTC shift).
- `localStorage` keys are `tw-*`; a one-time migration from the old `aneca-*` keys runs on load — keep it.
- Author credit "Benxamín Porto Domínguez" appears in the web footer, the PDF report and the config page — keep it.
- Match the existing style: teal brand accent `#0f6e78`; category colours are fixed and meaningful (see skill).

## Verify before committing
Drive the page headless with the pre-installed Chromium + Playwright
(`/opt/node22/lib/node_modules/playwright`, browser at
`/opt/pw-browsers/chromium-1194/chrome-linux/chrome`). Load `file://…/index.html`,
exercise the change (selects, `#applyTmpl`, drag day cells), read `#totals` /
`#periodTable`, sanity-check the numbers by hand, emulate `media:print` for the
report, and assert **no console/page errors**. See the skill for the pattern.

## Git & deploy
- Develop on branch **`claude/aneca-telework-calculator-rm4tss`**.
- GitHub Pages deploys from **`master`** → `https://elbenxo.github.io/CalculadoraTeletrabajoAneca/`.
- Keep both branches in sync (fast-forward `master`) and push both. Only push when the user asks; commit messages end with the required Co-Authored-By / Claude-Session trailers.
