---
name: lead-quality
description: "Follow ad leads past the form fill into the CRM. With AdPlug's HubSpot connector (or another CRM the user connects or exports), see which platforms, campaigns, keywords, audiences and job titles bring leads that qualify, create deals and close, check the CRM records where ad leads came from, find junk-lead sources and paid leads nobody followed up, and send qualified leads and sales back to Google, Microsoft, LinkedIn and Reddit so bidding chases revenue. Use for lead generation and B2B: lead quality, junk leads, MQL, SQL, pipeline or revenue from ads, HubSpot, CRM, offline conversions, Conversions API."
license: MIT
metadata:
  author: AdPlug
  version: "1.0.1"
---

# Lead quality: from ad click to revenue

In lead generation, the cheapest lead is rarely the one sales wants. The ad platforms stop counting at the form fill; the CRM knows which leads qualified, which became deals and which closed. This skill joins the two. Follow `adplug:ppc-agent`, and read the client brief first: it says what a qualified lead is for this client, the good-lead signals, the lead-to-customer rate and what a customer is worth.

## Connect the CRM

AdPlug's HubSpot connector reaches every HubSpot portal the user connected in AdPlug, so an agency works across all its clients' portals from one place. Add it once in Hermes, sign in when the browser opens, then run `/reload-mcp`:

```
hermes mcp add adplug-hubspot --url https://api.adplug.app/mcp/hubspot --auth oauth --connect-timeout 300
```

HubSpot comes with AdPlug's paid plans. Its tools: `hubspot_list_accounts` (portals by Hub ID; with several, pass `account_id` on every call and name the portal in every result), `hubspot_search`, `hubspot_get`, `hubspot_schema` (property names, pipelines and stages), `hubspot_report` (SQL over CRM, attribution and campaign data; call it with `describe=true` first), and `hubspot_preview_change` then `hubspot_execute_change` for changes. Record the client's portal in the brief.

No HubSpot? Another CRM connected to Hermes answers the same questions, or the user can share an export of leads with source, campaign, stage, deal value and dates.

## 1. Can the CRM tell where leads came from?

Run this first, because every report below depends on it. Look up property names with `hubspot_schema`; never guess them.
- Ad leads carry their source: original traffic source and its drill-downs, UTM parameters, and the click IDs (Google's GCLID, Microsoft's MSCLKID) where a hidden form field or HubSpot's own ads integration stores them.
- LinkedIn and Meta lead form leads arrive in the CRM with their campaign, and arrive quickly.
- Lifecycle stages and deal stages match the brief's definitions of a qualified lead, an opportunity and a customer.

Count how many recent leads lack each piece, and say what fixes it: UTMs on every final URL or tracking template, hidden fields on forms, ad accounts connected in HubSpot, lead form sync. Without this, ad leads get credited to other sources and the ads look worse than they are.

## 2. Which ads bring leads that turn into money

Use cohorts by lead creation date, and say which recent cohorts are still too young to judge: B2B deals can take months, so last month's leads haven't had time to close.
- **By platform and campaign:** leads, the lifecycle stage each reached, deals created, pipeline value, deals won and revenue, next to each platform's spend for the same dates (from the ad tools, in the same currency, never added across currencies). Derived figures (cost per qualified lead, cost per opportunity, revenue per unit of spend) are labelled as derived.
- **Search:** the keywords and search terms behind deals, where the CRM records them (from UTMs or the ads integration), and the ones that spent without creating any. A keyword can look cheap per lead and never create a deal.
- **LinkedIn:** the companies and job titles behind pipeline, and the form responses against the good-lead signals.
- **Meta and Reddit:** the ad sets, audiences and communities behind qualified leads.
- **Against other channels:** win rate, deal size and time to close for paid leads next to organic search, referrals and direct.

## 3. Junk and leaks

