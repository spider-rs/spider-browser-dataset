# Spider Browser Dataset

One of the toughest browser automation benchmarks available. 999 URLs across 327 domains spanning 18 categories — from simple static pages to sites behind aggressive WAFs (Akamai, PerimeterX, DataDome) with heavy fingerprinting. Designed to stress-test real-world reliability, not just happy-path demos.

Used to measure [spider-browser](https://github.com/spider-rs/spider-browser) success rates against production websites.

## Files

| File | Description |
|------|-------------|
| `domains.csv` | 327 domains with category, difficulty, and search keywords |
| `urls.csv` | 1,783 URLs with domain, category, difficulty, and page type |
| `results.csv` | Latest benchmark results (999 URLs) |
| `latest-summary.json` | Summary stats from the latest run |

## Showcase Recordings

Browser automation recordings demonstrating real workflows via the [spider-browser](https://github.com/spider-rs/spider-browser) TypeScript SDK. Captured at 5 FPS / 90% JPEG quality using the built-in screencast recording feature.

**TechCrunch Browse** — Tech news with hero images, article navigation

![techcrunch-browse](showcase/techcrunch-browse.gif)

**NYT Headlines** — Bot protection bypass + headline extraction + smooth scrolling

![nyt-headlines](showcase/nyt-headlines.gif)

**Stack Overflow Browse** — Questions list, click into Q&A, scroll through answers

![stackoverflow-browse](showcase/stackoverflow-browse.gif)

## Results

**100% pass rate** (999/999) across 327 domains — including sites behind aggressive WAFs (Akamai, PerimeterX, DataDome) with heavy fingerprinting.

### Benchmark Performance

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

## Usage

```bash
# Run the benchmark
cd spider-browser/typescript
SPIDER_API_KEY=sk-... npx tsx __tests__/stealth-test.ts --target=200

# Run full 1000-URL benchmark
SPIDER_API_KEY=sk-... npx tsx __tests__/stealth-test.ts --target=1000 --concurrency=25

# Retry only failed URLs from a previous run
SPIDER_API_KEY=sk-... npx tsx __tests__/stealth-test.ts --retry-csv=path/to/results.csv
```

## CSV Format

### urls.csv
```
url,domain,category,difficulty,page_type,passed,browser_used,content_length,title,content_preview,duration_ms
```

### results.csv
```
url,domain,category,difficulty,page_type,browser_used,passed,blocked,title,content_length,has_screenshot,content_preview,error,duration_ms,connect_ms,navigate_ms,content_ms,screenshot_ms,credits_used,cost_usd
```

## License

MIT
