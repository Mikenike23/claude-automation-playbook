# Claude Automation Playbook

A collection of production-tested Claude Cowork skills for automating job hunting, personal branding, content creation, and career management. Built and used daily — not a proof of concept.

## What This Is

This is a modular automation system built entirely with [Claude Cowork](https://claude.ai) skills. Each skill is a self-contained workflow that Claude executes on your behalf — scanning emails for jobs, scoring opportunities, generating tailored CVs, prepping for interviews, engaging on social media, and more.

The skills chain together into pipelines: email alerts flow into job scoring, which triggers CV generation and applications. Interview invitations trigger prep research, which feeds into post-interview analysis and career coaching. Newsletters become social content drafts with branded graphics.

Everything converges on Notion as a central hub, giving you full visibility and control.

## Skills

### Job Hunting Pipeline

| Skill | What it does |
|-------|-------------|
| `job-hunt` | Scores job descriptions (1–10), detects role archetypes, maps requirements to your experience, generates tailored CV + cover letter, logs to Notion, optionally engages on the job post via X |
| `email-scanner` | Scans Gmail for recruiter emails, job board alerts, and newsletter opportunities. Categorizes, tags archetypes, hands off to job-hunt |
| `telegram-job-scorer` | Batch-processes Telegram-sourced job postings from Notion. Auto-applies for 7.5+ scores, marks the rest as ineligible |
| `career-page-scanner` | Monitors target company career pages via Greenhouse/Lever/Ashby APIs. Deduplicates against Notion, filters by your criteria |

### Interview Pipeline

| Skill | What it does |
|-------|-------------|
| `interview-prep` | Researches company, role, product, and interviewer. Generates a battle card with scripted STAR+R answers, a reference memo, and a deep-dive research doc |
| `interview-reviewer` | Analyzes interview transcripts across 9 dimensions with archetype-weighted scoring. Tracks cumulative performance and identifies patterns |

### Content & Engagement

| Skill | What it does |
|-------|-------------|
| `content-scanner` | Scans Gmail newsletters, extracts top stories, drafts platform-specific content (X, LinkedIn, Telegram, TikTok) with your voice and perspective |
| `reply-guy` | X/Twitter list engagement automation. Navigates X Pro deck layout, discovers tweets via JS, drafts human-sounding replies, posts with randomized pacing. Works for personal, brand, or growth accounts — fork and customize the voice |
| `social-graphics` | Generates branded PDF carousels for LinkedIn and social posts using Python |

### Strategy & Utilities

| Skill | What it does |
|-------|-------------|
| `career-coach` | Strategic career advisor that filters decisions through your long-term goals, positioning, and industry framework |
| `skill-creator` | Meta-skill for creating, testing, and optimizing new skills with eval loops |
| `scheduler` | Wrapper for creating recurring or one-time scheduled tasks |

## How It Chains Together

```
Email/Telegram → Scanner → Job Scorer → CV/CL Generation → Application
                                ↓
                          Interview Prep → Interview → Reviewer → Career Coach
                                                                      ↑
Newsletters → Content Scanner → Drafts → Social Graphics          (feedback loop)
                                   ↓
                             Reply Guy → Engagement
```

All data flows through **Notion** as the central hub.

## Required Integrations

- **Gmail MCP** — for email and newsletter scanning
- **Notion MCP** — for job tracking, content management, and persistence
- **Claude in Chrome extension** — for X/Twitter engagement, LinkedIn, and web automation
- **Claude Cowork** — the runtime that executes these skills

## Getting Started

1. **Fork this repo**
2. **Pick a skill** — start with `job-hunt` or `reply-guy`, they're the most self-contained
3. **Open its `SKILL.md`** and follow the Customization Guide at the top
4. **Replace `YOUR_` placeholders** with your actual information
5. **Install in Claude Cowork** and start using it

Every skill has a customization block at the top listing exactly what to replace. Common placeholders:

| Placeholder | What to fill in |
|-------------|----------------|
| `YOUR_NAME` | Your name |
| `YOUR_POSITIONING` | How you position yourself professionally |
| `YOUR_X_HANDLE` | Your X/Twitter handle |
| `YOUR_NOTION_DB_ID` | Your Notion database ID |
| `YOUR_SALARY_MIN` | Your minimum acceptable compensation |
| `YOUR_EXPERTISE_1` | Your primary domain expertise |

See each skill's customization guide for the full list.

## Architecture

See [docs/architecture.md](docs/architecture.md) for the full system diagram, pipeline details, database schemas, and scheduling recommendations.

## What's Preserved vs. Sanitized

**Kept intact** — all structural logic, scoring frameworks, archetype systems, anti-AI-detection voice patterns, security rules, workflow automation, and integration patterns.

**Removed** — all personal names, company names, social handles, Notion IDs, salary figures, career history, and file paths. Replaced with clearly labeled placeholders.

## License

MIT — customize and use freely. If you build something cool on top of this, share it.# Claude Automation Playbook

Initial commit - full content coming via upload.
