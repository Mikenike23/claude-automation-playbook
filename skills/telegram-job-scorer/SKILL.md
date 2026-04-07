---
name: telegram-job-scorer
description: >
  Process Telegram job matches from your job tracking database with archetype pre-classification. Use this
  skill to score multiple job postings from Telegram sources via your Notion tracker, classify them by
  archetype, and either proceed with the full application workflow (score 7.5+) or mark as "Not eligible"
  (score below 7.5). Enhanced reporting with keyword trends. Runs best when you have a backlog of jobs
  waiting to be scored (20-100+ entries).
---

# Telegram Job Scorer

## Customization Guide

Replace these placeholders:
- **YOUR_NOTION_DB_ID** → Your Notion job tracker database ID
- **YOUR_LOCATION** → Your location (e.g., "Berlin, Germany")
- **YOUR_SALARY_MIN** → Your minimum acceptable salary
- **YOUR_LANGUAGES** → Your language proficiencies
- **YOUR_TARGET_FUNCTIONS** → Job functions you target (e.g., "Marketing, Growth, BD")

---

## Purpose

You are processing Telegram-sourced job postings that were pre-filtered and staged in your Notion Job Tracker database. Your job is to score each one using your job-hunt skill, classify them by archetype, and either proceed with the full application workflow (score 7.5+) or mark it as "Not eligible" (score below 7.5).

---

## Step 1: Find ALL unprocessed Telegram entries in Notion

**IMPORTANT: Do NOT use `notion-search` for discovery.** Use a Chrome-based filtered view instead:

1. Use `tabs_context_mcp` to get a browser tab
2. Navigate to your Job Tracker database
3. Find or create a filtered view: Stage = "To check"
4. Use `get_page_text` to extract all visible entries
5. Scroll down and repeat until you reach the bottom
6. For each entry found, use `notion-fetch` to get full details

**If zero entries are found, report "No new Telegram jobs to process" and stop.**

---

## Step 2: Quick-score in batches with archetype tagging

You have potentially 20-100+ entries to process. To be efficient:

### Read YOUR profile context ONCE at the start
Key facts for quick scoring:
- **Location:** YOUR_LOCATION. Remote only.
- **Salary min:** YOUR_SALARY_MIN
- **Languages:** YOUR_LANGUAGES
- **Target roles:** YOUR_TARGET_FUNCTIONS
- **NOT a fit for:** Pure engineering, security, design, legal, finance, HR, customer support, junior/intern roles, or roles requiring relocation

### For each entry, do a FAST pre-screen with archetype detection

Fetch the Notion page. Read Role, Note, and Description. Based on this surface info:

1. **Detect the archetype:**
   - **Growth / Performance Marketing**
   - **PR / Media / Thought Leadership**
   - **KOL / Affiliate / BD**
   - **Community / Ecosystem / DevRel**
   - **Full-Stack Marketing Lead**

2. **Quickly categorize** as eligible for deeper scoring or instant rejection:

**Instant "Not eligible" (no need to deep-score):**
- Role is clearly not YOUR_TARGET_FUNCTIONS
- Location requires on-site outside YOUR_LOCATION with no remote option
- Role is clearly junior/intern level
- Salary is stated and below YOUR_SALARY_MIN
- Role title is in a non-English language AND company has no English presence

**Needs full scoring:**
- Role is in your target area or ambiguous/could be relevant

### For roles that need full scoring:
1. If there's a job URL, try WebFetch to get the full JD
2. Build a quick scorecard (3-4 most important factors)
3. Score 1-10
4. **Tag the archetype** detected from the role description

---

## Step 3: Act on scores

**Score >= 7.5 — PROCEED with full workflow:**
1. Update Notion: Stage → "To apply", Prio → score mapping, Note → append score, Milestones → add "applied"
2. Add scorecard to page content including detected archetype
3. Pass detected archetype to your job-hunt skill
4. Generate tailored CV (.pptx)
5. Generate cover letter or outreach message
6. Save files to outputs folder

**Score < 7.5 — MARK AS NOT ELIGIBLE:**
1. Update Notion: Stage → "Not eligible", Note → append "Score [X]: [reason]", Milestones → add "check"
2. No CV or cover letter needed
3. Move to next entry

---

## Step 4: Enhanced summary report

After processing, provide a comprehensive report with:

### Run metadata
- **Time window checked**
- **Total entries found in "To check"**

### Processing results
- **Instant-filtered** (obvious non-matches): count + brief reasons
- **Full-scored entries**: count and list with scores
- **Proceeded (7.5+)**: count with file links
- **Marked Not eligible**: count
- **Remaining unprocessed**: count + reason

### Archetype distribution
Break down roles by archetype category.

### Top opportunities
List the top 3-5 highest-scoring roles with company, role, score, archetype, and key match reasons.

### Top ATS keywords seen
Extract the most commonly mentioned skills across all processed roles (top 10 with frequency).

### Comp data summary
If any roles mentioned salary:
- **Salary range observed**
- **Most common comp structure**
- **Outliers** (if any)

---

## THROUGHPUT TARGET

Process AT LEAST 30 entries per run. Be aggressive with instant-filtering — most entries will be obvious non-matches. Only spend time deep-scoring the ones that genuinely look like potential fits.

---

## Important rules

- Never fabricate experience
- Your key constraints: Remote only (YOUR_LOCATION), salary min YOUR_SALARY_MIN, YOUR_LANGUAGES
- If an entry's Stage is not "To check", skip it
- Use `notion-update-page` to update entries
- **Discovery must use Chrome filtered view, NOT notion-search**
- Archetype detection happens in the fast pre-screen BEFORE deep-scoring
- Pass archetype to job-hunt skill so it avoids re-detecting
