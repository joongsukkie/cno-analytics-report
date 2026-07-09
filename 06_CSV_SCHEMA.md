# CSV Schema — the contract between Rella and the report generator
### v1 · July 9, 2026

The report generator reads **one required CSV** (accounts) and **one optional CSV** (content). Column names are **exact, lowercase, snake_case**. Order doesn't matter; missing optional columns are handled gracefully (that section just doesn't render). This schema maps 1:1 to what the Rella connector returns, so the Claude→CSV export is a clean copy.

---

## File 1 — `accounts.csv` (REQUIRED)
**Grain:** one row per **client × platform × month.** Multiple months = trend charts. Multiple platforms = platform breakdown.

| Column | Required | Type | Notes |
|--------|----------|------|-------|
| `client` | ✅ | text | Exact client/space name, e.g. `Example Brand` |
| `platform` | ✅ | text | one of: instagram, tiktok, linkedin, facebook, pinterest, youtube, threads |
| `month` | ✅ | text | `YYYY-MM` (e.g. `2026-06`) |
| `followers_total` | ✅ | int | **Live snapshot** from Rella (same across months) — used for "current followers," not the growth trend |
| `followers_growth` | ✅ | int | Net follower change **in that month** — this drives the growth trend |
| `reach` | ✅ | int | Unique accounts reached |
| `engagement` | ✅ | int | Total interactions (Rella aggregate) |
| `views` | rec. | int | Views / impressions volume |
| `profile_views` | rec. | int | Profile visits |
| `impressions` | opt. | int | If distinct from views |
| `likes` | opt. | int | Usually blank at account level (see content.csv) |
| `comments` | opt. | int | Usually blank at account level |
| `shares` | opt. | int | Usually blank at account level |
| `saves` | opt. | int | Not exposed by Rella aggregate — leave blank unless known |
| `link_clicks` | opt. | int | Website/bio clicks if available |
| `top_age_group` | opt. | text | e.g. `25-34` (demographics) |
| `top_gender` | opt. | text | e.g. `F` |
| `top_countries` | opt. | text | Pipe-separated, e.g. `United States|Canada|Nigeria` |
| `industry` | opt. | text | For benchmark selection, e.g. `beauty` (else platform-default norm used) |
| `goal` | opt. | text | `growth` / `engagement` / `reach` / `leads` — sets lead KPI emphasis (Phase 2) |

**Engagement rate is NOT a column** — the generator computes it, so the math is identical every time:
- ER (by reach) = `engagement ÷ reach` → headline resonance metric
- ER (by followers) = `engagement ÷ followers_total` → used for the industry-benchmark comparison

---

## File 2 — `content.csv` (OPTIONAL — powers the "Top Content" section)
**Grain:** one row per top post.

| Column | Required | Type | Notes |
|--------|----------|------|-------|
| `client` | ✅ | text | Must match accounts.csv |
| `platform` | ✅ | text | same set as above |
| `month` | ✅ | text | `YYYY-MM` |
| `date` | rec. | date | Post date (ISO) |
| `post_type` | rec. | text | POST, REEL, CAROUSEL, STORY, VIDEO, SHORT, IMAGE, DOCUMENT, TEXT |
| `engagement` | ✅ | int | Total interactions on the post |
| `reach` | rec. | int | |
| `views` | opt. | int | views or impressions |
| `likes` `comments` `shares` `follows` | opt. | int | post-level splits |
| `hashtag_count` | opt. | int | |
| `caption_snippet` | opt. | text | Short snippet — **quote it** (contains commas/emojis) |

---

## Rules
- **UTF-8, comma-delimited.** Any field containing a comma, quote, or newline must be wrapped in double quotes (`"..."`), with internal quotes doubled (`""`). The generator's parser handles this.
- **One export can hold many clients and many months** — the generator lets the user pick client + month in the UI. (Default workflow: one combined CSV for all clients each month, appended to history.)
- **Blank ≠ zero.** Leave a cell empty if unknown; the generator hides missing metrics rather than showing `0`.
- **Stable client/platform spelling** across months so trends link up.

## Example (accounts.csv)
```
client,platform,month,followers_total,followers_growth,reach,engagement,views,profile_views,top_age_group,top_gender,top_countries,industry
Example Brand,instagram,2026-06,2400,61,4700,880,15400,390,25-34,F,United States|Canada,example
Example Brand,tiktok,2026-06,540,,,210,6100,,,,,example
```

Runnable templates live in `resources/template_accounts.csv` and `resources/template_content.csv`.
