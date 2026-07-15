# CSV input — how the report reads your data
### v2 · one upload, any platform

The generator takes **one upload that accepts multiple files at once**. You no longer need two separate inputs — drop in whatever exports you have and the tool figures out what each file is.

## What the tool detects automatically

Every file is classified row-by-row into one of two **grains**:

| Grain | What it is | Powers | Detected when a row has… |
|-------|-----------|--------|--------------------------|
| **Account-level** | One row per account per month **or day** | Profile insights, daily/weekly trends, paid metrics, audience, and business outcomes | a `followers`/`profile visits` column and a month/date with no populated post fields |
| **Post-level** | One row per individual post | Content insights — engagement, views, shares, format analysis, top content | a `post type` or `caption`, plus per-post metrics and a `date` |

You can upload **either, both, or many files** (e.g. one account export + one post export per platform). They merge into one dataset, and the client/period selectors let you slice it.

## Column names are normalized (multi-platform)

You do **not** need to rename columns. The tool maps common header names from Rella, Instagram/Meta, TikTok, LinkedIn, and YouTube to a shared vocabulary. Examples it understands:

- **reach** ← `reach`, `accounts reached`, `unique reach`
- **impressions** ← `impressions`, `impression count` (times displayed)
- **views** ← `views`, `plays`, `video views` (content/video consumption; never silently merged with impressions)
- **engagement** ← `engagement`, `interactions`, `total engagement`
- **followers** ← `followers`, `subscribers`, `audience`, `fans`
- **follower growth** ← `net followers`, `new followers`, `followers gained`
- **profile visits** ← `profile visits`, `profile views`
- **link clicks** ← `link clicks`, `website clicks`, `link taps`, `profile links taps`
- **shares / saves / comments / likes / replies / reposts** ← their obvious variants
- **date** ← `date`, `published`, `timestamp`, `post time`
- **post type** ← `post type`, `media type`, `format`
- plus `client`, `platform`, `caption`, `hashtags`, demographics (`gender`, `top countries`, `top cities`), etc.

The goal scorecard also recognizes `meaningful comments`, `comment replies`, `DMs`, `leads`, `bookings`, `membership signups`, `retail sales`, `revenue`, `event reach`, `event engagement`, `event registrations`, `event attendees`, `conversions`, `spend`, `paid reach`, `paid impressions`, `organic reach`, and `organic impressions`.

Use an optional `record_type` column (`account_daily`, `account_monthly`, or `post`) when account and post rows live in the same CSV. This removes any ambiguity.

Unrecognized columns are ignored, not fatal. Missing metrics just hide their card — nothing breaks.

## Minimum to get a report
- **Account-level file:** `client, platform, month` + at least one of `reach / followers / engagement`.
- **Post-level file:** `client, platform, date, post_type` + any per-post metrics.

## A note on Rella's follower count
Rella returns the **current** follower total regardless of the date range, so follower *trends* are computed from monthly **growth**, not by differencing the total. The tool handles this for you.

## Templates
Runnable examples: `resources/template_accounts.csv` and `resources/template_content.csv`. Use `resources/native_platform_comprehensive_test.csv` to exercise a combined Instagram + TikTok + LinkedIn file with daily totals, post detail, campaign/pillar tags, paid data, audience fields, and business outcomes.

## Export
The **Export data** button downloads your loaded data back out as normalized `accounts_normalized.csv` and `content_normalized.csv`.
