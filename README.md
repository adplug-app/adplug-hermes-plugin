# AdPlug: Google Ads, Meta Ads, LinkedIn Ads, Microsoft Ads and Reddit Ads plugin for Hermes Agent

Safely connect Google Ads, LinkedIn Ads, Meta Ads, Microsoft Ads and Reddit Ads to Hermes Agent, so your agent can check campaigns, flag what needs attention and make the changes you approve. Built for PPC specialists, agencies and in-house teams.

AdPlug's plugin gives Hermes Agent 16 PPC skills, written and used by a PPC expert with 12 years in the field and more than $100M in ad spend managed. The skills work through AdPlug's hosted MCP server, which you add with one command. Each ad platform then connects once, with its own sign-in.

## What the AdPlug plugin adds to Hermes Agent

- **One server for five ad platforms.** Google Ads, Meta Ads (Facebook and Instagram), LinkedIn Ads, Microsoft Ads and Reddit Ads all come through one AdPlug server, so one question can cover a client's Google, Meta and LinkedIn accounts at once. You connect each platform once, with its own sign-in.
- **16 PPC skills.** One per ad platform, with what good looks like there and what to check each day, week, month and quarter. The rest do the jobs across platforms: client briefs, audits, wasted spend, growth, new campaigns, ad copy, reviews, diagnosis, lead quality and routines.
- **A brief for every client.** The agent keeps one brand and goals file per client (the goal, targets, ideal customer, brand voice, guardrails and which ad accounts are theirs) and reads it before any work, scheduled runs included. To write it, the agent reads the client's website and ad accounts, then asks only what it couldn't find (nobody wants a 30-question form).
- **A preview and your yes before every change.** The agent shows you the account, exactly what will change and the money involved per day and per month, then waits for an explicit yes. New campaigns and ads start paused or as drafts, and scheduled runs only read and propose.
- **Lead quality with HubSpot.** If you run lead generation, add AdPlug's HubSpot server and the agent judges your ads by what the leads become in the CRM, then sends qualified leads back to Google, Microsoft, LinkedIn and Reddit as offline conversions.

