# LinkedIn Ads audit checklist

B2B accounts rarely fail on creative alone. They fail on audience saturation, frequency and Lead Gen Form set-up. LinkedIn's names: campaign groups (LinkedIn's "campaigns"), campaigns ("ad sets"), creatives ("ads").

## 1. Goals and measurement
- `linkedin_ads_list` conversions (rules, enabled or not) and campaign_conversions (which campaigns each rule is attached to).
- `linkedin_ads_performance` report_type conversion: post-click and post-view per rule.
- Conversions API: whether the client sends offline or CRM events (`linkedin_ads_send_conversion_event` is how AdPlug sends them).
- Lead Gen Form leads are counted separately (`report_type` lead_gen). AdPlug's demographics report has no lead columns, so read job titles from the form responses (`linkedin_ads_get` lead_form_responses) when lead quality matters.
- Attribution windows against the buying cycle.

## 2. Campaign group and campaign structure
- `linkedin_ads_list` campaign_groups and campaigns: groups split by funnel stage, persona, market or product, each with an objective that matches its job.
- Overlapping audiences across campaigns bidding against each other.
- Formats mixed inside one campaign when they need different objectives.

## 3. Targeting
- `linkedin_ads_get` campaign for each targeting set: job titles, functions, seniorities, skills, industries, company sizes, company lists, exclusions. Compare with the brief's ICP.
- Audience size (`linkedin_ads_targeting_lookup` audience_count): too narrow barely delivers or burns out fast; too broad pays LinkedIn's prices for people outside the ICP.
- Audience expansion and the LinkedIn Audience Network: on by choice or by default?
- Who actually sees and clicks: `linkedin_ads_performance` report_type demographics with MEMBER_JOB_TITLE, MEMBER_SENIORITY, MEMBER_INDUSTRY, MEMBER_COMPANY_SIZE, MEMBER_COMPANY. Spend on segments outside the ICP is waste even when CTR looks fine.

## 4. Audience strategy
- Matched audiences (`linkedin_ads_list` saved_audiences): company lists, contact lists, website and video retargeting, lead form openers. Status READY, sizes, how fresh.
- Existing customers and competitors excluded where the brief says so.

## 5. Creative
- `linkedin_ads_performance` report_type creative: CTR, engagement and video completion by creative. Format mix (single image, video, document, carousel, Thought Leader Ads).
- Fatigue: report_type reach shows frequency; frequency climbing while CTR falls means the audience has seen it enough.
- Copy that speaks to the persona's problem and carries proof; the destination matches the promise.
- Creatives with too few impressions to judge versus ones that should have been retired.

## 6. Lead generation
- `linkedin_ads_list` lead_forms and `linkedin_ads_performance` report_type lead_gen: form opens, completion rate, cost per lead per campaign.
- Too many fields, questions that don't qualify, no thank-you step.
- Lead quality: job titles from the form responses (`linkedin_ads_get` lead_form_responses) against the brief's good-lead signals. AdPlug's demographics report has no lead columns.

## 7. Bidding and budget
- Bid strategy (maximum delivery, cost cap, manual) per campaign; campaigns spending well above the target cost per lead; campaigns not spending their budget (bid too low, audience too small, ads not approved).
- Daily versus lifetime budgets; pacing of each campaign group against plan.

## 8. Landing pages
Message match between ad and page, mobile experience, form length.

## 9. Performance
- `linkedin_ads_performance` report_type campaign and daily against the previous period; cost per lead and per conversion by campaign group, campaign and creative.
- The biggest waste: audiences and creatives spending without results. The biggest levers: winning audiences with room to grow, retiring tired creatives.
