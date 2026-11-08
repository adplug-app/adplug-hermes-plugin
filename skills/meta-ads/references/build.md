# Building on Meta Ads

Check the live spec first: the preview tool with `operation="help"`. These are the orders and the traps that go wrong most. Copy limits are in `adplug:ad-copy` (`references/limits.md`).

Order: campaign (one of the six OUTCOME objectives), ad set (budget, optimisation event, audience, placements), creative (Page, Instagram account, media, copy, link, call to action), ad. `meta_ads_list_accounts` gives the Page and Instagram IDs.

- Objective and buying type are fixed at creation.
- Money in minor units (5000 = 50.00). Daily or lifetime budgets only.
- An Instagram account must be on the creative for Instagram placements. If the Page has none linked, say the ads will be Facebook-only.
- Lead forms need the Page to have accepted Meta's Lead Ads terms; custom audiences need Meta's Custom Audience terms. Neither can be accepted through the API: give the user the link Meta returns.
- Advantage+ creative enhancements can rewrite the ad once it runs. Set `enhancements` deliberately and tell the user what's on.
- A creative can't be edited. Make a new one and swap it onto the ad with `update_ad`, which keeps the ad's history.
