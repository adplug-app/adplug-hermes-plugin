# Google Ads tools in AdPlug

Tool names as the AdPlug server lists them; in Hermes they carry the server's prefix. Call the preview tool with `operation="help"` for the live list of changes and `params={"operation": NAME}` for one operation's fields.

Reading:
- `google_ads_list_accounts`: every account with name, currency, manager (MCC) tree. The MCC parent (`login_customer_id`) is worked out for you.
- `google_ads_account_performance`: several accounts in one call; `compare=true` adds the previous period. Manager accounts are skipped.
- `google_ads_campaign_performance`: `level` = campaign (impression share, lost to budget, lost to rank, bid strategy, targets), ad, keyword (Quality Score, first page and top of page bids), product, product_group, bidding_strategy. `campaign_id` narrows it. Paginate until `next_page_token` is null.
- `google_ads_daily_trends`: day by day, account or one campaign.
- `google_ads_conversion_performance`: conversions and value per conversion action.
- `google_ads_search_terms`: sorted by cost, with the match type that triggered each term. `dynamic_search_ads=true` gives Dynamic Search Ads terms with Google's headline and landing page.
- `google_ads_shared_set_criteria`: what's inside shared negative keyword lists.
- `google_ads_keyword_ideas`: Keyword Planner. `mode="ideas"` from seed keywords or a URL; `mode="historical"` for exact keywords; `monthly=true` for month by month searches; `geo_targets` and `language` set the market; without them it answers for the United States in English, so always pass the brief's markets.
- `google_ads_recommendations`: Google's own suggestions with Google's impact estimate. Present them as Google Ads Recommendations, not your advice.
- `google_ads_query`: any GAQL query (policy status and disapprovals, landing pages, change history, network settings, location options, assets, audiences). Call `google_ads_gaql_doc` for syntax and `google_ads_reporting_view_doc` for a view's fields first. `compact=true` for long pulls.

Changing:
- `google_ads_preview_mutation` then `google_ads_execute_mutation` (needs the `preview_id`). `operation="help"` lists every operation; `params={"category": ...}` narrows it to entity, targeting, asset, asset_group, asset_set, settings, shared_set, negatives, conversion, conversion_upload, value_rule, bidding_strategy, experiment, label, audience, user_list, demand_gen, shopping, dsa, keyword_plan or batch_job. `operation="batch"` runs up to 100 steps in one preview, and later steps can use earlier results (`"$0.asset_id"`).
- `google_ads_preview_add_negatives` then `google_ads_execute_add_negatives`: negatives on a campaign, an ad group or a shared list.
- Not available through the Google Ads API: Auction Insights, and creating or changing Video campaigns. YouTube video ads are built as Demand Gen.
