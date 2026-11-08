---
name: routines
description: Set up recurring AdPlug checks per client as Hermes cron jobs, covering each platform's own daily, weekly, monthly and quarterly checks without a pile of jobs. A morning health check that messages only when something needs attention (one digest for agencies), a Monday review, a weekly waste proposal, a weekly lead-quality check for lead generation, a monthly client report, a competitor sweep and a quarterly audit, delivered to Telegram, Slack, Discord or wherever the user works. Scheduled runs only read and propose. Use for "every morning", "every Monday", "each month", "schedule", "routine", "alert me when", "keep an eye on".
license: MIT
metadata:
  author: AdPlug
  version: "1.0.0"
---

# Routines

Saying "I'll keep an eye on it" is a promise a chat can't keep. A routine can. Each one is a Hermes cron job that loads AdPlug's skills, reads the client's brief, does its check and sends the result to the user's channel.

## The rule for every routine

A scheduled run has nobody to approve anything. It reads, analyses, reports and proposes. It never previews or executes a change, never turns anything on and never sends anything to a client. Every routine prompt ends with "Change nothing in the ad accounts or HubSpot", and proposals come back to the user, who approves them in a normal chat. A run may still update the client's brief ("Where we are" and the date it last reported), because each run is a fresh session with no memory of the last. If the AdPlug servers are set to `trust: untrusted` in Hermes (recommended in `adplug:ppc-agent`), Hermes also refuses change tools in unattended runs.

## Set one up

1. **Which client and which accounts.** One set of routines per client; the prompt names the client so the run reads the right brief (`~/adplug/clients/<client-slug>.md`, or the folder saved in Hermes' memory).
2. **Where results go.** Telegram, Slack, Discord or another connected channel. A job created from the terminal delivers to local files unless a channel is named, so always name one.
3. **When, in whose time zone.** Use a cron expression (`0 8 * * 1` is 08:00 every Monday) and check Hermes' time zone setting matches the user's; otherwise it runs on the server's clock.
4. **Which skills to attach:** `adplug:ppc-agent` plus the skill for the job. Attached skills load before the prompt runs, so the prompt can stay short.
5. **Run it once now** and show the user the result before leaving it to the schedule.

Create the job with Hermes' cron tool by asking in chat, with `/cron add`, or from the terminal, for example:

```
hermes cron create "0 8 * * 1-5" "Morning check for Acme: message me only what needs me. Change nothing." --skill adplug:ppc-agent --skill adplug:performance-review --name "Acme morning check" --deliver telegram
```

The gateway runs cron jobs, so it has to be running (`hermes gateway install`, then start it).

## Few jobs, every platform covered

Each platform skill has a rhythm: what to check every day, week, month and quarter on that platform. Don't turn each line into its own job. A handful of routines per client covers all of them:
- the morning check runs each connected platform's "every day" list,
- the Monday review, the waste routine and, for lead generation, the lead-quality routine run the "every week" lists,
- the monthly report and the competitor sweep run the "every month" lists, with account housekeeping in a note to you, not in the client's PDF,
- the quarterly audit runs the "every quarter" lists.

The run reads the client's brief, sees which platforms the client uses, and loads those platform skills itself. An agency with many clients runs one digest each morning, not one job per client.

## The standard set

Offer these, in this order, and adjust to the client. Each run calls AdPlug again and counts toward the plan's monthly requests, so match the frequency to the money at stake: a daily check pays off on accounts spending enough to lose real money in a day; smaller accounts do fine with the weekly ones.

| Routine | When | Skills | Prompt |
|---|---|---|---|
| Morning check | Each weekday morning | performance-review | Run the performance-review morning check for <client>. Message me only what needs me; if nothing does, say so. Change nothing. |
| Agency morning digest | Each weekday morning | performance-review | Run the performance-review morning check for every client in ~/adplug/clients and send me one message grouped by client, only what needs me. If nothing does, say so. Change nothing. |
| Monday review | Every Monday morning | performance-review | Weekly review for <client>: last week against the week before on every platform, what moved and why, pacing for the month, and what you'd change next. Don't add platforms' conversions together. Change nothing. |
| Waste and search terms | Every Monday or every other week | cut-wasted-spend | For <client>, find search terms, placements, audiences, job titles and communities that spent enough to judge without converting, over a window long enough for each account's volume, and that aren't already excluded. Give the negative or exclusion, level and match type you'd use, and the window you judged on. Change nothing in the ad accounts; I'll pick in chat. |
| Lead quality (lead generation with HubSpot) | Every Monday | lead-quality | For <client>: leads, qualified leads, deals and pipeline by platform and campaign, grouped by the month each lead was created, saying which months are too young to judge; how many paid leads have no owner or no activity, with links to the HubSpot records but no names, emails or phone numbers; where junk leads come from; and how many qualified leads are ready to send back to each platform. Change nothing in the ad accounts or HubSpot; I'll approve any upload in chat. |
| Client report | First working day of the month | performance-review | Build <client>'s client-ready PDF report for last month: short summary, results against target and the month before, the insights behind them, what changed and what changes next. Change nothing. |
| Competitor sweep | Monthly | find-opportunities | For <client>, check the LinkedIn and Meta Ad Libraries for the competitors in the brief: ads that started since the last sweep noted in the brief, the offers and formats they push, and what they seem to be testing. Meta only shows commercial ads that reached the EU or the UK, so say when a competitor shows nothing for that reason. Note the sweep's date in the brief. Change nothing in the ad accounts. |
| Audit and growth scan | Quarterly | account-audit, find-opportunities | Quarterly audit for <client> against the brief, plus the growth opportunities that matter most, sized. Change nothing. |
| Change check | On the date in the change log | performance-review | For <client>, check whether the change logged on <date> did what we expected, and recommend keeping or reverting it. Change nothing. |

## Managing them

List, pause, edit and remove jobs by asking in chat or with `hermes cron list`. When a client's brief changes (new target, new accounts, new channel), update their routines too. A run that finds nothing new since the last report sends one line, not a full report.
