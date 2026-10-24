# ebay scraper tool: what actually works, what it costs, and how to pick the right setup without overpaying

Most people searching for an ebay scraper tool want one of three things: current listing prices for a product line, sold-price data for resale research, or a repeatable way to pull competitor listings into a spreadsheet or database. The catch is that eBay sits behind aggressive bot detection, so the real decision isn't "which scraper" — it's whether you run your own code against eBay directly, pay a managed API to handle the blocking for you, or use a no-code actor that charges per item.

This guide covers all three routes, with verified current pricing for ScraperAPI's full plan lineup, the exact credit rules that determine what an eBay scrape actually costs, and an honest look at when a pay-per-item tool beats a pay-per-request API.

> **Quick picks**
> - Testing the waters or scraping a few hundred items a month: the **free plan (1,000 credits/month, no card required)** is genuinely enough to start.
> - Regular price tracking or competitor monitoring: **Hobby at $49/month (or $44.10/month billed annually)** — budget for fewer than 100,000 pages, since anti-bot surcharges can raise the per-page cost.
> - Want structured eBay search results without writing a parser? ScraperAPI has a dedicated async eBay endpoint that returns JSON directly.
> - 👉 [Check ScraperAPI's current plans and start the 7-day free trial](https://www.scraperapi.com/pricing/?fp_ref=coupons)

## What an ebay scraper tool needs to handle

An eBay scraper tool has to do three jobs: fetch pages from eBay without getting blocked, extract the fields you care about (title, price, condition, seller, shipping), and hand the result back in a usable format.

The blocking part is the hard one. eBay rotates through bot-detection systems and serves CAPTCHAs to suspicious traffic, which is why the DIY route tends to break every few weeks. That's the gap managed services sell against: ScraperAPI's pitch is that you send a GET request with your target URL and API key, and they handle JS rendering, CAPTCHA solving, and a pool of 40M+ proxies on their side. You get back the page HTML (or, with their structured endpoints, ready-made JSON) instead of a block page.

There's also a separate, higher-touch product on their site: a dedicated eBay Scraper solution aimed at companies doing competitor analysis, price tracking, and dynamic pricing at scale. That one has a no-code interface and scheduling built in, but pricing isn't public — you request a custom trial with custom credit and concurrency limits through their sales team. For most individuals and small teams, the standard plans below are the relevant path.

## ScraperAPI plans and pricing (all current tiers)

Every paid plan includes the same core feature set: JS rendering, premium proxies, CAPTCHA and anti-bot handling, JSON auto-parsing, custom headers and sessions, automatic retries, unlimited bandwidth, and a 99.9% uptime guarantee. Annual billing takes 10% off every tier.

| Plan | Monthly price | Annual (per month) | Credits | Concurrency | Get it |
| --- | --- | --- | --- | --- | --- |
| Free | $0 | — | 1,000/mo | 5 | [ Start free, no card needed](https://www.scraperapi.com/?fp_ref=coupons) |
| Hobby | $49 | $44.10 | 100,000 | 20 threads | [ Get Hobby](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Startup | $149 | $134.10 | 1,000,000 | 50 threads | [ Get Startup](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Business | $299 | $269.10 | 3,000,000 | 100 threads | [ Get Business](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Scaling | $475 | $427.50 | 5,000,000 | 200 threads | [ Get Scaling](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Professional | $975 | $877.50 | 10,500,000 | 300 threads | [ Get Professional](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Advanced | $1,975 | $1,777.50 | 21,500,000 | 500 threads | [ Get Advanced](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Enterprise | Custom | Custom | 22,000,000+ | 500+ threads | [ Talk to sales](https://www.scraperapi.com/?fp_ref=coupons) |

Two details worth knowing before you pick:

- Their documentation also lists a **Student plan** tier between free and Hobby; if that applies to you, it's worth asking support about it, since the pricing page doesn't headline it.
- Geotargeting outside the US and EU requires **Business ($299) or higher**. On Hobby and Startup, country-level targeting is limited to US and EU. If your use case is comparing eBay DE prices from a US IP context, check this before committing to the cheapest plan.

## What an eBay scrape actually costs in credits

This is the part most comparison articles skip, and it changes the math significantly.

A standard page costs **1 credit** on ScraperAPI, and eBay pages fall into that standard category — so 100,000 Hobby credits equals up to 100,000 eBay pages *at the base rate*. But the pricing FAQ is explicit that harder sites cost more: Amazon costs 5 credits per page, Google and Bing cost 25, LinkedIn costs 30 — and any site behind protection like Cloudflare, Datadome, or PerimeterX adds **10 credits per request** when their system bypasses it. In practice, plan around the worst-case number, not the best one.

Two practical takeaways for eBay work:

1. Before committing to a plan, use the **Domain Cost Estimator** in the dashboard to check the exact per-URL cost for the eBay pages you're targeting, and set a `max_cost` per scrape so a single request can't blow through your budget.
2. If your eBay scrapes suddenly start costing 11 credits instead of 1, that's the anti-bot surcharge kicking in — size your monthly volume to the higher figure.

If your volume is unpredictable, the overage mechanics matter too. By default, scraping pauses when credits run out. On Hobby through Professional you can opt into **auto-upgrade**, which bumps you to the next tier automatically (usually at a better price per credit). On Scaling and above, **pay-as-you-go** lets you keep scraping at a fixed per-credit rate up to a monthly spending cap you set — and that cap is a limit, not a pre-charge, so you won't get surprise bills unless you raise it yourself.

👉 [Compare credit allowances across all plans on the pricing page](https://www.scraperapi.com/pricing/?fp_ref=coupons) before you settle on a tier — the per-credit price drops noticeably as you go up.

## Getting structured data: the dedicated eBay Search API

If parsing HTML yourself sounds like a chore, ScraperAPI maintains async structured-data endpoints specifically for eBay. You POST to `async.scraperapi.com/structured/ebay/search` with your API key and a query, and the service returns the search results as JSON instead of raw HTML.

The parameter set is genuinely useful for product research:

- **`tld`** — targets 16 eBay marketplaces: com, co.uk, com.au, de, ca, fr, it, es, at, ch, com.sg, com.my, ph, ie, pl, nl
- **`condition`** — new, used, open_box, refurbished, for_parts, not_working (multi-select supported)
- **`buying_format`** — buy_it_now, auction, accepts_offers
- **`show_only`** — nine filters including completed_items, sold_items, returns_accepted, authorized_seller, and authenticity_guarantee
- **`sort_by`** — ending_soonest, newly_listed, price_lowest, price_highest, distance_nearest, best_match
- **`seller_id`**, `page`, and `items_per_page` (60, 120, or 240) for pagination and seller-specific pulls

One quality note from their own docs: condition label names aren't consistent across all listings — open_box sometimes shows up on eBay as "without tag" — so always pass values from the supported list rather than copying what the page displays. Results can also be delivered via webhook callback instead of polled, which matters if you're piping data into an automation.

For item-level detail pages, there's a matching async Product Page endpoint that extracts price, seller info, specs, and reviews by product ID. If you want to see how the structured endpoints behave before paying, the 7-day trial with 5,000 credits is the natural place to try them — no credit card needed.

## ScraperAPI vs. Apify vs. free open-source scripts

Since "ebay scraper tool" searches usually end up comparing these three approaches, here's the verified picture:

|  | ScraperAPI | Apify (eBay actor) | Open-source scripts (GitHub) |
| --- | --- | --- | --- |
| Pricing model | Credits per request | Pay-per-event, $0.003 per item returned | Free |
| Entry cost | $0 free plan; paid from $49/mo | Free tier to test; costs scale with items | $0 |
| Blocking handled | Yes (proxies, CAPTCHA, JS rendering) | Yes (runs on Apify platform with residential proxy rotation) | You handle it yourself |
| Output | HTML or structured JSON/CSV via dedicated endpoints | Flat JSON, includes sold listings (~90 days) | CSV/JSON you build yourself |
| Best for | Ongoing monitoring at predictable monthly cost | Occasional one-off pulls, sold-price comps | Learning, tiny one-off jobs |

The honest differences:

- **Apify's per-item billing** is attractive when your volume is small and bursty. Pulling 500 items costs about $1.50 plus negligible startup fees. But if you're polling eBay daily for months, a flat monthly API works out cheaper and more predictable — 100,000 items would run ~$300 on that actor versus $149 for a Startup month on ScraperAPI.
- **Apify's eBay all-in-one actor explicitly sells sold-listing data** — completed sales over the last ~90 days across 8 marketplaces (US, UK, DE, FR, IT, ES, CA, AU) — as a headline feature, and it's built specifically for resale comps and arbitrage workflows. ScraperAPI approaches this differently: its eBay search endpoint supports `show_only=sold_items` and `completed_items` filters, but if deep sold-comp tooling across many marketplaces is your *entire* use case, a purpose-built actor may fit better. If you're mixing price monitoring, seller data, and broader scraping targets, the general API pulls more weight.
- **Free GitHub scrapers** (Scrapy-based eBay scrapers, simple Python CLIs) exist and cost nothing, but they work by spacing out direct requests to eBay — which means you inherit the blocking problem, the maintenance burden, and the risk of your IP getting throttled. Fine for learning or a one-time export; frustrating as a daily driver.

## Who doesn't need to pay for any of this

Worth saying plainly: if you need one snapshot of 200 listings for a spreadsheet, a free GitHub script or a small Apify run will do the job, and no subscription is justified. ScraperAPI's paid plans make sense when scraping is recurring — daily price checks, weekly competitor sweeps, feeds that other systems depend on.

It's also worth being honest about scale. If you're currently running under a few thousand requests a month, Hobby's 100,000 credits is overkill on paper — but it's the entry paid tier, so the real question is whether the free 1,000 monthly credits cover you. If they do, stay free. The trial exists precisely so you can measure your real volume before committing, and if it turns out the service doesn't fit your use case, ScraperAPI has a 7-day no-questions-asked refund policy on paid plans — you can also cancel anytime from the dashboard at no charge.

## FAQ

**Does ScraperAPI work on eBay without a custom plan?**
Yes. The standard Scraping API returns eBay page HTML with any plan, including the free tier, and there's a dedicated async eBay Search endpoint that returns structured JSON. The separate "eBay Scraper" solution page with no-code scheduling is aimed at high-volume customers and starts with a custom sales trial.

**Can I scrape eBay sold listings?**
Through the structured eBay Search API, yes — `show_only` accepts `sold_items` and `completed_items` among its nine filter values (which also cover things like returns_accepted, authorized_seller, sale_items, and authenticity_guarantee). For ready-made sold-price comps across 8 marketplaces, Apify's all-in-one eBay actor is also built specifically for that.

**Is there a refund if it doesn't work for my use case?**
ScraperAPI offers a 7-day no-questions-asked refund — contact support and they process it. You can also cancel anytime from the dashboard at no charge, and the free trial requires no credit card, so the cheapest way to evaluate it costs nothing.

**Which eBay marketplaces are supported?**
The structured search endpoint covers 16 top-level domains including ebay.com, ebay.co.uk, ebay.de, ebay.com.au, ebay.ca, ebay.fr, ebay.it, ebay.es, and more. A `country_code` parameter controls the page language and currency, and the docs note that if you're working in a language not native to a listed TLD, you should keep `tld` set to `com`.

**What happens if I run out of credits mid-project?**
Scraping pauses by default until renewal. You can enable auto-upgrade (Hobby and up), use pay-as-you-go with a spending cap (Scaling and up), or request a custom plan from support.

## Picking your setup, in one pass

Here's the decision path based on the verified numbers:

- **One-off export, few hundred items** → a free GitHub scraper or a small pay-per-item run elsewhere. Spend nothing.
- **Sold-price research as your main use case** → a purpose-built actor with native sold-listings support fits more naturally than a general API.
- **Ongoing monitoring, under ~80,000 pages/month** → Hobby at $49/month ($44.10 annually), after confirming eBay's credit cost in the dashboard estimator.
- **Millions of pages, multi-country, team workflows** → Startup or Business, and check that geotargeting requirement — outside US/EU you need Business.

Before you click buy on any tier, run your target eBay URLs through the Domain Cost Estimator and set a `max_cost` on your first scrapes; it takes two minutes and tells you your real monthly credit burn instead of a guess. Then start with the 7-day trial and let actual usage decide the plan.

👉 [Start the free ScraperAPI trial and test it on your own eBay targets](https://www.scraperapi.com/?fp_ref=coupons)
