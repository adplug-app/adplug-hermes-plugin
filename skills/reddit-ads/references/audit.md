# Reddit Ads audit checklist

On Reddit, the community and keyword reports do the job search terms do on Google. Configured status is what the advertiser set; effective status is what Reddit actually does. Report both when they differ.

## 1. Goals and measurement
- `reddit_ads_list` pixels, and `reddit_ads_performance` report_type conversions.
- `reddit_ads_diagnose` flags ad groups optimising for an event the pixel hasn't received in the last 7 days.

## 2. Account and delivery
- `reddit_ads_diagnose` for the account: approval state, billing (servable funding instrument, card, credit), ads rejected and why, pixel events missing.
- `reddit_ads_list` funding_instruments for billing detail.

## 3. Structure
- `reddit_ads_list` campaigns and ad_groups: objective per campaign matches its job; ad groups split by targeting theme (communities, keywords, interests), not by tiny variations.
- Campaign budget optimisation versus ad group budgets, and whether budgets sit where the brief's priorities are.

## 4. Targeting
- Ad group targeting (`reddit_ads_get` ad group): communities, conversation keywords, interests, locations, devices, expanded targeting.
- Where ads delivered and what converted: `reddit_ads_performance` report_type community, keyword, interest, placement (feed versus comments page), device, country.
- Communities and keywords spending without conversions: exclusions (`update_ad_group` with `add_targeting` excluded_communities or excluded_keywords, or account-wide `update_account_exclusions`).
- Brand safety: account exclusions for communities and keywords the client must never appear next to.

## 5. Creative
- `reddit_ads_performance` report_type ad, carousel_card, video: CTR, cost per result, video quartiles.
- Posts that read like ads in communities that punish it; headlines that fit the community's language.
- Fatigue on long-running ads: frequency up, CTR down.

## 6. Bidding and budget
- Bids against Reddit's suggestion (`reddit_ads_targeting_lookup` bid_suggestion) and against the target cost.
- Ad groups not spending their budget: audience too small, bid too low, optimisation event with no data.

## 7. Landing pages
Open the destination of every live ad: loads, mobile, matches the post.

## 8. Performance
- `reddit_ads_performance` with `compare_previous_period=true` by campaign, ad group and daily.
- `reddit_ads_list` change_history behind any jump.
