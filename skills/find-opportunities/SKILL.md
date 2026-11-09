---
name: find-opportunities
description: Find what a client's Google Ads, Microsoft Ads, LinkedIn Ads, Meta Ads and Reddit Ads accounts are missing, measured against the client's goal and ideal customer. Budget headroom on campaigns that hit target, converting search terms that aren't keywords, keyword themes nobody covers, missing campaign types, unused audiences, markets and competitor angles, each sized and ranked. Use for "where can we grow", "what are we missing", "more leads at the same CPA", "new keywords", "what campaigns should we add", "scale this account".
license: MIT
metadata:
  author: AdPlug
  version: "1.0.0"
---

# Find what's missing

Read-only until the user picks what to build. Follow `adplug:ppc-agent`, and read the client brief first: an opportunity is only worth it if it serves the goal, reaches the ideal customer and fits the budget. Fix broken tracking and obvious waste before chasing growth (`adplug:account-audit`, `adplug:cut-wasted-spend`).

## Where to look, roughly in order of value

1. **Winners held back by budget.** Campaigns at or under the target cost that lose impression share to budget (Google `lost_is_budget`, Microsoft share_of_voice), or LinkedIn, Meta and Reddit campaigns under target that spend their full budget every day. The cheapest growth there is. Show the extra daily and monthly cost and the conversions it might bring, labelled as an estimate. Extra spend usually converts at a higher cost than the average, so use Google's own estimate where there is one (`google_ads_recommendations` type CAMPAIGN_BUDGET, or `google_ads_query` on `campaign_simulation`).
2. **Converting search terms that aren't keywords.** Google `google_ads_search_terms`, Microsoft `microsoft_ads_performance` search_query. Terms that converted at or under target, that fit the ICP, with no keyword of their own: add them as exact or phrase in the ad group whose ads and landing page fit them. Drop accidental or wrong-product conversions.
3. **Keyword themes nobody covers.** Take the brief's core themes, the personas' problems in their own words, the client's services and product pages, and check them against the live keywords. Research the gaps: `google_ads_keyword_ideas` from seed keywords and from the client's URLs (`monthly=true` to see seasonality), `microsoft_ads_targeting_lookup` keyword_ideas and keywords_for_url. Keep terms with real volume and buying intent for the ICP; drop job seekers, students, DIY and informational searches the brief excludes. Cluster into ad groups and show volume, competition and bid ranges per cluster.
4. **Campaign types that fit the business but don't exist.** Check against the business type in the brief:
   - Brand campaign, if the brand is searched and competitors bid on it.
   - Competitor campaign, if the brief allows it.
   - Remarketing: search audiences, Demand Gen, Meta, LinkedIn and Reddit retargeting of site visitors and lead form openers.
   - Ecommerce: Shopping or Performance Max with a product feed, product sets on Meta.
   - B2B: LinkedIn lead gen to the ICP's job titles, company lists for target accounts, Thought Leader Ads from the founder or experts once they approve sponsorship, Microsoft LinkedIn profile targeting on search.
   - Microsoft Ads at all, if the client runs Google Search without it: `migrate_google_campaigns` is a fast start.
   - Reddit, where the ICP discusses the problem: `reddit_ads_targeting_lookup` suggest_communities from the client's website and keyword_suggestions.
   - Searches the keywords miss: first the converting search terms above, added as phrase or exact. AI Max for Search matches beyond the keywords the way broad match does, so treat it as the same last stage: only where phrase and exact have run out of volume, and tested as an experiment (`create_experiment` with `ADOPT_AI_MAX`) rather than switched on blind. Google is replacing Dynamic Search Ads with AI Max (its switch-off begins February 2027), so don't build new Google DSA. Microsoft Dynamic Search Ads still fit a site with good page structure.
5. **Audiences going unused.** Customer lists not uploaded, lookalikes not built, in-market segments that match the ICP (`microsoft_ads_list` audiences with `audience_type="InMarket"`), LinkedIn matched audiences and company lists, Meta website audiences.
6. **Markets, devices and hours.** Locations, devices and times of day converting under target with low impression share or spend. Languages and regions the brief wants that aren't targeted.
7. **Google's own suggestions.** `google_ads_recommendations`, filtered to the ones that fit the brief. Present them as Google Ads Recommendations, with your view next to each; Google's suggestions often favour more spend.
8. **Competitor angles.** What competitors run: `linkedin_ad_library_search`, `meta_ad_library_search` (commercial ads only where they reached the EU or UK). Both return when each ad started: the ones that have run longest are the ones paying off, and new ones are tests. Group what lasts into angles and offers, then look for the angles nobody uses and the claims the client can beat with proof. Competitor overlap on search isn't available through AdPlug on Google or Microsoft: use impression share lost to rank, and send the user to each platform's Auction Insights for names.

If AdPlug's Search Console connector is added (`hermes mcp add adplug-search-console --url https://api.adplug.app/mcp/google-search-console --auth oauth --connect-timeout 300`), compare paid search terms with organic rankings: converting paid terms with no organic page are content ideas, and organic winners with no paid cover may be worth a test. For lead generation with HubSpot connected, rank opportunities by the pipeline and revenue each source created (`adplug:lead-quality`), not just by platform conversions.

## Size and rank them

For each opportunity:
- What it is, and the evidence (numbers, date range, account).
- Fit with the brief: which persona and goal it serves.
- Size: search volume, audience size or the extra conversions at current costs, labelled as an estimate.
- Cost per day and per month, and whether it fits the budget or needs more.
- Effort and risk (new tracking needed, learning period, brand risk).

Rank by expected effect on the client's goal for the money and effort. Lead with the ones that matter most, then the rest in a table. For the client, make it a short PDF or slides.

## Then act

Offer the next step for each one the user picks: keywords and negatives through a preview, new campaigns through `adplug:build-campaigns`, new ads through `adplug:ad-copy`, a budget move with its own yes. Note what was agreed in the brief's Decisions.
