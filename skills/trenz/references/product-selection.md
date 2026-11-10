# Product selection / market research SOP

Goal: start from a category or keyword and produce a shortlist of product candidates the user can decide on.

## Workflow

### 1. Confirm the inputs (ask the user if any are missing)

- Target market (country code, e.g. us / th / id)
- Category or keyword (e.g. "portable blender", "skincare")
- Optional: price range, seller type preference (cross-border / local / brand)

### 2. Build the candidate pool

Screen with search(entity=products) and page with offset until the pool is large enough. Size the pool to the task: if the user gave a size, use it; if not, choose one that fits the depth of the research and state it in the output. Don't draw conclusions from the first page alone.

When the user explicitly asks for an official ranking, use rankings instead, and pick the ranking, time granularity and filters from the current tool description. Before paging through many results, call `account_info` and plan around the current response; every call counts toward the plan's 7-day Agent allowance. Don't call search results sorted by a field an official ranking.

```
search(entity="products", country="us", keyword="portable blender",
       filters={"min_sold_7d": 100})
```

Common filters: min_price / max_price (price range), min_rating (quality floor, ≥4.0 recommended), min_sold_7d or min_sold_30d (sales floor), seller_type (cross-border non-brand / local / brand / non-brand; codes are in the search tool description), free_shipping.

Drop clearly unqualified items at screening: rating <4.0, zero sales in the last 7 days, price far outside the user's budget.

### 3. Dig into the top candidates (do this for the top 5–8)

Build a profile of each candidate:

| Call | What to look at |
|---|---|
| get_detail(products, id, country) | Price, SKU structure, sales and GMV metrics (sold_7d, sold_30d, gmv_30d) and the short recent trend it returns; when channel mix is needed, pick the sub-resource from the tool description |
| get_related(products, id, "reviews", country) | Where negative reviews cluster (quality / shipping / not as described); the share of negative reviews is the main risk signal |
| get_related(products, id, "creators", country) | Number and structure of promoting creators: dominated by a few top creators (hard to enter) or spread across a long tail (opportunity) |
| get_related(products, id, "similar", country) | Competitor density and price distribution, to judge how crowded the niche is |

For deep review analysis use AI: `ai_generate(type="review_analysis", params={"product_id": id, "country_code": country})` → poll check_task for the report.

When analyzing a product's trend:

- Use the sales and GMV metrics and recent trend from get_detail, and the spike value in search results. When the user asks about a specific date or period, use rankings with `date` and `granularity`; a product missing from a ranking is not evidence that it had no sales. If the user asks for an exact date range that the available metrics don't cover, say so instead of estimating.
- When comparing candidates, use the same market, period and metrics. If the user only says "recently", state which period the metrics cover.
- Distinguish daily values from cumulative values; never add up cumulative values to get a period total. Amounts are in the market's currency; don't compare amounts across currencies.
- Missing fields, missing data points or null are not zero sales; flag estimates and data gaps. When coverage is thin, state the limitation; don't conclude decline or lack of demand from it.
- Separate content-driven demand from hype. Real demand: the trend keeps rising and promotion is spread across a long tail of creators. Hype (a trap): sales were driven by a single top creator and the trend peaked and fell back (negative spike). Stocking up on a hype product leaves you with inventory.

### 4. Output

Output the shortlist as a table, one product per row. Suggested columns:

Product | Price | 7-day sales | 30-day GMV | Rating | Promoting creators | Trend | Main risk

After the table, give the recommendation in a few sentences: which 1–2 to prioritize, why, and the main risks. State the query date and market for the data.

The top 1–2 candidates should go on to a profit calculation (read [profit-model.md](profit-model.md)): selling well ≠ making money; you can only conclude after sourcing cost, inbound freight, commissions and returns are counted.

## Notes

- To call a product "rising", look at the direction of the recent trend in get_detail and the spike value; not just a growing cumulative total or the absolute 7-day sales.
- The same product performs very differently across countries; when the user targets several markets, run this SOP for each market separately.
- Screen first, then dig: at screening use only the list fields returned by search; don't call get_detail on every candidate. If the pool is too large, narrow it before digging in, to keep the context small.
