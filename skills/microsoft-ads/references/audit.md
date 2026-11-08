# Microsoft Ads audit checklist

Most Microsoft accounts are a Google import nobody has audited since. Check what only Microsoft needs: import drift, partner traffic and LinkedIn profile targeting. For a big account, `microsoft_ads_bulk_download` gives the whole structure in one go.

## 1. Goals and measurement
- MSCLKID auto-tagging on (`microsoft_ads_list` account_settings; `update_account_settings`). Without it, offline conversions have no click ID to match.
- `microsoft_ads_list` uet_tags (tag status) and conversion_goals (goal status, type, window).
- `microsoft_ads_performance` report_type conversion for conversions and assists per goal. It gives one total per goal, so run it for recent days and for a normal period and compare, to spot a goal gone quiet.
- An import from Google can bring campaigns without the UET goals they need.
- Offline conversions for long sales cycles: `upload_offline_conversions` sends them with the Microsoft click ID.

## 2. Google Ads import health
- `microsoft_ads_list` import_jobs and import_results: is a scheduled import still running, and what did the last runs change?
- A scheduled import can overwrite Microsoft-specific bids, budgets, negatives and ads. Flag work it would undo.
- What didn't come across and needs a Microsoft setup: UET goals, LinkedIn profile targeting, Audience Network choices.

## 3. Structure
Same as Google: campaigns with a clear job, brand apart from non-brand, no two campaigns competing for the same searches.

## 4. Targeting and settings
- Location intent (people in, versus people searching for), languages, devices, schedule (`microsoft_ads_list` campaign_criterions).
- Where ads run: Bing, AOL and Yahoo only, or search partners too, and the Audience Network. `microsoft_ads_performance` report_type placement shows the sites that took the money.
- LinkedIn profile targeting (company, industry, job function) and its bid adjustments, against `microsoft_ads_performance` report_type professional_demographics.
- Device and schedule adjustments that no longer match results.

## 5. Keywords and search queries
- `microsoft_ads_performance` report_type keyword (Quality Score, spend, conversions) and search_query.
- Negatives at account (`account_negative_keywords`), campaign, ad group and shared-list level, and negatives blocking keywords. Microsoft negatives are exact or phrase only.
- Keyword cannibalisation and broad match exposure.

## 6. Ads and assets
- Responsive search ads per ad group, editorial rejections (`appeal_editorial_rejections` asks for an exception), disapprovals.
- Sitelinks, callouts, snippets, call, location, price, promotion and image extensions (`microsoft_ads_list` ad_extensions).
- Automated extensions and auto-applied recommendations changing the account unreviewed (`microsoft_ads_list` account_settings, recommendations).

## 7. Bidding and budget
- Bid strategy against conversion volume; shared budgets and portfolio strategies (`microsoft_ads_list` budgets, bid_strategies).
- Impression share and the share lost to budget or rank, per campaign (`microsoft_ads_performance` report_type share_of_voice).
- Spend heavy with few conversions.

## 8. Audiences
- Remarketing, customer lists, in-market audiences (`microsoft_ads_list` audiences, `audience_type="InMarket"` to search); "bid only" versus "target and bid".

## 9. Landing pages
Same checks as Google: open the final URLs that get the spend.

## 10. Performance
- `microsoft_ads_performance` with `compare_previous_period=true`.
- Where Google runs alongside, compare CPC, CPA and conversion rate by campaign theme: Microsoft often converts at a lower cost on less volume.
