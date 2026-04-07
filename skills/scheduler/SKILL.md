---
name: scheduler
description: >
  Create a scheduled task that can be run on demand or automatically on an interval. Use this skill
  whenever you want to automate a recurring task, set up a reminder, or create a workflow that runs
  on a schedule. Trigger on "schedule", "recurring task", "automation", "set up a scheduled job",
  "remind me", "daily task", "weekly task", or any request to create something that runs repeatedly
  or at a specific time in the future.
---

# Task Scheduler

## What this skill does

This skill converts any repeatable task or workflow into a scheduled automation that can:
- Run on a recurring schedule (daily, weekly, specific times)
- Run once at a specific moment in the future (one-time reminders)
- Be triggered manually on demand

The resulting scheduled task is stored as a skill file and runs in its own session.

---

## When to use this skill

- **Recurring workflows:** "Check my inbox every morning", "scan for job opportunities daily", "review metrics weekly"
- **One-time reminders:** "Remind me to follow up on Thursday at 2pm", "schedule a task for next month"
- **Delegated automation:** "Run this process every day without me asking"

---

## How it works

### Step 1: Analyze the task

Look at what you want to accomplish and extract:
- The core objective (what should happen?)
- The trigger (when should it happen?)
- The expected output or success criteria

### Step 2: Draft a self-contained prompt

The prompt will be used for future autonomous runs — it must be entirely self-contained. Future runs will NOT have access to this conversation, so never reference "the current conversation," "the above," or any ephemeral context.

Include in the prompt:
- A clear objective statement (what to accomplish)
- Specific steps to execute
- Any relevant file paths, URLs, repositories, or tool names
- Expected output or success criteria
- Any constraints or preferences you expressed

Write in second-person imperative: "Check the inbox…", "Run the test suite…"

### Step 3: Choose a taskName

Pick a short, descriptive name in kebab-case (e.g., "daily-inbox-summary", "weekly-dep-audit", "monthly-report-check").

### Step 4: Determine scheduling

Pick ONE:

**Recurring (cronExpression):**
- Daily at 9am: `0 9 * * *`
- Weekdays at 5pm: `0 17 * * 1-5`
- Every Monday at 8:30am: `30 8 * * 1`
- First day of month at midnight: `0 0 1 * *`

Evaluated in your LOCAL timezone, not UTC. Use local times directly.

**One-time with a specific moment (fireAt):**
- ISO 8601 string with timezone offset
- Example: `2026-03-05T14:30:00-08:00` for March 5 at 2:30pm Pacific
- Use this for reminders, one-off future actions, or specific dates
- Must be in the future
- Task auto-disables after firing

**Ad-hoc (no automatic run):**
- Omit both cronExpression and fireAt
- User can trigger manually

### Step 5: Create the scheduled task

Call the `create_scheduled_task` tool with:
- `taskId`: The kebab-case name
- `prompt`: The self-contained instructions
- `description`: One-line summary of what it does
- `cronExpression` OR `fireAt` (or omit both for ad-hoc)

---

## Examples

### Example 1: Daily job opportunity scan

```
taskId: daily-job-scan
prompt: "Scan your Gmail for job opportunities from recruiters, job boards, and newsletters. Use your job-hunt skill to score any new roles that appear. Report findings in your job tracker."
description: "Daily scan for job opportunities in Gmail"
cronExpression: "0 9 * * *"
```

### Example 2: Weekly metrics review

```
taskId: weekly-metrics-review
prompt: "Review key metrics from the past week. Check [your specific metrics dashboard]. Identify trends, anomalies, and action items. Report findings in [your report location]."
description: "Weekly metrics review and reporting"
cronExpression: "0 18 * * 5"  # Friday at 6pm
```

### Example 3: One-time reminder

```
taskId: follow-up-acme-corp
prompt: "Send a follow-up email to [contact at ACME Corp] about [specific topic]. Draft the email and save to [location]."
description: "Remind me to follow up with ACME Corp"
fireAt: "2026-03-10T14:00:00-08:00"
```

### Example 4: Ad-hoc on-demand task

```
taskId: deploy-to-staging
prompt: "Run the deployment script to push the latest changes to staging. Verify health checks pass. Report status."
description: "Manual deployment to staging environment"
# No cronExpression or fireAt — user triggers manually
```

---

## Cron Expression Cheat Sheet

| Schedule | Expression |
|----------|-----------|
| Every day at 9am | `0 9 * * *` |
| Every weekday at 5pm | `0 17 * * 1-5` |
| Every Monday at 8:30am | `30 8 * * 1` |
| Every Friday at 4pm | `0 16 * * 5` |
| First day of month at midnight | `0 0 1 * *` |
| Every 6 hours | `0 */6 * * *` |
| Every 30 minutes | `*/30 * * * *` |
| Twice a day (9am and 5pm) | `0 9,17 * * *` |
| First Monday of month | `0 9 1-7 * 1` |

---

## Tips for Good Scheduled Tasks

1. **Be specific about file paths and URLs.** The future run won't have context about "that file we were discussing."

2. **State expected outputs clearly.** "Save results to [exact path]" not "let me know what happens."

3. **Include error handling.** "If the scan returns zero results, note it and continue. If it errors, report it."

4. **Keep the scope manageable.** A 30-minute task is good. A 3-hour task is risky (may time out).

5. **Test once before scheduling.** Run it manually once to make sure the prompt works standalone.

6. **Make it idempotent.** If the task runs twice in a row, it should produce the same result, not double-apply.

7. **Plan for timezone changes.** If you travel, cron times shift. Consider UTC offsets.

---

## What NOT to do

- Don't schedule tasks that require manual input or decision-making
- Don't schedule tasks with sensitive credentials or API keys in the prompt
- Don't schedule tasks that permanently delete data without confirmation
- Don't create recurring tasks for things that might change (use ad-hoc instead)
- Don't forget to include all context — future runs won't have your current conversation
