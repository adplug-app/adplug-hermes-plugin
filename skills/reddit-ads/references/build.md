# Building on Reddit Ads

Check the live spec first: the preview tool with `operation="help"`. These are the orders and the traps that go wrong most. Copy limits are in `adplug:ad-copy` (`references/limits.md`).

Order: campaign (objective), ad group (budget, bid, targeting, schedule, optimisation event), post (published as the business profile), ad. `operation="batch"` does all four in one preview.

- Targeting: communities, conversation keywords and interests from the personas (`reddit_ads_targeting_lookup` suggest_communities from the website, keyword_suggestions, validate_keywords). Check size and suggested bid before launch.
- Brand safety: account exclusions for communities and keywords the client must never appear next to (`update_account_exclusions`).
- Optimise only for a pixel event that actually fires; otherwise the ad group barely delivers.
- A published post can't be edited: make a new post and point the ad at it.
