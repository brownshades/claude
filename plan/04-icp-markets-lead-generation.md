# 04. Who to Target, Where, and How to Find Them for Free

## Countries: where to focus for the 19 days

| Rank | Market | Why | Channel |
|---|---|---|---|
| **1** | **United Kingdom** (+ Ireland) | TikTok Shop UK has been live since 2021 and is mature. English. **Business hours (2 to 10 PM PKT) fit our day perfectly.** Big South Asian business community (Urdu rapport). | Cold calls (main), email, DMs |
| **2** | **United States** | Biggest TikTok Shop market, most brands, biggest budgets. The ban question is settled: TikTok's US joint venture closed in January 2026. Time zone is harder (7 PM PKT = 9 AM Eastern). | Cold email (main), calls 7 to 9 PM PKT, Upwork |
| 3 | EU (Germany, France, Italy, Spain, Netherlands, etc.) | TikTok Shop expanded to ~10 European markets by June 2026. Language barrier. | Later. Only English-speaking leads for now |
| ✗ | Australia | TikTok Shop is **not live** there yet (no launch date announced). | Skip |
| ✗ | Pakistan | TikTok Shop does not operate in Pakistan, so local businesses can't be clients for this service. | Skip for TikTok Shop (fine for our other services) |
| ✗ | Southeast Asia | Live markets, but prices clients will pay are much lower. | Skip |

---

## Ideal clients, ranked (call the top tiers first)

| Tier | Who | Why they buy fast | Where to find them |
|---|---|---|---|
| **A1** | **Amazon FBA brand owners** (private label, US/UK), selling well on Amazon but **not on TikTok Shop** or barely active | They already have stock, photos, reviews and cash. They can ship TikTok orders **from their own FBA stock via Amazon MCF**, so there is no new warehouse. The founder decides alone. | Amazon category best-seller lists, seller profiles, brand websites |
| **A2** | **Shopify / DTC brands** in beauty, skincare, fashion, jewellery, home, pets, gifts, **running Meta ads** but not on TikTok Shop | Ads prove budget. They already make content. TikTok Shop is a new sales channel they keep hearing about. | Meta Ad Library, Google search, Instagram |
| **A3** | **South Asian / Pakistani-owned boutiques and brands in the UK and US** (lawn suits, bridal, modest wear, abayas, attar/perfume, jewellery, skincare) | **Urdu-speaking founders get instant trust (unity).** Many already sell on Instagram/WhatsApp and do lives. They hold their own stock. | Google Maps (Southall, Green Street, Birmingham, Manchester, Bradford, Leicester; Houston, Chicago Devon Ave, New Jersey, New York), Instagram |
| **B1** | **Existing TikTok Shop sellers who are stuck**: shop is live but low sales, few creators, weak listings | They already understand the platform; the pain is fresh. | TikTok search and Shop tab (look for low "sold" counts), TikTok Shop seller groups |
| **B2** | **Companies hiring a TikTok Shop manager** (job posts on Upwork, LinkedIn, Indeed) | They have budget and urgency **right now**. We cost less than a hire. | Upwork, LinkedIn Jobs, Indeed US/UK |
| **B3** | **Self-published authors and small publishers** (romance, fantasy, self-help, children's) | BookTok sells books. Single decision-maker. **Lower budgets**, and they need their own printed stock (print-on-demand copies can't be shipped from Amazon to TikTok buyers). | Amazon KDP best-seller lists, author websites, r/selfpublish, Facebook author groups |
| **C** | Local product makers found on Google Maps (candles, gift shops, bakeries without perishables, pet treats, soap) | Owner decides, but often small. | Google Maps |

**Don't target now:** supplements and health products (TikTok category approval and claim rules), food that spoils, batteries/electronics with shipping limits, big brands with in-house teams, and anyone whose plan is "buy from Amazon/Walmart and ship to TikTok buyers" (policy risk, see below).

### Why Amazon FBA brands are our best target

- Our first client's model (buy on Amazon, ship to TikTok buyers) is risky: a recent report says TikTok Shop's US Order Packaging Requirements (Sept 2026) ban dropshipping directly from other retailers. **Confirm in Seller Center.**
- The legal version of "Amazon fulfilment" is a brand fulfilling TikTok orders from **its own** FBA stock with **Amazon Multi-Channel Fulfillment (MCF)**. Amazon officially supports this for TikTok Shop through connector apps (Pipe17, Rithum, WebBee, CedCommerce, ChannelEngine, Order Desk, 4Seller and others, as reported). Per the connector guides, the TikTok shipping option must be "Shipped by Seller", and TikTok must be blocked from Amazon Logistics in MCF settings before orders sync (otherwise TikTok rejects them). Use MCF's unbranded (blank box) packaging. This is a perfect pitch: **"Your FBA stock can fulfil TikTok orders. We set it all up and run it."**

### Best niches on TikTok Shop for Q4

Beauty and skincare, women's fashion and modest wear, jewellery and accessories, home and kitchen gadgets, pet products, toys and gifts, fitness accessories, books (BookTok), candles and home fragrance. **Q4 angle:** gift sets and bundles for December.

---

## Free lead generation methods

### 1. Google Maps (for calling lists)

**Searches to run** (one city at a time):

| UK cities | US cities |
|---|---|
| London (Southall, Wembley, Ilford, Green Street), Birmingham, Manchester, Bradford, Leicester, Leeds, Glasgow, Dublin (IE) | Houston, Dallas, Chicago, New York, New Jersey (Edison, Jersey City), Los Angeles, Atlanta, Miami |

