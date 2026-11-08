---
name: microsoft-ads
description: Run Microsoft Advertising (Bing Ads) through AdPlug the way a senior PPC specialist would. Search on Bing, Yahoo and partners, the Microsoft Audience Network, Performance Max, Dynamic Search Ads, LinkedIn profile targeting and Google imports. What good looks like, what to check each day, week, month and quarter, and AdPlug's Microsoft tools, audit checklist and build rules. Use for anything on Microsoft Ads or Bing Ads, including search queries, UET tags, conversion goals, editorial rejections and copying Google campaigns across.
license: MIT
metadata:
  author: AdPlug
  version: "1.0.0"
---

# Microsoft Ads

Follow `adplug:ppc-agent` for approvals and reporting, and read the client brief first. This skill holds what's specific to Microsoft Advertising; the jobs themselves have their own skills, which load this one for the Microsoft details.

Load the references with `skill_view("adplug:microsoft-ads", "references/<file>")`:
- `references/tools.md`: every Microsoft tool, report type and list type.
- `references/audit.md`: the audit checklist, area by area.
- `references/build.md`: build orders and traps.

## Its job in the plan

More search volume on the same intent as Google, usually at a lower cost per click and with less volume, from an audience that skews toward desktop and work. For B2B it has something Google doesn't: LinkedIn profile targeting (company, industry, job function) on search ads. Judge it on its own numbers, never as a smaller copy of Google.

## What good looks like

**Measurement.** A UET tag that fires and conversion goals that match the brief, each with recent conversions. MSCLKID auto-tagging on (`microsoft_ads_list` account_settings), so offline conversions have a click ID to match. Offline conversions for lead generation sent back with the MSCLKID or a hashed email or phone (`upload_offline_conversions`, `adjust_offline_conversions`; see `adplug:lead-quality`). When tracking breaks for a few days, add a data exclusion (`create_data_exclusion`) so automated bidding ignores those days; before a short promotion, a seasonality adjustment (`create_seasonality_adjustment`) tells it to expect the change.

**Google imports under control.** Most Microsoft accounts started as a Google import. A scheduled import keeps overwriting Microsoft-specific bids, budgets, negatives and ads. Either stop it and manage Microsoft on its own, or keep it and know exactly what it overwrites (`microsoft_ads_list` import_jobs and import_results). What never comes across from Google: UET goals, LinkedIn profile targeting and Audience Network choices.

**Structure.** The same principles as Google: brand apart from non-brand, campaigns with a clear job, ad groups themed by intent, no two campaigns competing for the same searches. Keep it consolidated: Microsoft volume is lower, so thin structures starve automated bidding sooner than on Google.

**Bidding.** Match the strategy to conversion volume, as on Google, and remember volume is lower: a campaign that has enough conversions for a CPA target on Google may not have enough here. Portfolio strategies help pool data across small campaigns.

**Keywords and search queries.** Read the search query report on a rhythm. Microsoft negatives are exact or phrase only, so a word-level block needs phrase. Account-level negatives (`add_account_negative_keywords`) for junk that applies everywhere.

**Where the ads show.** Bing, Yahoo, AOL and syndicated search partners, plus the Microsoft Audience Network for native ads. The placement report shows which sites took the money. Partners and the Audience Network earn their place on some accounts and drain others: decide per campaign from the results. A new website exclusion list replaces the old one, so read the current list first and keep every entry.

**LinkedIn profile targeting.** Company, industry and job function from the ICP (`microsoft_ads_targeting_lookup` action profiles). Start as a bid adjustment and compare before narrowing, and read `professional_demographics` to see which professions convert.

**Ads and extensions.** Responsive search ads per ad group; sitelinks, callouts, snippets and image extensions. Editing an ad sends it back to editorial review, and Microsoft's editorial review is stricter than Google's on trademarks, claims and some industries. `appeal_editorial_rejections` asks for an exception. Automated extensions and auto-applied recommendations (`set_automated_extensions`, `set_auto_apply_recommendations`) change the account unreviewed: switch them off unless the client wants them.

## The rhythm

What a senior specialist checks on Microsoft Ads. These checks fold into the client's routines (`adplug:routines`); don't create a scheduled job per check.

- **Every day** (the morning check): spend and conversions per campaign against normal, UET goals still recording, editorial rejections, campaigns that stopped serving.
- **Every week:** search queries and negatives, the placement report for partners and the Audience Network, pacing, and the last import's results if a scheduled Google import runs.
- **Every month:** impression share lost to budget and rank on the campaigns that hit target (`share_of_voice`, per campaign), professional demographics against the ICP, profile bid adjustments, Microsoft's recommendations worth taking, automated extensions and auto-apply still off where they should be.
- **Every quarter:** structure against Google and the brief (is Microsoft still a stale copy?), keyword research from the client's pages (`keywords_for_url`), and an experiment (`create_experiment`) for the biggest open question.

## In AdPlug

- Customer is the manager account; account is the spend account. Most tools take `account_id`.
- Every change: `microsoft_ads_preview_mutation`, then `microsoft_ads_execute_mutation` with the same operation and params. `operation="help"` gives the catalogue by category. `operation="batch"` runs several steps in one go.
- `microsoft_ads_bulk_upload` has no preview: show exactly what the file changes and get a yes first.
- New campaigns, ad groups, ads and keywords start paused, including keywords and ads added to a live ad group, which don't serve until enabled: when that's the intent, include enabling them in the same yes. Money is plain currency.
- The conversion report gives one total per goal with no dates, and the share of voice report is per campaign with no competitor names.
- `migrate_google_campaigns` copies Google Ads Search campaigns across (campaigns paused; ad groups, keywords and ads keep their Google status). `microsoft_ads_generate_ad_copy` drafts ad copy from a landing page and creates nothing.

## Which skill does the job

| Job | Skill |
|---|---|
| Audit the account | `adplug:account-audit`, with `references/audit.md` |
| Search queries, negatives, partner and Audience Network sites | `adplug:cut-wasted-spend` |
| Budget headroom, new keywords, LinkedIn profile targeting | `adplug:find-opportunities` |
| Copy Google across, new campaigns | `adplug:build-campaigns`, with `references/build.md` |
| Responsive search ads and extensions | `adplug:ad-copy` |
| Weekly and monthly results, pacing | `adplug:performance-review` |
| A drop or a campaign that stopped | `adplug:diagnose` |
| Offline conversions and lead quality | `adplug:lead-quality` |
