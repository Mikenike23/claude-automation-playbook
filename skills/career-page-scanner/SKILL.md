---
name: career-page-scanner
description: >
  Company career page monitoring and job discovery. Proactively scans target company career pages for
  new job postings matching your profile. Trigger on "scan career pages", "check company jobs",
  "career page scan", "any new roles on company sites", "run the career scanner", or any request to
  proactively look for jobs on company websites. Also trigger on "add company to scanner" or "update
  scanner list" when you want to add/remove companies. Run proactively if you mention wanting to find
  new roles beyond email and messaging sources.
---

# Career Page Scanner

## Customization Guide

Replace these placeholders:
- **YOUR_NAME** → Your name
- **YOUR_LOCATION** → Your location (e.g., "Berlin, Germany")
- **YOUR_SALARY_MIN** → Your minimum acceptable salary
- **YOUR_LANGUAGES** → Your language proficiencies
- **YOUR_TARGET_FUNCTIONS** → Job functions you target
- **YOUR_MASTER_PROFILE_PATH** → Path to your experience profile
- **YOUR_NOTION_DB_ID** → Your Notion job tracker database ID
- **TARGET_COMPANIES_PATH** → Path to your target companies list (see Step 0)

---

## Purpose

Proactively scan a curated list of target company career pages for job postings that match your profile. This fills the gap between reactive channels (email via email-scanner, Telegram via telegram-job-scorer) and active searching. Many companies post roles on their careers page days before they hit job boards.

The scanner checks each company's career page, extracts relevant postings, deduplicates against your Notion job tracker, detects archetypes, and hands off matches to your job-hunt skill for scoring.

---

## Step 0: Load context

1. Read your master profile: `YOUR_MASTER_PROFILE_PATH`
2. Read your target company list: `references/target_companies.md` in this skill's directory
3. Check your Notion job tracker for recently processed entries to deduplicate

**Key profile facts for filtering:**
- **Location:** YOUR_LOCATION. Remote only.
- **Salary min:** YOUR_SALARY_MIN
- **Languages:** YOUR_LANGUAGES
- **Target functions:** YOUR_TARGET_FUNCTIONS

---

## Step 1: Scan career pages

Work through the target company list. For each company, try these discovery methods in order:

### Method A: Greenhouse / Lever / Ashby API (preferred)

Many companies use Greenhouse, Lever, or Ashby for their ATS. If noted in `references/target_companies.md`, hit the public API:

- **Greenhouse:** `https://boards-api.greenhouse.io/v1/boards/{company_slug}/jobs`
- **Lever:** `https://api.lever.co/v0/postings/{company_slug}`
- **Ashby:** `https://api.ashbyhq.com/posting-api/job-board/{slug}`

Use WebFetch to call these. The JSON response gives you structured job data: title, location, department, and job URL.

**Important: If an API returns 404**, the company may have switched ATS platforms. Try the other two ATS APIs before falling through to Method B.

### Method B: WebFetch on careers page

If there's no known ATS API, use WebFetch on the company's careers page URL. Parse the HTML/text for job listings. Look for patterns like:
- Job title + location + department in repeated structures
- Links containing `/jobs/`, `/careers/`, `/positions/`, `/openings/`
- Common ATS embed patterns (Greenhouse iframe, Lever embed, Workable widget)

### Method C: WebSearch fallback

If WebFetch fails (page requires JavaScript, returns no content, or is blocked), use WebSearch:
```
site:{company_domain} careers OR jobs OR "open positions" YOUR_TARGET_FUNCTIONS
```

### Rate limiting
- Process companies in batches of 5-10
- If a company's page consistently fails, note it and move on
- Don't spend more than ~30 seconds per company

---

## Step 2: Filter and classify

For each discovered posting, apply these filters:

### Positive keyword filter (role must match at least one)
YOUR_TARGET_FUNCTIONS (and other relevant keywords like Marketing, Growth, BD, Community, etc.)

### Negative keyword filter (auto-skip if title matches)
Engineer, Developer, Backend, Frontend, Solidity, Security, Designer, UI/UX, Legal, Finance, HR, Customer Support, Junior, Intern, iOS, Android, QA, DevOps, Data Scientist, Machine Learning

### Staleness filter (CRITICAL)
ATS APIs often return ALL historical postings, including old ones. Stale roles waste time.

- **Posted within 60 days:** PASS
- **Posted 61-90 days ago:** FLAG with "(⚠️ Posted ~2-3 months ago)" — include but note
- **Posted more than 90 days ago:** AUTO-SKIP. Log as skipped.
- **No date available:** PASS with "(⚠️ Post date unknown)" note

### Location filter
- Remote, Remote-friendly, or no location specified: PASS
- Europe, EU, YOUR_LOCATION, or with remote option: PASS
- **US-only, requires relocation, or "must be based in [US city]": AUTO-SKIP.**
- **UK-only on-site with no remote: AUTO-SKIP**
- Location unclear, hybrid, or "remote with occasional travel": FLAG for you

### Archetype detection
Assign each passing role one of your 6 archetypes:
- **Growth / Performance Marketing**
- **PR / Media / Thought Leadership**
- **KOL / Affiliate / BD**
- **Community / Ecosystem / DevRel**
- **DeFi / Institutional Marketing** (or YOUR_INDUSTRY equivalent)
- **Full-Stack Marketing Lead**

---

## Step 3: Deduplicate against Notion

For each passing posting, check your Notion job tracker:
1. Use `notion-search` to look for the company + role combination
2. If an entry already exists with Stage "To apply", "Interview", "Offer", or "Rejected", skip it
3. If an entry exists with Stage "To check" or "Not eligible", note it but don't create a duplicate

Only create NEW Notion entries for genuinely new postings.

---

## Step 4: Create Notion entries and hand off to job-hunt

For each NEW passing posting:

1. **Create a Notion entry** using `notion-create-pages` with:
   - Project (company name)
   - Role (exact title)
   - userDefined:URL (direct link to job posting)
   - Stage ("To check" — you'll score it manually or via job-hunt skill)
   - Note (source: "career-page-scanner", company, detected archetype)
   - Description (one-line summary)
   - Category (tags: company industry, role type)

2. **Pass the detected archetype to your job-hunt skill** so it doesn't re-detect

3. **If score 7.5+:** Full job-hunt workflow (CV, cover letter, Notion update)
   **If score < 7.5:** Mark as "To check" for manual review later

---

## Step 5: Completion Report

After scanning all companies, provide a report with:

### Scan metadata
- **Time window:** When the scan was run
- **Companies scanned:** Count of unique companies
- **ATS platforms encountered:** Greenhouse, Lever, Ashby, custom, etc.

### Results
- **Total roles found:** Count
- **Instant-filtered:** Count + reasons (location, salary, seniority, function)
- **Passed initial screening:** Count
- **Deduplicated:** Count (already in Notion)
- **Archived as "To check":** Count
- **Handed off to job-hunt (7.5+):** Count with company/role list

### Top opportunities
- List top 3-5 highest-scoring new roles
- Include company, role, detected archetype, key match reasons

### Archetype distribution
Break down roles by archetype (helps you see where your opportunities cluster).

### Comp data summary
If any roles mentioned salary:
- Salary range observed
- Most common comp structure
- Outliers

---

## Important rules

- Never fabricate experience
- Your key constraints: Remote only (YOUR_LOCATION), salary min YOUR_SALARY_MIN, YOUR_LANGUAGES
- Use Chrome filtered view for Notion discovery (not semantic search)
- Archetype detection happens in the fast pre-screen
- Always pass detected archetype to job-hunt skill
- Don't create duplicate entries — check Notion before creating new ones
