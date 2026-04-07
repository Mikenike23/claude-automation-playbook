# Claude Automation Playbook: System Architecture

This document describes the complete architecture of the Claude Automation Playbook system, including data flows, component interactions, and pipeline orchestration.

## System Overview

The Claude Automation Playbook is a modular skill-based system that automates four distinct career management pipelines:

1. **Job Hunting Pipeline** — from email alerts to applications
2. **Interview Pipeline** — from invitation to feedback and career coaching
3. **Content Pipeline** — from newsletters to published social content
4. **Engagement Pipeline** — from platform monitoring to targeted outreach

All pipelines converge on **Notion as the central hub**, with **Chrome automation** for manual execution and **Email integration** for inbound data.

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        EXTERNAL DATA SOURCES                             │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  Job Alerts          Newsletters         Social Feeds        Interview  │
│  (Email)             (Email)             (LinkedIn, X)       (Calendar) │
│    │                   │                   │                    │       │
└────┴───────────────────┴───────────────────┴────────────────────┴───────┘
     │                   │                   │                    │
     ▼                   ▼                   ▼                    ▼
┌──────────────┐   ┌──────────────┐   ┌──────────────┐   ┌──────────────┐
│ Email        │   │ Newsletter    │   │ Reply Guy    │   │ Interview    │
│ Scanner      │   │ Scanner       │   │ Base         │   │ Prep         │
└──────┴───────┘   └──────┴───────┘   └──────┴───────┘   └──────┴───────┘
       │                  │                  │                  │
       ▼                  ▼                  ▼                  ▼
    Jobs              Content           Engagement         Interview
    DB                Drafts DB         Log DB              Notes DB
     │                  │                  │                  │
     └──────────────────┴──────────────────┴──────────────────┘
                        │
              ┌─────────▼─────────┐
              │  NOTION HUB       │
              │  (Central Store)  │
              └─────────┬─────────┘
                        │
         ┌──────────────┴──────────────┐
         │              │              │
         ▼              ▼              ▼
    ┌─────────┐   ┌──────────┐   ┌──────────┐
    │ Job     │   │ Content  │   │Engagement│
    │ Scorer  │   │ Graphics │   │ Chrome   │
    └────┬────┘   └────┬─────┘   │Extension │
         │             │         └──────────┘
         ▼             ▼
    ┌─────────────────────────────┐
    │ Job Hunt Manager & CV Gen   │
    │ Cover Letter Generator      │
    └────────────┬────────────────┘
                 │
         ┌───────▼────────┐
         │ Chrome: Apply  │
         │ LinkedIn/Sites │
         └────────────────┘

                    ▼
         ┌──────────────────────┐
         │ Interview Reviewer   │
         │ & Career Coach       │
         └──────────────────────┘
