---
name: meta-ads
description: Run Meta Ads (Facebook and Instagram) through AdPlug the way a senior paid social specialist would. Campaigns, ad sets, ads and creatives, Advantage+ audience and budgets, pixel and Conversions API datasets, custom and lookalike audiences, lead forms, catalogues, A/B tests and automated rules. What good looks like, what to check each day, week, month and quarter, and AdPlug's Meta tools, audit checklist and build rules. Use for anything on Meta, Facebook Ads or Instagram Ads, including creative fatigue, the learning phase, delivery problems and Ad Library research.
license: MIT
metadata:
  author: AdPlug
  version: "1.0.0"
---

# Meta Ads

Follow `adplug:ppc-agent` for approvals and reporting, and read the client brief first. This skill holds what's specific to Meta; the jobs themselves have their own skills, which load this one for the Meta details.

Load the references with `skill_view("adplug:meta-ads", "references/<file>")`:
- `references/tools.md`: every Meta tool, report type and list type.
- `references/audit.md`: the audit checklist, area by area.
- `references/build.md`: build order and traps.

## Its job in the plan

Creating demand among people who aren't searching yet, and bringing back the ones who visited. Meta's delivery system decides who sees each ad, so the levers that matter most are the conversion event it optimises for, the budget that event needs, and the creative. Targeting matters less than it used to.

## What good looks like

**Measurement.** The pixel plus the Conversions API on a dataset, so events still arrive when browsers block the pixel. Each ad set optimises for the deepest event that happens often enough: a purchase over an add to cart, a qualified lead (sent from the CRM) over a raw form fill. Optimising for an event that rarely fires keeps ad sets stuck. Attribution is 7-day click and 1-day view by default. Meta stopped reporting the 7-day view and 28-day view windows in January 2026, so older reports that used them aren't comparable. AdPlug reports 1-day view, 1-day click and 7-day click, and each ad set's own setting is in `attribution_spec`: say which window a result uses.

**Structure.** Consolidated. Each ad set needs enough optimisation events to leave the learning phase (Meta's guide is around 50 in a week), so fewer, bigger ad sets usually beat many small ones. Prospecting apart from retargeting, and existing customers excluded from prospecting where the brief wants new ones. Ad sets that never exit learning (`learning_stage_info`) are the clearest sign of a structure that's too thin.

**Audiences.** Advantage+ audience with the client's customer lists, website visitors and lookalikes (`create_lookalike`) as suggestions usually beats hand-built interest stacks. Under Advantage+ audience, age and gender are weak levers. Exclusions (purchasers, staff, current customers) still matter.

**Creative is the targeting.** Meta shows each ad to the people it predicts will respond to it, so variety in angle, hook and format reaches different people. Several genuinely different concepts per ad set, vertical video for Reels and Stories, square or vertical images for feeds, and the brief's personas as the source of angles. Advantage+ creative enhancements can change the ad once it runs (`enhancements`): decide them deliberately and tell the user what's on.

**Budgets.** Daily or lifetime, on the ad set or (with Advantage+ campaign budget) on the campaign. There's no monthly budget, so pacing is "spending its budget or not". Each ad set needs enough budget for its event to happen regularly. A spend cap on the account (`set_spend_cap`) is a sensible guardrail for clients who want one.

**Lead ads.** Form questions that qualify, a higher-intent form type where quality matters more than volume, and leads synced to the CRM quickly. Judge lead ads by what the leads become (`adplug:lead-quality`), not by cost per lead.

**Testing.** Meta's own split tests (`create_ab_test`) for questions worth proving, rather than comparing ad sets that compete for the same people. Automated rules (`create_automated_rule`) can pause or adjust things without anyone asking first, so create one only when the user explicitly wants that rule, and say exactly what it will do.

## The rhythm

What a senior specialist checks on Meta. These checks fold into the client's routines (`adplug:routines`); don't create a scheduled job per check.

- **Every day** (the morning check): ads that should be running and aren't (`meta_ads_diagnose`; judge by `effective_status`), rejections, spend against budget, pixel and dataset events still arriving.
- **Every week:** creative fatigue (frequency up, CTR down, cost per result up, by ad), ad sets stuck in learning, cost per result against target by ad set, and what changed (`activity_logs`). Read placement, age and country splits as context, not as cuts: Meta moves spend to where the next result is cheapest, so a placement with a higher average cost per result is often still worth having (Meta calls this the breakdown effect).
- **Every month:** new creative concepts to replace the tired ones, audiences and exclusions refreshed, catalogue health for ecommerce (`catalog_diagnostics`), Advantage+ creative enhancements still as agreed, test results, and what competitors run (`meta_ad_library_search`, commercial ads only where they reached the EU or UK).
- **Every quarter:** structure against the brief, the offer and the creative strategy, and whether the optimisation event can move deeper in the funnel.

## In AdPlug

- `meta_ads_list_accounts` returns ad accounts, Facebook Pages and linked Instagram accounts; a new ad needs all three. Without a linked Instagram account the ads are Facebook-only: say so.
- Every change: `meta_ads_preview_mutation`, then `meta_ads_execute_mutation`. For campaigns, ad sets and ads Meta validates the change without making it; updates come with a field-by-field diff. `bulk_create` makes many items of one level per call, so preview campaigns, then ad sets, then ads.
- Everything is created paused. Money is in the currency's minor unit (5000 means 50.00).
- Objective and buying type are fixed at creation. A creative can't be edited: make a new one and swap it onto the ad with `update_ad`, which keeps the ad's history.
- Custom Audience terms and Lead Ads terms can't be accepted through the API: give the user the link Meta returns.
- `meta_ads_diagnose` counts paused ads as blocked, so look for the ones that should be live.

## Which skill does the job

| Job | Skill |
|---|---|
| Audit the account | `adplug:account-audit`, with `references/audit.md` |
| Ad sets, ads and placements above target | `adplug:cut-wasted-spend` |
| New audiences, formats, competitor angles | `adplug:find-opportunities` |
| New campaigns, ad sets, lead forms | `adplug:build-campaigns`, with `references/build.md` |
| New creatives and fatigue refreshes | `adplug:ad-copy` |
| Weekly and monthly results | `adplug:performance-review` |
| Ads not delivering, results dropped | `adplug:diagnose` |
| What Meta leads become in the CRM | `adplug:lead-quality` |
