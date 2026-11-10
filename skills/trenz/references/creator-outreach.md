# Creator selection / outreach SOP

Goal: find creators who fit a product or category and produce an outreach list the user can act on.

## Workflow

### 1. Confirm the inputs (ask the user if any are missing)

- Target market (country code)
- Which product or category to promote
- Optional: budget level (decides top-tier vs mid-tier creators), whether contact info is required

### 2. Build the creator pool

Every call counts toward the 7-day Agent allowance. Call `account_info` first and size the pool to what is left (see SKILL.md).

**Start here — work back from best-selling similar products** (more precise): find similar products that sell well, then see who promotes them. Pick products with real recent sales and a price close to the user's; skip products with only a handful of promoting creators.

```
search(entity="products", country="us", keyword="portable blender")
get_related(entity="products", id=competitor_id, relation="creators", country="us")
```

**Only if that pool is too small — search creators by keyword**:

```
search(entity="creators", country="us", keyword="beauty",
       filters={"min_followers": 10000, "min_gmv_30d": 5000,
                "min_engagement": 2, "has_contact": true})
```

Use keywords for the product category, not brand names you haven't checked; a brand-name product search can return unrelated items.

When the user explicitly asks for a creator ranking, use the ranking tools directly: `get_top_commerce_creators` (creators by sales they drive), `get_fastest_growing_creators` (follower growth), `get_top_creator_agencies` (agencies). The first two need `date` (YYYYMMDD) + `granularity` (daily/weekly/monthly); the agency ranking always covers the last 30 days. Rankings build the pool; still run step 3's detail and related-data checks on the creators you pick.

For selling ability, look at gmv_30d and engagement first, not just follower count. Mid-tier creators (10k–100k followers) with high engagement usually give the best value.

### 3. Evaluate one by one (do this for the top 5–10)

| Call | What to look at |
|---|---|
| get_detail(creators, id, country, include="portrait") | Audience profile: do gender / age / region match the target customers |
| get_related(creators, id, "products", country) | Past categories promoted: have they promoted similar products, does the price range match |
| get_related(creators, id, "videos", country) | Content style: talking head / review / skit, video quality and average views |

Elimination signals: audience doesn't match the target market, past categories are completely unrelated, recent video metrics dropped off sharply (possibly throttled or inactive).

### 4. Output

Outreach list table. Always include these columns:

Creator | Followers | Engagement rate | 30-day GMV (all products) | Main categories | Content style | Contact | Rates | Fit rating

- Engagement rate and 30-day GMV come from the creator's own data, not from one product's promotion; when you also cite sales of a similar product, label them as that product's.
- Contact: what the creator's profile shows (for example an email in the bio); write "not listed" when there is none.
- Rates: always "ask during outreach".
- Say when a creator's main category differs from the product's, even if they have sold a similar product.

After the table, give tiered advice: which to contact first, which are backups, and the reason for each tier.

Refer to creators by name or handle. Don't infer a creator's gender from their name, photo or content; Trenz data doesn't include it.

### When the allowance runs out mid-task

Stop calling tools. Tell the user:

- that the plan's 7-day Agent allowance is used up and resets when the 7-day window ends (use the reset time `account_info` returns), and that plan details are at https://www.trenz.ai/pricing;
- which candidates you finished checking and which you didn't, so the list isn't mistaken for a complete review.

Then deliver the list from the checks that finished.

## Notes

- has_contact=true shrinks the pool; add it only when the user explicitly wants to reach out, not for pure research
- Trenz data does not include creator rates; don't invent rates in the list, mark them "ask during outreach"
- When the user wants outreach messages in bulk, you can tailor an opening line to each creator's content style (plain text generation, no tool calls)