```

## Pipeline Details

### 1. Job Hunting Pipeline

**Flow**: Email → Scanner → Scorer → Hunt Manager → Applications

#### Email Scanner
- **Trigger**: Daily schedule or manual
- **Input**: Gmail inbox (job alert emails from LinkedIn, Indeed, Greenhouse, recruiters)
- **Process**:
  - Search Gmail for job alert keywords: `job`, `position`, `role`, `application`, `opportunity`
  - Parse email content to extract: job title, company, link, description, requirements
  - De-duplicate against Applications DB
  - Tag with metadata: source (LinkedIn/Indeed/Recruiter), date found, salary (if visible)
- **Output**: New jobs added to Jobs DB in Notion

#### Job Scorer
- **Trigger**: When new jobs appear in Jobs DB
- **Input**: Job record (title, company, description, requirements, metadata)
- **Process**:
  - Score against target profile (roles, companies, salary range, growth potential)
  - Evaluate: alignment with archetype, market signal (is this company hiring people like me?), location fit
  - Assign score 0-10 where:
    - 8-10: Ideal match (apply immediately)
    - 6-7.5: Good fit (review details, then apply)
    - 4-6: Possible fit (requires research before deciding)
    - 0-4: Poor fit (mark Not Eligible)
  - Reject if match contains rejection keywords (mandatory office, visa sponsorship unavailable, etc.)
- **Output**: Score, rationale, and eligibility tag in Jobs DB

#### Job Hunt Manager
- **Trigger**: When Job Scorer completes
- **Input**: Scored jobs (7.5+), job descriptions, application URLs
- **Process**:
  - Create Application record in Notion
  - Extract CV-relevant keywords from job description
  - Extract cover letter themes (company values, role emphasis, growth narrative)
  - Prepare for CV/Cover Letter generation
  - Queue for next step (CV/CL generation)
- **Output**: Application record with job context in Notion

#### CV Generator
- **Trigger**: When Application record is ready
- **Input**: Job description, CV template, Notion skills/experience
- **Process**:
  - Read base CV from Notion
  - Extract key requirements from job description (ATS keywords, role focus, tech stack)
  - Reorder experience and skills to match job priorities
  - Emphasize relevant projects and achievements
  - Keep total within 1 page (customize format if needed)
- **Output**: Tailored CV for specific job (stored in Application record)

#### Cover Letter Generator
- **Trigger**: When Application record is ready
- **Input**: Job description, company research, personal brand story
- **Process**:
  - Extract company culture signals (website, LinkedIn, Glassdoor)
  - Write opening paragraph addressing specific role + company
  - Detail 2-3 relevant achievements with quantified impact
  - Connect career narrative to the role
  - Closing call to action
  - Total: 250-350 words
- **Output**: Draft cover letter (stored in Application record)

#### Chrome Automation (Apply)
- **Trigger**: Manual or scheduled on Application records marked "Ready to Apply"
- **Input**: Application record with CV, cover letter, company application portal
- **Process**:
  - Navigate to application URL (LinkedIn, company careers page, etc.)
  - Fill form fields: name, email, phone, cover letter, attach CV
  - Submit application
  - Capture confirmation page screenshot
  - Log application timestamp and portal type
- **Output**: Application marked "Applied", confirmation stored in Notion

### 2. Interview Pipeline

**Flow**: Interview Invitation → Interview Prep → Interview Notes → Interview Reviewer → Career Coach

#### Interview Prep
- **Trigger**: When interview is scheduled (calendar event or manual trigger)
- **Input**: Job description, company research, interview type (phone/video/panel)
- **Process**:
  - Generate likely interview questions (behavioral, technical, cultural)
  - Research company: recent news, culture, product roadmap
  - Prepare talking points: why you want the role, key achievements to mention, questions for interviewer
  - Suggest technical prep topics (for engineering roles)
  - Create interview notes template in Notion
  - Suggest 30-min study agenda
- **Output**: Interview Prep doc in Notion with questions, talking points, and agenda

#### Interview Notes (Manual)
- **During Interview**: User captures freeform notes on:
  - Questions asked
  - Your answers (topics covered, examples given)
  - Interviewer name, role, and signals (enthusiasm, concerns, timeline)
  - Company insights learned
  - Next steps promised
- **Output**: Notes stored in Interview Notes DB in Notion

#### Interview Reviewer
- **Trigger**: After interview (manual or scheduled)
- **Input**: Interview notes, job description, interview type
- **Process**:
  - Analyze answer quality: did you cover key achievements? Were stories specific?
  - Evaluate for red flags: vague answers, contradictions with CV, missed opportunities
  - Score interview performance 1-10 (based on question coverage, clarity, confidence signals in writing)
  - Extract lessons learned: what went well, what to improve
  - Identify follow-up actions: thank you email, additional materials, timeline
  - Compare performance against your archetype (what does success look like for your persona?)
- **Output**: Interview review with score, feedback, and action items in Notion

#### Career Coach
- **Trigger**: After interview reviewer or when making career decisions
- **Input**: Interview feedback, job context, career goals, past interview outcomes
- **Process**:
  - Synthesize interview feedback with past patterns
  - Assess overall interview health (improving? stagnating?)
  - Recommend preparation adjustments based on weak areas
  - Suggest role/company changes if misalignment detected
  - Update personal narrative based on learnings
  - Provide strategic guidance: timing to negotiate, when to pivot, skill gaps
- **Output**: Career coaching memo in Notion with recommendations and insights

### 3. Content Pipeline

**Flow**: Newsletters → Scanner → Drafts → Graphics → Publishing

#### Newsletter Scanner
- **Trigger**: Daily or on-demand
- **Input**: Gmail inbox (newsletters from Substack, Hackernews, industry sources)
- **Process**:
  - Search Gmail for newsletter emails
  - Extract top 3-5 stories from each newsletter (by prominence)
  - Parse: headline, summary, link, why it matters (based on your interests)
  - De-duplicate across newsletters
  - Tag by topic: AI, startups, marketing, careers, markets, personal growth, etc.
- **Output**: New content entries in Content Drafts DB in Notion

#### Content Drafts
- **Trigger**: When new content is scanned
- **Input**: Newsletter content (headline, summary, link, tags)
- **Process**:
  - Read original article (via Chrome if needed)
  - Write Twitter thread (5-10 tweets covering key insight, why it matters, your take)
  - Draft LinkedIn article (300-500 words, more polished than thread)
  - Write email newsletter snippet (100 words, formatted for email)
  - Tag with your personal POV: what's the insight? Why do you care? What does it mean?
  - Suggest visual angle for graphics (data, quote, concept)
- **Output**: Full draft content in Content Drafts DB with multiple formats

#### Social Graphics
- **Trigger**: When content drafts are ready
- **Input**: Content draft, key quote, data point, or concept
- **Process**:
  - Create clean 1-3 slide carousel PDF (LinkedIn or Twitter format)
  - Extract or create key visual: quote, data point, process diagram, or concept illustration
  - Add branding: your name/handle, color scheme, fonts
  - Optimize for social (readable on mobile, high contrast)
  - Include source attribution
- **Output**: PDF carousel graphic in Notion, ready to share

#### Chrome Automation (Publishing)
- **Trigger**: Manual or scheduled
- **Input**: Content (Twitter thread, LinkedIn post, email)
- **Process**:
  - Navigate to social platform (Twitter, LinkedIn)
  - Paste or type content (thread posts one by one for Twitter)
  - Upload graphics if applicable
  - Schedule or publish immediately
  - Capture post URL
  - Log post to Engagement Log
- **Output**: Published content, post URL stored in Notion

### 4. Engagement Pipeline

**Flow**: Platform Monitoring → Reply Guy Base + Archetypes → List-Based Targeting → Chrome Publishing

#### Reply Guy Base
- **Trigger**: Manual request or scheduled sweep
- **Input**: X/Twitter feed, LinkedIn comments, keywords of interest
- **Process**:
  - Search X for posts matching your interests: your industry, thought leaders you follow, relevant keywords
  - Identify high-engagement posts (100+ replies, recent, from credible sources)
  - Generate 3-5 authentic reply options (not spam, actually thoughtful)
  - Each reply: adds value, continues conversation, shows your expertise without shameless self-promotion
  - Score replies by quality (likely to get engagement, authentic, not salesy)
- **Output**: Reply options in Engagement Log, ranked by quality

#### Personal/Brand Archetype Variants
- **Trigger**: User selects a reply option
- **Input**: Base reply, archetype style (personal vs. brand)
- **Process**:
  - **Personal Archetype**: Conversational, authentic, storytelling angle, vulnerable insights
  - **Brand Archetype**: Professional, analytical, thought leadership, business impact focus
  - Rewrite same reply in both styles
  - Preserve core insight, adjust tone and framing
  - Allow user to choose which variant fits the moment
- **Output**: Multiple archetype variants of the same reply

#### List-Based Engagement
- **Trigger**: Periodic (weekly/bi-weekly)
- **Input**: Your X lists (e.g., "People to know", "VCs", "Builders"), your reply guy base
- **Process**:
  - Scan recent posts from list members
  - Find 3-5 posts per week where engagement makes sense
  - Match posts to relevant replies from your base
  - Customize replies for each person (reference their recent work, add specific insight)
  - Log engagement target: who, which post, which reply option
- **Output**: Weekly engagement targets in Engagement Log

#### Chrome Automation (Engage)
- **Trigger**: Manual or scheduled engagement
- **Input**: Engagement Log targets (person, post, reply)
- **Process**:
  - Navigate to post
  - Compose reply (either with ready reply or custom text)
  - Post reply
  - Optional: like the original post, follow if not already following
  - Capture reply URL
  - Log in Engagement Log: timestamp, person engaged with, post URL, reply URL
- **Output**: Engagement logged in Notion, reply published on X

## Shared Components

### Notion as Central Hub

**Central Databases**:
- **Jobs DB**: All job opportunities, scores, eligibility status
- **Applications DB**: Job applications with CVs, cover letters, status (Applied/Interviewing/Rejected), and interview records
- **Interview Notes DB**: Interview experiences with date, company, role, notes, score, and reviewer feedback
- **Content Drafts DB**: All content with source (newsletter), drafts in multiple formats, graphics status, publishing status
- **Engagement Log DB**: All engagement activities with date, platform, person engaged, reply, and engagement metrics

**Why Notion**:
- All skills read/write to Notion, creating a single source of truth
- Easy to audit: see all applications, all interviews, all content in one place
- Enables filtering and sorting: jobs by score, applications by status, content by topic
- Notion views provide accountability: how many jobs scored? How many applied? What's the interview:application ratio?

### Chrome Extension

**Capabilities**:
- Navigate to URLs (job applications, social posts, company websites)
- Fill forms (names, emails, text areas)
- Click buttons (submit, apply, post)
- Extract page content (parse company careers page, extract post text)
- Screenshot results (confirm application submission, capture engagement)
- Scroll and interact with dynamic pages (LinkedIn feeds, Twitter threads)

**Usage**:
- Job hunting: apply to jobs, capture confirmation
- Content: publish to X, LinkedIn, other platforms
- Engagement: reply to posts, like, follow
- Research: navigate company websites, extract careers page data

**Security**:
- No credentials stored; user authenticates once on each platform
- Claude can only interact with content it can see (no access to private data)
- All actions logged in Notion for auditability

### Email Integration (Gmail MCP)

**Capabilities**:
- Search Gmail by keyword, sender, date range
- Extract email metadata: subject, sender, date, body text
- Parse email content: extract links, tables, key data
- No deletion or modification (read-only)

**Usage**:
- Email Scanner: find job alert emails
- Newsletter Scanner: find newsletter emails
- Content extraction: pull articles and research from forwarded content

**Security**:
- Read-only access; no ability to modify, delete, or send emails
- Search queries use Gmail's native search syntax
- Email content is processed locally; not transmitted externally

## Data Flows in Detail

### Job Application Flow

```
Job Alert Email
    ↓