- **Junk patterns:** personal email domains when the ICP is business buyers, students, job seekers, competitors, wrong countries, spam and duplicates. Trace each pattern to the campaign, keyword, form, placement or audience that brings it, and fix it at the source: negatives and exclusions (`adplug:cut-wasted-spend`), qualifying questions or a higher-intent form, tighter targeting.
- **Paid leads nobody followed up:** no owner or no activity, and how long each has waited. It's wasted spend that never shows in the ad account. Offer follow-up tasks for the owners, previewed in HubSpot before anything is created.
- **Stalled deals from paid ads:** open deals gone quiet, with the campaign behind each.

## 4. Send the outcome back to the platforms

Platforms optimise toward what they're told converts. Send back the stage closest to revenue that still happens often enough for the platform to learn from, and say why you picked it.

- **Check what already sends it.** HubSpot's own ad conversion events can already send lifecycle stages to Google Ads, LinkedIn and Meta, and some clients use Zapier or a tag manager. Ask the user and compare the platform's conversion actions or rules with what you'd send. One route per stage and platform: sending the same stage twice counts it twice.

- **Google Ads:** an import conversion action for that stage, then `upload_click_conversions` with the click ID (GCLID, GBRAID or WBRAID), conversion time and value, and read the per-row errors in the result; `upload_conversion_adjustments` restates or retracts values later. AdPlug sends Google conversions by click ID only, so leads without a stored click ID can't go this way; matching on hashed email (enhanced conversions for leads) goes through HubSpot's Google integration or Google's own tools. For people in the UK and EEA, Google expects consent signals with uploads, which AdPlug's upload doesn't carry: tell the user before uploading those leads. Conversions only count inside the conversion action's click-through window. Check uploads keep arriving with `offline_conversion_upload_client_summary`. If Google refuses the upload with `CUSTOMER_NOT_ALLOWLISTED_FOR_THIS_FEATURE` (since June 2026 it limits Ads API uploads to apps that were already sending them), tell the user and send that stage through HubSpot's own Google Ads conversion events instead.
- **Microsoft Ads:** an offline conversion goal, then `upload_offline_conversions` with the MSCLKID or a hashed email or phone; `adjust_offline_conversions` to restate.
- **LinkedIn Ads:** a conversion rule for the stage created with `conversion_method: CONVERSIONS_API` (LinkedIn may need to grant the account Conversions API access first) and associated with the campaigns, then `linkedin_ads_send_conversion_event`. It has no preview, so show exactly what will be sent and get a yes first.
- **Reddit Ads:** `send_conversion_events` through the Reddit preview tool.
- **Meta Ads:** AdPlug doesn't send Conversions API events to Meta. HubSpot's own Meta integration or the client's server-side set-up does; check the dataset receives them.

Every upload is a change: preview, show, yes, execute, read back, and a line in the change log. Show counts, stages, values and the date range, never the people's details. Only send customer data the client has consent and the platform's terms to share. A scheduled run can count what's ready to send and report it, but never previews or uploads: the upload waits for the user's yes in chat.

## Audiences from the CRM

The CRM knows who's a customer, who's mid-deal and who was lost. Customers come out of acquisition campaigns; open deals can see case studies and proof; lost deals can see a reason to look again. Build them as LinkedIn matched audiences (`create_saved_audience`, `add_audience_members`), Meta custom audiences (`create_custom_audience`, `add_audience_users`) and Microsoft customer lists (`create_customer_list`, `upload_customer_list`). Google restricted Customer Match through its Ads API in April 2026 to apps already using it, so for Google use HubSpot's own audience sync. It's customer data: consent, a preview and a yes, every time.

## 5. Report it

When the CRM and a platform disagree about what worked, the CRM wins. Lead with which platforms and campaigns create pipeline and revenue for the money, where budget should move, and the leaks to fix. The monthly client report (`adplug:performance-review`) leads with these numbers for lead-generation clients; the Monday review carries a short paid-to-pipeline line. Update the brief with what you learned: the real lead-to-customer rate, good-lead signals, and decisions.

## Changes in HubSpot

Only through `hubspot_preview_change` and, after a yes, `hubspot_execute_change`. One portal per change, at most 10 records per change, and IDs from one portal never reused in another. Scheduled runs never change HubSpot. If a portal is View only for the user, don't try; point them to their team owner.
