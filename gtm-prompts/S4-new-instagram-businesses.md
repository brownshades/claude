# S4 · New businesses on Instagram

Run after 00. Paste the box into a new Claude Code chat.

```
You are the S4 lead builder for a US TikTok Shop management agency.
Work in ~/gtm/segments/s4 and use the shared modules in ~/gtm/shared
(read ~/gtm/shared/README.md first). If ~/gtm/shared is missing, stop and tell me
to run the foundation chat first.

## Who we want (S4)
US small businesses that launched in roughly the last 0 to 12 months, sell
PHYSICAL products (clothing, boutique, beauty, lip gloss, lashes, candles,
jewelry, accessories, home decor, kids), sell through Instagram/DMs or a simple
site, have under ~10k followers, and are not on TikTok Shop.
Pain points to tag: low reach, sales only through DMs, no checkout or a weak one,
can't afford ads, no time for content.

## Important limit
Instagram hashtag pages need a login, so we do NOT use hashtags. We find
profiles through public search engines and public link pages instead, then read
each public profile anonymously.

## Step 1: Discover profiles (no login)
A) shared.search.web_search with queries like:
   site:instagram.com "small business" "<city>"      (city list below)
   site:instagram.com boutique "<city>" "shop now"
   site:instagram.com "est. 2026" (boutique OR candles OR "lip gloss")
   site:instagram.com "launching soon" (candles OR jewelry OR boutique)
   site:instagram.com "DM to order" "<state>"
   site:linktr.ee (boutique OR candles OR "lip gloss") "<city>"
   Cities: Houston, Dallas, Atlanta, Charlotte, Chicago, Miami, Orlando, Phoenix,
   Los Angeles, New York, Philadelphia, Detroit, Memphis, Nashville, Las Vegas.
   Extract Instagram usernames from result URLs (skip /p/, /reel/, /explore/).
B) New Shopify stores: web_search('site:myshopify.com <niche> "<state>"') and
   brand sites that link to Instagram; then use the store's /products.json
   earliest created_at (via shared.site_enrich) to keep stores < 12 months old.
C) Small Meta advertisers from the S6 raw data (page_likes < 10k) that have an
   Instagram handle: read ~/gtm/segments/s6/raw if it exists.

## Step 2: Read each public profile anonymously
Tool: github.com/instaloader/instaloader (Python API, NO login). For each
username use Profile.from_username and read: full_name, biography, external_url,
followers, is_business_account, business_category_name, the date of the latest
post, and any public business email/phone the profile metadata exposes.
Anonymous access has strict rate limits: about 1 profile every 30 to 60 seconds,
stop for an hour on a 429/"please wait" message, cap at a few hundred per day.
Never log in, never use a session file.

## Step 3: Qualify
Keep only if: followers between ~100 and ~10,000; latest post within 14 days;
sells physical products (bio/category/website, ask shared.llm if unclear); US
location (bio, website address, or city in the search query that found them);
not a reseller of big brands, not MLM (skip "join my team" bios), not a coach or
service business.

## Step 4: Website, TikTok Shop check, contacts
If external_url exists: shared.site_enrich.enrich_website (Linktree pages too:
follow its shop/website link) for email, phone, city/state, Shopify, store age.
shared.tiktok_check.check_tiktok_shop: keep "no"/"unknown".
Email: bio or site email first; shared.contacts.find_email only if we have a
domain. Phone: business number from bio or site only.

## Step 5: Score and first line
shared.llm.score_and_write(lead, segment_brief=S4). Example: "Love the new
sunset candle drop. Are you selling on TikTok Shop yet?" Reference something real
from their bio or latest post.

## Output
shared.schema.write_leads(df, "S4"), channel=instagram (plus email / call when
we have them), source=instagram, profile_url, source_url=search result or
Linktree, signal like "1,240 followers, posted 3 days ago, Shopify store 4 months
old, Houston TX, no TikTok Shop".
Print counts: discovered, read, kept, with email, with phone, by city.

## Rules
Logged out only. No hashtag scraping, no follower scraping, no messaging.
Slow rates as above, checkpoints, resume. If Instagram blocks the IP, stop and
tell me. First run: 2 cities end to end, show me 15 sample rows, then wait.
```
