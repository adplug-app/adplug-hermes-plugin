---
name: linkedin-ads
description: Run LinkedIn Ads through AdPlug the way a senior B2B specialist would. Campaign groups, campaigns and creatives, job title, function, seniority, company and matched audience targeting, Lead Gen Forms, Thought Leader Ads, document and video ads, conversion rules and the Conversions API. What good looks like, what to check each day, week, month and quarter, and AdPlug's LinkedIn tools, audit checklist and build rules. Use for anything on LinkedIn Ads or Campaign Manager, ABM and account lists, lead quality from LinkedIn, and audience size or frequency questions.
license: MIT
metadata:
  author: AdPlug
  version: "1.0.0"
---

# LinkedIn Ads

Follow `adplug:ppc-agent` for approvals and reporting, and read the client brief first. This skill holds what's specific to LinkedIn; the jobs themselves have their own skills, which load this one for the LinkedIn details.

Load the references with `skill_view("adplug:linkedin-ads", "references/<file>")`:
- `references/tools.md`: every LinkedIn tool, report type and entity type.
- `references/audit.md`: the audit checklist, area by area.
- `references/build.md`: build order and traps.

**Names.** LinkedIn's "campaigns" are `campaign_groups` in the API, its "ad sets" are `campaigns`, and its "ads" are `creatives`. When a user says campaigns, they usually mean campaign groups. Use LinkedIn's own words when you talk to them.

## Its job in the plan

Reaching the ideal customer by who they are at work: job title, function, seniority, company, industry and company size. Clicks cost more than anywhere else, so LinkedIn only pays when the targeting is tight, the offer is worth a professional's time and lead quality is judged in the CRM, not by cost per lead.

## What good looks like

**Measurement.** The Insight Tag on the site, conversion rules for the brief's goal associated with the campaigns that should count them (`associate_conversion`), and Lead Gen Form leads tracked separately. B2B cycles are long, so the platform alone sees only the first step: send qualified leads, opportunities and deals back through the Conversions API (`linkedin_ads_send_conversion_event`; see `adplug:lead-quality`). Once qualified leads arrive regularly, a lead generation campaign can optimise for them (`optimization_target_type` MAX_QUALIFIED_LEAD, with maximum delivery), and the lead_gen report shows qualified leads per campaign.

**Structure.** One objective per campaign group. Campaigns split by audience (one persona, one company list, one retargeting pool each), so results show which audience works. Don't mix formats that need different objectives in one campaign.

**Targeting.** Built from the brief's ICP: job titles or function plus seniority, company size and industry, company lists for account-based work. Check the size before launch (`linkedin_ads_targeting_lookup` action audience_count): LinkedIn won't run an audience under 300 members, a narrow one burns out fast, and a broad one pays LinkedIn's prices for people outside the ICP. Audience expansion and the LinkedIn Audience Network (`audience_expansion_enabled`, `offsite_delivery_enabled`) change who sees the ads: off unless chosen. Exclude existing customers, competitors and the client's own staff where the brief says so.

**Who actually sees it.** The demographics report (`MEMBER_JOB_TITLE`, `MEMBER_SENIORITY`, `MEMBER_COMPANY`, `MEMBER_INDUSTRY`, `MEMBER_COMPANY_SIZE`) shows where the money really goes. It has spend, clicks and website conversions by segment but no lead columns in AdPlug, so judge Lead Gen Form quality from the form responses (`linkedin_ads_get` lead_form_responses) and the CRM.

**Bidding and budget.** Maximum delivery to learn what a result costs, then a cost cap once you know, or manual bidding where control matters more than volume. Watch campaigns that never spend their budget: the audience is too small, the bid too low or the creative not approved.

**Creative.** Several creatives per campaign so LinkedIn can rotate, each speaking to one persona's problem. Document ads work as a lead magnet in B2B; Thought Leader Ads from a founder's or expert's post often beat company posts once the person approves sponsorship; video builds retargeting pools of viewers. Fatigue shows as frequency rising while CTR falls (report_type reach).

**Lead Gen Forms.** Few fields, pre-filled from the profile, qualifying questions only for the brief's good-lead signals, hidden fields for campaign tracking, and a thank-you step that offers the next action. Make sure leads reach the CRM quickly: a lead nobody calls is wasted spend.

**Retargeting.** Matched audiences of website visitors, video viewers, lead form openers and company page engagers (`linkedin_ads_list` saved_audiences): status READY, big enough, and fresh.

**Scaling.** The reach report shows how much of each audience has already seen the ads (audience penetration). When most of it has, add audiences rather than budget; when reach is still growing at the target cost, budget is the lever. For account lists, check spend by company (demographics with `MEMBER_COMPANY`): a few large companies can soak up most of the budget, so split lists by company size where that happens.

## The rhythm

What a senior specialist checks on LinkedIn. These checks fold into the client's routines (`adplug:routines`); don't create a scheduled job per check.

- **Every day** (the morning check): spend against budget per campaign group, campaigns that stopped delivering, creatives rejected, and leads still arriving.
- **Every week:** cost per lead and per conversion by campaign and creative, frequency against CTR for fatigue, the job titles and companies behind the spend against the ICP, and the latest form responses against the good-lead signals.
- **Every month:** audience saturation (reach and frequency over the month), matched audience sizes and freshness, conversion rules still associated, the budget split across campaign groups against where qualified pipeline comes from, and what competitors run (`linkedin_ad_library_search`).
- **Every quarter:** a new persona or offer test, company lists refreshed from the CRM, and the Conversions API feedback loop checked end to end.

## In AdPlug

- Every change: `linkedin_ads_preview_mutation`, then `linkedin_ads_execute_mutation` with the same operation and params, one operation at a time. `linkedin_ads_send_conversion_event` has no preview: show exactly what will be sent and get a yes first.
- New campaign groups, campaigns and creatives are drafts. `create_inline_ad` makes a complete ad in one step (single image, video, document, carousel, text, spotlight, message, event, Thought Leader Ads).
- Money: budgets take plain currency with a currency code that defaults to USD, so pass the account's currency. Reports give spend in US dollars whatever the account's currency: say so before comparing with a target in another currency.
- LinkedIn has no change history through AdPlug: read campaigns' status, budgets and end dates instead.
- `linkedin_ad_library_search` returns competitors' ads; the copy and images sit behind each `ad_url`.

## Which skill does the job

| Job | Skill |
|---|---|
| Audit the account | `adplug:account-audit`, with `references/audit.md` |
| Job titles and companies outside the ICP | `adplug:cut-wasted-spend` |
| New audiences, company lists, Thought Leader Ads | `adplug:find-opportunities` |
| New campaign groups, campaigns, lead forms | `adplug:build-campaigns`, with `references/build.md` |
| Creatives and refreshes | `adplug:ad-copy` |
| Weekly and monthly results | `adplug:performance-review` |
| A campaign that stopped delivering | `adplug:diagnose` |
| What LinkedIn leads become in the CRM, Conversions API | `adplug:lead-quality` |
