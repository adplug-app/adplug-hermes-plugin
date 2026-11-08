---
name: client-brief
description: Set up or update a client's brief, the brand and goals file every AdPlug job works from. Business and main conversion, what a conversion is worth, CPA or ROAS targets, budget, ideal customer profile and personas, offer, brand voice and visual rules, claims and legal lines, competitors, CRM and lead stages, guardrails, reporting, and which Google, LinkedIn, Meta, Microsoft and Reddit ad accounts belong to the client. Use for a new client, a new account, "here's what we're trying to achieve", or when goals, targets, brand or audience change. Also keeps the client's change log.
license: MIT
metadata:
  author: AdPlug
  version: "1.0.0"
---

# The client brief

Every good PPC decision starts with what the client is trying to achieve and who they sell to. The brief writes that down once, so each audit, cleanup, build, report and routine works toward it, and so the next session (or a scheduled run) knows it too.

One file per client: `~/adplug/clients/<client-slug>.md`, built from `templates/brief.md`. If the user keeps briefs somewhere else, use their folder and save it to Hermes' memory so the next session and scheduled runs find it. An in-house team has one client: the company. Read the brief at the start of any work on that client, and keep it current.

## Set up a new brief

Do the homework first, then ask only what you couldn't find. Nobody wants a 30-question form.

1. **Which client, which accounts.** Ask for the client's name and website if you don't have them. List the accounts on every connected platform (`*_list_accounts`) and propose which belong to this client. Confirm the mapping with the user. Flag accounts that look like tests or sandboxes and ask before counting their spend.
2. **Read what already exists.** Anything the user pasted or pointed you to (a proposal, a strategy doc, brand guidelines, last month's report), plus the client's website: what they sell, prices, the main offer and call to action, proof (reviews, logos, numbers), the words they use for their customers, and how the brand sounds and looks. If HubSpot is connected, find the client's portal (`hubspot_list_accounts`) and read its lifecycle stages and deal pipeline (`hubspot_schema`).
3. **Read the accounts.** Choose a window that has enough data for this account's volume, and say which you chose.
   - Which conversion actions exist, which ones bidding uses, and which are still recording (`google_ads_conversion_performance`, `microsoft_ads_list` conversion_goals, `linkedin_ads_list` conversions, `meta_ads_list` pixels and custom_conversions, `reddit_ads_list` pixels).
   - Spend, conversions and cost per conversion per platform and campaign. This is the baseline the targets get compared with.
   - Who actually converts: the search terms and keywords (Google, Microsoft), the job titles, seniorities, industries and company sizes (LinkedIn demographics, Microsoft professional_demographics), the age, placements and countries (Meta), the communities and keywords (Reddit). This is evidence for the ideal customer.
   - Brand terms: the search terms that contain the client's name.
4. **Draft the brief** from all of that. Mark where each line came from: client, website, account data, or assumption to test.
5. **Ask for the gaps,** a few at a time, the most important first:
   - What is the one conversion that matters (a qualified lead, a demo, a sale, a call), and what is it worth? For lead generation, roughly how many leads become customers and what a customer is worth. From those, work out the most a lead can cost and still pay back (what a customer is worth times the share of leads that become customers), and show the sum.
   - The target: cost per lead or acquisition, or return on ad spend. If they don't know, propose one from the account's own history and the conversion's value, and mark it "proposed".
   - Monthly budget, how it should split across platforms, and how far it may flex.
   - Markets and languages, and when the business is busy or quiet.
   - Brand rules the site doesn't show: words and claims to avoid, legal lines that must appear, competitor names the ads may or may not use, and visual rules for social creative.
   - For lead generation: which CRM, what counts as a qualified lead and an opportunity, and roughly how many leads become customers.
   - Guardrails: who approves changes, what must never be touched (brand campaigns, a CEO's pet campaign), how big a budget move is acceptable without a call.
   - Reporting: how often, to whom, through which channel (Telegram, Slack, email), and in what form (a short message, a PDF report, slides).
6. **Write the ideal customer and personas** (see below) and the plan per platform.
7. **Save the file, then show a short summary:** the goal, the targets (confirmed or proposed), the accounts, the personas, and what you'd look at first. Ask the user to confirm or correct the goal and targets. Never mark a target confirmed until they say so.

## The ideal customer and personas

The ideal customer profile (ICP) says which companies or people are worth paying for. Personas say who inside them searches, clicks and decides. Write only as many personas as the account can target separately.

For each persona:
- Who they are: job title and seniority (B2B), or the customer segment (consumer).
- The problem that sends them looking, in their own words, and what they'd type into Google or Bing.
- What they want to happen, and what makes them hesitate (price, risk, switching effort, internal approval).
- What proof they need before they act.
- Where to reach them: which platform and which targeting (search themes, LinkedIn job titles or company lists, Meta interests or lookalikes, Reddit communities).
- What tells you they're a good lead or customer (job title, company size, order value), so reports can judge lead quality, not just volume.

Back personas with the account data from step 3 where it exists. When the data contradicts what the client believes (they think it's IT directors, the leads come from office managers), say so plainly. Both go in the brief, and the conflict becomes a test.

## The plan per platform

For every connected platform, one or two lines: its job for this client, which conversion it optimises toward, its target and its share of the budget. For example, Google Search catches people already looking. LinkedIn reaches the job titles in the ICP, often with lead forms. Meta and Reddit build demand and bring visitors back. Microsoft adds search volume and LinkedIn profile targeting on Bing.

This is what lets every other skill judge a campaign against its own job: a LinkedIn awareness campaign isn't failing because its cost per lead is high.

## Make the accounts match the brief

When the brief and the accounts disagree, propose fixes through the normal preview and yes (`adplug:ppc-agent`):
- **Google Ads:** the right conversion actions as primary and the rest as secondary (`update_conversion_action`), campaign conversion goals that match the campaign's job (`set_campaign_conversion_goal`), conversion values that reflect what a conversion is worth, bid targets that match the brief.
- **Microsoft Ads:** conversion goals (`create_conversion_goal`, `update_conversion_goal`) and a working UET tag.
- **LinkedIn Ads:** conversion rules associated with the campaigns they belong to (`associate_conversion`).
- **Meta Ads:** ad sets optimising for the event that matches the goal, and custom conversions for it (`create_custom_conversion`).
- **Reddit Ads:** ad groups optimising for an event the pixel actually receives.

## Keep it current

- When the user states a new fact (a new target, a new offer, a lost market), update the brief straight away and say what changed.
- **Change log:** after every executed change, add one line: date, platform, account, what changed, why, what you expect, and when to check it. Check it the next time you work on that client.
- **Decisions:** strategy calls the user makes ("no more display", "brand stays at its own budget") go under Decisions with the date, so you don't propose them again.
- After a review, refresh the "Where we are" snapshot: the latest results against target, per platform.

## Pitfalls

- Never invent targets and present them as the client's. Propose, label, confirm.
- Never copy a password, token or personal customer data into the brief. Contact names and roles are fine; customer lists are not.
- One brief per client. Don't mix two clients' accounts in one file, even when they share a manager account.
- If you can't write files in this session, show the brief in full and ask the user to save it or allow file access.