Email Scanner extracts: title, company, URL, description
    ↓
Job Scorer evaluates: target match (0-10)
    ↓
IF score >= 8:
    ├─ Job Hunt Manager creates Application record
    ├─ CV Generator creates tailored CV
    ├─ Cover Letter Generator creates customized letter
    └─ Chrome automation submits application
         ↓
         Application marked "Applied" + confirmation screenshot
         ↓
         Wait for interview or rejection
ELSE IF score 6-7.5:
    ├─ Application marked "Review before apply"
    └─ User reviews, decides to apply or skip
ELSE:
    └─ Job marked "Not Eligible" + rejection reason
```

### Interview Feedback Flow

```
Interview scheduled
    ↓
Interview Prep generates Q&A, company research, talking points
    ↓
User conducts interview, captures notes in Notion
    ↓
Interview Reviewer analyzes notes:
    ├─ Did you answer well? (quality score)
    ├─ What went well? (strengths to build on)
    ├─ What to improve? (next interview focus)
    └─ Compare against archetype (is this your best self in interviews?)
    ↓
Career Coach synthesizes all interviews:
    ├─ Overall interview health improving?
    ├─ Which types of questions do you nail? Which do you struggle with?
    ├─ Should you pivot your approach or target different companies?
    └─ What's the career narrative you should be telling?
    ↓
