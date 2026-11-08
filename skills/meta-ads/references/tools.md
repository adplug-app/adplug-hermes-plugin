# Meta Ads tools in AdPlug

Tool names as the AdPlug server lists them; in Hermes they carry the server's prefix. Call the preview tool with `operation="help"` for the live list of changes and `params={"operation": NAME}` for one operation's fields.

Structure: campaign, ad set, ad, creative. Budgets, targeting, schedule and optimisation live on the ad set unless the campaign uses Advantage+ campaign budget. Budgets are daily or lifetime; there's no monthly budget.

Reading:
- `meta_ads_list_accounts`: ad accounts, Facebook Pages and linked Instagram accounts in one call (a new ad needs all three).
- `meta_ads_performance`: `report_type` = account, campaign, adset, ad, creative, daily, daily_account, demographic, placement, device, country. `date_preset` or `start_date`/`end_date`. Default attribution is 7-day click and 1-day view. Meta stopped 7d_view and 28d_view in January 2026; AdPlug reports 1d_view, 1d_click and 7d_click. `conversion_events` picks events by name.
- `meta_ads_list`: campaigns, adsets, ads, creatives, custom_audiences, saved_audiences, lead_forms, posts, catalogs, product_sets, products, pixels, datasets, custom_conversions, rules, recommendations, activity_logs (change history), instagram_accounts, catalog_diagnostics, images, videos. Use `effective_status`, not `status`, to judge what's live.
- `meta_ads_get`: one entity in full.
- `meta_ads_diagnose`: why ads aren't delivering (account, billing, parents, review). It counts paused ads as blocked, so look for ads that should be running.
- `meta_ads_targeting_search`: interests, behaviours, job titles, employers, geo, placements; audience size for a targeting spec.
- `meta_ad_library_search`: competitors' ads. Commercial ads only show where they reached the EU or the UK.

Changing:
- `meta_ads_preview_mutation` (for campaigns, ad sets and ads Meta validates the change without making it; updates come with a field by field diff) then `meta_ads_execute_mutation`. Money in minor units (5000 = 50.00). Everything created is paused. A creative can't be edited: make a new one and swap it onto the same ad with `update_ad`. Objective and buying type are fixed at creation.
