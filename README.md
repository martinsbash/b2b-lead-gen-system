# B2B Lead Gen System

A prompt-driven workflow for finding, verifying, and qualifying local business leads using Apify and Claude AI. Started as outreach infrastructure for a web design agency; built to be general enough to work across most B2B prospecting use cases.

This is not a traditional codebase. It is a workflow system that runs through Claude conversation, with Apify handling scraping and Claude handling verification, enrichment, and output.

---

## How It Works

The workflow runs in four stages.

### Stage 1: Scrape Businesses via Google Maps

Use Apify's Google Maps with Contact Details actor to pull local businesses by category and location.

Example prompt (with Apify MCP connected):

> "Find me 20 HVAC businesses in Oshawa, Ontario that don't have a website. Use the Google Maps scraper."

Claude calls the actor and returns: business name, address, phone, rating, review count, and whether a website is listed.

### Stage 2: Verify No Website Exists

Scraper data is not always accurate. Claude cross-checks each lead across Google Search, Yellow Pages, and Facebook to confirm whether a website actually exists.

Each lead gets a confidence rating:
- **HIGH** - confirmed no website anywhere
- **MEDIUM** - possible site exists under a different name
- **Flag** - needs manual check

### Stage 3: Enrich and Build a Lead Profile

Once verified, Claude enriches each lead with actionable intel: owner name, Google review highlights, services offered, market positioning, and a pitch angle for outreach.

Sources: Ontario provincial registries, Google Business Profile, Facebook, Instagram.

Output per lead:
- Business name, category, address, phone, email (if found)
- Owner or contact name and title
- Google rating and review count
- Online presence summary
- Market position and business size
- Notes (awards, press mentions, social proof, anything useful before a call)
- Pitch angle (no website / outdated site / upgrade opportunity)

### Stage 4: Organize and Export

Claude compiles everything into a structured Excel file:

| Sheet | Contents |
|-------|----------|
| Confirmed No Website | Gold leads, verified across multiple sources |
| Website Upgrade Prospects | Have a site but it needs work |
| Needs Re-Verification | Low confidence, check manually |
| Pipeline Summary | Stats and status tracking |

---

## Output Example

Here is what a completed lead sheet looks like:

![Lead output example](assets/output-example.png)

Columns include: Business Name, Contact Name, Title, Category, City, State, Email, Phone, Instagram, Website, Business Size, Market Position, and Notes.

---

## Prerequisites

### 1. Apify Account

Sign up at [apify.com](https://apify.com). The free tier gives enough credits to get started.

Key actor: **Google Maps with Contact Details** by Lukas Krivka.

### 2. Claude Desktop with Apify MCP

1. Open Claude Desktop
2. Go to Settings > MCP Servers
3. Add the Apify actors MCP server with your API key

Once connected, Claude can call actors directly from conversation.

### 3. Claude in Chrome (Optional but Recommended)

For verification and enrichment, Claude in Chrome improves accuracy significantly by enabling real-time web search and cross-checking.

---

## Example Prompts

**Find leads:**
> "Use the Google Maps scraper to find 30 businesses in the GTA in these categories: hair salons, barbershops, and nail salons. Filter for ones without a website."

**Verify and clean:**
> "Here are 30 leads from the scraper. For each one, confirm whether they actually have a website. Create two lists: confirmed no website, and has a website."

**Enrich for outreach:**
> "For each confirmed no-website lead, find the owner's name, Google rating, review count, and what services they offer. I need this for cold calling."

**Prep for a specific call:**
> "I'm about to call Brandon's Auto Repair in Oshawa. Pull everything you can find about them and give me a call script based on my pitch: I've already built them a demo website."

---

## Tech Stack

| Component | Tool | Purpose |
|-----------|------|---------|
| Scraping | Apify (Google Maps actor) | Pull business listings by location and category |
| Verification | Claude + Web Search | Cross-check leads across Google, YP, Facebook |
| Enrichment | Claude + RAG Web Browser | Find owner names, emails, and business intel |
| Analysis | Claude AI | Score leads, generate call scripts, write emails |
| Storage | Excel (.xlsx), Google sheet | Track leads, pipeline status, and outreach results |
| Automation | Claude Scheduled Tasks | Auto-run daily lead batches on a schedule |

---

## Status

The current repo documents the workflow as it runs today. The next version is being rebuilt as a proper end-to-end tool that produces structured output without needing manual prompts at each stage.
