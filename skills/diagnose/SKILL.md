---
name: diagnose
description: Work out why a Google Ads, LinkedIn Ads, Meta Ads, Microsoft Ads or Reddit Ads campaign, ad set, ad group or ad stopped spending, stopped converting, dropped suddenly or never took off, and say what to do next. Use for "why isn't this running", "why did leads drop", "why is CPA up", "why is it not spending". Read-only; proposes fixes for approval.
license: MIT
metadata:
  author: AdPlug
  version: "1.0.0"
---

# Diagnose a drop or a stall

Read-only. Find the cause, show the evidence, and hand over the fix as a proposal. Follow `adplug:ppc-agent` and read the client brief first: a drop only matters against the goal.

## 1. Pin down what moved

- Which platform, account and entity, and which metric: spend, impressions, clicks, conversions, cost per conversion, conversion value.
- Compare the problem period with a normal one of the same length. Use the daily view to find the day it changed (`google_ads_daily_trends`, `linkedin_ads_performance` daily, `microsoft_ads_performance` daily, `meta_ads_performance` daily, `reddit_ads_performance` daily). Today is partial, and recent days undercount conversions that haven't arrived yet (longer for B2B and long click windows).
- Is it one campaign or the whole account? Account-wide drops usually mean tracking, billing, a policy problem or the market. One campaign usually means a change, a budget, a bid or a creative.

## 2. Check the boring causes first

Most drops are one of these. Check them before blaming the algorithm.

- **Someone changed something.** Google change history (`google_ads_query` on `change_event`, last 30 days only, with a LIMIT), Microsoft `change_history`, Meta `activity_logs`, Reddit `change_history`, plus the client's change log in the brief. A budget cut, a bid strategy switch, a new conversion goal or a paused ad group lines up with the day things moved.
- **Tracking went quiet.** A conversion action that normally records conversions recorded none. Check per action and per day. If clicks held steady and conversions fell to zero overnight, suspect the tag before the campaign.
- **Billing or account status.** Card declined, prepaid funds used up, account suspended. `meta_ads_diagnose` and `reddit_ads_diagnose` check this; on Google, `campaign.primary_status` and `campaign.primary_status_reasons` (`google_ads_query`) say why a campaign is limited or not serving; on Microsoft, read the campaign's status.
- **Disapprovals.** Ads, keywords or assets disapproved or limited, including "destination not working". Google policy summary via `google_ads_query`; Microsoft editorial status; LinkedIn creative review status; Meta `meta_ads_diagnose`; Reddit `rejection_reason`.
- **The landing page.** Open the final URLs with your browser or web fetch. A 404, a broken form or a slow page kills conversions while clicks carry on.
- **Seasonality and the market.** Same period last year, a holiday, a competitor's launch. Say so and move on.

## 3. Walk the tree

**Not spending or not delivering**
- Limited by budget: a large share of impression share lost to budget (Google `lost_is_budget`). The campaign is capped. Raise the budget or free spend with negatives.
- Losing on rank: impression share lost to rank, low Quality Score, bids below first page. Improve relevance and ad strength, or revisit the bid target.
- Bid target too tight: a target CPA or ROAS far stricter than what the campaign achieves. The campaign throttles itself.
- Audience too small: LinkedIn `audience_count`, Meta reach estimate, Reddit `audience_estimate`. Widen targeting or loosen exclusions.
- Meta: an ad that should be running but isn't. `meta_ads_diagnose` reads effective status through the parents; paused ads show as blocked, so focus on the ones the user expects live.
- Reddit: an ad group optimising for a pixel event that hasn't fired lately can barely deliver. `reddit_ads_diagnose` flags it.
- New or just changed: still learning. Say so, and say when it's fair to judge.

**Spending, but few clicks (low CTR)**
- Creative fatigue on LinkedIn, Meta and Reddit: frequency rising while CTR falls (`linkedin_ads_performance` reach, `meta_ads_performance`). Refresh the creative (`adplug:ad-copy`).
- Search ads: weak or repetitive RSA assets, poor ad strength, ads that don't mention what the keyword asks for.
- Brand CTR falling on search: a competitor may be bidding on the brand. Check the brand campaign's impression share and top-of-page rate.
- Wrong audience: the targeting reaches people outside the ICP in the brief.

**Clicks, but no conversions**
- Tracking first (step 2).
- The landing page doesn't match the ad's promise, or the form is long, broken or slow on mobile. Compare device performance.
- Search terms drifted: broad match or close variants buying the wrong intent. Hand over to `adplug:cut-wasted-spend`.
- LinkedIn and Microsoft: leads coming from the wrong job titles or companies (demographics and professional_demographics reports).

**Converting on the platform, but sales says the leads are bad**
- Lead quality is a CRM question: trace the bad leads to the campaign, keyword, form or audience that brings them (`adplug:lead-quality`).
- A platform optimising for raw form fills finds the people most likely to fill forms, not to buy. Send qualified leads back so it learns the difference.

**Converting, but not enough volume**
- Capped by budget while under target: the clearest case for more budget. Show the daily and monthly cost.
- Audience saturated: high frequency on a small audience. Expand with lookalikes, matched audiences or new search themes (`adplug:find-opportunities`).

## 4. Report

Lead with the most likely cause and the evidence for it, then the other candidates still in play, each with:
- what you saw (numbers, dates, account),
- how sure you are, and what would confirm it,
- the fix, as a proposal: which skill or which preview to run.

End with what you couldn't check. If the entity is still learning or changed in the last few days, say "leave it for now" and when to look again. Offer to set a check for that date (`adplug:routines`).
