---
name: performance-review
description: Check how a client's Google Ads, Microsoft Ads, LinkedIn Ads, Meta Ads and Reddit Ads are doing against their goal. A morning health check that flags only what needs attention, a weekly review against the week before, budget pacing for the month, and a client-ready monthly report (PDF or slides) with insights and next steps. Read-only. Use for "how are my ads doing", "morning check", "weekly report", "monthly report", "client report", "pacing", "are we on track".
license: MIT
metadata:
  author: AdPlug
  version: "1.0.0"
---

# Review performance

Read-only. Follow `adplug:ppc-agent` for how numbers are reported. Read the client brief: results only mean something against the target, the budget and each platform's job.

## Morning health check

Short, and only about what needs the user. For an agency, run it across every client in one pass and send one message grouped by client, not one per client (`google_ads_account_performance` with no account IDs covers every Google Ads account in one call). Check every connected account:
- Spend, clicks and conversions against a normal day, by campaign: spikes and drops, not spend alone.
- Campaigns that stopped spending or stopped converting: budget, billing, disapprovals, limited by budget.
- Tracking that went quiet: a conversion action, goal, rule or pixel event that normally records and recorded nothing.
- Ads, keywords, assets or posts disapproved or rejected, including "destination not working".
- Landing pages behind live ads that don't load or redirect badly (open them).
- Settings that changed: search partners or Display expansion switched on, location options, a different conversion goal, a new auto-applied recommendation (change history on Google, Microsoft, Meta and Reddit; LinkedIn has none, so read its campaigns' status, budgets and end dates).
- Pacing: who will overspend or underspend by the end of the month at the current rate.

Use yesterday as the latest full day; today is partial, and recent days undercount conversions still coming in (conversion lag), so compare like with like before calling a drop. Compare with the same weekday in recent weeks, and check the brief for known events (a sale, a launch, a site change) before calling anything unusual. Don't alert on tiny numbers: a handful of conversions moving is noise. Keep open issues in the brief's "Where we are": an issue already reported gets one line ("still open since <date>") and comes back in full only if it got worse. If nothing needs the user, say so in one line, so a quiet morning still sends a message and silence means something broke. For anything that does, name the account, campaign, what's wrong, since when, and the skill or fix to look at next.

## Weekly review

Default: the last full week against the week before, and against the recent weekly average where one week is too little data; say the dates. Per platform and campaign:
- Spend, clicks, CTR, CPC, conversions, cost per conversion and, where revenue is tracked, ROAS. On Meta, results and cost per result; on LinkedIn, leads, cost per lead and lead form completion.
- The big movers, with the reason behind each: a change in the change log or change history, search terms, a disapproval, budget limits, fatigue, seasonality. A table without reasons isn't a review.
- Pacing for the month.
- Targets and budgets: campaigns beating or missing their CPA or ROAS target, and whether to move the target or the budget. Google's simulations (`google_ads_query` on `campaign_simulation` and `bidding_strategy_simulation`) estimate what a different target or budget would do.
- With HubSpot connected: one paid-to-pipeline line per platform (leads, qualified leads, deals and pipeline created that week), so a week where spend rose and pipeline didn't stands out.
- What's next: the changes you'd make, with numbers attached, each pointing to the skill that would carry it out.

## Budget pacing

For each campaign or campaign group with a monthly budget: spent so far, the daily rate over recent days, and the projected month end against plan. Google can spend up to twice a campaign's daily budget on a single day and balances it over the month, so one heavy day isn't a fault. Meta budgets are daily or lifetime with no monthly figure: report "not spending its budget" or "ahead of its lifetime schedule" instead of monthly pacing. Underspending usually means the budget isn't the limit: check bids, targets, audience size and approvals before raising it.

## Monthly client report

A client-ready PDF report or slides, never a spreadsheet as the deliverable. Ask who reads it, or use the brief's reporting section.
1. A short executive summary: results against target and against the month before, in plain words.
2. Results per platform, each against its own job from the brief, with its own conversions (never added across platforms as one figure, because two platforms can claim the same sale).
3. The insights behind the numbers: what worked, what didn't and why.
4. What changed this month (from the change log) and what it did.
5. What changes next month, and what you need from the client (budget, creative, landing pages, approvals).

Leave out IDs, internal jargon and raw tool output. Keep currencies apart.

For lead generation with HubSpot connected, lead the report with what the ads created in the CRM: qualified leads, deals and revenue by platform and campaign next to each platform's spend (`adplug:lead-quality`). For B2B that's the number the client cares about.

## After the review

Update the brief's "Where we are" line. Check the change log for changes whose check date has come, and report whether they did what was expected. Offer to set the review up as a routine (`adplug:routines`) if it isn't one already.