The plugin is a portable [Agent Plugins v1](https://agent-plugins.org) package: `plugin.json` and 16 skills, with no code, no scripts and no hooks. The skills drive AdPlug's hosted MCP server at `https://api.adplug.app/mcp`.

## What you need

- An AdPlug account. The free plan connects one ad platform, has a monthly request limit and is view only: the agent can read your accounts and preview changes, but not apply them. Paid plans connect every ad platform and HubSpot, and let the agent apply the changes you approve. Compare the plans at [adplug.app/pricing](https://adplug.app/pricing).
- Hermes Agent 0.21.6 or later.

## Install the AdPlug plugin and MCP server in Hermes Agent

Install and enable the plugin:

```bash
hermes plugins install adplug
hermes plugins enable adplug
```

Then add AdPlug's server, once. Plugins can't set up a sign-in on their own, so it takes one extra command:

```bash
hermes mcp add adplug --url https://api.adplug.app/mcp --auth oauth --connect-timeout 300
```

A browser tab opens on AdPlug's sign-in. Sign in, or create a free account, and click **Approve**. Then run `/reload-mcp` in your session (or start a new one). On a server with no browser, write the `adplug` entry into `~/.hermes/config.yaml` yourself (the YAML below shows it) and run `hermes mcp login adplug`. Then paste the redirected URL back at the prompt, or use an SSH port forward.

Last, connect your ad accounts at [adplug.app/dashboard/connections](https://adplug.app/dashboard/connections), each with the platform's own sign-in. Each sign-in reaches the ad accounts that login can see, such as every account under a Google Ads manager (MCC) account (on Meta, the ones you pick when you sign in), so an agency connects once per platform, not once per client.

### Add HubSpot for lead quality

If you run lead generation with HubSpot as your CRM, add AdPlug's HubSpot server as well. The agent then judges your ads by what the leads become: which platforms, campaigns, keywords and job titles bring leads that qualify, become deals and close. It also finds paid leads nobody followed up. And it sends qualified leads back to Google, Microsoft, LinkedIn and Reddit as offline conversions, so each platform's bidding can learn to chase qualified leads instead of form fills.

```bash
hermes mcp add adplug-hubspot --url https://api.adplug.app/mcp/hubspot --auth oauth --connect-timeout 300
```

Connect each client's HubSpot portal at [adplug.app/dashboard/connections](https://adplug.app/dashboard/connections). HubSpot is on AdPlug's paid plans: Pro connects 3 portals, Max 10 and Agency 10 per person.

To see paid and organic search side by side, add AdPlug's Google Search Console server the same way:

```bash
hermes mcp add adplug-search-console --url https://api.adplug.app/mcp/google-search-console --auth oauth --connect-timeout 300
```

### Ask before every change (recommended)

To have Hermes ask you before any AdPlug tool that changes an ad account or a HubSpot portal, add `trust: untrusted` to each AdPlug server in `~/.hermes/config.yaml`:

```yaml
mcp_servers:
  adplug:
    url: https://api.adplug.app/mcp
    auth: oauth
    trust: untrusted
  adplug-hubspot:
    url: https://api.adplug.app/mcp/hubspot
    auth: oauth
    trust: untrusted
```

Run `/reload-mcp` to apply it. Reading and previewing still run without a prompt. Changes ask first, and a scheduled run, with nobody there to answer, can't make them. `/yolo` and `approvals.mode: off` skip the prompt, so leave them off for AdPlug.

It's a second lock on top of the agent's own preview and your yes. If you manage client money, turn it on.

## The 16 PPC skills for Hermes Agent

AdPlug's plugin has one skill per ad platform and 11 job skills. Each platform skill says what good looks like on that platform and what to check every day, week, month and quarter. It also holds the platform's audit checklist and build rules. The job skills work across all five platforms and load the platform skills they need.

**Platforms**

| Skill | What it covers |
|---|---|
| `adplug:google-ads` | Search, Performance Max, Demand Gen (including YouTube video ads), Shopping and AI Max. Measurement, structure, Smart Bidding, keywords, assets, audiences and the settings that leak money quietly. |
| `adplug:microsoft-ads` | Search on Bing, Yahoo and partners, the Microsoft Audience Network, LinkedIn profile targeting, UET, offline conversions, and scheduled Google imports and what they overwrite. |
| `adplug:linkedin-ads` | Targeting by job title, function, seniority and company, audience size and saturation, Lead Gen Forms, Thought Leader Ads, document ads and the Conversions API. |
| `adplug:meta-ads` | Pixel and Conversions API checks, consolidated structure, Advantage+ audiences, creative as the targeting, fatigue, lead ads, catalogues and split tests. |
| `adplug:reddit-ads` | Communities and conversation keywords, brand safety exclusions, posts written for each community, the Reddit Pixel and the Conversions API. |

**Jobs**

| Skill | What it does |
|---|---|
| `adplug:ppc-agent` | The working rules, loaded for any ads request: the client's goal first, tracking checked before judging anything, a preview and your yes before every change, and numbers that never mix currencies or add up conversions across platforms. |
| `adplug:client-brief` | One brief per client, the brand and goals file every other skill reads: the goal, what a conversion is worth, targets, budget, ideal customer and personas, brand voice and claims, competitors, CRM stages, guardrails, reporting, and which ad accounts are theirs. Keeps the client's change log too. |
| `adplug:account-audit` | A senior audit of one platform or all of a client's accounts, ranked by severity, with a week-by-week plan. |
| `adplug:find-opportunities` | What's missing, sized and ranked: budget headroom on campaigns that hit target, converting search terms that aren't keywords, uncovered themes, missing campaign types, unused audiences and competitor angles. |
| `adplug:cut-wasted-spend` | Search terms and negatives, Performance Max search terms and placements, Microsoft partner sites, LinkedIn job titles that don't fit the ideal customer, Meta ad sets over target, Reddit communities, and negatives that block good keywords. |
| `adplug:build-campaigns` | Groundwork for an ad account you've just opened (tracking, settings, exclusions, audiences) and new campaigns on all five platforms, planned from the brief and created paused or as drafts. |
| `adplug:ad-copy` | Search ads and assets, Performance Max asset groups, and LinkedIn, Meta and Reddit ads, written from the personas and the landing page, with character limits checked and every claim backed by the landing page or the brief. |
| `adplug:performance-review` | A morning check that flags only what needs you (one digest across every client for agencies), a weekly review, budget pacing and a client-ready monthly report. |
| `adplug:lead-quality` | With HubSpot or a CRM export: what ad leads become, where junk leads come from, paid leads nobody followed up, audiences built from the CRM, and qualified leads sent back to Google, Microsoft, LinkedIn and Reddit as offline conversions. |
| `adplug:diagnose` | Why something stopped spending or converting, and what to do about it. |
| `adplug:routines` | Morning checks, weekly reviews, waste reports, monthly client reports and quarterly audits as a few Hermes cron jobs per client, delivered to Telegram, Slack or Discord. Scheduled runs only read and propose. |

Just ask, and the agent loads the skills it needs with `skill_view("adplug:<skill>")`. To pick one yourself, name it: "use adplug:account-audit".

## Try it

Swap Acme for your client.

- "Set up a brief for Acme from acme.com and their connected Google Ads and LinkedIn Ads accounts. Work out what they sell, who converts and what it costs now, then ask me only what you couldn't find."
- "Audit Acme's Google Ads against the brief, then show me the negatives you'd add and add the ones I approve."
- "Where can Acme get more leads at the same cost per lead?"
- "Which of Acme's LinkedIn campaigns brought leads that became deals in HubSpot this year, and what did each deal cost in LinkedIn spend?"
- "Every weekday morning, check Acme's accounts and message me on Telegram with anything that needs me."

## Disclosure

- **Network:** every AdPlug tool call goes to `https://api.adplug.app` (the `/mcp` server, plus `/mcp/hubspot` or `/mcp/google-search-console` if you add them). Ad platform, HubSpot and Search Console calls happen on AdPlug's servers, which log each call against your plan's limits.
- **Websites and files:** to write briefs and ads, the agent reads the client's website and landing pages with Hermes' own web tools, plus any file you point it to.
- **Credentials:** you sign in to AdPlug with OAuth 2.1 (dynamic client registration and PKCE). Hermes stores the token in `~/.hermes/mcp-tokens/`, one file per server, readable only by you. Your Google, LinkedIn, Meta, Microsoft, Reddit and HubSpot sign-ins are stored encrypted on AdPlug's servers and never pass through Hermes.
- **Spend:** on a paid plan the agent can create and change real campaigns that spend real money. The skills have the agent show you each change, wait for your explicit yes, and create new campaigns and ads paused or as drafts. AdPlug's server also refuses any Google Ads, Search Console or HubSpot change that wasn't previewed first. On LinkedIn, Meta, Microsoft and Reddit, the preview is the skills' rule. With `trust: untrusted`, Hermes asks you before every AdPlug tool that makes a change, unless you've switched approvals off with `/yolo` or `approvals.mode: off`.
- **Customer data:** sending qualified leads back to an ad platform, or building audiences from the CRM, shares hashed or click-level customer data with that platform. The agent shows what it will send (counts, stages, values and dates) and waits for your yes. Only send data you have consent to share.
- **Local files:** the client-brief skill writes one Markdown file per client to `~/adplug/clients/`, or to a folder you choose. No passwords, tokens or customer lists go in it. When you ask for a client report as a PDF or slides, the agent creates that file on the machine Hermes runs on.
- **Scheduled jobs:** the routines skill creates Hermes cron jobs only when you ask for them. Hermes' gateway runs them, so it has to stay running. Each run reads, reports and proposes, never makes a change, and its AdPlug calls count toward your plan's monthly requests.
- **No telemetry** in this package.

Privacy: [adplug.app/privacy](https://adplug.app/privacy) · Terms: [adplug.app/terms](https://adplug.app/terms) · Support: support@adplug.app

## Licence

MIT. See [LICENSE](LICENSE).
