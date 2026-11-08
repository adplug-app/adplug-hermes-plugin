---
name: account-audit
description: Senior-level audit of a Google Ads, Microsoft Ads, LinkedIn Ads, Meta Ads or Reddit Ads account (or all of them for one client), judged against the client's goal and ideal customer. Tracking, structure, settings, keywords and search terms, audiences, ads and assets, bidding and budget, landing pages and performance, ranked by severity with a week-by-week plan. Use for a new or inherited account, a new client, a quarterly review, "audit my account", "review this account before we take it on".
license: MIT
metadata:
  author: AdPlug
  version: "1.0.0"
---

# Audit an account

Read-only until the user picks fixes. Follow `adplug:ppc-agent`.

## Before you start

- **The brief.** Load `adplug:client-brief`. An audit without the goal can only check hygiene; with it, it can say whether the account is built to hit the target. If there's no brief, ask at least: the conversion that matters, what it's worth or the target cost, and the monthly budget. Offer to save the rest as a brief afterwards.
- **Scope.** Which platforms and accounts. For a client on several platforms, audit each one and then the whole (below).
- **Window.** Long enough for the account's volume to show patterns, with the same period before it for trends. Say which you chose.

## What to check

Work through the areas in this order, because each one changes how you read the next. For each platform, load its skill (what good looks like there) and its checklist (the checks and the tools that answer them): `skill_view("adplug:google-ads", "references/audit.md")`, and the same for `adplug:microsoft-ads`, `adplug:linkedin-ads`, `adplug:meta-ads` and `adplug:reddit-ads`. Section 1 of each checklist is tracking.

1. **Goals and measurement.** Do the conversion actions match the brief's goal? Is the right one primary, is anything counted twice, is anything gone quiet, do values reflect what a conversion is worth? Everything after this depends on it: if tracking is broken, say so first and treat cost per conversion as unreliable.
2. **Structure.** Does the account follow the brief's plan per platform? Brand and non-brand apart, prospecting and remarketing apart, campaigns with a clear job, no two campaigns competing for the same searches or people, no structure so thin that bidding can't learn.
3. **Settings.** Locations and location options, languages, networks (search partners, Display expansion, Audience Network, LinkedIn Audience Network), schedules, devices, auto-applied recommendations, automated assets and creative enhancements. Settings changed after launch are a classic silent leak.
4. **Targeting.** Search: keyword coverage of the brief's themes, match types, Quality Score, search terms and negatives, conflicts where a negative blocks a keyword. Social: job titles, seniorities, company lists, interests, communities and audience size against the ICP.
5. **Ads and assets.** Enough ads per ad group or ad set to test, ad strength, disapprovals, copy that speaks to the personas and matches the landing page, assets (sitelinks, callouts, snippets, images, lead forms), creative fatigue.
6. **Bidding and budget.** Bid strategy against conversion volume, targets against the brief, campaigns limited by budget while under target, campaigns spending without converting, budget split against where results come from.
7. **Audiences.** Remarketing, customer lists, lookalikes and in-market audiences: used, sized, excluded where they should be (existing customers out of acquisition campaigns, for example).
8. **Landing pages.** Open the top final URLs with your browser or web fetch: they load, they're fast enough on mobile, they match the ad and keyword, the form or checkout works, the call to action is clear.
9. **Performance.** Spend, clicks, CTR, CPC, conversions, conversion rate, cost per conversion, ROAS, impression share and trends. Where the money goes and where the results come from. The biggest leaks and the biggest levers.

## Across platforms, for one client

After the platform audits, look at the whole:
- Budget split against where conversions actually come from, and against the brief's plan. For lead generation with a CRM connected, judge it on qualified leads, pipeline and revenue (`adplug:lead-quality`), not platform conversions.
- Platforms claiming the same conversion: compare with the client's own numbers (CRM, shop orders) where available, and never add the platforms' conversions together.
- The same people targeted by several platforms with different messages, or brand searches paid for on both Google and Microsoft without a reason.
- Gaps the brief implies and nobody covers. Hand those to `adplug:find-opportunities`.

## The report

- Start with a five-bullet executive summary: the state of tracking, the biggest leak, the biggest opportunity, the risk to fix this week, and the overall verdict against the target.
- Then findings by area. For each: the problem, why it matters for this client's goal (money where you can show it), the evidence (numbers, date range, account), and the fix.
- Rank by severity: Critical (money lost or tracking broken now), Important (clear gains), Low (hygiene).
- End with a week-by-week plan for the next month, or the horizon the user prefers, starting with tracking and the leaks.
- Say exactly what you couldn't check and what data would settle it.
- If the user wants it for a client, make it client-ready: a PDF report or slides with a short summary, the findings that matter to them and the plan, without internal jargon.

## After the report

Offer to fix the findings, starting with the critical ones, through the right skill (`adplug:cut-wasted-spend`, `adplug:ad-copy`, `adplug:build-campaigns`) and the usual preview and yes. Record the verdict and the agreed plan in the brief ("Where we are" and "Decisions").
