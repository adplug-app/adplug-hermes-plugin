# Meta Ads audit checklist

Judge everything live by `effective_status`, not `status`: an active ad under a paused campaign isn't running. Budgets are daily or lifetime, on the ad set or (with Advantage+ campaign budget) the campaign.

## 1. Goals and measurement
- `meta_ads_list` pixels (last fired time), datasets and custom_conversions.
- `meta_ads_performance` with `conversion_events` to see the event each ad set optimises for, day by day.
- Ad sets optimising for an event the pixel rarely records will struggle; say so.
- Conversions API events next to the pixel's, for clients who send server-side events.
- Attribution: 7-day click and 1-day view by default. Meta stopped the 7-day view and 28-day view windows in January 2026.
- Each ad set's attribution setting (`meta_ads_list` adsets with `fields` attribution_spec): ad sets on different settings aren't comparable, and one that counts views claims more than one that counts clicks only. Say which setting each result uses.

## 2. Structure
- `meta_ads_list` campaigns and adsets: objective per campaign matches its job; prospecting and retargeting apart; not so many ad sets that each one starves.
- Ad sets competing for the same people with the same goal.
- Learning: ad sets that never leave the learning phase (`learning_stage_info` in `fields`) usually have too little budget for their event, or too many ad sets split it.

## 3. Delivery
- `meta_ads_diagnose` for the account: billing, spend cap, ads blocked by parents, review rejections. Focus on ads that should be running.
- Account status and spend cap from `meta_ads_list_accounts`.

## 4. Audiences and targeting
- Ad set targeting (`meta_ads_list` adsets with `fields` targeting): Advantage+ audience on or off, interests, lookalikes, custom audiences, exclusions (existing customers, recent converters) against the brief's ICP.
- Audience size estimates for narrow ad sets (`meta_ads_targeting_search` with a targeting spec).
- `meta_ads_performance` report_type demographic, country, placement, device: where the money goes and what converts. Read these as context, not cuts: Meta moves spend to where the next result is cheapest, so a segment's average cost per result misleads (the breakdown effect). Under Advantage+ audience, age and gender are weak levers.

## 5. Creative
- `meta_ads_performance` report_type ad or creative: results, cost per result, CTR and frequency per ad.
- Fatigue: frequency up, CTR down, cost per result up over recent weeks.
- Format mix and placements: vertical video for Reels and Stories, square or vertical images for feeds.
- Advantage+ creative enhancements switched on (`meta_ads_list` creatives with `fields` enhancements): what Meta may change once the ad runs.
- Instagram account on the creatives where Instagram placements are wanted.

## 6. Budget and bidding
- Budget per ad set against the cost of the optimisation event: too little budget to get enough events keeps an ad set learning.
- Bid strategies (lowest cost, cost cap, bid cap) and whether caps throttle delivery.
- Spend concentrated on ad sets above the target cost per result.

## 7. Landing pages
Open the destination of every live ad: loads, fast on mobile, matches the ad. A big gap between link clicks and landing page views points at a slow or broken page.

## 8. Lead forms and catalogues
- Lead forms (`meta_ads_list` lead_forms with the Page as parent): questions, qualifying fields, Lead Ads terms accepted on the Page.
- Catalogues and product sets for ecommerce (`catalog_diagnostics`).

## 9. Performance
- `meta_ads_performance` report_type campaign, adset, daily against the previous period; cost per result and ROAS against the brief's target.
- `meta_ads_list` activity_logs for changes behind any jump.
