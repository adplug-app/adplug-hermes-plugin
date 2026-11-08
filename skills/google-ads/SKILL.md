---
name: google-ads
description: Run Google Ads through AdPlug the way a senior PPC specialist would. Search, Performance Max, Demand Gen (YouTube), Shopping and Dynamic Search Ads. What good measurement, structure, bidding, keywords, ads, audiences and settings look like, what to check each day, week, month and quarter, and AdPlug's Google tools, audit checklist and build rules. Use for anything on Google Ads or AdWords, including keywords, search terms, Quality Score, Smart Bidding, PMax, asset groups, Merchant Center products and MCC accounts.
license: MIT
metadata:
  author: AdPlug
  version: "1.0.0"
---

# Google Ads

Follow `adplug:ppc-agent` for approvals and reporting, and read the client brief first. This skill holds what's specific to Google Ads. The jobs themselves (audit, waste, growth, builds, ads, reviews) have their own skills, which load this one for the Google details.

Load the references with `skill_view("adplug:google-ads", "references/<file>")`:
- `references/tools.md`: every Google tool, report level and change category.
- `references/audit.md`: the audit checklist, area by area.
- `references/build.md`: build orders and traps for Search, Performance Max, Demand Gen and Shopping.

## Its job in the plan

Google catches demand that already exists: people searching for what the client sells. Search does that most precisely. Performance Max and Demand Gen reach further (YouTube, Display, Discover, Gmail, Maps) and need a strong conversion signal to spend well. Brand search protects the client's own name from competitors and usually converts cheapest, so judge it apart from everything else.

## What good looks like

**Measurement.** Smart Bidding chases whatever conversion it's given. Each campaign optimises toward the brief's real goal (`set_campaign_conversion_goal`); everything else is secondary and observed only. When leads differ in worth, give them values, and use value rules (`create_value_rule`) where location, device or audience changes what a conversion is worth. Auto-tagging stays on (`customer.auto_tagging_enabled`), or there's no click ID to send results back with. For lead generation, the strongest signal is what happened after the form: qualified leads and sales sent back with the click ID (`upload_click_conversions`, `upload_conversion_adjustments`; see `adplug:lead-quality`).

**Structure.** As few campaigns as the brief allows, so each one gets enough conversions to steer by. Split only where budget, target, message, market or landing page differ. Brand, competitor and non-brand in separate campaigns, never sharing a budget or a portfolio bid strategy (brand soaks up the shared pool). Ad groups themed by intent, each with ads and a landing page that answer the same search. When Performance Max runs next to Search, set brand exclusions (`add_pmax_brand_exclusion`), and remember that a Search keyword identical to the search term takes priority over Performance Max, so cover the terms you want to control with Search keywords.

**Bidding.** Match the strategy to the conversion data. Little history: maximise conversions, or maximise clicks with a CPC cap. Steady volume: a CPA or ROAS target set from what the campaign actually achieves, then tightened in steps. A target far stricter than reality throttles delivery. Don't change a target and a budget on the same day, and leave a campaign through its learning period before judging it. Google's simulations (`google_ads_query` on `campaign_simulation` and `bidding_strategy_simulation`) estimate what a different target or budget would do.

**Keywords and search terms.** Phrase and exact for core themes. Broad match only where Smart Bidding, solid tracking and working negatives back it. Search terms reviewed on a rhythm, negatives kept in shared lists by theme (jobs, free, competitors, wrong product) and attached where they belong. Quality Score's three parts (expected CTR, ad relevance, landing page experience) tell you what to fix; the score itself isn't a target. Google is moving Dynamic Search Ads, automatically created assets and campaign-level broad match into AI Max (automatic upgrades from September 2026; Dynamic Search Ads from February 2027), so check what each upgraded campaign now matches and rewrites, and don't build new Dynamic Search Ads. Since September 2026 Google no longer takes language targeting on Search: ads match by the language of the ads and landing page.

**Ads and assets.** Responsive search ads per ad group as `adplug:ad-copy` sets out, with unique headlines that each stand alone and no pinning unless something must always show. Sitelinks, callouts, structured snippets, images, business name and logo on every campaign; price, promotion and call assets where they fit. Ad strength is a hint, not the goal. For Performance Max, full asset groups per theme or product category, and brand guidelines (`enable_brand_guidelines`) when the client wants control of the business name and logo.

