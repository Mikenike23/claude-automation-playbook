---
name: interview-prep
description: >
  Interview preparation assistant. Trigger this skill the moment you mention being invited to an interview,
  getting a call scheduled, or wanting to prepare for an upcoming interview — even if you just say
  "I have an interview at X" or "they want to meet". Also trigger for follow-up rounds. Now includes
  archetype-adaptive framing, STAR+R answers, story bank integration, mandatory product deep-dive,
  and companion research document. The skill researches the company, role, product, and interviewer,
  then builds a practical battle card (2–3 page docx) with scripted answers, plus a reference memo and
  deep-dive research doc. Updates your Notion job tracker. Run proactively whenever an interview is
  confirmed.
---

# Interview Prep Skill

## Customization Guide

Replace these placeholders:
- **YOUR_NAME** → Your name
- **YOUR_LOCATION** → Your location
- **YOUR_MASTER_PROFILE_PATH** → Path to your experience profile
- **YOUR_POSITIONING** → How you position yourself (e.g., "Institutional DeFi GTM Strategist")
- **YOUR_INTERVIEW_ANALYSIS_PATH** → Path to your cumulative interview analysis
- **YOUR_STORY_BANK_PATH** → Path to your story bank
- **YOUR_KEY_WEAKNESS** → A weakness you're actively working on
- **YOUR_ARCHETYPE_1**, **YOUR_ARCHETYPE_2**, etc. → Your target archetypes

---

## What this skill does

When you're invited to an interview, this skill runs a complete prep workflow:

1. **Loads your profile** — source of truth for all answers
2. **Loads interview performance history** — pulls insights from past interviews
3. **Detects which round this is** — adapts output accordingly
4. **If follow-up round:** loads the previous round's transcript and identifies what to fix
5. **Researches the company, role, product, and interviewer** — deep, specific intel
6. **Classifies the role archetype** — shapes story selection, opening, and proof points
7. **Generates THREE outputs:**
   - A **full reference memo** (markdown) — comprehensive research
   - A **battle card** (docx, 2–3 pages) — the thing you actually read before the call
   - A **deep-dive research doc** (markdown) — platform mechanics, competitors, terminology, red flags
8. **Updates your Notion job tracker**
9. **Integrates with story bank** — archives new STAR+R stories for future prep

---

## CRITICAL DESIGN PRINCIPLES

### The battle card is the primary output
You will NOT re-read 8 pages before a call. The battle card must be **2–3 pages max**, organized by what you need DURING the interview: scripted opening, predicted questions with scripted answers, key numbers, questions to ask, and things to avoid. The reference memo is backup research. The deep-dive doc is research reference.

