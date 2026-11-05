# GTM lead engine: Claude Code chat prompts (USA, free, open source, no login)

One chat per file. Run them in this order:

| Order | File | Segment | Why this order |
|---|---|---|---|
| 1 | [00-foundation-setup.md](00-foundation-setup.md) | Shared tools + one lead format + daily merge | Every other chat imports it |
| 2 | [S6-meta-advertisers-no-tiktok-shop.md](S6-meta-advertisers-no-tiktok-shop.md) | Brands running Meta ads, no TikTok Shop | Most reliable no-login source (Ad Library scraper measured working logged out, Oct 2026) |
| 3 | [S5-amazon-brands-no-tiktok-shop.md](S5-amazon-brands-no-tiktok-shop.md) | Amazon brands, no TikTok Shop | Public best-seller and seller pages; gives US state for the call list |
| 4 | [S4-new-instagram-businesses.md](S4-new-instagram-businesses.md) | New small businesses on Instagram | Reuses S6 small advertisers; slow because anonymous Instagram is rate-limited |
| 5 | [S3-flippers-resellers.md](S3-flippers-resellers.md) | Flippers (people) + resale shops (storefronts) | Reddit + Discord/Facebook join lists + Google Maps |
| 6 | [S1-stuck-tiktok-shop-owners.md](S1-stuck-tiktok-shop-owners.md) | Verified TikTok Shop, no sales | TikTok comments + Reddit; hands "wants to start" people to S2 |
| 7 | [S2-side-income-seekers.md](S2-side-income-seekers.md) | Want to start a TikTok Shop | Uses S1's handoff file |

The cold-calling phone chat (your Gemini-upgraded prompt) runs after these and reads `~/gtm/queues/calls.csv`.

## Daily loop
1. Each segment chat runs its job (overnight on the GPU laptop).
2. `python ~/gtm/shared/merge.py` dedupes everything and writes `~/gtm/queues/`:
   `reddit.csv, tiktok.csv, instagram.csv, facebook.csv, discord.csv` (manual DM queues with link + quote + first line), `email.csv` (to your email automation), `calls.csv` (business phones by state/city).
3. Founders open their channel queue and send by hand.

## What works without a login (checked against each repo's README, Nov 2026)

| Source | No login? | Repo | Notes |
|---|---|---|---|
| Meta Ad Library (FB + IG ads) | Yes | `2scraper/facebook-ads-scraper`, `promisingcoder/MetaAdsCollector` (also an MCP server), `athm793/meta-ads-scraper` | Gives page, ad count, run dates, landing URL |
| Facebook Pages | Partly | via the Ad Library results | Page details sit behind a login wall; get contacts from the website |
| Facebook groups | Discovery only | `deedy5/ddgs` search (`site:facebook.com/groups ...`) | Posts and members need a login: founders join and read by hand |
| Instagram profiles | Yes, slowly | `instaloader/instaloader` (anonymous) | About 1 profile per 30–60 s |
| Instagram hashtags | **No** | | Instaloader docs: hashtags require login. Discover profiles with search engines instead |
| Reddit | Yes | `omkarcloud/reddit-scraper` (local API), `Coding-Doctor-Omar/reddrix` | Reddit's old `.json` endpoints are now login-walled |
| TikTok videos, comments, profiles | Yes | `omkarcloud/tiktok-scraper` (local API), `davidteather/TikTok-Api`, `RomySaputraSihananda/tiktok-comment-scrapper` | TikTok-Api needs an `ms_token` from a logged-out visit |
| TikTok Shop product/shop pages | **No** (captcha) | | Measured Sept 2026: blocked for every free setup. Check TikTok Shop presence from websites, search and profiles instead |
| Discord servers | Discovery only | `ddgs` search + Discord's public invite endpoint | Members and messages need joining: founders do it by hand |
| Amazon best sellers + seller info | Yes | `omkarcloud/amazon-scraper` (local API, has `/sellers/details`) | Slow rate or captchas |
| Google Maps | Yes | `gosom/google-maps-scraper` | Has its own timezone field |
| Shopify stores | Yes | `ahmedxomar101/shopify-domain-detector`, public `/products.json` | `/products.json` dates show store age |
| Websites (email, phone, owner) | Yes | `unclecode/crawl4ai` | Pairs with local Ollama for extraction |

## Repos to skip
- `vooltex8egp/tiktok-shop-scraper`: the repo is empty (only a LICENSE file).
- `angeloestelle88/Amazon-Bestsellers-Scraper`: promo links to Telegram/Discord and a mismatched image. It looks like a template repo; use `omkarcloud/amazon-scraper` instead.
- `2scraper/tiktok-shop-scraper`: honest README, but it says it only works through a paid scraping browser.
- Any Discord member-list scraper, Facebook group member scraper, or anything that needs your logged-in cookies.

Note: the `omkarcloud/*` repos are new and have few stars. Each prompt tests them on a small run first and has a fallback.