**Audiences.** On Search, audiences in observation to learn who converts. Remarketing lists (`create_rule_based_user_list`) and Customer Match lists as signals for Performance Max and Demand Gen, and existing customers excluded from acquisition campaigns where the brief says so. Performance Max signals include search themes (`create_asset_group_signal` with `SEARCH_THEME`).

**Settings that leak money quietly.** Location option "presence or interest" instead of "presence", search partners and Display expansion on Search, auto-applied recommendations (`disable_all_auto_apply`), AI Max on Search (it matches searches beyond the keywords, can rewrite headlines and send traffic to other pages), automatically created assets and final URL expansion. Each is fine when chosen and expensive when nobody noticed. AdPlug reads them all (`google_ads_query` on `campaign.ai_max_setting.enable_ai_max` and `campaign.asset_automation_settings`) and can change the first three; the user switches the rest in Google Ads. `segments.search_term_match_source` shows which search terms AI Max bought, and `create_experiment` with `ADOPT_AI_MAX` tests it.

## The rhythm

What a senior specialist checks on Google Ads, and how often. These checks fold into the client's routines (`adplug:routines`); don't create a scheduled job per check.

- **Every day** (the morning check): spend and conversions per campaign against a normal day, conversion actions still recording, disapproved ads and assets (including "destination not working"), campaigns limited by budget while under target, why any campaign isn't serving (`campaign.primary_status_reasons`), and offline conversion uploads still arriving where bidding relies on them (`offline_conversion_upload_client_summary`).
- **Every week:** search terms and negatives for Search, Dynamic Search Ads and Performance Max (Performance Max terms with cost: `google_ads_query` on `campaign_search_term_view`), pacing against the month, campaigns against their CPA or ROAS target, and what changed (`change_event`, last 30 days only, with a LIMIT).
- **Every month:** impression share lost to budget and to rank, Quality Score parts on the keywords that spend most, assets labelled low with enough impressions to judge, Performance Max and Demand Gen placements to exclude, Google's Recommendations worth taking, and audience list sizes.
- **Every quarter:** structure against the brief, keyword research for new themes and seasonality (`google_ads_keyword_ideas` with `monthly=true`), and an experiment for the biggest open question (`create_experiment`).

## In AdPlug

- Every change starts with `google_ads_preview_mutation` (or `google_ads_preview_add_negatives`), which returns a `preview_id`. Each preview has its own ID and expires after 15 minutes, so take each change from preview to yes to execute in one go. `operation="batch"` runs up to 100 steps in one preview, with later steps using earlier results (`"$0.asset_id"`).
- New campaigns, responsive search ads, Demand Gen ads and asset groups start paused. Ad groups, keywords and asset links (sitelinks, callouts and other assets attached to a campaign or ad group) start enabled, so anything added to a live campaign goes live on execute: say so in the preview. Dynamic Search Ads and product ads start enabled unless you pass `status: PAUSED`, so pass it.
- Money is plain currency in the account's currency; the `min_cost_micros` filter on search terms is in micros.
- The manager account (`login_customer_id`) is found for you.
- Not available through Google's API: Auction Insights (use impression share, and send the user to Google Ads for competitor names) and creating or changing Video campaigns (YouTube ads are built as Demand Gen).

## Which skill does the job

| Job | Skill |
|---|---|
| Audit the account | `adplug:account-audit`, with `references/audit.md` |
| Search terms, negatives, placements, Performance Max waste | `adplug:cut-wasted-spend` |
| Budget headroom, new keywords, missing campaign types | `adplug:find-opportunities` |
| New account foundations or campaigns | `adplug:build-campaigns`, with `references/build.md` |
| Responsive search ads, assets, asset groups | `adplug:ad-copy` |
| Weekly and monthly results, pacing | `adplug:performance-review` |
| A drop or a campaign that stopped | `adplug:diagnose` |
| Which keywords create pipeline, offline conversions | `adplug:lead-quality` |
