# 08. Automation Stack: Claude Code, the Repos, and the GPU Laptop

Automation should make **lists, research and copy** faster. It should not pretend to be a human on social platforms. Calls, meetings and closing stay human.

---

## The repos we picked: what each one is for

All of these exist and were read for this plan. Install the skills into `~/.claude/skills` on the machine running Claude Code.

| Repo | What we use it for | Who uses it |
|---|---|---|
| [rediumvex/hormozi-sales-cold-outreach-claude](https://github.com/rediumvex/hormozi-sales-cold-outreach-claude) | `alex-hormozi-pitch` (offer, guarantee, naming), `cold-outreach-board` (fixes for "no replies", "ghosted", "can't close"), `alex-hormozi-from-zero` (stage-based business advice) | Hamza, Zain |
| [coreyhaines31/marketingskills](https://github.com/coreyhaines31/marketingskills) | `cold-email` (copy rules, sequences, deliverability, reply handling), `sales-enablement` (call scripts, objection docs) | Hamza |
| [growthenginenowoslawski/coldoutboundskills](https://github.com/growthenginenowoslawski/coldoutboundskills) | `google-maps-list-builder`, `icp-prompt-builder` (qualify leads), `list-quality-scorecard`, `campaign-copywriting`, `email-deliverability-audit`, `spam-word-checker`, `positive-reply-scoring`, playbooks: `ad-library`, `job-posting-language`, `tech-on-website` | Founder 4, Hamza |
| [msitarzewski/agency-agents](https://github.com/msitarzewski/agency-agents) | Specialist personas (growth hacker, project manager etc.) for planning and reviewing work. Optional. | Anyone |
| [crevideo/crevideo-reach](https://github.com/crevideo/crevideo-reach) | TikTok Shop **creator (affiliate) outreach** packaged as a Claude plugin. For **client delivery**, not for finding clients. | Operator, Huraiz |
| [cogwork-labs/TikTok-Shop-Affiliate-Pipeline](https://github.com/cogwork-labs/TikTok-Shop-Affiliate-Pipeline), [hazelhu644-collab/tiktok-creator-sop-tool](https://github.com/hazelhu644-collab/tiktok-creator-sop-tool) | Tracking the creator pipeline (sourcing, invites, follow-ups, samples) | Operator |
| [1Milkdeliver/tiktok-shop-creator-scraper](https://github.com/1Milkdeliver/tiktok-shop-creator-scraper) | Finding creators for client products | Operator |

**Before connecting any creator tool to a client's shop:** Huraiz checks how it talks to TikTok. Prefer tools that use TikTok's official Seller Center / Affiliate Center and official APIs. TikTok's own **Affiliate Center target collaborations** already do invites at volume and are always safe. Never give a third-party tool the client's main login.

### Install (once)

```bash
mkdir -p ~/.claude/skills && cd ~/.claude/skills

# Hormozi skills
git clone https://github.com/rediumvex/hormozi-sales-cold-outreach-claude.git
mv hormozi-sales-cold-outreach-claude/alex-hormozi-pitch \
   hormozi-sales-cold-outreach-claude/alex-hormozi-from-zero \
   hormozi-sales-cold-outreach-claude/cold-outreach-board .

# Cold email + sales enablement
git clone --depth 1 https://github.com/coreyhaines31/marketingskills.git
cp -r marketingskills/skills/cold-email marketingskills/skills/sales-enablement .
```

The cold outbound skills (Google Maps, list quality, deliverability) are set up the way their README says: clone the repo into its own folder, add only the API keys you need, and run Claude Code from inside that folder.

```bash
git clone https://github.com/growthenginenowoslawski/coldoutboundskills.git ~/cold-email-ai-skills
cd ~/cold-email-ai-skills
cp .env.example .env   # add only the keys for skills you use (e.g. RAPIDAPI_KEY for Google Maps)
npm install -g tsx
```

Restart Claude Code after installing.

---

## Prompts to run in Claude Code (copy and paste)

**Morning list build (Founder 4)**
> Use google-maps-list-builder to pull "Pakistani clothes shop" and "Asian bridal boutique" in Birmingham, Manchester and London (Southall, Green Street, Ilford). Output a CSV with name, phone, website, rating, review count, address. Then run icp-prompt-builder on 50 rows: keep only independent shops with a website or Instagram, rating 4.0+, 20+ reviews, selling physical products. Apply to all rows and save `uk-a3-YYYY-MM-DD.csv` in the lead-database format in templates/lead-database.csv.

**Amazon brand research (Hamza / Founder 4)**
> Here is a list of 50 Amazon product URLs in [category]. For each, find the brand name, the brand website, whether the brand has a TikTok Shop, and one specific detail for the first line of an email (review count, best-selling product). Mark brands with no TikTok Shop as tier A1. Output in the lead-database format.

**Copy review (Hamza)**
> Use cold-email and cold-outreach-board. Here is my email 1 for Amazon FBA brands [paste]. Prospects aren't replying (0.3% positive). Diagnose the weakest part and give me 2 new versions at a 3rd-grade reading level, no em dashes, one CTA.

**Offer check (Zain/Hamza)**
> Use alex-hormozi-pitch. Our offer: [paste offer from 05-offer-and-scripts.md]. Price $400/month. Prospects say "too expensive" or "need to think". Improve the value stack and guarantee without lowering the price.

**Weekly diagnosis (Founder 4, Sunday)**
> Here is our KPI tracker for the week [paste CSV]. Find the funnel step furthest below target (see 06-19-day-sprint-and-targets.md) and suggest one change for next week.

---

## The GPU laptop

Good uses:
- **Run a local LLM** (for example with Ollama) to qualify and personalise thousands of leads overnight at no API cost: "Is this a Shopify store? Is it on TikTok Shop? Write one first line."
- **Run list-building jobs overnight** (Karachi night = no calling, so the laptop works while we sleep).
- **Host the lead database** (Google Sheet sync, or SQLite) and the de-duplication script.
- **Your local agent (the "OpenBot" you mentioned):** use it for research and data tasks (website checks, enrichment, report drafts). Keep it away from logging into social accounts.

---

## What we will NOT automate (and why)

The pasted plan we started from suggested anti-detect browsers, multi-account farms, private-API Instagram DM bots, Facebook group member scraping and mass DMs to TikTok commenters. **We are skipping all of that.**

| Tactic | Problem |
|---|---|
| Anti-detect browsers (Camoufox, ShardBrowser etc.) and bulk/fake accounts | Built to get around platform detection. Breaks TikTok, Meta and LinkedIn rules. Accounts get banned, and the ban can follow devices and IPs to our real accounts. |
| Private-API DM bots (instagrapi senders, bulk DM tools) | Against Instagram's terms; high ban risk; looks like spam to the person receiving it. |
| Scraping TikTok commenters / Facebook group members to mass message | Platform terms, privacy laws (UK GDPR), and it targets people who never showed interest in us. |
| "TikTok Shop automation" pitches with income promises | Selling "passive income" stores to investors has drawn regulator action against others. If we ever serve investor-type clients: no earnings promises, ever. |

**Why it matters more for us than for most:** our business is running TikTok Shops. If our TikTok presence, or a client's shop, gets flagged because of our outreach, we lose the product we sell. The allowed channels (calls, cold email from separate domains, manual DMs, Upwork, referrals, TikTok's own Affiliate Center) give us enough volume for 4 clients in 19 days.
