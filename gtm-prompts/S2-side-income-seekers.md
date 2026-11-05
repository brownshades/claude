# S2 · Side-income seekers who want to start a TikTok Shop

Run after 00 (and ideally after S1, which hands S2 comments over). Paste the box
into a new Claude Code chat.

```
You are the S2 lead builder for a US TikTok Shop management agency.
Work in ~/gtm/segments/s2 and use the shared modules in ~/gtm/shared
(read ~/gtm/shared/README.md first). If ~/gtm/shared is missing, stop and tell me
to run the foundation chat first.

## Who we want (S2)
US adults who want to start selling on TikTok Shop (or a side business) and are
asking how: no products, no sourcing, no idea where to begin. They can open a US
TikTok Shop themselves (US citizen/resident or US business). Our offer: we help
them set it up and we run products, listings, orders and creators.
Pain points to tag: "where do I start", "what should I sell", "is it worth it",
"how do I get approved", "no time", "scared of getting banned".
Exclude: people selling courses, coaching, "automation" or agency services
(competitors), and anyone outside the US.

## Step 1: Reddit (logged out)
Tool: github.com/omkarcloud/reddit-scraper (LOCAL API, python run.py) or
github.com/Coding-Doctor-Omar/reddrix as fallback. Confirm each subreddit
exists and is active first:
  r/sidehustle, r/Entrepreneur, r/smallbusiness, r/ecommerce, r/WorkOnline,
  r/TikTokShop, r/passive_income, r/dropship
Search the last 30 days: "tiktok shop", "start selling online", "what should I
sell", "side hustle ideas", "is tiktok shop worth it", "how to start tiktok shop".
Pull comments on posts with 10+ comments: the askers in comments are often
better leads than the poster.

## Step 2: TikTok comments (logged out)
Read ~/gtm/segments/s1 handoff file of S2-classified comments if it exists.
Then github.com/omkarcloud/tiktok-scraper /search/videos for "how to start tiktok
shop", "tiktok shop for beginners", "tiktok shop side hustle", "is tiktok shop
worth it", and /videos/comments on videos with 20+ comments. Classify with
shared.llm; keep "wants to start" with intent_score >= 3. Read /users/profile
for bio, bio link and region.

## Step 3: Facebook groups and Discord servers (discovery only)
- shared.search.web_search for site:facebook.com/groups ("tiktok shop" OR
  "side hustle" OR "make money online" OR "ecommerce beginners") (USA OR
  "United States"). Keep name, URL, member count and description from the
  snippet or public meta tags. Groups need a login to read, so the output is a
  join list for founders.
- shared.search.web_search for site:disboard.org ("tiktok shop" OR ecommerce OR
  "side hustle") and "discord.gg" "tiktok shop". Confirm invites with Discord's
  public endpoint https://discord.com/api/v10/invites/<code>?with_counts=true (no
  auth): name, description, member counts. Output is a join list. No joining,
  no member lists, no DMs.

## Step 4: Score and first line
shared.llm.score_and_write(lead, segment_brief=S2). Helpful, reply-style, about
their question: "Saw you asking what to sell on TikTok Shop. Happy to share what's
working for a shop we run right now." No income promises or earnings claims.

## Output
shared.schema.write_leads(df, "S2"):
- people: channel=reddit or tiktok, handle, profile_url, source_url=exact
  post/comment link, signal=short quote, pain_point, first_line, email only if
  self-published in their bio
- groups/servers: channel=facebook or discord, business_name, source_url or
  discord_invite, followers=member count, status="to_join"
Print counts per source.

## Rules
Logged out only. Discovery only: no posting, voting, joining, following or DMs.
Store only public handle, profile link, public words and a self-published email.
No personal phone numbers. Polite rates, checkpoints, resume, skip on blocks.
First run: 2 subreddits + 2 TikTok searches + 5 group/server searches, show me
samples, then wait for my OK.
```
