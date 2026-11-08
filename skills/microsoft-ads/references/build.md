# Building on Microsoft Ads

Check the live spec first: the preview tool with `operation="help"`. These are the orders and the traps that go wrong most. Copy limits are in `adplug:ad-copy` (`references/limits.md`).

- Quickest start: `migrate_google_campaigns`. It copies Search campaigns only (Performance Max, Shopping, Display, Video, Demand Gen and App are skipped), creates campaigns paused while ad groups, keywords and ads keep their Google status, and copies Google's budgets and bids: pass `currency_rate` when the accounts use different currencies, and set budgets from the brief. Then add what never comes across: the UET tag and conversion goals, LinkedIn profile targeting, Audience Network choices. Check editorial rejections after the first review.
- B2B: LinkedIn profile targeting by company, industry and job function (`microsoft_ads_targeting_lookup` action profiles, then `add_profile_targeting`). Start it as a bid adjustment and compare before narrowing.
- Microsoft's own copy draft: `microsoft_ads_generate_ad_copy` from a landing page, then edit it with `adplug:ad-copy`.
- Editorial reviews can reject ads and keywords; `appeal_editorial_rejections` asks for an exception.
