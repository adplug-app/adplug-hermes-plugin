---
name: cut-wasted-spend
description: Find and stop wasted ad spend against the client's goal. Google Ads search terms and negatives, Microsoft search queries, Performance Max placements and brand exclusions, Microsoft partner and Audience Network sites, LinkedIn job titles and companies outside the ICP, Meta ad sets and placements, Reddit communities and keywords, and negatives that block good keywords. Proposes each change and applies it only after a yes. Use for "search terms", "SQR", "negatives", "wasted spend", "what's not converting", "clean up the account".
license: MIT
metadata:
  author: AdPlug
  version: "1.0.0"
---

# Cut wasted spend

Follow `adplug:ppc-agent`. Read the client brief: its ICP, never-buy themes, brand terms and target decide what counts as waste. A search term that converts at three times the target is waste for one client and fine for another.

## Before cutting anything

- **Tracking first.** Check the conversion actions are recording (the tracking checks in `adplug:ppc-agent`, and section 1 of each platform's checklist: `skill_view("adplug:<platform>", "references/audit.md")`). If they went quiet, everything looks like waste. Stop and say so.
- **Lead quality, where there's a CRM.** A lead-generation campaign can convert well on the platform and still bring leads sales rejects. When HubSpot is connected, check what the leads became (`adplug:lead-quality`) before calling a campaign a winner or a waste.
- **Enough data.** Judge a term, placement or audience only once it has spent enough that, at the target cost per conversion, it should have converted more than once. Use a window long enough for the account's volume and say which. On a small account, widen the window rather than lowering the bar.
- **Check the assist.** Upper-funnel campaigns and remarketing audiences can look empty on last click. Look at view-through conversions (Google, Meta's 1-day view, LinkedIn post-view) and Microsoft's assists before cutting them. Google's assisted conversions aren't in its API: say so.
- **Read what's already excluded.** Existing negatives and exclusions at every level, so you don't duplicate them, and so a replacement list doesn't drop old entries.

## Where waste hides

**Google Ads search terms** (`google_ads_search_terms`; DSA terms with `dynamic_search_ads=true`)
Classify in two passes. First the common junk, checked against the brief so you never cut what the client sells (a training company sells courses): jobs and careers (job, career, salary, hiring), free and DIY (free, diy, template, download, open source), platforms and help (reddit, youtube, forum, login, sign in), education (course, degree, student), and complaints (scam, refund: review these, don't auto-negate). Then judge the rest against the ICP and each campaign's intent: right product, right buyer, right stage. Classify on intent, not on modifiers like a place or a size.

**Microsoft search queries** (`microsoft_ads_performance` report_type search_query): the same. Microsoft negatives are exact or phrase only.

**Performance Max:** search terms that spent without converting, from `google_ads_query` on `campaign_search_term_view`: select `campaign_search_term_view.search_term` with `metrics.cost_micros` and `metrics.conversions`, and leave keyword segments out or its rows drop (this view isn't in `google_ads_reporting_view_doc`). `campaign_search_term_insight` groups them into categories. Try blocking wrong terms with `google_ads_preview_add_negatives` at campaign level; if Google rejects it for that campaign, the user adds the negative in Google Ads. Also check where its ads showed (`performance_max_placement_view` has impressions only, so exclude on relevance and brand safety with `add_placement_exclusion`), and brand traffic it claims when a brand Search campaign exists (`add_pmax_brand_exclusion`). Display and Demand Gen placements with cost and conversions are in `detail_placement_view`.

**Microsoft search partners and Audience Network** (`microsoft_ads_performance` report_type placement): sites spending without converting. A new website exclusion list replaces the old one: read the current list first (`microsoft_ads_list` website_exclusions) and keep every entry. Say whether partners or the Audience Network are worth keeping at all for that campaign.

**LinkedIn** (`linkedin_ads_performance` report_type demographics with MEMBER_JOB_TITLE, MEMBER_SENIORITY, MEMBER_COMPANY, MEMBER_INDUSTRY, MEMBER_COMPANY_SIZE): segments outside the ICP taking budget. Exclusions go into the campaign's targeting. AdPlug's demographics report has spend and website conversions but no lead columns, so for Lead Gen Form quality read job titles from the form responses and the CRM.

**Meta** (`meta_ads_performance` report_type adset and ad): ad sets and ads well above the target cost per result, once they've spent enough to judge. Don't cut placements, ages or countries because their average cost looks high: Meta moves spend to where the next result is cheapest, so those averages mislead. A placement that must go for brand or policy reasons becomes an exclusion the user sets in Ads Manager. Prefer pausing the ad or ad set, or moving budget.

**Reddit** (`reddit_ads_performance` report_type community, keyword, interest, placement): communities and keywords spending without conversions. Exclude them on the ad group (`update_ad_group` with `add_targeting` excluded_communities or excluded_keywords) or account-wide (`update_account_exclusions`).

**Patterns, not just single terms.** Break the non-converting search terms (Google, Microsoft, Reddit keywords, Performance Max) into one-, two- and three-word chunks and add up each chunk's cost and conversions. The worst chunks become phrase negatives that catch terms you haven't seen yet; chunks that convert well are themes for new ad groups (`adplug:find-opportunities`). Check misspellings and plurals too: a negative doesn't cover them.

**Negatives blocking good keywords.** Over the years negatives pile up and start blocking keywords the client pays for. Compare every negative (account, campaign, ad group, shared list) with the positive keywords; flag the conflicts, worst first where the blocked keyword used to convert. Mind the scope and the match: a shared list blocks only the campaigns it's attached to, and a broad negative blocks a search only when every one of its words is in it.

**Overlap.** The same search term served by two campaigns: negate it in the weaker one (worse cost or wrong intent). Keep brand out of non-brand campaigns with a routing negative; that's structure, not waste.

## Choosing the fix

- **Negatives before pauses.** A negative is precise, reversible and keeps the campaign learning. Pausing an ad group to stop one bad search term is a blunt tool.
- **Level:** junk that shows up everywhere goes on a shared negative list attached to the campaigns; waste in one campaign goes on that campaign; a term that only hurts one theme goes on the ad group.
- **Match type:** phrase by default for a pattern; exact when only that exact wording is the problem but its words are useful elsewhere, or when phrase would block a good long tail.
- **Never negate the brand,** never negate a term that has converted unless it's clearly the wrong product or buyer (and say why), and never add a negative that blocks one of the client's own keywords.
- **The other direction too:** converting terms that aren't keywords go to `adplug:find-opportunities` as additions.
- Order of fixes: negatives and exclusions, then bids, copy or creative, then pauses, then removals. Removals say what can't be undone.

## Show it, then apply

Put every proposed change in one review table before anything is previewed: platform, account, level and where (campaign, ad group, list), the change, why (the trigger: no conversions, off-ICP, wrong intent, competitor), the evidence (spend, clicks, conversions, window), what it would have saved over that window (labelled as an estimate), and whether it's reversible. Group rows by level so the user can approve in bulk.

When the user approves rows, preview exactly those: Google `google_ads_preview_add_negatives` (campaign, ad group or shared list), other changes with each platform's preview tool. Execute after the yes, read back, and log it in the client's change log with a date to check the effect. End with a reminder to review again on the client's routine (`adplug:routines`), because search terms shift as keywords and match types mature.
