---
name: job-hunt
description: >
  Full job application pipeline for evaluating and applying to roles. Use this skill whenever a job
  description, job URL, or role inquiry is pasted. The skill scores the role 1–10 against your profile,
  stops at scorecard if below 7.5 (unless you say proceed), and when proceeding runs the full workflow:
  job tracker entry + tailored CV (.pptx) + cover letter (.pptx) or outreach message. Includes archetype
  detection, requirement-to-CV matching, ATS keyword extraction, STAR+R story mapping, and comp research.
  Trigger on any job posting, even if pasted without context. Also trigger on "let's proceed", "ok go
  ahead", "yes proceed" if a scorecard was just presented.
---

# Job Hunting Skill — Fully Customizable Template

## Customization Guide

Before using this skill, you must replace the following placeholders with your actual information:

- **YOUR_NAME** → Your full name
- **YOUR_LOCATION** → Your city/region and country (e.g., "New York, USA" or "Berlin, Germany")
- **YOUR_SALARY_MIN** → Your minimum acceptable salary (e.g., "$100,000/year" or "€4,000/month")
- **YOUR_SALARY_TARGET** → Your target/preferred salary
- **YOUR_LANGUAGES** → Your language proficiencies (e.g., "English (Native), Spanish (B1)")
- **YOUR_COMPANIES** → Your employment history as a CSV list
- **YOUR_MASTER_PROFILE_PATH** → Path to your experience profile file
- **YOUR_NOTION_DB_ID** → Your Notion job tracker database ID
- **YOUR_KEY_ACHIEVEMENT_1**, **YOUR_KEY_ACHIEVEMENT_2**, etc. → Major accomplishments
- **YOUR_DESIRED_ARCHETYPES** → The 6 job archetypes relevant to YOUR profile
- **YOUR_POSITIONING** → How you position yourself professionally (e.g., "Institutional DeFi GTM Strategist")

---

# Job Hunting Skill (v2)

## What this skill does

This is a fully delegated job application pipeline. You paste a JD → the skill classifies it into an archetype → scores it with requirement matching and ATS analysis → if strong enough, builds all application assets and logs it to your job tracker. No questions needed before scoring; no waiting for instructions — just execute.

## Step 0: Load your profile

Before scoring any role, find and read your master profile using glob for `**/YOUR_MASTER_PROFILE_PATH`.
Known location: `/sessions/.../mnt/Claude/Job hunt/CV/YOUR_NAME_Experience_Profile.md`

This is the single source of truth. Never fabricate experience. Everything in the CV must come from this file.

**Key facts (fallback if file not accessible):**
- **Location:** YOUR_LOCATION. Remote only, not relocating unless specified.
- **Salary min:** YOUR_SALARY_MIN | preferred YOUR_SALARY_TARGET | open to part-time roles if applicable
- **Languages:** YOUR_LANGUAGES
- **Career:** YOUR_COMPANIES (list in reverse chronological order)
- **AI tools:** Tools you actively use (e.g., ChatGPT, Claude, n8n)

**⚠️ CRITICAL CV RULES — apply to EVERY application:**

1. **About related companies:** If you've worked for companies that merged, rebranded, or were part of the same team, state this clearly to avoid HR confusion. Format: "Company A / Company B (Same founding team; Company B was the team's earlier product)" to show continuity.

2. **Photo on CV:** Include by default. ONLY remove when applying to UK or US companies (to avoid bias liability concerns in those markets).

3. **Certifications — ALWAYS include all of these in Education & Certifications:**
   - List your actual certifications here with dates (e.g., "Certification Name — Organization (Month Year)")

---

## Step 0.5: Archetype Detection

Before scoring, classify the JD into one of your core archetypes. This determines which proof points to prioritize in the scorecard, how to rewrite the CV summary, which experience leads the CV, which STAR stories to prepare, and how to frame the cover letter.

**The archetypes to use (customize these based on YOUR background):**

| Archetype | Core Focus | What They Value | Your Best Angle |
|-----------|-----------|-----------------|-------------------|
| **Archetype 1** | [e.g., Growth Marketing] | [e.g., Data-driven testing, scaling] | [YOUR_KEY_ACHIEVEMENT_1] |
| **Archetype 2** | [e.g., PR / Thought Leadership] | [e.g., Media relationships, credibility] | [YOUR_KEY_ACHIEVEMENT_2] |
| **Archetype 3** | [e.g., Partnerships / BD] | [e.g., Relationship management] | [YOUR_KEY_ACHIEVEMENT_3] |
| **Archetype 4** | [e.g., Institutional / DeFi] | [e.g., Trust, precision] | [YOUR_KEY_ACHIEVEMENT_4] |
| **Archetype 5** | [e.g., Community / Ecosystem] | [e.g., Grassroots growth] | [YOUR_KEY_ACHIEVEMENT_5] |
| **Archetype 6** | [e.g., Full-Stack Marketing Lead] | [e.g., End-to-end ownership] | [YOUR_KEY_ACHIEVEMENT_6] |