Update interview prep strategy, repeat
```

### Content Publishing Flow

```
Newsletter received (email)
    ↓
Newsletter Scanner extracts top 3-5 stories
    ↓
Content Drafts generates:
    ├─ Twitter thread
    ├─ LinkedIn article
    ├─ Email snippet
    └─ Visual angle suggestion
    ↓
User reviews drafts, makes edits
    ↓
Social Graphics generates branded carousel PDF
    ↓
Chrome automation publishes:
    ├─ Post to Twitter (thread format)
    ├─ Post to LinkedIn (article + carousel)
    └─ Send email (if applicable)
    ↓
Log post URLs in Engagement Log
    ↓
Track engagement: likes, replies, shares (manual review)
```

### Engagement Targeting Flow

```
Weekly engagement sweep
    ↓
Reply Guy Base scans your X lists for recent posts
    ↓
Identify high-engagement posts from people you want to know
    ↓
For each target post:
    ├─ Generate base reply (thoughtful, adds value)
    ├─ Create Personal + Brand archetype variants
    └─ Log in Engagement Log
    ↓
User reviews and selects replies
    ↓
Chrome automation posts reply to target post
    ↓
Log in Engagement Log: who, what post, reply posted
    ↓
Track: do targeted people engage back? Do you gain followers?
```

## Orchestration & Scheduling

### Recommended Schedule

```
Daily (6 AM):
  - Email Scanner: find new job alerts
  - Newsletter Scanner: find new newsletters

