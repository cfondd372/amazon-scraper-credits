# amazon scraper free: What 5,000 Free Credits Actually Get You, and Which Plan Is Worth Paying For

Short answer: yes, you can scrape Amazon for free, and ScraperAPI is one of the more straightforward ways to do it — you get 5,000 API credits free for your first 7 days with no credit card, then 1,000 credits a month on the permanent free plan. The catch that most articles skip: an Amazon request costs 5 credits, so that 5,000-credit trial works out to roughly 1,000 Amazon product pages, and the monthly free tier covers about 200. Whether that's enough depends entirely on what you're building.

This guide walks through the real math, the full pricing table, how the Amazon endpoint works, and which plans make sense once you outgrow the free tier.

## Quick picks

If you just want the bottom line:

> **Testing Amazon scraping for a side project?** Start with the free tier. You get 5,000 credits for 7 days (no card required), then 1,000 credits monthly — enough for around 200 Amazon pages a month. 👉 [Start the free 7-day trial with 5,000 credits](https://www.scraperapi.com/signup?fp_ref=coupons)
>
> **Running regular Amazon pulls?** Hobby at $49/month (100,000 credits ≈ 20,000 Amazon requests) is the realistic entry point.
>
> **Comparing free options?** Bright Data gives 5,000 free credits monthly (~$7.50 in value), MrScraper's free plan includes 1,000 tokens, and Oxylabs offers a free trial. No credit card on any of these — worth knowing before you commit anywhere.

## What "free" actually means in 2026's Amazon scraping landscape

The free Amazon scraping world splits into three camps, and they solve different problems.

Open-source scripts on GitHub (requests + BeautifulSoup tutorials, browser extensions that export Amazon listings to CSV) cost nothing and work fine for a few hundred pages. They break when Amazon shows you a CAPTCHA, rotates its layout, or your IP gets throttled — which, at any real volume, happens quickly.

Managed APIs with free tiers handle the blocking for you. The currently published free offers look like this:

| Tool | Free offer | Notes |
| --- | --- | --- |
| ScraperAPI | 5,000 credits for 7 days, then 1,000 credits/month | No credit card; 5 concurrent connections on the free plan |
| Bright Data | 5,000 free credits every month (~$7.50 value) | Applies to their scrapers including Amazon; no credit card |
| MrScraper | Free plan with 1,000 tokens | Token-based pricing; Pro runs $199/month |
| Oxylabs | Free trial of their Amazon Scraper API | Trial length set at signup |

If your volume is genuinely tiny, the monthly free tiers from several of these will carry you. ScraperAPI's edge is what happens after the trial: a 7-day no-questions-asked refund on any paid plan, documented per-site credit costs, and billing only for successful requests. That refund window effectively doubles your risk-free testing period — you can trial for 7 days, then pay and still have another 7 days to change your mind.

## The full ScraperAPI plan table

Here's everything currently listed on the pricing page. Every plan includes JS rendering, premium residential and mobile IPs, JSON auto parsing, CAPTCHA and anti-bot handling, automatic retries, unlimited bandwidth, and a 99.9% uptime guarantee — so the differences below come down to volume, concurrency, and geotargeting.

| Plan | Monthly | Annual (per mo) | Credits | Concurrent threads | Get it |
| --- | --- | --- | --- | --- | --- |
| Hobby | $49 | $44.10 | 100,000 | 20 | [ Get Hobby](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Startup | $149 | $134.10 | 1,000,000 | 50 | [ Get Startup](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Business | $299 | $269.10 | 3,000,000 | 100 | [ Get Business](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Scaling | $475 | $427.50 | 5,000,000 | 200 | [ Get Scaling](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Professional | $975 | $877.50 | 10,500,000 | 300 | [ Get Professional](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Advanced | $1,975 | $1,777.50 | 21,500,000 | 500 | [ Get Advanced](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Enterprise | Custom quote | Custom | 22,000,000+ | 500+ | [ Talk to sales](https://www.scraperapi.com/contact-sales?fp_ref=coupons) |

Annual billing takes about 10% off each plan. The official documentation also mentions a Student plan in its billing FAQ, though no public price is listed on the pricing page — if that applies to you, ask support directly.

## The credit math most articles get wrong

This is the part that determines whether ScraperAPI is cheap or expensive for you, and it's where most comparison posts stop at the sticker price.

A standard page costs 1 credit. Amazon costs 5. Google and Bing cost 25, LinkedIn costs 30, and sites behind Cloudflare, DataDome, or PerimeterX add another 10 credits per request when the bypass succeeds. Turning on JavaScript rendering also costs 10 credits.

Run the numbers for an Amazon-only workload:

- **Free trial (5,000 credits)**: about 1,000 Amazon product pages, or fewer if you enable rendering or hit protected pages
- **Monthly free tier (1,000 credits)**: roughly 200 Amazon pages per month
- **Hobby ($49/mo, 100,000 credits)**: about 20,000 Amazon requests — call it 667 product pages per day
- **Startup ($149/mo, 1,000,000 credits)**: around 200,000 Amazon requests, or 6,600+ per day

Two details make this manageable. The dashboard has a Domain Cost Estimator that shows the exact credit cost for any URL before you run it, and you can set a `max_cost` per request so a single expensive scrape can't drain your balance. A 2026 third-party review put it well: before picking a plan, estimate your credit multiplier — it matters more than the price on the pricing page.

One billing note worth internalizing: credits refresh each billing cycle, so a big one-off job doesn't justify a big annual plan. Size for your steady-state monthly volume.

## How to actually scrape Amazon with it

The Amazon integration needs no separate product or setup — it's the same API endpoint with an optional parsing flag. For product pages, offer listings, reviews, and search results, add `autoparse=true` and the HTML comes back as structured JSON instead:

bash
curl "http://api.scraperapi.com/?api_key=YOURAPIKEY&url=https://www.amazon.com/dp/B07V1PHM66&autoparse=true"


That ASIN example pulls a product page and returns parsed fields — title, price, ratings — without you touching a single HTML selector. Drop `autoparse=true` and you get raw HTML instead, which is useful if you already have parsing code.

A few practical settings from the docs: ScraperAPI parameters go before the `url` parameter, the recommended client timeout is 70 seconds (protected domains can take up to 60 seconds to return), and if your script runs past the concurrency limit you'll get a 429 response rather than a queue — the request is simply rejected and you're not charged for it. Retry logic on your side handles the rest.

Beyond ad-hoc requests, paid plans include the DataPipeline job runner (up to 100,000 URLs, keywords, or ASINs per project, with scheduling and webhook delivery) and a full crawler. If your Amazon workflow is "pull these 500 ASINs every morning," DataPipeline is what turns it into a set-and-forget job.

## Which plan actually fits your workload

Matching plans to situations works better than matching them to budget brackets:

- **Learning or validating an idea** → Stay on the free tier. 200 Amazon pages a month is small, but it's enough to build and debug your pipeline. Ask support if you need extra test credits — they grant additional testing credits on request.
- **A few thousand Amazon pages a month, hobby scale** → Hobby at $49. The 20-thread cap and US/EU-only geotargeting are the real constraints to check against your needs.
- **Production monitoring, price tracking across hundreds of ASINs** → Startup or Business. Business is also the first tier with country-level targeting beyond US and EU — if you need to scrape amazon.de as Germany or amazon.co.jp as Japan, that's a $299 decision, and there's no cheaper workaround.
- **You keep running out of credits mid-month** → Scaling ($475) is where Pay-As-You-Go kicks in, letting you continue at a fixed per-credit rate with a monthly spending cap you set. Auto-upgrade (opt-in, available from Hobby up) is the lighter-touch alternative: when credits run out, your account moves to the next tier automatically, usually at a better per-credit price.

A note on who this tool is wrong for: if most of your targets are JavaScript-heavy sites or heavy Google/LinkedIn scraping, those requests cost 10–30 credits each and the math turns against you fast. If you want ready-made hosted scrapers with zero code, an Actor-platform competitor will fit better. ScraperAPI's sweet spot is developers who already have scraping code that works locally and gets blocked in production.

## What users complain about — and what holds up

The 2026 third-party review of ScraperAPI ran its complaints against the facts, and a few are worth knowing before you buy:

- **Geolocation limits: confirmed.** Country targeting outside US and EU starts at Business. If that's a dealbreaker at your budget, it's better to know now than after checkout.
- **Limited free tier: confirmed.** 1,000 credits a month after the trial is "enough to test, not to run anything," in the review's words — consistent with the 200-Amazon-pages math above.
- **No proxy micro-control: by design.** You choose country, session, and IP tier, but not specific IPs or cities. If you need that level of control, raw proxies are the right purchase instead.
- **Slow responses on hard domains: sometimes.** Simple pages return in seconds; protected ones can take up to 60 seconds. The Async API exists for volume jobs where waiting matters less.
- **"Limited features" complaints: outdated.** The 2026 product includes structured data endpoints for Amazon and about nine other sites, DataPipeline scheduling, a crawler, and even an MCP server that plugs into Claude Code. The company also acquired Traject Data (the team behind the Rainforest and SerpWow APIs) in 2026, folding those e-commerce data APIs into the same credit system.

## FAQ

**Do I need a credit card for the free trial?** No. The 7-day trial with 5,000 credits requires no card, and neither does the ongoing free plan.

**Can I get a refund after paying?** Yes — ScraperAPI offers a 7-day no-questions-asked refund on paid plans. Contact support and they process it. You can also cancel anytime from your dashboard at no charge.

**Do unused credits roll over?** Credits reset with each billing cycle rather than accumulating, so plan around your monthly volume instead of stockpiling. Annual plans have one useful trick: you can renew early on your next billing date to get a fresh batch of annual credits if you burn through them.

**Is 1,000 free credits per month enough for Amazon?** For testing and small scripts, yes — that's about 200 Amazon requests. For anything resembling continuous monitoring, you'll want Hobby or above within the first week.

**Does the free plan include Amazon parsing?** Yes. The `autoparse=true` parameter and Amazon structured output work on every plan, including the free one — the free tier limits you to 5 concurrent connections and 1,000 credits, not features.

## Before you decide

Match your monthly Amazon volume to the table above and the choice mostly makes itself: under ~200 pages, stay free; a few thousand, Hobby; country-level targeting or mid-five-figure request counts, Business or Scaling. Two things to verify before checkout: confirm your target Amazon marketplace request costs using the Domain Cost Estimator (it's free and takes a minute), and remember the refund window — paid plans carry a 7-day no-questions-asked refund, so a wrong plan guess isn't permanent. When you're ready, 👉 [compare all ScraperAPI plans and current annual pricing](https://www.scraperapi.com/pricing?fp_ref=coupons) and start with the tier that covers one month of your real workload, not your aspirational one.
