# LinkedIn Ads tools in AdPlug

Tool names as the AdPlug server lists them; in Hermes they carry the server's prefix. Call the preview tool with `operation="help"` for the live list of changes and `params={"operation": NAME}` for one operation's fields.

Naming: LinkedIn's "Campaigns" are `campaign_groups`, "Ad sets" are `campaigns`, "Ads" are `creatives`. When a user says campaigns, they usually mean campaign groups.

Reading:
- `linkedin_ads_list_accounts`
- `linkedin_ads_performance`: `report_type` = campaign, creative, daily, conversion, lead_gen, demographics (with `demographic_type` MEMBER_JOB_TITLE, MEMBER_SENIORITY, MEMBER_JOB_FUNCTION, MEMBER_INDUSTRY, MEMBER_COMPANY_SIZE, MEMBER_COMPANY, MEMBER_COUNTRY_V2, MEMBER_REGION_V2) or reach (reach and frequency, up to 92 days).
- LinkedIn has no change history through AdPlug: read campaigns' status, budgets and end dates instead.
- `linkedin_ads_list`: `entity_type` = campaign_groups, campaigns, creatives, conversions, campaign_conversions, lead_forms, saved_audiences (matched audiences), targeting_audiences, targeting_facets.
- `linkedin_ads_get`: one campaign, campaign group, creative, targeting audience, saved audience, or lead form responses.
- `linkedin_ads_targeting_lookup`: `action` = search (job titles, companies, skills, industries), resolve_urns, audience_count (audience size before launch).
- `linkedin_ad_library_search`: any advertiser's LinkedIn ads (copy and images sit behind each `ad_url`).

Changing:
- `linkedin_ads_preview_mutation` then `linkedin_ads_execute_mutation` with the same operation and params. Campaign groups, campaigns, creatives, inline ads (`create_inline_ad`: single image, video, document, carousel, text, spotlight, message, event, Thought Leader Ads), lead forms, targeting audiences, conversion rules, matched audiences, asset uploads.
- `linkedin_ads_send_conversion_event`: Conversions API events. No preview: show what will be sent first.
