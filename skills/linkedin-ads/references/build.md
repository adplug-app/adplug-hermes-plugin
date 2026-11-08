# Building on LinkedIn Ads

Check the live spec first: the preview tool with `operation="help"`. These are the orders and the traps that go wrong most. Copy limits are in `adplug:ad-copy` (`references/limits.md`).

Order: campaign group (LinkedIn's "campaign"), campaign (LinkedIn's "ad set") with targeting and budget, then creatives or `create_inline_ad`, lead form if used, conversion rules associated with the campaign.

- One objective per campaign group. Campaigns split by audience: job titles or job function plus seniority, company lists (matched audiences), retargeting.
- Check the audience size before launch (`linkedin_ads_targeting_lookup` action audience_count). Too small barely delivers; too broad wastes the high cost per click on people outside the ICP.
- Audience expansion and the LinkedIn Audience Network change who sees the ads. Off unless chosen.
- Lead gen forms usually convert better than sending B2B buyers to a website; keep the questions few, and ask qualifying questions only for the brief's good-lead signals.
- New campaigns and creatives are drafts. A creative's copy can be updated in place on some formats; otherwise make a new creative.
- The Insight Tag is installed on the website by the user; AdPlug creates and associates the conversion rules.