Hourly (during business hours):
  - Job Scorer: score new jobs, trigger Job Hunt Manager
  - Interview Reviewer: if interview notes exist, provide feedback

Weekly (Monday, 9 AM):
  - Career Coach: synthesize week's interviews, provide guidance

Bi-weekly (Monday, 10 AM):
  - List-Based Engagement: identify engagement targets, suggest replies

On-demand:
  - Interview Prep: trigger when interview scheduled
  - CV/Cover Letter Generator: trigger when application ready
  - Chrome automation (apply, publish, engage): trigger manually or per schedule

Manual/One-time:
  - Newsletter Scanner: rescan old newsletters
  - Career Coach: before major career decisions
  - Interview Prep: customize for specific companies
```

### Skill Dependencies

```
Email Scanner → Job Scorer → Job Hunt Manager → [CV + Cover Letter] → Chrome Apply
                                ↓
                            Applications DB

Newsletter Scanner → Content Drafts → Social Graphics → Chrome Publish
                         ↓
                    Content Drafts DB

Interview Prep (manual) → [During interview, user captures notes]
                              ↓
                        Interview Reviewer → Career Coach
                              ↓
                        Interview Notes DB

Reply Guy Base → [Archetype variants] → Chrome Engage
                       ↓
                 Engagement Log DB
```

## Database Schema Summary

### Jobs DB
- Job Title, Company, URL, Description
- Source (LinkedIn/Indeed/Email)
- Date Found, Salary Range
- Score (0-10), Eligibility Status
- Rejection Reason (if applicable)

### Applications DB
- Job (linked to Jobs DB)
- Status (Pending, Applied, Interviewing, Offer, Rejected)
- CV (PDF or text), Cover Letter (text)
- Application Date, Applied URL
- Interview Status, Interview Notes (linked to Interview Notes DB)
- Offer Details (if applicable)

### Interview Notes DB
- Application (linked to Applications DB)
- Interview Date, Company, Role
- Notes (freeform)
- Interviewer Name, Interviewer Role
- Interview Reviewer Score (1-10)
- Interview Reviewer Feedback (strengths, improvements)

### Content Drafts DB
- Source (newsletter, URL, or manual)
- Topic (AI, Startups, Career, etc.)
- Twitter Thread, LinkedIn Article, Email Snippet
- Graphics Status (Not Started, In Progress, Done)
- Publish Date, Post URLs (Twitter, LinkedIn)

### Engagement Log DB
- Date, Platform (X, LinkedIn)
- Person Engaged, Their Post URL
- Your Reply (text)
- Your Reply URL (published)
- Engagement Metrics (optional manual tracking)

## Extension: Building New Pipelines

To add a new pipeline (e.g., "Speaking Opportunities", "Podcast Guest", "Investment Tracking"):

1. **Identify the inbound data source** (email, web feed, newsletter, etc.)
2. **Create a Scanner skill** to parse that data into a Notion DB
3. **Create an Analyzer skill** to score/prioritize opportunities
4. **Create an Automation skill** to take action (reach out, apply, etc.)
5. **Create an Outcome Tracker** to log results and learn

Example (Speaking Opportunities Pipeline):
```
Email (conference invites) → Speaking Scanner → Conference Scorer → Proposal Generator → Chrome Email → Speaking Log
```

## Performance & Observability

All Notion databases serve as audit logs:
- **Jobs DB**: How many jobs are you seeing per week? What's your quality filter working (how many are actually eligible)?
- **Applications DB**: Applications per week, conversion rates (interviews per application), offer conversion
- **Interview Notes DB**: Interview frequency, performance trends (are your scores improving?), time to offer
- **Content Drafts DB**: Content production rate, engagement per post (manual tracking)
- **Engagement Log DB**: Engagement actions per week, reply rate (how many people reply back?)

Use Notion views and filters to monitor pipeline health and identify bottlenecks.

## Security & Privacy

- **Notion**: All data is encrypted in transit and at rest; access controlled via MCP permissions
- **Gmail**: Read-only access; credentials never stored; search-based only
- **Chrome**: User authenticates directly with platforms; no credential storage
- **No external APIs**: All data stays within your Notion workspace and local browser automation

---

**This architecture is designed for autonomy with oversight.** Skills run automatically, but all outputs land in Notion where you can audit, edit, and decide before taking action. The system accelerates your work but never locks you out of the decision loop.
