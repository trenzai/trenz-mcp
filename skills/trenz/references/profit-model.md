# Profit model SOP (TikTok Shop)

Goal: work out the net profit per unit and the break-even price for a candidate product, answering "does this product actually make money".
After the product shortlist ([product-selection.md](product-selection.md)) is ready, run the key candidates through this profit calculation before the final recommendation.

## Data sources (first sort out where each number comes from)

| Source | Data |
|---|---|
| Trenz tools | Price, sales, rating, reviews (get_detail / get_related reviews); how similar products' creators perform (get_related creators) |
| **Must ask the user** | Sourcing cost (supplier or factory price), unit weight and volume, how goods reach the warehouse (e.g. sea or air freight) and the freight quote, planned creator commission rate, whether they will run ads |
| Industry defaults | The benchmark tables below, used when the user has no actual quote; mark them "estimate" in the output |

If the user doesn't even have a sourcing cost, you can only calculate the "platform-side take-home rate" (what is left of the price after platform fees). Tell the user clearly that this is not a full profit figure.

## Per-unit profit formula

```
Net profit per unit = Price
  − Goods cost (sourcing cost, converted to the shop's currency if different)
  − Inbound freight per unit (unit weight kg × freight rate per kg)
  − Warehouse cost (receiving fee + storage share + outbound handling fee)
  − Fulfillment / last mile (FBT fee, or third-party warehouse last-mile shipping)
  − Platform commission (price × 6%)
  − Creator commission (price × commission rate, only on creator-driven orders)
  − Ad cost per order (ad spend ÷ orders from ads)
  − Returns reserve (see below)
```

Break-even price: solve for net profit = 0. Also give the price that reaches a target gross margin of ≥30%.

<!-- Maintenance: the benchmark values below were collected in 2026-07; re-check them before every skill release -->
## Industry benchmarks (web research collected 2026-07; all estimates, they change over time)

These figures are for TikTok Shop US. They do not carry over to other markets; for other markets use the user's numbers or mark the item as unknown.

### Platform fees (TikTok Shop US)

- Platform commission: **6%** (jewelry 5%; payment processing included). New sellers get 3% for 30 days after their first sale
- Creator commission, category medians: beauty & personal care ~15%, health ~20%, womenswear & underwear ~10%, sports & outdoors ~10%, accessories ~8% (the range is wide; prefer the actual level seen for similar products via get_related creators)
- Ads (optional): CPM about $9–30; Smart Promotion about 3.5% of GMV; GMV Max minimum about $50/day during the learning phase
- Settlement cycle: from 31 days for new sellers; reserve cash flow for it

### Logistics costs (US)

- Inbound freight: use the user's freight quote; if they have none, mark inbound freight as missing rather than guessing
- FBT (Fulfilled by TikTok): about **$2.86–4.28 per unit** (tiered by weight); since 2026, cross-border sellers in the US must use FBT, upgraded logistics or CBT
- Third-party warehouse: receiving ~$0.5/kg, storage ~$0.1/kg/month, outbound handling ~$0.3–1 per unit, last mile USPS ≤1kg ~$10 per unit
- Small, light items: prefer FBT; consider a third-party warehouse plus a last-mile carrier only for large or irregular items

### Return costs

Platform rules (US):
- Refund administration fee: 20% of the commission, capped at $5 per SKU
- Return shipping share: the seller pays 20% when the shop's SPS score is ≥4, otherwise 50%
- FBT return processing fee: about $3 per unit

```
Returns reserve = estimated return rate × (goods cost + inbound freight per unit + fulfillment fee + return processing fee)
```

(Conservative basis: returned goods are assumed unsellable; for categories that can be refurbished, discount accordingly.)

## Estimating the return rate (from rating and reviews)

Without actual shop data, estimate a tier from the product rating, the content of negative reviews and the category benchmark:

| Signal | Estimated return rate |
|---|---|
| Rating ≥4.5, negative reviews don't center on quality/size | Low end of the category benchmark |
| Rating 4.0–4.5 | Middle of the category benchmark |
| Rating <4.0, or negative reviews cluster on quality / not as described / size | High end of the category benchmark or above |

Category benchmarks (estimates): apparel, shoes & hats 10–25% (driven by sizing), consumer electronics 5–10%, beauty & personal care 3–8%, home & household 2–8%.

How: pull negative reviews with get_related(products, id, "reviews") and check whether the main complaints are "quality / size / not as described" (these turn into returns) or "slow shipping" (usually no return). For deeper analysis use ai_generate(type="review_analysis").

## Output format

Per-unit profit breakdown table (one column or one table per candidate):

| Item | Amount (shop currency) | % of price | Source |
|---|---|---|---|
| Price | | 100% | Platform data |
| Goods cost | | | User provided |
| Inbound freight per unit | | | User provided |
| Fulfillment (FBT) | | | Estimate |
| Platform commission | | 6% | Platform rule |
| Creator commission | | | User set |
| Returns reserve | | | Estimate (return rate x%) |
| **Net profit per unit** | | **Net margin %** | |

After the table you must give: ① the break-even price; ② sensitivity (how net profit changes if creator commission rises 5 percentage points or the return rate doubles); ③ which numbers are estimates, with a suggestion to check them against actual quotes.

## Hard requirements

- **Label the source of every number**: platform data / user provided / industry estimate, kept as three separate categories, never mixed
- Benchmarks go stale: state in the output "fee benchmarks collected in 2026-07; check the platform's latest rules and the logistics provider's actual quote before ordering"
- Flag the risk clearly for products with a net margin <15% (running ads will make them lose money); only ≥30% counts as healthy
- When costs and the price are in different currencies, use an approximate exchange rate for the day of the calculation and state it; don't use an old rate from memory
