# Google Ads audit checklist

For each check: what to look at, then the tool. Field names for `google_ads_query` come from `google_ads_reporting_view_doc`.

## 1. Goals and measurement
- `google_ads_conversion_performance` for conversions and value per action.
- Auto-tagging on (`google_ads_query` on `customer.auto_tagging_enabled`; `update_customer` turns it on). Without it there's no GCLID, so offline conversions can't be sent.
- `google_ads_query` on `conversion_action` for status, category, primary or secondary (`primary_for_goal`), counting (one or every), attribution and click window; segment by `segments.conversion_action` and `segments.date` to see an action go quiet.
- Campaign goals: campaigns optimising for account goals versus their own (`set_campaign_conversion_goal` fixes a mismatch).
- Compare Conversions with All conversions: a wide gap means bidding ignores most of what's recorded.
- Offline imports and enhanced conversions for leads: `google_ads_query` on `offline_conversion_upload_client_summary` shows the last upload and its success rate. An import that stopped makes a lead-gen account look like it stopped converting.
- Attribution model and click windows against the sales cycle.

## 2. Structure
- Campaigns organised by intent, product or service, funnel stage or market, each with a clear job from the brief. `google_ads_campaign_performance` (campaign level, with type and bid strategy).
- Brand, competitor and non-brand in separate campaigns. Brand terms showing up in non-brand campaigns (and the reverse) in `google_ads_search_terms`.
- Overlap: the same search term served by two campaigns or ad groups. Negate it in the weaker one.
- Brand sharing a budget or a portfolio bid strategy with non-brand: it soaks up the shared pool.
- Campaigns Google moved to AI Max (from Dynamic Search Ads, automatically created assets or campaign-level broad match): what they now match and rewrite.
- Performance Max next to Search: is PMax taking brand traffic (brand exclusions set?), and is it fed the right conversion goal?
- Ad groups tightly themed: a handful of related keywords, one landing page, ads that speak to them.

## 3. Settings
`google_ads_query` on `campaign` and `campaign_criterion`:
- Location option "presence" (people in or regularly in) rather than "presence or interest", unless the brief wants interest.
- Search partners and Display expansion on Search campaigns, and whether they earn their keep (segment by `segments.ad_network_type`).
- Ad schedule against when conversions happen, device bid adjustments against device results. Language targeting no longer applies to Search since September 2026; old language criteria still show in queries.
- Enabled campaigns with no location targeting (they serve worldwide).
- Auto-applied recommendations switched on without the client knowing (`google_ads_query` on `recommendation_subscription`, and `change_event` for what they changed).
- AI Max on Search campaigns (`campaign.ai_max_setting.enable_ai_max`), final URL expansion and automatically created assets (`campaign.asset_automation_settings`). AdPlug reads these; the user changes them in Google Ads.

## Performance Max
- Search terms it buys, with cost and conversions (`google_ads_query` on `campaign_search_term_view`; leave keyword segments out or its rows drop), against the brief's themes and never-buy list. `campaign_search_term_insight` groups them into categories.
- Where it shows (`performance_max_placement_view`, impressions only) and brand exclusions when a brand Search campaign exists.
- Asset groups: ad strength and assets per slot (`asset_group`, `asset_group_asset`), listing groups for ecommerce.
- Conversion goal: Performance Max optimises toward whatever it's given, so it must be the brief's real goal.
- For ecommerce, new against returning customers (`segments.new_versus_returning_customers`, conversion metrics only). If it's empty while there are conversions, say it isn't measured, not that it's zero.
- Assets per asset group: `primary_status` and each asset's own results (`asset_group_asset`), and slots left empty (no video, one image ratio).

## 4. Keywords and search terms
- Match types: phrase and exact are the default. Flag broad match running before phrase and exact ran out of volume, or without Smart Bidding, solid tracking and working negatives.
- High-spend keywords with few or no conversions, judged against the target (`google_ads_campaign_performance` level keyword).
- Quality Score and its parts: expected CTR, ad relevance, landing page experience (`google_ads_query` on `keyword_view`).
- Search terms: wasted spend, wrong intent, competitor names, and converting terms that aren't keywords yet (`google_ads_search_terms`).
- Negatives: shared lists and where they're attached (`google_ads_shared_set_criteria`), campaign and ad group negatives, and conflicts where a negative blocks a keyword the client wants.
- Coverage: themes in the brief with no keywords. Hand to `adplug:find-opportunities`.

## 5. Ads and assets
- Responsive search ads per ad group, ad strength, and each asset's own results (`google_ads_query` on `ad_group_ad` and `ad_group_ad_asset_view`). Google no longer returns performance labels for every asset type, so judge on the metrics.
- Pins and keyword insertion: pins outside brand ad groups (other than a required legal line), brand ad groups without the brand pinned to headline 1, generic ad groups with no keyword insertion headline, and keyword insertion in competitor ad groups.
- Stale copy: past years, ended offers and out-of-season words in ads and assets.
- Disapproved or limited ads and assets, including "destination not working" (`policy_summary` fields).
- Copy that speaks to the personas, matches the keyword and the landing page, and carries the brief's offer and proof.
- Sitelinks, callouts, structured snippets, images, business name and logo, call and lead form assets where they fit.

## 6. Bidding and budget
- Bid strategy against conversion volume: target CPA or ROAS without enough conversions to steer by struggles; manual CPC on a mature account leaves money on the table.
- Targets against the brief: a target CPA far below what the campaign achieves throttles it.
- `lost_is_budget` on campaigns under target: the clearest growth lever. `lost_is_rank`: relevance or bid.
- Spend heavy with few conversions: name it, with the window.
- Portfolio strategies (`google_ads_campaign_performance` level bidding_strategy) and shared budgets (`google_ads_query` on `campaign_budget`): which campaigns share them.

## 7. Audiences
- Remarketing lists and Customer Match in use and big enough (`google_ads_query` on `user_list`).
- Observation versus targeting: targeting by mistake narrows reach.
- Existing customers excluded from acquisition campaigns where the brief wants new customers.

## 8. Landing pages
- `google_ads_query` on `landing_page_view` for the pages that get the spend, then open each: loads, mobile speed, message match, form or checkout works.

## 9. Performance
- `google_ads_account_performance` with `compare=true`, `google_ads_campaign_performance`, `google_ads_daily_trends`.
- Spend on zero-conversion search terms and keywords, as an amount and a share of spend.
- Concentration: how much of the results depends on a few keywords or one campaign.
- Device and location splits where results differ a lot.

## Not available through the API
Auction Insights (use impression share instead and send the user to Google Ads for competitor names); creating or changing Video campaigns.
