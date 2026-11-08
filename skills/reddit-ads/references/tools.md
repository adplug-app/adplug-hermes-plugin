# Reddit Ads tools in AdPlug

Tool names as the AdPlug server lists them; in Hermes they carry the server's prefix. Call the preview tool with `operation="help"` for the live list of changes and `params={"operation": NAME}` for one operation's fields.

Structure: business, ad account, campaign, ad group, ad. An ad promotes a post. Objectives sit on the campaign; budget, bid, schedule and targeting on the ad group (unless campaign budget optimisation is on).

Reading:
- `reddit_ads_list_accounts`: businesses, ad accounts, profiles and pixels.
- `reddit_ads_performance`: `report_type` = account, campaign, ad_group, ad, daily, daily_campaign, hourly, community, keyword, interest, country, region, metro, placement, device, gender, language, carousel_card, video, conversions. `compare_previous_period=true` adds the change.
- `reddit_ads_list` and `reddit_ads_get`: campaigns, ad groups (full targeting), ads (rejection reasons, preview links), posts, custom_audiences and saved_audiences, pixels, funding_instruments (billing), lead_gen_forms, product_catalogs, change_history.
- `reddit_ads_diagnose`: why ads aren't spending or conversions stopped (account, billing, status, pixel events).
- `reddit_ads_targeting_lookup`: search_communities, suggest_communities (from keywords or a website), keyword_suggestions, validate_keywords, interests, geolocations, audience_estimate, bid_suggestion.

Changing:
- `reddit_ads_preview_mutation` then `reddit_ads_execute_mutation` with the same operation and params. Plain currency. Created paused unless status is ACTIVE. `operation="batch"` builds campaign, ad group, post and ad in one go. A published post can't be edited: make a new post and point the ad at it.
