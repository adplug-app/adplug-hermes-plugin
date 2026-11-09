# Building on Google Ads

Check the live spec first: the preview tool with `operation="help"`. These are the orders and the traps that go wrong most. Copy limits are in `adplug:ad-copy` (`references/limits.md`).

## Search

Order: budget, campaign (paused), ad groups, keywords, responsive search ads, assets (sitelinks, callouts, structured snippets, images, business name and logo), campaign negatives and the shared negative list, audiences in observation.

- Ad groups by intent theme: each one's keywords, ad and landing page should answer the same question. Keep brand, competitor and non-brand in separate campaigns so their budgets and results never mix.
- Match types: phrase and exact. Broad match is the last stage and not worth it for most accounts: only once phrase and exact have run out of volume, with Smart Bidding, solid conversion tracking and a negative list that already does its job. Don't put the same keyword in two match types in one ad group.
- Responsive search ads: up to 15 headlines (30 characters) and 4 descriptions (90 characters), all unique, built from the brief's personas, offer and proof. Brand ad groups pin the brand name to headline 1. Generic ad groups get a keyword insertion headline and no pins. Competitor ad groups get neither (`adplug:ad-copy`).
- Locations: target people "in or regularly in" the locations (presence), not "interest", unless the brief says otherwise. Search partners and Display expansion off unless chosen on purpose.
- Locations use IDs: the targeting operations resolve names, but check the preview shows the right place. Since September 2026 Google refuses language targeting on Search campaigns: ads match by the language of the ads and landing page, so write them in the market's language.
- Conversion goal: set the campaign to the brief's primary conversion (`set_campaign_conversion_goal`) when the account-level goals don't match it.

## Performance Max

Order: budget, campaign (paused), asset group per product category or theme, assets, audience signals (customer lists, website visitors, custom segments built from the brief's search themes), listing groups for ecommerce, brand exclusions.

- Needs conversion tracking that records the real goal. Without it, Performance Max optimises toward whatever it can find.
- Exclude the brand (`add_pmax_brand_exclusion`) when a brand Search campaign exists, or PMax will claim the brand's conversions.
- Give each asset group enough assets in every slot (headlines, long headlines, descriptions, images, logos, video). Missing video means Google may make its own.
- Decide on final URL expansion with the user: it can send traffic to pages the client didn't intend. AdPlug can't set it, so the user switches it in Google Ads before launch.

## Demand Gen and YouTube

- YouTube video ads are created as Demand Gen (`create_demand_gen_ad` with the user's YouTube links). Video campaigns themselves can't be created or changed through the API.
- Audiences carry Demand Gen: lookalikes from customer lists, remarketing, custom segments from the brief's themes.

## Shopping

- Needs a Merchant Center link, set in Google Ads by the user.
- Product ads and listing groups: `create_product_ad` (pass `status: PAUSED`), `set_product_groups`. Split product groups where margin or performance differs.
