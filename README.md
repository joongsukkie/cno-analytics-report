# CNO Creative Co — Monthly Analytics Report

A standalone, deterministic reporting tool for CNO Creative Co. Feed it a CSV of social media data and it produces the same branded, client-ready analytics report every time.

**No install. No API key. No build step. Works offline.**

---

## What it does

Most social tools show you what happened. This one explains **what it means and what to do next.** It derives analysis the platforms don't give you:

- **Audience funnel** — reach → profile visits → follows, with conversion rates at each step
- **Efficiency ratios** — amplification (reach ÷ followers), view frequency, click-through and follow conversion
- **Statistical anomaly detection** — flags spikes and dips against a rolling 3-month baseline, so you know what's a real signal vs. normal noise
- **Content-format intelligence** — which formats actually earn reach and engagement (median, not averages skewed by outliers)
- **Effort vs. return by platform** — posts published against what those posts actually earned; flags channels producing no measurable return
- **Engagement mix** — likes vs. comments vs. shares vs. follows, because shares and follows compound while likes don't
- **Deterministic narrative + recommendations** — plain-English summary written in CNO's brand voice, identical for identical input

## Usage

Open `index.html` in any browser.

1. Click **Load sample** to see a fully worked example, or
2. Load your own **Accounts CSV** (required) and **Content CSV** (optional)
3. Pick a client and month
4. **Export CSV** to download the underlying raw data
5. **Print / PDF** for the client leave-behind

## Data format

See [`06_CSV_SCHEMA.md`](06_CSV_SCHEMA.md) for the full contract.

- `accounts.csv` — one row per **client × platform × month** (required)
- `content.csv` — one row per **post** (optional; unlocks content intelligence)

Sample files live in [`resources/`](resources/).

> **Note:** `followers_total` from Rella is a live snapshot (identical across months), so follower *trends* are derived from `followers_growth`, not from differencing the total.

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
| `fonts.css` | Embedded brand-substitute fonts (Fraunces, Jost), base64 — works offline |
| `cno-logo.png` | CNO lockup used in the report header/footer |
| `resources/brand-style.md` | Official brand palette, type, and voice |
| `01_METRICS_DICTIONARY.md` | Every metric and its exact formula |
| `06_CSV_SCHEMA.md` | The CSV contract |

## Brand

Terracotta is the hero color, balanced by sage and cream. Headings use Fraunces and labels use Jost — free substitutes for CNO's licensed Quiche and Futura. Swap `fonts.css` if the licensed files become available.
