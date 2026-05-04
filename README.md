# B2B Lead Gen System

An automated pipeline for finding, enriching, and qualifying local business leads using Apify + Claude AI. Built to power outreach for a web design agency targeting businesses with no website or outdated sites.

This isn't a traditional codebase. It's a **prompt-driven workflow system** that combines Apify's web scraping actors with Claude's analysis and verification capabilities through Claude Desktop (Cowork mode).

---

## How It Works

The system runs in 4 stages:

### Stage 1: Scrape Businesses via Google Maps

Use Apify's **Google Maps with Contact Details** actor to pull local businesses by category and location.

**Example prompt to Claude (with Apify MCP connected):**

> "Find me 20 HVAC businesses in Oshawa, Ontario that don't have a website. Use the Google Maps scraper."

Claude calls the actor with parameters like:
- `searchTerms`: "HVAC Oshawa Ontario", "plumber Whitby Ontario", etc.
- `maxResults`: 20
- Returns: business name, address, phone, rating, review count, website (or lack of)

**What you get back:** A raw list of businesses with name, phone, address, Google rating, review count, and whether they have a website.

### Stage 2: Verify No Website Exists

The raw scraper data isn't always accurate. Some businesses have websites that just aren't listed on Google Maps. Claude cross-checks each lead:

**Example prompt:**

> "For each business on this list, search Google, Yellow Pages, and Facebook to confirm they actually don't have a website. Only keep the ones that are truly confirmed no-website."

Claude runs parallel searches for each business across:
- Google Search (business name + city)
- YP.ca / Yellow Pages listing
- Facebook business page
- Instagram presence

Each lead gets a confidence rating: HIGH (confirmed no site anywhere), MEDIUM (possible site exists under different name), or flagged for manual check.

### Stage 3: Enrich and Build a Lead Profile

Once verified, Claude enriches each lead with actionable intel:

**Example prompt:**

> "For each confirmed lead, find the owner's name, pull their Google review highlights, check what services they offer, and note anything useful for a cold call."

Sources used:
- Ontario provincial regulators (College of Physicians, CPA Ontario, etc.)
- Google Business Profile review responses (owner names often appear here)
- Facebook/Instagram profiles
- Ontario Business Registry

**Output per lead:**
- Business name, category, address, phone, email (if found)
- Owner/contact name
- Google rating + review count
- Online presence summary (Google Maps only, Facebook only, etc.)
- Pitch angle (no website vs. outdated website vs. upgrade opportunity)

### Stage 4: Organize and Export to Spreadsheet

Claude compiles everything into a structured Excel file with multiple sheets:

- **Confirmed No Website:** Gold leads, verified across multiple sources
- **Website Upgrade Prospects:** Have a site but it's outdated/thin
- **Needs Re-Verification:** Low confidence, check manually
- **Pipeline Summary:** Stats and status tracking

---

## Prerequisites

### 1. Apify Account

Sign up at [apify.com](https://apify.com). The free tier gives you enough credits to get started.

**Key actor you need:**
- [Google Maps with Contact Details](https://apify.com/lukaskrivka/google-maps-with-contact-details) by Lukas Krivka, the core scraper that pulls business listings by location and category

### 2. Claude Desktop with Apify MCP

Connect Apify to Claude Desktop so Claude can run scrapers directly from conversation:

1. Open Claude Desktop
2. Go to Settings > MCP Servers (or install via the Apify MCP plugin)
3. Add the Apify actors MCP server with your API key
4. Once connected, Claude can call actors like `lukaskrivka/google-maps-with-contact-details` directly

### 3. Claude Desktop with Browser Tools (Optional but Recommended)

For the verification and enrichment stages, Claude uses web search and browser tools to cross-check leads. Having Claude in Chrome or web search enabled improves accuracy significantly.

---

## Example Prompts

### Find leads in a specific area

> "Use the Google Maps scraper to find 30 businesses in the GTA (Toronto, Mississauga, Scarborough, Markham) in these categories: hair salons, barbershops, and nail salons. Filter for ones without a website."

### Verify and clean a lead list

> "Here are 30 leads from the scraper. For each one, search Google and Yellow Pages to confirm whether they actually have a website or not. Create two lists: confirmed no website, and has a website. Include the URL for any that do have sites."

### Enrich leads for cold outreach

> "For each confirmed no-website lead, find the owner's name using Google reviews, Facebook, and provincial registries. Also note their Google star rating, review count, and what services they appear to offer. I need this for cold calling."

### Prep for a specific cold call

> "I'm about to call Brandon's Auto Repair in Oshawa. Pull everything you can find about them: Google rating, reviews, whether they have a website, owner name, address, phone. Give me a call script based on my pitch: I've already built them a demo website."

---

## Tech Stack

| Component | Tool | Purpose |
|-----------|------|---------|
| Scraping | Apify (Google Maps actor) | Pull business listings by location and category |
| Verification | Claude + Web Search | Cross-check leads across Google, YP, Facebook |
| Enrichment | Claude + RAG Web Browser | Find owner names, emails, and business intel |
| Analysis | Claude AI | Score leads, generate call scripts, write emails |
| Storage | Excel (.xlsx) | Track leads, pipeline status, and outreach results |
| Automation | Claude Scheduled Tasks | Auto-run daily lead batches on a schedule |

---

## Author

Built by [Martins Bash](https://github.com/martinsbash), co-founder of [Afro Creative Group](https://github.com/martinsbash/afrocreativegroup), a web and creative agency in Ontario, Canada.
