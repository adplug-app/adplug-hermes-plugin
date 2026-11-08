# Microsoft Ads tools in AdPlug

Tool names as the AdPlug server lists them; in Hermes they carry the server's prefix. Call the preview tool with `operation="help"` for the live list of changes and `params={"operation": NAME}` for one operation's fields.

Customer is the manager account, account is the spend account. Most tools take `account_id`.

Reading:
- `microsoft_ads_list_accounts`
- `microsoft_ads_performance`: `report_type` = account, campaign, daily, ad_group, ad, keyword, search_query, geographic, user_location, placement, age_gender, professional_demographics (LinkedIn company, industry, job function), share_of_voice, audience, conversion, product_dimension, product_partition, video, asset_group, change_history. `compare_previous_period=true` adds the change.
- `microsoft_ads_list`: campaigns, ad_groups, ads, keywords, budgets, negative_keyword_lists and their items, account_negative_keywords, asset_groups, audiences (`audience_type="InMarket"` searches Microsoft's in-market segments), audience_signals, uet_tags, conversion_goals, ad_extensions, bid_strategies, campaign_criterions, ad_group_criterions, website_exclusions and exclusion lists, account_settings (auto-tagging, tracking, automated extensions, auto-apply), recommendations, labels, experiments, media, import_jobs and import_results, seasonality_adjustments, data_exclusions, conversion_value_rules.
- `microsoft_ads_get`: one campaign, ad group, ad or keyword.
- `microsoft_ads_targeting_lookup`: `action` = keyword_ideas, search_volume, traffic_estimate, bid_estimate, keywords_for_url, locations, profiles (LinkedIn company, industry, job function).
- `microsoft_ads_generate_ad_copy`: Microsoft's own AI drafts responsive search ad copy from a landing page. Creates nothing.
- `microsoft_ads_bulk_download`: whole-account export for big audits.

Changing:
- `microsoft_ads_preview_mutation` then `microsoft_ads_execute_mutation` with the same operation and params. `operation="help"` gives the catalogue by category. Includes campaigns, ad groups, ads, keywords, negatives and shared lists, budgets and bid strategies, targeting (locations, LinkedIn profiles, audiences, devices, schedule), extensions, UET tags and conversion goals, remarketing and customer lists, Performance Max, Dynamic Search Ads, editorial appeals, recommendations, experiments, labels, account settings, and `migrate_google_campaigns` (copies Google Ads campaigns into Microsoft).
- `microsoft_ads_bulk_upload`: no preview. Show the file's changes and get a yes first.
