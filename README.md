# Spider Browser Dataset & Web Scraping Examples

Benchmark data and ready-to-run web scraping examples for [spider-browser](https://github.com/spider-rs/spider-browser) — a browser automation library with built-in stealth, anti-bot bypass, and smart retry across 6 browser backends.

## Web Scraping Examples

Production-ready scraping scripts for 20+ popular websites. Each example is a standalone TypeScript file using the [spider-browser](https://www.npmjs.com/package/spider-browser) SDK.

### Quick Start

```bash
cd examples
npm install
SPIDER_API_KEY=sk-... npx tsx amazon-scraper.ts
```

### E-Commerce Scrapers

| Example | Target | Method | Data Extracted |
|---------|--------|--------|----------------|
| [Amazon Scraper](examples/amazon-scraper.ts) | amazon.com | `extractFields()` | Title, price, rating, reviews, availability, images |
| [Walmart Scraper](examples/walmart-scraper.ts) | walmart.com | `extractFields()` | Name, price, rating, fulfillment status |
| [eBay Scraper](examples/ebay-scraper.ts) | ebay.com | `evaluate()` | Listing title, price, shipping, condition |
| [Target Scraper](examples/target-scraper.ts) | target.com | `extractFields()` | Name, price, rating, fulfillment options |
| [Costco Scraper](examples/costco-scraper.ts) | costco.com | `evaluate()` | Product name, price, rating |
| [Home Depot Scraper](examples/homedepot-scraper.ts) | homedepot.com | `extractFields()` | Name, price, model number, rating, reviews |

### Search Scrapers

| Example | Target | Method | Data Extracted |
|---------|--------|--------|----------------|
| [Google Search Scraper](examples/google-search-scraper.ts) | google.com | `evaluate()` | Result title, URL, snippet, featured snippet |
| [Google Play Scraper](examples/google-play-scraper.ts) | play.google.com | `extractFields()` | App name, developer, rating, genre, description |

### Job Scrapers

| Example | Target | Method | Data Extracted |
|---------|--------|--------|----------------|
| [Google Jobs Scraper](examples/google-jobs-scraper.ts) | google.com/jobs | `evaluate()` | Job title, company, location |
| [Indeed Scraper](examples/indeed-scraper.ts) | indeed.com | `evaluate()` | Job title, company, location, salary |
| [Glassdoor Scraper](examples/glassdoor-scraper.ts) | glassdoor.com | `evaluate()` | Review title, rating, pros, cons |

### Travel & Real Estate Scrapers

| Example | Target | Method | Data Extracted |
|---------|--------|--------|----------------|
| [Expedia Scraper](examples/expedia-scraper.ts) | expedia.com | `evaluate()` | Hotel name, price, rating |
| [Zillow Scraper](examples/zillow-scraper.ts) | zillow.com | `evaluate()` | Address, price, property details |
| [Airbnb Scraper](examples/airbnb-scraper.ts) | airbnb.com | `evaluate()` | Listing title, price per night, rating |
| [TripAdvisor Scraper](examples/tripadvisor-scraper.ts) | tripadvisor.com | `evaluate()` | Restaurant name, rating, reviews, cuisine |

### News & Media Scrapers

| Example | Target | Method | Data Extracted |
|---------|--------|--------|----------------|
| [Google News Scraper](examples/google-news-scraper.ts) | news.google.com | `evaluate()` | Headline, source, publication time |
| [YouTube Scraper](examples/youtube-scraper.ts) | youtube.com | `extractFields()` | Title, channel, views, date, description |

### Social Scrapers

| Example | Target | Method | Data Extracted |
|---------|--------|--------|----------------|
| [Reddit Scraper](examples/reddit-scraper.ts) | reddit.com | `evaluate()` | Post title, score, comments, author |
| [LinkedIn Scraper](examples/linkedin-scraper.ts) | linkedin.com | `extractFields()` | Company name, industry, size, description |

### Other Scrapers

| Example | Target | Method | Data Extracted |
|---------|--------|--------|----------------|
| [Yelp Scraper](examples/yelp-scraper.ts) | yelp.com | `evaluate()` | Business name, rating, reviews, categories |
| [ChatGPT Scraper](examples/chatgpt-scraper.ts) | chatgpt.com | `evaluate()` | Conversation turns, roles, content |

### extractFields vs evaluate

**`extractFields()`** — Use for single-element extraction. Pass a map of CSS selectors, get back a typed object. Supports both text content and attribute extraction:

```typescript
const data = await page.extractFields({
  title: "#productTitle",                                    // text content
  price: ".a-price .a-offscreen",                            // text content
  image: { selector: "#landingImage", attribute: "src" },    // attribute
});
// { title: "AirPods Pro", price: "$189.99", image: "https://..." }
```

**`evaluate()`** — Use for list extraction where you need to iterate over multiple elements:

```typescript
const data = await page.evaluate(`(() => {
  const items = [];
  document.querySelectorAll(".product").forEach(el => {
    const name = el.querySelector(".name")?.textContent?.trim();
    const price = el.querySelector(".price")?.textContent;
    if (name) items.push({ name, price });
  });
  return JSON.stringify(items);
})()`);
```

## Benchmark Dataset

One of the toughest browser automation benchmarks available. 999 URLs across 327 domains spanning 18 categories — from simple static pages to sites behind aggressive WAFs (Akamai, PerimeterX, DataDome) with heavy fingerprinting.

### Files

| File | Description |
|------|-------------|
| `domains.csv` | 327 domains with category, difficulty, and search keywords |
| `urls.csv` | 1,783 URLs with domain, category, difficulty, and page type |
| `results.csv` | Latest benchmark results (999 URLs) |
| `latest-summary.json` | Summary stats from the latest run |

### Showcase Recordings

Browser automation recordings demonstrating real workflows. Captured at 5 FPS / 90% JPEG quality using the built-in screencast recording feature.

**TechCrunch Browse** — Tech news with hero images, article navigation

![techcrunch-browse](showcase/techcrunch-browse.gif)

**NYT Headlines** — Bot protection bypass + headline extraction + smooth scrolling

![nyt-headlines](showcase/nyt-headlines.gif)

**Stack Overflow Browse** — Questions list, click into Q&A, scroll through answers

![stackoverflow-browse](showcase/stackoverflow-browse.gif)

### Benchmark Results

**100% pass rate** (999/999) across 327 domains — including sites behind aggressive WAFs (Akamai, PerimeterX, DataDome) with heavy fingerprinting.

| Metric | Value |
|--------|-------|
| Total URLs | 999 |
| Concurrency | 25 |
| Elapsed time | ~19 min |
| Avg page time | 16.0s |
| Median page time | 11.5s |
| p95 page time | 39.3s |
| Fastest page | 0.9s |
| Slowest page | 79.7s |

**Timing breakdown (avg):** connect 5.7s, navigate 4.8s, content 1.5s, screenshot 0.9s

### Categories

| Category | Examples |
|----------|----------|
| E-Commerce | amazon, ebay, walmart, target |
| News | cnn, bbc, nytimes, reuters |
| Technology | github, stackoverflow, medium |
| Finance | bloomberg, coindesk, yahoo finance |
| Social | reddit, twitter, linkedin |
| Travel | booking, tripadvisor, airbnb |
| Entertainment | youtube, twitch, spotify |
| Food | allrecipes, bonappetit, epicurious |
| Health | webmd, mayoclinic, healthline |
| Real Estate | zillow, realtor, redfin |

### Difficulty Levels

- **easy** — Static sites, minimal bot protection
- **medium** — SPAs, moderate WAF (Cloudflare)
- **hard** — Heavy WAF (Akamai, PerimeterX, DataDome), aggressive fingerprinting

## Running the Benchmark

```bash
cd spider-browser/typescript
SPIDER_API_KEY=sk-... npx tsx __tests__/stealth-test.ts --target=200

# Full 1000-URL benchmark
SPIDER_API_KEY=sk-... npx tsx __tests__/stealth-test.ts --target=1000 --concurrency=25

# Retry only failed URLs
SPIDER_API_KEY=sk-... npx tsx __tests__/stealth-test.ts --retry-csv=path/to/results.csv
```

## SDKs

spider-browser is available for TypeScript, Python, and Rust:

| SDK | Package | Install |
|-----|---------|---------|
| TypeScript | [spider-browser](https://www.npmjs.com/package/spider-browser) | `npm install spider-browser` |
| Python | [spider-browser](https://pypi.org/project/spider-browser/) | `pip install spider-browser` |
| Rust | [spider-browser](https://crates.io/crates/spider-browser) | `cargo add spider-browser` |

## License

MIT
