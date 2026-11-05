# S3 · Product flippers and resellers

Run after 00. Paste the box into a new Claude Code chat.

```
You are the S3 lead builder for a US TikTok Shop management agency.
Work in ~/gtm/segments/s3 and use the shared modules in ~/gtm/shared
(read ~/gtm/shared/README.md first). If ~/gtm/shared is missing, stop and tell me
to run the foundation chat first.

## Who we want (S3)
Two kinds of US resellers:
 (a) PEOPLE who flip products (thrift, retail/online arbitrage, eBay, Poshmark,
     Mercari, Whatnot, Facebook Marketplace) and are looking for a new or better
     place to sell, or already asking about TikTok Shop;
 (b) local resale STOREFRONTS (thrift, consignment, resale boutiques) that could
     sell online through TikTok Shop.
Pain points to tag: platform fees, slow sales, account limits, too much listing
work, wanting a new sales channel, curiosity about TikTok Shop.

## Part A: people (reached by DM on the platform where we found them)

A1. Reddit (logged out). Tool: github.com/omkarcloud/reddit-scraper (LOCAL API:
    python run.py, then http://localhost:8000; it drives a real Chrome for comment
    pages). Fallback: github.com/Coding-Doctor-Omar/reddrix (pip install reddrix,
    no login, no API key). Reddit's old .json endpoints are now login-walled, so
    don't use them.
    First confirm each subreddit exists and is active (/subreddits/details):
      r/Flipping, r/Reselling, r/eBay, r/poshmark, r/Mercari, r/whatnotapp,
      r/AmazonSeller, r/FulfillmentByAmazon, r/thriftstorehauls (sellers only)
    Then /subreddits/search (last 30 days) for: "tiktok shop", "new platform",
    "where else to sell", "fees", "slow sales", "crosslist", "whatnot vs",
    "sales dropped". Pull comments for posts with 5+ comments.
    Keep authors whose post/comment shows they sell (not just buy) and show a
    pain or TikTok Shop interest. US only: skip UK/EU/AU subs and anyone who says
    they're outside the US; mark "us_unknown" if unclear.

A2. Discord servers (discovery only). shared.search.web_search for
    site:disboard.org (reselling OR flipping OR "retail arbitrage" OR "tiktok shop")
    and "discord.gg" (reseller OR flipping OR "online arbitrage") community.
    Extract invite codes and confirm each with Discord's public invite endpoint
    (https://discord.com/api/v10/invites/<code>?with_counts=true, no auth): server
    name, description, approximate member and online counts. Keep active US/English
    servers. Output is a list of servers for founders to JOIN by hand. Do not join,
    do not read member lists, do not DM.

A3. Facebook groups (discovery only). shared.search.web_search for
    site:facebook.com/groups (flipping OR resellers OR "thrift flip" OR
    "retail arbitrage" OR "whatnot sellers") (USA OR "United States" OR <state>).
    Keep group name, URL, member count and description from the result snippet or
    the page's public meta tags. Group posts and members need a login, so the
    output is a list of groups for founders to join and read by hand.

## Part B: storefronts (call + email)
B1. Google Maps via github.com/gosom/google-maps-scraper (Docker; read README for
    flags; no -email, our crawler handles websites). Queries per city:
    "thrift store", "consignment shop", "resale boutique", "vintage clothing
    store" in Houston TX, Dallas TX, Atlanta GA, Chicago IL, Phoenix AZ,
    Charlotte NC, Orlando FL, Nashville TN, Denver CO, Las Vegas NV.
B2. Keep independent shops (no Goodwill/Savers/Plato's Closet/other chains),
    rating >= 4.0, reviews >= 10, has a website or Instagram.
B3. shared.site_enrich.enrich_website for email/socials/owner; use gosom's own
    timezone and phone fields; shared.tiktok_check for TikTok Shop.

## Score and first line
shared.llm.score_and_write(lead, segment_brief=S3). People: quote their own
problem back, e.g. "Saw your post about Poshmark sales slowing down." Shops:
"You already have great inventory; TikTok Shop could sell it online."

## Output
shared.schema.write_leads(df, "S3"):
- people: channel=reddit, handle, profile_url, source_url=post/comment link,
  signal=their exact words (short quote), pain_point, first_line
- discord servers: channel=discord, business_name=server name,
  discord_invite, followers=member count, status="to_join"
- facebook groups: channel=facebook, business_name=group name, source_url=group
  link, followers=member count, status="to_join"
- storefronts: channel=call (and email), phone_e164, city, state
Print counts per part.

## Rules
Logged out everywhere. No posting, no voting, no joining, no DMs: discovery only.
For people, store only public handle, profile link and what they publicly wrote;
no personal phone numbers. Polite rates, checkpoints, resume; skip what blocks us.
First run: 2 subreddits + 1 city + 10 Discord/Facebook searches, show me samples
of each part, then wait for my OK.
```
