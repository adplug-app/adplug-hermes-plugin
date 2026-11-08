---
name: build-campaigns
description: Plan and build a new ad account or new campaigns on Google Ads (Search, Performance Max, Demand Gen, Shopping), Microsoft Ads, LinkedIn Ads, Meta Ads and Reddit Ads from the client's brief, with tracking, structure, keywords, audiences, ads and assets, created paused and approved before anything spends. Use for "set up a new account", "launch a campaign", "build a search campaign", "copy Google into Microsoft", "launch LinkedIn lead gen", "start on Reddit".
license: MIT
metadata:
  author: AdPlug
  version: "1.0.0"
---

# Build a new account or new campaigns

Follow `adplug:ppc-agent` for every change. Read the client brief first; if there isn't one, build it (`adplug:client-brief`) before planning. A campaign built without a goal, a target and an ideal customer is a guess with a budget.

## A brand-new ad account

AdPlug works inside ad accounts; it can't open a new one. Ask the user to create it in the platform (Google Ads under their manager account, Microsoft Advertising, LinkedIn Campaign Manager, Meta Business settings, Reddit Ads), add billing, then connect it or let it appear under an already-connected login at `https://adplug.app/dashboard/connections`. Check it shows in `*_list_accounts`, then lay the foundations before any campaign:

- **Tracking:** the conversion actions from the brief, with values, and which one is primary. A tag or pixel that actually fires (UET tag for Microsoft, Insight Tag and conversion rules for LinkedIn, pixel or dataset events for Meta and Reddit). Nothing optimises without this, so don't launch until it records. For lead generation, plan how qualified leads will flow back from the CRM from day one (`adplug:lead-quality`): the click IDs and UTMs have to be captured before the first lead arrives.
- **Account settings:** Google auto-tagging and auto-apply recommendations (`disable_all_auto_apply` unless the client wants Google applying changes), Microsoft auto-tagging and tracking template (`update_account_settings`).
- **Shared exclusions:** an account-level negative keyword list from the brief's "never buy" themes, brand exclusions for Performance Max where brand has its own campaign, placement and content exclusions the client needs.
- **Audiences:** website visitors and customer lists for remarketing and as signals (Google `create_rule_based_user_list` and Customer Match lists, Microsoft remarketing and customer lists, LinkedIn matched audiences, Meta website and custom audiences, Reddit custom audiences).
- **Naming:** one pattern for the whole account, for example `Client | Platform type | Funnel | Theme or audience | Market`. Reports and routines rely on it.

## Plan before you build

Show the plan and get it approved before the first preview:

1. **Which platforms and why,** from the brief's plan per platform. Search first when people already look for what the client sells; LinkedIn when the ICP is defined by job title and company; Meta and Reddit to create demand and bring visitors back; Microsoft to add search volume cheaply, often by copying Google.
2. **Structure per platform:** campaigns, ad groups or ad sets, and why each exists. Separate brand from non-brand, and prospecting from remarketing. Split where budgets, targets or messages differ, and nowhere else: thin structures starve automated bidding.
3. **Budget:** daily and implied monthly per campaign, against the brief's budget. Enough to get the volume the bid strategy needs to learn.
4. **Bidding:** match it to the conversion data. With little or no conversion history, start with maximise conversions, or maximise clicks with a CPC cap, and move to a CPA or ROAS target once the campaign has enough conversions to steer by.
5. **Targeting:** keywords and match types from the brief's themes and the client's real search terms; LinkedIn job titles, functions, seniorities and company lists from the ICP; Meta and Reddit audiences, communities and interests from the personas. Check the size first (`linkedin_ads_targeting_lookup` audience_count, `meta_ads_targeting_search`, `reddit_ads_targeting_lookup` audience_estimate, `google_ads_keyword_ideas` and `microsoft_ads_targeting_lookup` for search volume).
6. **Ads:** written with `adplug:ad-copy`, from the personas' problems and the brief's offer and proof.

Build orders, required fields and traps are in each platform skill: `skill_view("adplug:google-ads", "references/build.md")`, and the same for `adplug:microsoft-ads`, `adplug:linkedin-ads`, `adplug:meta-ads` and `adplug:reddit-ads`. Read the one for the platform before you preview.

## Build

- Use one batched preview where the platform supports it: Google, Microsoft and Reddit `operation="batch"`, where later steps use the IDs earlier steps create. Meta's `bulk_create` makes many items of one level per call, so preview campaigns, then ad sets, then ads. LinkedIn goes one operation at a time.
- New campaigns start paused, and so do Google responsive search ads, Demand Gen ads and asset groups, and everything on Meta, Reddit and Microsoft (LinkedIn creates drafts). Google ad groups, keywords and asset links start enabled, so anything added to a live campaign goes live on execute: say so in the preview. Google Dynamic Search Ads and product ads start enabled unless you pass `status: PAUSED`, so pass it. On Microsoft, keywords and ads added to a live ad group start paused and don't serve until enabled. Say what's paused and what's live when the build is done.
- If a step fails halfway, read what was created before retrying anything. Report what exists, what failed and what never ran.
- After execution, read every object back and give the user a short table: account, campaigns, ad groups or ad sets, keywords or audiences, ads, assets, status, budget per day and per month.

## Go live

Turning a build on is its own change with its own yes. Before asking, check:
- tracking records on the conversion the campaign optimises for,
- the final URLs load and match the ads,
- locations, languages and networks are what the brief says (Google: presence, not presence or interest, and search partners and Display expansion only if chosen; LinkedIn: Audience Network and audience expansion only if chosen; Meta: placements and Advantage+ settings understood),
- the budget per day and per month is what the client approved.

Then log the launch in the brief's change log and offer a check after the learning period (`adplug:routines`).

## Test instead of guessing

For a change worth proving (a new bid strategy, Performance Max next to Search, a new landing page), offer an experiment instead of a before-and-after comparison: Google `create_experiment`, `schedule_experiment`, then `promote_experiment` or `end_experiment`; Microsoft `create_experiment`. Agree the success measure and how long it runs before it starts.

## Copy Google Ads into Microsoft Ads

`microsoft_ads_preview_mutation` with `operation="migrate_google_campaigns"` copies Google Ads campaigns into Microsoft, created paused. Afterwards: add LinkedIn profile targeting for B2B (`add_profile_targeting`), set up UET and conversion goals, check editorial approvals, and set budgets from the brief rather than Google's. Microsoft often has less volume at a lower cost per click, so judge it on its own numbers.