**How to detect:**
- Read the JD for emphasis keywords relevant to YOUR archetypes
- Cross-reference the hiring team structure
- If multiple archetypes apply, pick the primary one (appears 3+ times in the JD)

**Output this classification in the scorecard as:** "Archetype: [Name] — aligning with [core focus] via [Your best angle]"

---

## Step 1: Enhanced Scorecard with CV Match Table

Map every JD requirement to your actual experience. Build an enhanced scorecard with three parts:

### A) Requirement-to-CV Match Table

For each major JD requirement, cite the EXACT line(s) from your profile that proves it. Format:

| JD Requirement | Your Evidence | Strength |
|---|---|---|
| "[requirement]" | "[specific accomplishment from YOUR_MASTER_PROFILE_PATH]" | Strong match / Direct hit / Partial / Gap |

### B) Gap Analysis with Mitigation

For each gap identified, categorize it and suggest a mitigation:

| Gap | Category | Adjacent Experience | Cover Letter Mitigation |
|---|---|---|---|
| "[gap]" | Nice-to-have / Hard blocker if core | [related experience you DO have] | [how you address this] |

### C) ATS Keyword Extraction

Extract 15–20 keywords from the JD that should appear in your tailored CV. List them, then use them in the CV summary and bullet rewrites.

---

## Step 1 (continued): Core Scorecard

After the match table, build the traditional scorecard:

| Factor | Assessment | Weight (High/Medium/Low) |
|---|---|---|
| [requirement] | ✅/⚠️/❌ + one-line evidence | High/Medium/Low |

Score 1–10. Be honest — underselling is as unhelpful as overselling.

**Scoring anchors:**
- 9–10: Near-perfect fit, strong proof on every major requirement
- 8–8.9: Strong fit, minor gaps only
- 7.5–7.9: Good fit, one meaningful gap but manageable
- 6–7.4: Partial fit, proceed only if you explicitly say so
- Below 6: Clear mismatch, stop unless you override

**Always flag these as potential hard blockers:**
- In-office requirement outside YOUR_LOCATION
- Visa sponsorship required
- Core function fundamentally different from your background
- Hard language requirement you don't meet

**Decision rule:**
- Score ≥ 7.5 → proceed automatically to Step 2
- Score < 7.5 → present scorecard only, wait for explicit "proceed"
- If JD is incomplete but looks promising → flag as "Needs manual review — insufficient JD", log it as "To check", and present for your evaluation

---

## Step 1.5: STAR+R Story Mapping (after scorecard)

After the scorecard, extract 3–5 STAR+R stories mapped to the top JD requirements.

Check first: if a story bank file exists at `/sessions/.../mnt/Job hunt/Interview Prep/story-bank.md`, read it. Reuse stories from there when they fit, and cite them. For new stories, generate them and append to the bank.

Format:

| JD Requirement | Story Title | Situation | Task | Action | Result | Reflection |
|---|---|---|---|---|---|---|
| "[requirement]" | [title] | [situation] | [task] | [action] | [result] | [what you learned] |

---

## Step 1.6: Comp Research (after scoring)

After the scorecard is built, use WebSearch to find:
- Salary range for this specific role (Glassdoor, Levels.fyi, industry surveys)
- Company's reputation for compensation (if searchable)
- Whether the stated comp (if any in the JD) is competitive

Add a one-line comp note to the scorecard output:
- "Comp: [range] (market average [market average]) — on the high/low end, competitive"
- "Comp not disclosed. Market range for this role: [range]"
- "Comp: [amount] (below market average [market average]) — flag to discuss"

---

## Step 2: Create or update Notion entry

Use the Notion MCP to log the role. The Notion entry is the single source of truth for every application.

**Database collection ID:** `YOUR_NOTION_DB_ID`

Check for existing entry first: If you've previously created a Notion entry for this role, fetch and update it. Only create new if not found.

### Step 2a: Create the page with properties (before generating assets)

**Three-step pattern for NEW entries:**

