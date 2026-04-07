---
name: reply-guy
description: >
  Generic X/Twitter list engagement automation skill for "reply guy" workflows. Use this skill whenever
  you want to engage across your X/Twitter lists — replying and being active across multiple feeds in
  one session. Triggers on "reply guy", "engage on X", "do the Twitter lists", "reply on X", "engage
  my lists", or any request to run through Twitter/X lists and interact with tweets. This skill
  implements a reliable process for navigating X Pro's multi-column deck layout and posting natural,
  human-sounding replies at scale without getting flagged as a bot.
---

# Reply Guy — X/Twitter List Engagement Skill

## Customization Guide

Replace these placeholders:
- **YOUR_LISTS** → Your list names (e.g., "Technology", "Finance", "Industry X")
- **YOUR_POSITIONING** → How you position yourself professionally
- **YOUR_VOICE_PATTERN_1**, **YOUR_VOICE_PATTERN_2**, etc. → Your natural communication patterns

---

## Core Principles

- **HARD RULE — One reply per post, ever. No exceptions.** Never reply to the same tweet more than once. Keep a running dedup list of every post/tweet URL you've replied to during the session. Replying twice to the same post is an obvious AI tell.
- **Comment only.** No separate like pass. Reply is the signal.
- **Casual and human first.** Sound like a knowledgeable person dashing off a reply between meetings — short sentences, lowercase fine, no hashtags unless organic, no em dashes (—). Occasional "lol", "rn", "tbh" are fine.
- **Timing matters.** Prioritize tweets posted within the last 30 minutes. Replying early puts you near the top of reply threads.
- **Quantity with quality.** Target 10–15 replies per list.

---

## Session Setup

1. Call `tabs_context_mcp` to get the tab ID.
2. Take **one screenshot** to orient yourself — identify which columns are visible and their approximate x-positions.
3. If not on X Pro, navigate to the deck URL.
4. Proceed with all visible lists unless you specify otherwise.
5. For each list, run the **JS Batch Discovery** flow below — no further browsing screenshots needed.

---

## The Core Loop (per list)

### Step 1 — Batch-discover tweets via JS (no screenshot needed)

At the start of each column, run this JS to extract all visible article text. Use the column's approximate x-position to filter to just that column:

```javascript
// Adjust X_MIN / X_MAX to isolate the target column
const X_MIN = 640, X_MAX = 980;
const arts = Array.from(document.querySelectorAll('article'))
  .filter(a => {
    const r = a.getBoundingClientRect();
    return r.left >= X_MIN && r.left < X_MAX;
  })
  .map(a => a.innerText.slice(0, 400).replace(/\n+/g, ' '));
JSON.stringify(arts);
```

Read the returned texts. Pick 10–15 good targets (apply the blocklist below). Note a short unique phrase from each tweet you'll reply to.

---

## The Blocklist — Always Skip

**Never reply to:**
- Tweets labeled "Promoted" or "Paid partnership" (ads)
- Gambling / betting accounts
- Crypto noise: repost-for-prizes, "airdrop claim" notices, pure price shilling
- Official brand accounts doing pure product promotion
- Bare retweets (no added comment)
- Anything where your reply would clearly feel random or forced

**Prefer:**
- Real people sharing opinions, takes, data, or personal moments
- Tweets that invite a response — questions, hot takes, news, milestones
- Tweets under 30 minutes old (check the timestamp in the article text)

---

## Reply Voice Guide

**General rules (all lists):**
- Never open with "Great tweet!" or empty affirmations
- Add something — a reaction, an angle, a small observation. Don't just agree.
- Under 2 sentences unless the content genuinely warrants more
- No generic hype words: "amazing", "love this", "so true", "fire"
- No unsolicited advice
- No em dashes (—) — use a period or line break instead
- Never mention you're an AI

---

## Step 2 — Plan all replies before opening any tweet

Draft every reply text before touching the DOM. Keep the queue in the session log.

---

## Step 3 — Execute each reply (one at a time)

**3a. Open the tweet detail:**
```javascript
let r = 'nf';
for (const a of document.querySelectorAll('article')) {
  const t = a.innerText || '';
  if (t.includes('YOUR_UNIQUE_FINDER_KEY')) {
    const l = Array.from(a.querySelectorAll('a[href*="/status/"]'))
      .find(l => !l.href.includes('analytics'));
    if (l) { l.click(); r = 'ok'; }
    break;
  }
}
r;
```

**3b. Find and click the reply textbox:**
Use `find("Post your reply textbox")` to get a ref, then click the ref directly.

**3c. Type and send:**
- Use `type` action to type your pre-drafted reply.
- Submit with `key: "cmd+Return"`.

**3d. Verify sent (JS, no screenshot needed):**
```javascript
const b = document.querySelector('[data-testid="tweetTextarea_0"]');
b ? (b.innerText.trim() || 'SENT') : 'gone';
```

**3e. Close the detail view:**
```javascript
const b = Array.from(document.querySelectorAll('button'))
  .find(b => b.getAttribute('aria-label') === 'Close stack');
if (b) { b.click(); 'closed'; } else 'nf';
```

**3f. Log it** and immediately fire the next article finder JS.

---

## Column X-Position Guide

Column x-positions shift when the deck is scrolled horizontally. Check actual positions with:

```javascript
const arts = Array.from(document.querySelectorAll('article'));
const positions = [...new Set(arts.map(a => Math.round(a.getBoundingClientRect().left / 100) * 100))];
positions;
```

---

## When to Take Screenshots

Take a screenshot **only when:**
- Initial session orientation (once, at the very start)
- Something breaks and you need to visually debug
- The column x-position JS check is ambiguous

In all other cases, trust the JS results.

---

## Live Log Format

Keep a running log as you go:

```
📋 SESSION LOG
List: YOUR_LIST_1
✅ @Handle1 → "brief reply here"
✅ @Handle2 → "another reply"
...

List: YOUR_LIST_2
✅ @Handle3 → "short take"
...
```

---

## Completing the Session

When all lists are done:
1. Post the full session log with reply counts per list
2. Flag any lists that were low quality (mostly old tweets, ads, or spam)
3. Note any errors or unexpected behavior
4. **Close all tabs opened during this session.** Use `tabs_context_mcp` to list open tabs and close them with `tabs_close_mcp`. Leave only tabs that were already open before the session started.

---

## Common Issues and Fixes

| Problem | Fix |
|---------|-----|
| Article finder returns \`'nf'\` | Tweet scrolled out of DOM. Scroll the column and retry. |
| Navigated to \`/compose/post\` accidentally | Navigate back to deck URL directly. Always use \`find()\` ref clicks. |
| \`Close stack\` button not found | Try \`key: "Escape"\` as fallback. |
| Reply spinner keeps spinning | Wait 5–10s, run verify-sent JS — if \`SENT\`, it worked. |
| Text typed twice | Clear with \`key: "cmd+a"\` then \`key: "Backspace"\`, retype. |
| Column articles at wrong x-position | Deck was scrolled. Run position-check JS and update X_MIN/X_MAX. |
