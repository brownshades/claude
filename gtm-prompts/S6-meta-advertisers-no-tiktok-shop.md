# S6 · Brands running Meta ads with no TikTok Shop

Run after 00. Paste the box into a new Claude Code chat.

```
You are the S6 lead builder for a US TikTok Shop management agency.
Work in ~/gtm/segments/s6 and use the shared modules in ~/gtm/shared
(read ~/gtm/shared/README.md first). If ~/gtm/shared is missing, stop and tell me
to run the foundation chat first.

## Who we want (S6)
US e-commerce brands selling PHYSICAL products that are actively paying for
Facebook/Instagram ads but are NOT selling on TikTok Shop.
Pain points to tag: rising Meta ad costs (CPM/CAC), ad creatives wearing out,
constant need for new UGC videos, heavy discounting to convert. Our angle: on
TikTok Shop, creators make the videos and are paid commission only per sale.

## Step 1: Pull advertisers from the Meta Ad Library (no login)
Primary: github.com/2scraper/facebook-ads-scraper (reads the Ad Library logged
out; Playwright engine; --region US). Backup: github.com/promisingcoder/MetaAdsCollector
(pip install meta-ads-collector; CLI: python -m meta_ads_collector -q "<kw>" -c US
-n <N> -o out.csv). Read each README and confirm the current flags before running.
Keywords (one run each, active ads only, US):
  skincare, serum, lip gloss, hair growth, lash, jewelry, necklace, earrings,
  candle, home decor, kitchen gadget, dog harness, cat toy, baby, toddler,
  activewear, leggings, swimwear, boutique, modest fashion, phone case,
  "free shipping", "shop now", "limited stock", "new collection"
Start with 3 keywords x 100 ads. Scale only after I approve the sample.

## Step 2: Group by advertiser and qualify
Group ads by Facebook Page. Per advertiser keep: page_name, page_url, page_likes,
active_ad_count, oldest_active_ad_start_date, landing_domain, ad_text samples, CTA.
Keep the advertiser only if ALL are true:
- active_ad_count >= 5 and at least one ad running >= 14 days (they have budget)
- landing page is their own store (not Amazon, Etsy, a marketplace, an app store,
  a course, a service, a lead form, or political/issue ads)
- page_likes under ~500k (skip giant brands with in-house teams)
- English copy aimed at the US
Save rejects with the reason in raw/rejects.csv so we can tune rules.

## Step 3: Enrich the website
For each kept advertiser run shared.site_enrich.enrich_website(landing_domain):
platform (Shopify?), has_meta_pixel, has_tiktok_pixel, emails, phones, address,
city/state, owner name, instagram/tiktok links, product_count.
Optional: github.com/athm793/meta-ads-scraper can match the brand's FB/IG handles
from its website; use it only if handles are missing.

## Step 4: TikTok Shop check
shared.tiktok_check.check_tiktok_shop(brand, domain, tiktok_handle).
Keep "no" and "unknown". Drop "yes" (move them to raw/s6_has_tiktok_shop.csv:
they are later leads for "we'll grow your TikTok Shop").

## Step 5: Contacts
shared.contacts.find_email(domain, owner_name). Phone from the website (business
line only), normalized to E.164 with state/city. If no phone and we have the city,
note it so the phone chat can look them up on Google Maps.

## Step 6: Score and write the first line
shared.llm.score_and_write(lead, segment_brief=S6). The first_line must use
their real evidence, e.g. "Saw you're running 12 ads for your vitamin C serum
on Instagram." Never invent numbers.

## Output
shared.schema.write_leads(df, "S6") with segment=S6, channel=email (plus
facebook if they have a page, so the Page "Message" button is in the Facebook
queue), source=meta_ad_library, source_url=Ad Library link, signal like
"14 active ads, oldest 41 days, Shopify, no TikTok Shop found".
Print: advertisers scanned, kept, with email, with phone, by state.

## Rules
No login, no cookies, no captcha solving, no messaging anyone. Slow and polite:
small batches, random delays, resume from checkpoint. If the Ad Library blocks
us, stop that run, report it, and suggest waiting rather than pushing harder.
First run: 3 keywords end to end, show me 15 sample rows, then wait for my OK.
```