**API Call 1 — `notion-create-pages`:** Create the page with ALL property fields EXCEPT Milestones. EVERY field below is REQUIRED unless marked optional:
- `Project` — company name
- `Role` — exact role title from JD
- `userDefined:URL` — direct link to the job posting. NEVER leave blank.
- `Stage` — "To apply" (for 7.5+ scores) or "To check" (if below threshold and you override)
- `Prio` — score mapping: 9-10 → "1", 8-8.9 → "2", 7.5-7.9 → "3"; below threshold → omit
- `Note` — must include ALL of: score, archetype, comp/salary, location, source
- `Description` — 2-3 sentence role summary. NEVER leave blank.
- `source` — where the JD came from
- `Category` — tag array (e.g., ["YOUR_INDUSTRY_1", "YOUR_INDUSTRY_2"])

**API Call 2 — `notion-update-page`:** Immediately update with Milestones → "claude"

### Step 2b: Write FULL page content (after generating CV + CL in Steps 3-4)

After CV and cover letter are generated, call `notion-update-page` with FULL page content including:

```markdown
## Archetype & ATS Keywords
Detected: [Archetype Name]
Keywords: [15-20 extracted keywords]

## Requirement-to-CV Match
[Match table from Step 1A]

## Gap Analysis
[Gap analysis from Step 1B]

## Scorecard
Score: [X/10]
[Scoring table]

## STAR+R Stories
[3-5 stories mapped to top requirements]

## Comp Research
[Salary data and market comparison]

## Job Description
[Condensed bullet list of key requirements]

## My Strengths & Weaknesses
**Strengths:**
- [3-5 specific strength bullets]

**Weaknesses:**
- [2-3 specific weakness bullets]

## Assets
- CV: CV_YOUR_NAME_[Company].pptx
- Cover Letter: CoverLetter_[Company].pptx
- [Additional assets as generated]
```

---

## Step 3: Generate the tailored CV (.pptx)

Use your branded PPTX template and generation script. Structure the CV using the detected archetype from Step 0.5 to determine which experience leads.

**Key decisions for every CV:**
- Lead with the experience that matches the archetype
- Rewrite summary for this specific role (use ATS keywords from Step 1C)
- Max 4 roles, max 8 bullets per role
- Nothing fabricated — every claim must trace to your master profile

---

## Step 4: Write cover letter or outreach

**Routing rules:**

| Signal in JD | Asset to produce |
|---|---|
| "DM us on Telegram" | 4-line Telegram message |
| "Send email to X@company.com" | PPTX cover letter + plain text version |
| LinkedIn Easy Apply / platform | PPTX cover letter |
| Early-stage / founder-led | PPTX cover letter — short, direct, builder energy |
| Any standard application | PPTX cover letter (default) |

**Your cover letter voice:**
- Confident but not arrogant. Proof-first, never generic.
- Opens with a genuine connection, not flattery.
- Clean, direct sentences. No buzzword padding.
- 3-4 paragraphs max.

**Banned constructions (these scream "AI-generated"):**
- No em dashes. Use commas, periods, or short dashes.
- No "intersection of X and Y"
- No "I am excited to apply"
- No "I believe I would be a great fit"
- No stacking three adjectives before a noun
- No passive voice patterns

---

## Step 4b: Generate portfolio (if applicable)

If the JD emphasizes creative work, relationships, or demonstrable outcomes, consider generating a portfolio PPTX.

---

## Step 4c: Upload assets to Google Drive (if applicable)

After generating the CV, cover letter, and optional portfolio locally, optionally upload them to Google Drive for persistent access and Notion linking.

---

## Step 5: Engage on X/Twitter (if the JD came from a tweet)

This step only applies when you shared an X/Twitter link. If the JD came from another source, skip to Step 6.

Use Claude in Chrome to:
1. **Like the tweet** — click the like/heart button
2. **Reply with interest** — write a short, casual, confident reply (1–2 sentences max)
3. **Follow the poster** — navigate to their profile and follow
4. **Add to a list** — add the tweet author to a relevant list if applicable

---

## Step 6: Deliver

Provide all created files (with links if uploaded to Google Drive). State clearly:
- What archetype was detected and why
- What angle/positioning was used and why
- What experience leads and in what order
- ATS keywords integrated
- Any open questions or flags
- STAR+R stories prepared for interviews (with story titles)
- X engagement status (if applicable)

---

## What NOT to do

- Don't ask clarifying questions before scoring — just score
- Don't skip archetype detection — it drives all downstream decisions
- Don't fabricate experience — every claim must trace to your master profile
- Don't apply automatically for roles below 7.5 without explicit go-ahead
- Don't leave Notion pages blank — every page MUST have full content
- Don't skip the cover letter or outreach message
