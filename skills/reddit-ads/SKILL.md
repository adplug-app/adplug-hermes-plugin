---
name: reddit-ads
description: Run Reddit Ads through AdPlug the way a senior specialist would. Campaigns, ad groups and promoted posts, community, conversation keyword and interest targeting, the Reddit Pixel and Conversions API, brand safety exclusions and community-native creative. What good looks like, what to check each day, week, month and quarter, and AdPlug's Reddit tools, audit checklist and build rules. Use for anything on Reddit Ads, subreddit or community targeting, keyword targeting, Reddit conversions, or ads that aren't spending.
license: MIT
metadata:
  author: AdPlug
  version: "1.0.0"
---

# Reddit Ads

Follow `adplug:ppc-agent` for approvals and reporting, and read the client brief first. This skill holds what's specific to Reddit; the jobs themselves have their own skills, which load this one for the Reddit details.

Load the references with `skill_view("adplug:reddit-ads", "references/<file>")`:
- `references/tools.md`: every Reddit tool, report type and targeting action.
- `references/audit.md`: the audit checklist, area by area.
- `references/build.md`: build order and traps.

## Its job in the plan

Reaching people where they discuss the problem the client solves: the communities they read and the conversations they have. It suits products people research and argue about before buying (software, tech, gaming, finance, hobbies). Reddit users are quick to punish ads that read like ads, so the creative has to earn its place. Reddit retired its onsite lead forms on 30 September 2026, so lead generation sends people to a landing page.

## What good looks like

**Measurement.** The Reddit Pixel plus the Conversions API (`send_conversion_events`), so conversions still count when the pixel is blocked. Send the same `conversion_id` with the pixel event and the Conversions API event, or Reddit counts the conversion twice. Ad groups optimise only for an event that actually fires; one optimising for an event the pixel hasn't seen lately barely delivers (`reddit_ads_diagnose` flags it).

**Structure.** Ad groups split by targeting theme, so results show which approach works: one for communities, one for conversation keywords, one for interests, rather than tiny variations of each. Campaign budget optimisation where the ad groups share a goal; ad group budgets where priorities differ.

**Targeting.** Communities and conversation keywords from the personas' problems and the words they use (`reddit_ads_targeting_lookup` suggest_communities from the client's website, keyword_suggestions, validate_keywords). Check the audience estimate and suggested bid before launch. The community and keyword reports do the job search terms do on Google: they show where the money went and what converted.

**Brand safety.** Account-wide exclusions (`update_account_exclusions`) for communities and keywords the client must never appear next to, set before the first campaign spends.

**Creative.** Write each post like a useful post in that community, not a banner: plain, specific, honest about being an ad, in the community's language. Decide whether comments are allowed (`allow_comments`) and, if they are, make sure someone reads them; replies in comments are part of the ad. A published post can't be edited, so a change means a new post.

**Bidding.** Start near Reddit's suggestion (`bid_suggestion`) and compare with the target cost. Ad groups that don't spend their budget usually have an audience that's too small, a bid that's too low, or an event with no data.

## The rhythm

What a senior specialist checks on Reddit. These checks fold into the client's routines (`adplug:routines`); don't create a scheduled job per check.

- **Every day** (the morning check): ads that should be running and aren't (configured against effective status), rejections and their reasons, billing, pixel events still arriving.
- **Every week:** the community and keyword reports: exclusions for what spends without converting, and winners worth their own ad group. Cost per result against target by ad group.
- **Every month:** creative refresh where frequency rises and CTR falls, new communities and keywords to test (`suggest_communities`, `keyword_suggestions`), Conversions API events still matching the pixel, and what changed (`change_history`).
- **Every quarter:** whether Reddit earns its share of the budget against the brief's plan, and a new angle or offer test.

## In AdPlug

- Every change: `reddit_ads_preview_mutation`, then `reddit_ads_execute_mutation` with the same operation and params. `operation="batch"` builds campaign, ad group, post and ad in one preview.
- Everything is created paused unless the status is ACTIVE; activating starts spending and sends the ads to Reddit's review, so it needs its own yes.
- Money is plain currency.
- Configured status is what the advertiser set; effective status is what Reddit actually does (pending billing, pending approval, rejected, parent paused). Report both when they differ.
- Exclusions: on the ad group (`update_ad_group` with `add_targeting` excluded_communities or excluded_keywords) or account-wide (`update_account_exclusions`).

## Which skill does the job

| Job | Skill |
|---|---|
| Audit the account | `adplug:account-audit`, with `references/audit.md` |
| Communities and keywords spending without converting | `adplug:cut-wasted-spend` |
| New communities, keywords and audiences | `adplug:find-opportunities` |
| New campaigns, ad groups and posts | `adplug:build-campaigns`, with `references/build.md` |
| Posts and refreshes | `adplug:ad-copy` |
| Weekly and monthly results | `adplug:performance-review` |
| Ads not spending, conversions stopped | `adplug:diagnose` |
| Conversions API from the CRM | `adplug:lead-quality` |
