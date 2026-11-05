# 00 · Foundation setup (run this chat FIRST, once)

Paste everything inside the box into a new Claude Code chat on the GPU laptop.

```
You are the lead-engineering setup agent for a US TikTok Shop management agency.
Build the shared foundation that six separate "segment" chats will use. Each
segment chat will write leads in ONE common format so everything merges cleanly.
Use only free, open-source tools. No logins to any social platform, ever.

## Workspace
Create this layout in ~/gtm (ask me before using a different path):
  ~/gtm/shared/        shared Python modules (you write these)
  ~/gtm/segments/s1 .. s6/   one folder per segment chat (empty for now)
  ~/gtm/leads/         each segment writes leads/sN_YYYY-MM-DD.csv here
  ~/gtm/queues/        merged, routed output (built by merge.py)
  ~/gtm/config/        do_not_contact.csv, settings.toml
  ~/gtm/raw/           raw scraper output, never deleted
One Python venv at ~/gtm/.venv shared by all chats.

## Install and smoke-test each tool (show me the test result for each)
- github.com/unclecode/crawl4ai  (pip install crawl4ai; run its setup/doctor command)
- github.com/deedy5/ddgs          (pip install ddgs). Free metasearch. Test one
  query with region="us-en". Use several backends ("duckduckgo,bing,brave,mojeek")
  so one rate-limit doesn't stop us.
- github.com/ahmedxomar101/shopify-domain-detector (pip install from the repo tag in
  its README). Test on gymshark.com (should be confirmed-shopify).
- github.com/s0md3v/wappalyzer-next (pip install wappalyzer + playwright chromium).
  Used to detect Meta Pixel / TikTok Pixel / store platform.
- github.com/john-kurkowski/tldextract, github.com/rapidfuzz/RapidFuzz,
  phonenumbers, pandas, pydantic.
- github.com/ollama/ollama with one local instruct model that fits my GPU
  (e.g. qwen3:8b or llama3.1:8b). Test one short JSON-mode prompt.
- github.com/batuhanaky/mailscout (email pattern generation).
- github.com/reacherhq/check-if-email-exists (Docker backend). NOTE: it needs
  outbound port 25. First test whether port 25 is open from this machine. If it
  is blocked, don't fight it: write verify_email() so it returns "unverified" and
  tell me; I'll run Reacher on a small cloud server later.
- github.com/laramies/theHarvester (domain email discovery from public sources).

## Shared modules to write in ~/gtm/shared
1. schema.py: the ONE lead format. Columns, in this order:
   lead_id, segment, channel, source, source_url, profile_url, handle, name,
   business_name, website, domain, email, email_status, phone_e164, city, state,
   instagram, facebook, tiktok, discord_invite, followers, last_active,
   signal, pain_point, intent_score, first_line, on_tiktok_shop, found_at,
   status, assigned_to, notes
   - segment: S1..S6. channel: reddit/tiktok/instagram/facebook/discord/email/call
   - signal = the exact evidence (quote of what they said, ad count, review count)
   - intent_score 1 to 5. status starts as "new".
   - lead_id = stable hash of (segment + best unique key: domain, else handle+platform).
   Provide write_leads(df, segment) that validates columns and appends to
   ~/gtm/leads/{segment}_{date}.csv.
2. search.py: web_search(query, max_results) wrapper around ddgs with backend
   rotation, 3 to 6 s random delay between calls, exponential backoff on
   rate-limit, and a local cache (sqlite) so we never repeat the same query.
3. site_enrich.py: enrich_website(url) using crawl4ai:
   - find contact/about/policy pages from the homepage links or sitemap
     (crawl4ai AsyncUrlSeeder) instead of guessing paths
   - extract emails, phones, street address, city, state, social links
     (instagram/facebook/tiktok/linktree), founder/owner name
   - use crawl4ai's LLM extraction with a Pydantic schema pointed at the local
     Ollama model, with regex as the first pass and the LLM only for owner name
     and anything regex missed
   - also return: is_shopify (detector), platform, has_meta_pixel,
     has_tiktok_pixel (wappalyzer-next or HTML check for fbevents.js and
     analytics.tiktok.com), links_to_tiktok_shop (any shop.tiktok.com or
     tiktok.com/@.../shop style links)
   - if it's Shopify, read /products.json?limit=250 (public) and return
     product_count and earliest product created_at (a store-age signal)
4. tiktok_check.py: check_tiktok_shop(brand_name, domain, known_handle=None)
   returns yes/no/unknown with evidence. IMPORTANT: TikTok Shop product pages are
   behind a slide-puzzle captcha for free tools (measured Sept 2026), so do NOT
   scrape shop pages. Use instead: links on their website, web_search for
   site:tiktok.com "<brand>", and their TikTok profile via
   github.com/omkarcloud/tiktok-scraper (local API, no key) if a handle is known.
   When evidence is weak, return "unknown". Never block the pipeline on this.
5. contacts.py: find_email(domain, owner_name) = site emails first, then
   theHarvester, then mailscout patterns (first@, first.last@, firstlast@) checked
   with Reacher; normalize_phone() to E.164 with phonenumbers (US only);
   timezone from area code as a fallback.
6. llm.py: score_and_write(lead, segment_brief) asks the local model for JSON:
   {intent_score, pain_point, first_line, segment_fit, reason}. first_line must be
   short, plain, specific to their evidence, no hype, no fake claims.
7. merge.py (daily): read all ~/gtm/leads/*.csv, drop anything in
   config/do_not_contact.csv, normalize domains (tldextract) and phones,
   dedupe on domain, then phone, then handle+platform, then rapidfuzz match on
   business_name within the same city (>= 88). Keep the richest row and keep all
   source_urls in notes. Then write:
   queues/reddit.csv, tiktok.csv, instagram.csv, facebook.csv, discord.csv
   (manual DM queues: link, handle, signal, pain_point, first_line),
   queues/email.csv (verified or likely emails only),
   queues/calls.csv (business phones only, sorted by state then city),
   and print counts per queue and per segment.

## Rules for every module
- Public pages only, logged out. No account creation, no cookies from a
  logged-in session, no captcha solving, no posting or messaging anywhere.
- Polite rates: random delays, small batches, retries with backoff, and a
  checkpoint file so any job can resume. If a site blocks us, skip it, log it,
  move on.
- Individuals (people found on Reddit/TikTok/Discord/Facebook groups): store only
  their public handle, profile link, what they publicly wrote, and an email ONLY
  if they published it in their bio or site. Never hunt personal phone numbers.
- Every module gets a tiny test in ~/gtm/shared/tests and a README.md that says
  how each segment chat should import and use it.

## Done when
All tools pass their smoke test (or are clearly marked blocked with the reason),
all seven modules exist with passing tests, and you show me a one-row example
lead produced end to end from a real Shopify domain I give you.
```
