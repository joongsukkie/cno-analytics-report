# CNO Creative Co — Monthly Analytics Report

A standalone, deterministic reporting tool for CNO Creative Co. Feed it a CSV of social media data and it produces the same branded, client-ready analytics report every time.

**No install. No API key. No build step. Works offline.**

---

## What it does

Most social tools show isolated platform numbers. This one turns them into a **visual, client-specific measurement system** spanning awareness, audience quality, campaigns, and business outcomes.

**Reads any export.** One upload accepts multiple files at once and normalizes column names across Rella, Instagram/Meta, TikTok, LinkedIn, and YouTube — no renaming required. It auto-detects account-level vs. post-level data. See [`06_CSV_SCHEMA.md`](06_CSV_SCHEMA.md).

**Interactive, like a real dashboard.**
- Pick any **time period inside the report** — a month, last 30/90 days, all time, or a custom date range — and everything recomputes vs. the previous comparable period. No re-uploading.
- **Interactive charts** with hover tooltips and a current-vs-previous overlay, plus a sparkline on every metric card.
- A visualization-first performance board, client success scorecard, attention-to-action journey, profile/content grids, campaign and content-pillar comparisons, and the full source spreadsheet.

**Analysis the platforms don't give you.**
- **Business journey** — reach → profile visits → website clicks → leads → bookings, using only the steps present in the upload
- **Efficiency ratios** — amplification (reach ÷ followers), view frequency, follow conversion
- **Anomaly detection** — spikes/dips flagged against a rolling 3-month baseline (real signal vs. noise)
- **Format intelligence** — which formats earn reach/engagement, by median (not outlier-skewed averages)
- **Effort vs. return per platform** — posts published against what they actually earned; flags dead channels

**Story-first and yours to shape.**
- The report **opens with the story and the client's goals**, then the numbers. A warm opening letter, a headline, and **Working Now / Needs Attention / Best Next Move**, every claim grounded in the data.
- **Customize** panel: report goal (reorders/emphasizes KPIs), featured-KPI picker, section toggles, client display name — remembered across visits
- **Every summary, finding, and recommendation is editable inline** and prints as edited
- Deterministic by default: identical input always yields the identical report

**Optional AI narrative (bring your own key).**
- Paste an **OpenAI or Anthropic API key** in Customize and the opening letter and takeaways are rewritten from *this client's actual numbers* in the voice of a veteran analyst who can tell a story. No em dashes, no template filler.
- The key is stored **only in your browser**. It is never uploaded and never included in a share link. The client sees only the finished words, which stay fully editable.

**Private, view-only sharing.**
- A share link contains **only the one client's data** — never anyone else's — and opens as a **locked, view-only report**: no uploading, no client switching, no editing, no export. This holds even without a password, so one client can never reach another's results.

**Installable and offline.**
- It is a **PWA**: add it to the home screen or install it as an app on Mac, Windows, or phone, and it keeps working offline. Bookmark it and come back anytime.
- It can also be packaged as a **native desktop app** (a real `.exe` / `.dmg`) for Windows and macOS. GitHub builds both installers for you — see [`DESKTOP.md`](DESKTOP.md).

## Usage

Open `index.html` in any browser (or visit the deployed site with `#demo` to auto-load a sample).

1. **Upload CSV(s)** — drop in any account-level and/or post-level exports, or click **Load demo**
2. Pick a **client** and **period** (month / last 30 / 90 / all / custom range)
3. **Customize** the goal, featured KPIs, and sections; edit any text inline
4. **Export data** to download normalized CSVs, or **Print / PDF** for the client leave-behind

Templates: `resources/template_accounts.csv`, `resources/template_content.csv`. A larger synthetic test file covering Instagram, TikTok, and LinkedIn is at `resources/native_platform_comprehensive_test.csv`.

## Keeping reports refreshed

No API key or backend is required. For each reporting cycle, export the available date range from Rella and/or the native platforms, add any offline outcomes from the client’s booking/CRM/sales records, then upload all CSVs together. The report merges them, separates platforms, and recalculates the selected period. Create a new password-protected share link after the refresh; that link contains a private snapshot of the uploaded report data.

> **Note:** Rella returns the *current* follower total for any date range, so follower *trends* come from monthly **growth**, not by differencing the total. The tool handles this.

## Architecture

Two decoupled halves, on purpose:

1. **Data export** — pull analytics from Rella into the CSV schema.
2. **Report generator** — this repo. A single static page that turns that CSV into the report, deterministically. Same input always yields the same output, which is why it's a program and not a prompt.

## Deploying

Static site, no build. On [Render](https://render.com): connect the repo, choose **Static Site**, leave the build command empty, set publish directory to `.`. The included `render.yaml` does this automatically.

## Files

| File | Purpose |
|------|---------|
| `index.html` | The generator (the whole app) |
| `fonts.css` + `resources/*.woff2` | CNO website typography (Cormorant Garamond and DM Sans), with offline fallbacks |
| `cno-logo.png` / `cno-seal.png` | CNO lockup and wax seal used in the report and the opening letter |
| `manifest.webmanifest` + `sw.js` + `icon-*.png` | Installable/offline PWA support |
| `resources/brand-style.md` | Official brand palette, type, and voice |
| `01_METRICS_DICTIONARY.md` | Every metric and its exact formula |
| `06_CSV_SCHEMA.md` | The CSV contract (including optional `<metric>_target` goal columns) |

## Brand

The interface mirrors cnocreative.co: Cormorant Garamond editorial headings, DM Sans controls and labels, terracotta, sage, cream, fine rules, the CNO lockup, and the wax seal.
