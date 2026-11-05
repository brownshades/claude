# S1 · Stuck TikTok Shop owners (like Silvia)

Run after 00. Paste the box into a new Claude Code chat.

```
You are the S1 lead builder for a US TikTok Shop management agency.
Work in ~/gtm/segments/s1 and use the shared modules in ~/gtm/shared
(read ~/gtm/shared/README.md first). If ~/gtm/shared is missing, stop and tell me
to run the foundation chat first.

## Who we want (S1)
US people who already have a verified TikTok Shop but have zero or few sales,
don't know what to sell, picked a crowded niche (often clothing), or are stuck
on creators, samples or account health. Our offer: we pick trending products,
list them, run the orders and creators; they keep the shop.
Pain points to tag: "no sales", "no views", "what should I sell", "sent samples,
no videos", "violation/health score", "late shipment penalty", "no time".

## Important limit
TikTok Shop product and shop pages are behind a slide-puzzle captcha for free
tools (measured Sept 2026). Do NOT try to scrape shop pages. We find these people
through TikTok videos, comments and profiles, and through Reddit.

## Step 1: TikTok videos about being stuck (no login)
Tool: github.com/omkarcloud/tiktok-scraper (LOCAL API: python run.py, then
http://localhost:8000; works from an ordinary residential IP without a proxy).
Fallbacks: github.com/davidteather/TikTok-Api (needs an ms_token cookie from a
LOGGED-OUT browser visit to tiktok.com, never a logged-in one) and
github.com/RomySaputraSihananda/tiktok-comment-scrapper for comments.
/search/videos (last 90 days) for:
  "tiktok shop no sales", "my tiktok shop has no sales", "tiktok shop help",
  "tiktok shop tips for beginners", "what to sell on tiktok shop",
  "tiktok shop samples no videos", "tiktok shop violation", "tiktok shop seller"
Results may be localized to our IP's country: check whether the API takes a
region/country parameter and set it to US if it does.

## Step 2: Comments = the real leads
For each video with 20+ comments, pull /videos/comments (and replies).
Use shared.llm to classify each comment:
  S1 (has a shop, stuck), S2 (wants to start, no shop yet: send to the S2 file),
  noise. Keep S1 with intent_score >= 3.
Examples of S1: "I have a shop but 0 sales", "approved last month, nothing sells",
"how do you get affiliates to post?".

## Step 3: Read the commenter's public profile
/users/profile: bio, bio link, followers, video count, region if exposed, and
check the profile data for seller/commerce fields (for example ttSeller or
commerce info; confirm the real field names on the first run and record them in
README). Follow the bio link with shared.site_enrich (Linktree/website) for an
email they published. US only: use region, bio location or their own words;
mark us_unknown if unclear and keep it.

## Step 4: Reddit (logged out)
Tool: github.com/omkarcloud/reddit-scraper (local API) or
github.com/Coding-Doctor-Omar/reddrix. Confirm subreddits exist first
(r/TikTokShop and any similar seller subs that /search/subreddits finds).
Search the last 60 days for "no sales", "zero sales", "what should I sell",
"samples", "violation", "shop health", "need help". Keep sellers with a real
problem; quote them in signal.

## Step 5: Score and first line
shared.llm.score_and_write(lead, segment_brief=S1). Reply-style, helpful, quotes
their problem: "Saw your comment about 0 sales since you got approved. That's
usually the product choice, not you." No income promises, ever.

## Output
shared.schema.write_leads(df, "S1"), channel=tiktok or reddit, handle,
profile_url, source_url=the exact comment/post link, signal=short quote,
pain_point, first_line, email only if they published it.
Print: videos scanned, comments read, S1 kept, S2 handed off, with email.

## Rules
Logged out only. No posting, liking, following or messaging: discovery only.
Store only public handle, profile link, public words and a self-published email.
No personal phone numbers. Polite rates, checkpoints, resume, skip on blocks.
First run: 3 search terms end to end, show me 20 sample rows, then wait.
```
