---
name: interview-reviewer
description: >
  Interview transcript analyzer with archetype-aware scoring and multi-dimensional evaluation.
  Trigger on "review my interview", "how did I do", "analyze this interview", when you upload interview
  transcripts, or any interview performance question. Scores transcripts across 9 dimensions (Opening,
  Product Understanding, Storytelling, Answer Structure, Strategic Thinking, Questions Asked, Salary
  Negotiation, Closing Strength, Reflection & Learning). Tracks progress across multiple interviews,
  adapts advice by role type/archetype. Run proactively on new transcripts.
---

# Interview Reviewer Skill

## Customization Guide

Replace these placeholders:
- **YOUR_NAME** → Your name
- **YOUR_LOCATION** → Your location
- **YOUR_MASTER_PROFILE_PATH** → Path to your experience profile
- **YOUR_INTERVIEW_ANALYSIS_PATH** → Path to your cumulative interview analysis file
- **YOUR_STORY_BANK_PATH** → Path to your story bank file
- **YOUR_KNOWN_WEAKNESS_1** → A specific weakness you're working on (e.g., "Product understanding")
- **YOUR_KEY_STRENGTH** → A strength you consistently demonstrate
- **YOUR_ARCHETYPE_1**, **YOUR_ARCHETYPE_2**, etc. → Your target archetypes

---

## What this skill does

Analyzes your interview transcripts to provide actionable coaching in two modes:

1. **Single transcript review** — When you upload a new interview transcript
2. **Portfolio review** — When you ask about patterns, progress, or general performance across all interviews

Enhanced with **archetype-aware scoring**, **reflection quality assessment**, and **portfolio performance breakdown**.

---

## Step 0: Load context

### Your profile
Read your master profile for ground truth:
`/sessions/*/mnt/Job hunt/CV/YOUR_MASTER_PROFILE_PATH`

**Key fallback facts:**
- **Location:** YOUR_LOCATION
- **Career path:** [Your employment history in reverse chronological order]
- **Key numbers:** [Your major accomplishments with metrics]
- **Languages:** [Your language proficiencies]

### Existing analysis
Read your cumulative analysis file if it exists:
`/sessions/*/mnt/Job hunt/Interview Prep/YOUR_INTERVIEW_ANALYSIS_PATH`

This contains your running assessment across all past interviews.

### Story bank
Check if this exists:
`/sessions/*/mnt/Job hunt/Interview Prep/YOUR_STORY_BANK_PATH`

Review it for past strong stories you can draw on.

---

## Step 1: Detect the role type (Archetype-Aware)

Classify the role into one of your archetypes. This determines how you weight the dimensions:

| Archetype | Your Best Story | Key Angle | Dimension Weights |
|-----------|------------------|-----------|-------------------|
| **YOUR_ARCHETYPE_1** | [Best story] | [Key angle] | [How to weight dimensions] |
| **YOUR_ARCHETYPE_2** | [Best story] | [Key angle] | [How to weight dimensions] |
| **YOUR_ARCHETYPE_3** | [Best story] | [Key angle] | [How to weight dimensions] |

---

## Step 2: Analyze the transcript

Evaluate these 9 dimensions on a 1-5 scale:

### 2a. Opening & Self-Introduction
- Did you deliver a crisp, relevant intro tailored to THIS role?
- Did you lead with the most relevant experience?
- Was it under 2 minutes or did it ramble?

### 2b. Product/Company Understanding
- Could you explain what the company does in simple terms?
- Did you show you researched beyond the surface?
- Did you connect their product to your experience?

**Your known weakness:** YOUR_KNOWN_WEAKNESS_1. Watch for instances where this appears.

### 2c. Storytelling & Examples
- Did you use specific numbers and outcomes (not just activities)?
- Were examples relevant to THIS role?
- Did you use STAR format (Situation, Task, Action, Result)?

**Your strength:** YOUR_KEY_STRENGTH. Watch for instances where you deploy this.

### 2d. Answer Structure
- Were answers focused and under 90 seconds?
- Or did you start with "yeah, basically..." and meander?
- Did the interviewer have to redirect?

### 2e. Strategic Thinking
- When asked "how would you approach X for us?", did you give a concrete framework?
- Did you demonstrate strategic thinking beyond execution?
- Did you offer unsolicited suggestions showing initiative?

### 2f. Questions Asked
- Did you ask smart questions showing strategic thinking?
- Or just logistics (team size, salary)?
- Did you ask about challenges, priorities, success metrics?

### 2g. Salary Negotiation
- Did you lead with the top of your range or the bottom?
- Did you volunteer to accept less (self-sabotage)?
- Were you confident or apologetic?

### 2h. Closing Strength
- Did you end with a confident close expressing interest and specific value?
- Or did it just fizzle out?

### 2i. Reflection & Learning Signals
- When describing past work, did you show what you LEARNED?
- Did you mention what you'd do differently with hindsight?
- Did you demonstrate growth mindset vs. just reciting accomplishments?

**Score 1-5:**
- 1: Pure activity recital, no learning signals
- 3: Some reflection when prompted
- 5: Naturally weaves lessons and growth into answers

---

## Step 3: Generate the assessment

### For a SINGLE transcript review:

```
## [Company Name] — [Role Title]

**Role type / Archetype:** [from classification above]
**Interview stage:** [HR screen / Hiring manager / Founder / Technical]
**Date:** [if known]

### Scorecard (Archetype-Weighted)
| Dimension | Score (1-5) | Archetype Weight | Notes |
|-----------|-------------|------------------|-------|
| Opening & Self-Intro | X | [e.g., Standard] | ... |
| Product/Company Understanding | X | [e.g., High] | ... |
| Storytelling & Examples | X | [e.g., Standard] | ... |
| Answer Structure | X | [e.g., High] | ... |
| Strategic Thinking | X | [e.g., High] | ... |
| Questions Asked | X | [e.g., Standard] | ... |
| Salary Negotiation | X | [e.g., High] | ... |
| Closing Strength | X | [e.g., High] | ... |
| Reflection & Learning Signals | X | [e.g., Standard] | ... |
| **Weighted Overall** | **X/5** | | |

### What went well
[2-4 bullet points of genuine strengths shown in this interview]

### Opportunities for next time
[2-4 specific areas to improve, with concrete examples from this transcript]

### Patterns from your history
[If you have prior interviews analyzed, reference patterns]

### Next steps
[Specific action for the next interview or next round if applicable]
```

---

## Step 4: For Portfolio Reviews

If you ask for a broader performance analysis across multiple interviews:

1. **Read all transcripts** in your interview folder
2. **Track progress over time** — Are you improving? In which dimensions?
3. **Identify consistent patterns** — What do you do well? What trips you up?
4. **Highlight highest/lowest dimension scores** across all interviews
5. **Surface your strongest interviews** — What was different about them?
6. **Recommend focus areas** for the next 2-3 interviews

---

## What NOT to do

- Don't make assumptions about what you said — read the transcript carefully
- Don't skip the archetype classification — it shapes the entire analysis
- Don't be falsely cheerful — be honest about weaknesses
- Don't over-index on recency — patterns across 3-5 interviews matter more than one interview
- Don't fabricate interview details — only comment on what appears in the transcript