| Search term | Tier |
|---|---|
| "Pakistani clothes shop", "Asian bridal boutique", "Indian clothing store", "abaya shop", "modest fashion", "attar perfume shop" | A3 |
| "women's boutique", "clothing boutique", "kids clothing store", "jewelry store" (independent), "jewelry designer" | A2 / C |
| "cosmetics store", "skin care products", "beauty supply store", "hair products store" | A2 |
| "candle store", "gift shop", "home decor store", "pet supply store" (independent) | C |

**Keep a lead only if:** independent (not a chain), has a **website or Instagram**, rating 4.0+ with 20+ reviews, sells **physical products they own**. Bonus: has a Shopify website, posts on Instagram/TikTok, no TikTok Shop yet.

**How to pull the data**
- **Manual (free, ~40 leads/hour):** copy name, phone, website, rating, reviews, address into the lead sheet.
- **Faster, still low/no cost:** the `google-maps-list-builder` skill from `growthenginenowoslawski/coldoutboundskills` (uses the Maps Data API on RapidAPI, which has a free tier), or the official **Google Places API** (has a free monthly usage allowance). Run it on the GPU laptop overnight. Then run the `icp-prompt-builder` skill on a sample of 50 to filter out chains and bad fits *before* spending time on them.

**Finding the decision-maker (owner) for free**
- UK limited companies: search the business on **Companies House** (free, official). It lists the **directors' names**. Ask for them by name on the call.
- Website "About us", Instagram bio, LinkedIn company page → "People".
- US: state Secretary of State business search often lists officers.
- On the call: "Who looks after your online sales, like the website and social media?"

### 2. Amazon brand research (for Tier A1, email and calls)

1. Open Amazon US or UK best sellers / "Movers & Shakers" in Beauty, Home & Kitchen, Pet Supplies, Toys, Clothing.
2. Pick products with **200 to 5,000 reviews** (established but not giant brands).
3. Click the seller name → seller profile → **"Detailed Seller Information"** shows the business name and address.
4. Find the brand's website and Instagram. Search the brand on TikTok: no Shop tab, or few sales = lead.
5. Find the founder on LinkedIn. Get the email (website, free tiers of Hunter/Apollo/Snov, or first@domain pattern) and **verify it** before sending.
6. Note the hook for email: product name, review count, "FBA, so MCF-ready", "not on TikTok Shop".

### 3. Meta Ad Library (for Tier A2)

Free at facebook.com/ads/library. Search niche keywords ("skincare", "lawn suits", "dog harness") in the UK or US. Brands running many active ads have budget. Check if they are on TikTok Shop. The `playbook-ad-library` skill from coldoutboundskills turns this into a one-line personalisation ("noticed you're running ads for your vitamin C serum").

### 4. TikTok itself (for Tier B1)

- Search niche keywords in TikTok and the Shop tab. Products with low "sold" counts, poor images or no creator videos = a stuck seller.
- **TikTok Creative Center** (free) shows top products and ads by country and category. Use it to show prospects "what's selling in your niche" on audit videos.

### 5. Shopify store discovery (for Tier A2)

- Google: `site:myshopify.com skincare` , `"powered by shopify" lawn suits uk`
- The `playbook-tech-on-website` skill (coldoutboundskills) checks if a site runs Shopify.

### 6. Hiring signals (for Tier B2), highest intent

- **Upwork:** search "TikTok Shop" jobs daily; send 10 tailored proposals. Also list a Fiverr gig for inbound.
- **LinkedIn Jobs / Indeed US + UK:** search "TikTok Shop manager", "TikTok Shop specialist", "TikTok Shop account manager". Find the founder or hiring manager and pitch: "Saw you're hiring a TikTok Shop manager. We run shops for $400/month, and you can start this week."
- The `playbook-job-posting-language` skill does this at scale.

### 7. Communities (manual, helpful, no spam)

Reddit (r/TikTokShop, r/FulfillmentByAmazon, r/AmazonSeller, r/shopify, r/selfpublish), Facebook groups for TikTok Shop sellers UK/US, Amazon FBA sellers, Shopify owners. **Answer questions well**, post a useful guide (e.g. "How to fulfil TikTok Shop orders from FBA stock"), and only DM people who ask for help.

### 8. Warm network and referrals (fastest, start Day 1)

- Every founder messages **50 people** in their contacts: "Do you know anyone in the UK or US who sells products online? We help them start selling on TikTok Shop." Friends and family abroad often know shop owners.
- Ask the first client for referrals; offer **one free month for every referral that signs**.

---

## Lead database (one Google Sheet for everyone)

Template: [templates/lead-database.csv](../templates/lead-database.csv). Columns:

`lead_id, date_added, added_by, source, market, tier, niche, business_name, website, phone, email, email_verified, decision_maker, title, linkedin, instagram, tiktok, on_tiktok_shop, amazon_url, signal, tps_checked, owner, status, last_touch, next_action, next_action_date, notes`

**Status stages:** `new → called → connected → meeting_booked → meeting_held → proposal_sent → won / lost / nurture_30d / do_not_contact`

**Rules**
- One owner per lead. No double-calling.
- `do_not_contact` is permanent and respected on every channel.
- Every lead that is not `won/lost/do_not_contact` has a `next_action_date`.
- Founder 4 de-duplicates every night.