### Script the first 60 seconds
The opening sets the tone. Always provide a scripted 3-beat opening:
- **Who:** One sentence — name, location, years of experience
- **What:** One sentence — sweet spot / what you do best (tailored to this role's archetype)
- **Why here:** One sentence — why this specific role at this specific company
- Then STOP. Include a visible reminder: "⚠️ Stop and let them talk."

### Organize by "what will they ask"
Don't create sections like "Your Experience" or "Company Research." Instead, predict the 6–8 most likely questions and script the answer for each.

### Answers must sound like you
Your real speech patterns from transcript analysis. Script answers to match YOUR voice, not a generic template.

### Know the product cold
The battle card must include:
- An **elevator pitch** (2 sentences) for the company/product
- Pricing / business model in 1 sentence
- 3–5 **specific facts** (user counts, funding, features, names)
- Use the company's own terminology throughout

### Round-aware prep
**Round 1 (Recruiter screen):** Focus on clear background summary, motivation, salary expectations, availability.

**Round 2 (Hiring manager):** Focus on depth, how you'd do the job, specific stories, strategic questions.

**Round 3 (Task/assignment):** Focus on structured deliverable, showing thinking process.

**Round 4+ (Executive/Director):** Focus on strategic fit, culture, bigger picture.

### For follow-up rounds: debrief the previous round
If a transcript exists, read it and identify:
- 3–4 things that went well → build on these
- 3–4 things that were weak/vague → fix these with better scripted answers
- Include a "PREVIOUS ROUND DEBRIEF" section at the top of the battle card

---

## Step 0: Load your profile

Read your master profile at:
`/sessions/*/mnt/Job hunt/CV/YOUR_MASTER_PROFILE_PATH`

This is the source of truth for all STAR answers. Never fabricate experience.

---

## Step 0.5: Load interview performance history

Read your cumulative interview analysis file:
`/sessions/*/mnt/Job hunt/Interview Prep/YOUR_INTERVIEW_ANALYSIS_PATH`

Use it to:
1. **Prioritize questions** — script answers for the highest hit-rate questions first
2. **Counter known weaknesses** — build specific fixes into the battle card
3. **Play to strengths** — design narrative around what you do well
4. **Add PERSONAL REMINDERS** to the battle card (3–5 items before the checklist)

---

## Step 0.7: Load previous round context (if follow-up round)

Check if this is a follow-up round by:
1. Asking you or inferring from context
2. Looking in your interviews folder for a transcript matching the company
3. Looking in your interview prep folder for a previous prep file

If a previous transcript exists:
- Extract: questions asked, how you answered, what the interviewer focused on
- Identify what went well and what to improve
- This becomes the "PREVIOUS ROUND DEBRIEF" section in the battle card

---

## Step 1: Archetype Detection + Product Deep-Dive Research

Classify the role into an archetype:

| Archetype | Core Focus | Your Best Angle |
|-----------|-----------|-------------------|
| **YOUR_ARCHETYPE_1** | [Focus] | [Your angle] |
| **YOUR_ARCHETYPE_2** | [Focus] | [Your angle] |
| **YOUR_ARCHETYPE_3** | [Focus] | [Your angle] |

The archetype shapes:
- Which STAR stories to script in the battle card
- How you describe your background and motivation
- What strategic thinking framework to use
- Which questions to ask

---

## Step 2: Research workflow

### Company Research
- Business model, revenue, funding, stage, competitors
- Mission/vision and how they're different
- Recent news, product launches, challenges

### Role Research
- Reporting line, team size, key success metrics
- Day-to-day vs. strategy split
- Growth path and promotion potential

### Product Research (CRITICAL)
- Elevator pitch (2 sentences)
- Core features and user benefits
- Pricing model
- Competitive differentiation
- Key metrics (users, revenue, growth rate)

### Interviewer Research
- Background, role, tenure
- LinkedIn profile, prior companies
- Their public writing or speaking (if searchable)
- Why they're interviewing for this role

---

## Step 3: Generate battle card

**Format: 2–3 pages max**

```
## [Company Name] — [Role Title]

**Archetype:** [Detected archetype]
**Interviewer(s):** [Name, title, background]
**Round:** [Round number and stage]

### ⚠️ PREVIOUS ROUND DEBRIEF (if follow-up)
[3-4 things that went well]
[3-4 things to improve]
[Specific script changes made]

### ⚠️ PERSONAL REMINDERS
[3-5 reminders specific to your patterns]
- [Reminder 1]
- [Reminder 2]

### 60-Second Opening (SCRIPTED)
[Script your 3-beat opening — Who, What, Why Here]

⚠️ Stop and let them talk.

### Predicted Questions & Scripted Answers

#### Q1: "Tell me about yourself"
[Script your answer — 60 seconds max]

#### Q2: "[Predicted question from JD]"
[Script your answer]

[Repeat for 6-8 most likely questions]

### Key Numbers to Remember
- Company: [Key metric], [Key metric]
- Your background: [Key accomplishment], [Key accomplishment]
- Why this role: [Top reason], [Secondary reason]

### The Company & Product (60-second elevator pitch)
[2-sentence pitch you can deliver out loud]

Pricing/Business model: [1 sentence]

Key facts: [3-5 specific facts]

### Questions to Ask
[5-8 strategic questions that show your thinking]

### Things to Avoid
[Based on your interview history and this role]
- [Avoid 1]
- [Avoid 2]

### Pronunciation & Terminology
[Key names, products, company-specific terms spelled out and correct pronunciation]
```

---

## Step 4: Generate reference memo and deep-dive doc

**Reference memo:** Full research background (markdown) — company history, competitive landscape, interviewer backgrounds, product mechanics.

**Deep-dive research doc:** Terminology, red flags, platform mechanics, competitors, public communications.

---

## Step 5: Update Notion job tracker

Update the job tracker entry with:
- Interview stage and date/time
- Interviewer name
- Interview prep documents (links to battle card, reference memo, deep-dive)

---

## Step 6: Connect with interviewer on LinkedIn (optional)

If researching the interviewer revealed a LinkedIn profile and you're not already connected, send a connection request with a brief, warm message.

---

## What NOT to do

- Don't ask clarifying questions before prep — just research and script
- Don't build the battle card more than 3 pages
- Don't script robotic answers — answers must sound like you
- Don't fabricate product knowledge — if you can't find info, say so in the battle card
- Don't skip the research phase — going in cold hurts you
- Don't ignore your interview history — your patterns matter
