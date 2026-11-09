---
name: ad-copy
description: Write, rewrite and refresh ads from the client's brief and personas. Google Ads and Microsoft Ads responsive search ads and assets (sitelinks, callouts, structured snippets), Performance Max asset groups, LinkedIn Ads creatives and Thought Leader Ads, Meta Ads primary text and headlines, Reddit Ads posts. Includes ad tests and fixing creative fatigue. Checks every character limit and claim before previewing. Use for "write ads", "new RSAs", "headlines", "sitelinks", "refresh creatives", "ad fatigue", "what should we test".
license: MIT
metadata:
  author: AdPlug
  version: "1.0.0"
---

# Write and refresh ads

Follow `adplug:ppc-agent`. Copy is written from evidence, not imagination: the brief (personas, offer, proof, voice, words to avoid), the keywords and search terms or the audience, the ads already running and how they perform, and the landing page.

## Gather first

1. **The brief.** Which persona this ad speaks to, their problem in their words, the offer and call to action, and the brand section: voice, words and claims to avoid, claims with their proof, legal lines, spelling, and visual rules for social creative. No brief: ask for the ideal customer, the product in one sentence and the brand voice before writing.
2. **The account.** For search: the ad group's keywords and the search terms that convert, the current ads with their ad strength and each asset's own results (Google no longer returns performance labels for every asset type), and Quality Score parts (`google_ads_campaign_performance` level ad and keyword, `google_ads_query` on `ad_group_ad_asset_view`; Microsoft `microsoft_ads_list` ads and keywords). For social: the creatives' results, CTR and frequency (`linkedin_ads_performance` creative and reach, `meta_ads_performance` ad, `reddit_ads_performance` ad).
3. **The landing page.** Open the final URL: headline, benefits, proof, call to action, tone. Only claims on the page (or in the brief) may go in an ad. If the page can't be read, ask for its copy.

Work ad group by ad group, highest spend first, in small batches the user can review. Stop after each batch and ask whether to continue.

## Search ads (Google and Microsoft)

- **Two responsive search ads per ad group** when testing: A leads with the main keyword and the core value, proof and call to action; B takes a different angle (the pain, speed, simplicity, the outcome, risk reversal, credibility). Keep the total enabled RSAs per ad group within three; list the weakest existing ones to pause and let the user decide.
- **Fifteen unique headlines and four descriptions.** Mix lengths. Each headline must make sense on its own, because the platform combines them. Spread them across keyword, benefits, proof, outcomes, ease and calls to action. Put the keyword theme in several headlines and at least one description, in the words people search with.
- **Descriptions** each stand alone (often only one or two show), front-load the point, and don't repeat the headlines.
- **Pins and keyword insertion depend on the campaign type:**
  - **Brand:** pin a headline with the brand name to position 1. Pin more than one brand-name variant there, so the platform can still rotate them. No keyword insertion.
  - **Generic (non-brand):** at least one headline with keyword insertion (`{KeyWord:Default text}`), with default text that reads well alone and keyword themes tight enough that every keyword reads well in the ad. No pins.
  - **Competitor:** no keyword insertion, because it would put the competitor's name in the ad. No pins.
  - The only other pin is a line the brief says must always show, such as a legal disclaimer.
  - Pin with `{"text": ..., "pinned_position": 1}` on Google and `{"text": ..., "pin": 1}` on Microsoft.
- **Paths** describe the page topic in readable words.
- **Assets:** sitelinks to different useful pages (each its own action: pricing, demo, case studies, a key service), with both description lines; callouts with benefits that aren't in the headlines; a structured snippet with real values from the site (header such as Services). Use only URLs that exist in the account or on the site.
- **Microsoft:** `microsoft_ads_generate_ad_copy` can draft from the landing page as a starting point. Editing an ad sends it back to editorial review. A headline or description list you pass replaces the current one, so pass the full list.

## Performance Max asset groups

Headlines, long headlines, descriptions, images, logos and video per asset group, built around its theme or product category. Check the operation's spec (`operation="help"`) for the counts and limits. Replace the weakest assets, judged on their own results and `primary_status` once they've had enough impressions, never all at once.

## LinkedIn Ads

Speak to one persona's job and problem. Open with the problem or the outcome, not the company. Keep the intro text's first line strong (LinkedIn truncates it), and the headline short and concrete. Formats: single image, document (a useful asset as the offer works well in B2B), video, carousel, conversation, Thought Leader Ads from the founder's or an expert's post once they've approved sponsorship in Campaign Manager. Use `create_inline_ad` for a new ad in one step.

## Meta Ads

Primary text that hooks in its first line (Meta shows about the first 125 characters before "more"), a short headline, a clear call to action button. Write variants for each format (feed image, Reels and Stories vertical video, carousel). A creative can't be edited: create a new one and swap it onto the same ad with `update_ad`, which keeps the ad's history. Decide the Advantage+ creative enhancements on purpose (`enhancements`) and tell the user which are on.

## Reddit Ads

Write the post like a useful post in that community, not a banner: plain, specific, honest about being an ad. Headlines in the community's language. A published post can't be edited: make a new post and point the ad at it.

## Refreshing tired creative

Tell three things apart before refreshing. Reach still growing while CTR falls: the creative is tired. Reach flat while frequency climbs: the audience is used up, so widen it rather than only swapping the ad. Cost per thousand impressions rising while CTR and reach hold: auction pressure (season or competitors), not the creative. For tired creative, keep what worked (the angle, the offer) and change what people have seen (the hook, the image, the format). Launch new variants paused next to the old ones, and pause the tired ones only when the new ones deliver.

## Check before previewing

Run every check and fix failures before you show anything:
- Every character limit (see `references/limits.md`; count characters, spaces included; for keyword insertion only the default text counts).
- No duplicate headlines or descriptions within an ad, no duplicate sitelinks or callouts within a campaign.
- No price, discount or saving language unless the brief or the page states it.
- Every claim (numbers, awards, guarantees, timeframes) is on the landing page or in the brief; flag the rest.
- Editorial: no exclamation marks in headlines, no repeated punctuation, no emoji, no words in all capitals except real acronyms, no phone numbers in ad text, no third-party brand names unless the client is authorised.
- Keyword insertion syntax exactly `{KeyWord:Default text}`, in generic ad groups only. Pins in brand ad groups only (plus any line the brief requires).
- The copy matches the page's spelling, formality and voice.
- No past years, ended offers or out-of-season words.

Show each ad as a table with a character count column and one or two lines on why it's written that way. Then preview: new ads paused (`create_rsa`, Microsoft `create_responsive_search_ad`, LinkedIn `create_inline_ad`, Meta `create_creative` and `create_ad`, Reddit `create_post` and `create_ad`), or replacements for existing ads (`update_rsa`, Microsoft `update_ad`). Batch where the platform allows. Apply after the yes, read back, log it in the brief, and set a date to judge the test once each ad has enough impressions.
