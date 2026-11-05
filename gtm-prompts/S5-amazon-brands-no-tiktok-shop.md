# S5 · Amazon brands with no TikTok Shop

Run after 00. Paste the box into a new Claude Code chat.

```
You are the S5 lead builder for a US TikTok Shop management agency.
Work in ~/gtm/segments/s5 and use the shared modules in ~/gtm/shared
(read ~/gtm/shared/README.md first). If ~/gtm/shared is missing, stop and tell me
to run the foundation chat first.

## Who we want (S5)
US private-label brands selling well on Amazon.com that are NOT on TikTok Shop.
Pain points to tag: rising Amazon fees and ad costs, all sales from one channel
(suspension risk), no time/team for TikTok, no idea how to get creators. Our
angle: their FBA stock can fulfil TikTok Shop orders through Amazon MCF, so no
new warehouse.

## Step 1: Pull best sellers (no login)
Tool: github.com/omkarcloud/amazon-scraper. It runs a LOCAL API (python run.py,
then http://localhost:8000). Read its README and use:
  /best-sellers/categories  to list category ids
  /best-sellers             Top 100 Best Sellers, New Releases, Movers & Shakers
  /products/details         seller name/id/link, brand, review count, rating, BSR
  /sellers/details          seller profile (business name and address, from the
                            "Detailed Seller Information" section)
Categories to start with: Beauty & Personal Care, Home & Kitchen, Pet Supplies,
Clothing/Shoes/Jewelry (women's jewelry, accessories), Baby, Sports & Outdoors
(fitness accessories), Toys & Games, Kitchen & Dining.
Marketplace: amazon.com only. If the local API breaks or is blocked, fall back
to crawl4ai on the same public pages at a slow rate.

## Step 2: Qualify
Keep a product's brand only if:
- review_count between 200 and 5,000 and rating >= 4.0
- the seller is the brand itself (seller name fuzzy-matches the brand, rapidfuzz
  >= 80) or the brand clearly owns the listing; skip resellers, Amazon-sold
  (Amazon.com as seller), and giant household names
- seller business address is in the US (keep the state, it feeds the call list)
- not supplements/medical claims, not perishable food, not battery-heavy
  electronics (TikTok category restrictions)
One row per BRAND (merge its products; keep the best seller as the example).

## Step 3: Find the brand's own website
shared.search.web_search('"<brand>" official site') plus '<brand> shop'; pick
the domain whose homepage mentions the brand and is not Amazon/Walmart/eBay.
Then shared.site_enrich.enrich_website(domain): emails, phones, address,
socials, owner/founder name, Shopify, pixels.

## Step 4: TikTok Shop check
shared.tiktok_check.check_tiktok_shop(brand, domain, tiktok_handle).
Keep "no" and "unknown"; park "yes" in raw/s5_has_tiktok_shop.csv.

## Step 5: Find the founder and their email
- Owner name from the website About page first.
- Then shared.search.web_search('site:linkedin.com/in "<brand>" (founder OR owner
  OR CEO)') and read only the search-result titles/snippets (don't open LinkedIn).
- shared.contacts.find_email(domain, owner_name), then email_status.
- Phone: website business line, else leave blank and add city/state for the
  phone chat.

## Step 6: Score and first line
shared.llm.score_and_write(lead, segment_brief=S5). Example first_line:
"Saw your silicone stretch lids have 1,850 reviews on Amazon, but I couldn't find
them on TikTok Shop." Only real numbers from the data.

## Output
shared.schema.write_leads(df, "S5"), channel=email (and call if a phone exists),
source=amazon, source_url=the product link, signal like
"BSR #6 in Women's Pendant Necklaces, 1,850 reviews, seller in TX, no TikTok Shop".
Print: brands scanned, kept, with website, with email, with phone, by state.

## Rules
Public pages only, logged out, no captcha solving. Amazon blocks fast scraping:
one request every few seconds, small batches, checkpoints, resume. On repeated
captchas, stop and tell me instead of retrying harder.
First run: 2 categories end to end, show me 15 sample rows, then wait for my OK.
```
