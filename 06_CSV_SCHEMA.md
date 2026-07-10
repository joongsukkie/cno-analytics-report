# CSV input — how the report reads your data
### v2 · one upload, any platform

The generator takes **one upload that accepts multiple files at once**. You no longer need two separate inputs — drop in whatever exports you have and the tool figures out what each file is.

## What the tool detects automatically

Every file is classified row-by-row into one of two **grains**:

| Grain | What it is | Powers | Detected when a row has… |
|-------|-----------|--------|--------------------------|
| **Account-level** | One row per account per period (a monthly/period snapshot) | Profile insights — followers, growth, profile visits, reach, the funnel | a `followers`/`profile visits` column and a `month` (or a date with no post fields) |
| **Post-level** | One row per individual post | Content insights — engagement, views, shares, format analysis, top content | a `post type` or `caption`, plus per-post metrics and a `date` |

You can upload **either, both, or many files** (e.g. one account export + one post export per platform). They merge into one dataset, and the client/period selectors let you slice it.

## Column names are normalized (multi-platform)

You do **not** need to rename columns. The tool maps common header names from Rella, Instagram/Meta, TikTok, LinkedIn, and YouTube to a shared vocabulary. Examples it understands:

- **reach** ← `reach`, `accounts reached`, `unique reach`
- **views** ← `views`, `impressions`, `plays`, `video views`
- **engagement** ← `engagement`, `interactions`, `total engagement`
- **followers** ← `followers`, `subscribers`, `audience`, `fans`
- **follower growth** ← `net followers`, `new followers`, `followers gained`
- **profile visits** ← `profile visits`, `profile views`
- **link clicks** ← `link clicks`, `website clicks`, `link taps`, `profile links taps`
- **shares / saves / comments / likes / replies / reposts** ← their obvious variants
- **date** ← `date`, `published`, `timestamp`, `post time`
- **post type** ← `post type`, `media type`, `format`
- plus `client`, `platform`, `caption`, `hashtags`, demographics (`gender`, `top countries`, `top cities`), etc.

Unrecognized columns are ignored, not fatal. Missing metrics just hide their card — nothing breaks.

## Minimum to get a report
- **Account-level file:** `client, platform, month` + at least one of `reach / followers / engagement`.
- **Post-level file:** `client, platform, date, post_type` + any per-post metrics.

## A note on Rella's follower count
Rella returns the **current** follower total regardless of the date range, so follower *trends* are computed from monthly **growth**, not by differencing the total. The tool handles this for you.

## Templates
Runnable examples: `resources/template_accounts.csv` and `resources/template_content.csv`.

## Export
The **Export data** button downloads your loaded data back out as normalized `accounts_normalized.csv` and `content_normalized.csv`.
