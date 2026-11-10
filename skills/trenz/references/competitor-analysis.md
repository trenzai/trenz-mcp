# Competitor benchmarking / shop analysis / ad research SOP

Goal: understand competitors' products, shops and ad strategies, and produce an analysis of the competitive landscape.

## Three angles

### A. Start from a product (the user named a competitor)

1. search(entity="products") to locate it by name and get the id (when the user gave only a name)
2. get_detail(products, id, country) — price, SKU strategy, sales and GMV metrics (sold_7d, sold_30d, gmv_30d) and the short recent trend it returns. When the user asks about a specific date or period, use rankings with `date` and `granularity`. When the user already has the ID and only wants the trend, start at this step
3. get_related(products, id, "similar", country) — matrix of similar competitors, to see the price distribution
4. get_related(products, id, "creators", country) — the competitor's creator network (candidates to win over)
5. get_related(products, id, "videos", country) — the competitor's best-performing content approach

When comparing trends, use the same market, period and metrics, and distinguish daily from cumulative values; don't fill missing data with zero, and flag estimates and thin coverage. The full rules are in [product-selection.md](product-selection.md) under "Dig into the top candidates"; exact fields and date conventions follow the current tool description.

### B. Start from a shop (analyze one seller)

1. search(entity="sellers", country, keyword=shop name) to locate it
2. get_detail(sellers, id, country, include="core,channel") — total GMV, product count, rating, channel mix
3. get_related(sellers, id, "products", country) — the shop's product structure: hero products vs long tail, which ones are gaining sales
4. get_related(sellers, id, "creators", country) — the creators the shop works with
5. get_related(sellers, id, "videos", country) — the shop's content marketing approach

When the user explicitly asks for an official shop ranking, use rankings `sellers_top_selling` (best-selling) or `sellers_most_promoted` (most promoted) to build the pool first; both need `date` (YYYYMMDD) + `granularity` (daily/weekly/monthly). Then take the IDs from the ranking back through the detail chain above. Don't present search results sorted by a field as an official ranking.

### C. Start from ads (research ad creatives)

1. search(entity="ads", country, keyword=category term, filters={"ad_type": "1"}) — limit to TikTok Shop ads (other ad_type values are in the tool description)
2. get_detail(ads, id, country) — one ad's run time, spend level, engagement
3. search(entity="creatives", ...) + get_detail(creatives, id, country, include="analysis") — performance at the creative level (GPM, completion, engagement)
4. For creatives worth breaking down: ai_generate(type="script_breakdown", params={"video_id": video id, "country_code": country}) to break down the script structure → give it to the user as a reference for their own version. Prefer the video id field in the creative result for video_id; if the breakdown reports an invalid video_id, first locate the matching video with search(entity="videos") to get its id, then break it down

## Output

Organize around the user's question. Two common forms:

- **Competitor matrix**: Competitor | Price | 30-day sales/GMV | Rating | Creators | Content approach in one sentence
- **Single-shop deep report**: shop overview → product structure → creator network → content/ad approach → takeaways for the user (gaps they can enter)

End on actionable takeaways for the user; don't just list data.

## Notes

- Ad spend and GMV are platform estimates; mark them "estimate" in the report
- Competitor analysis often combines entities (product → shop → ads); go one step at a time and decide the next step from the previous result
