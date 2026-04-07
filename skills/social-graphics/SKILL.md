---
name: social-graphics
description: >
  PDF carousel generator for LinkedIn and social media. Use this skill whenever you ask to create a
  carousel, social media graphic, slide deck for a post, visual for LinkedIn, image for a post, or
  anything involving turning post content into visual slides. Also trigger on "make this into a carousel",
  "create slides for this post", "LinkedIn PDF", "social graphic", "visual for this", "carousel slides",
  "post graphic", or any request to create visual content to accompany a social media post.
---

# Social Graphics Generator

## Customization Guide

Replace these placeholders:
- **YOUR_NAME** → Your name
- **YOUR_POSITIONING** → Your professional positioning
- **YOUR_X_HANDLE** → Your X/Twitter handle (e.g., @YourHandle)
- **YOUR_BRAND_COLOR** → Your brand accent color (e.g., "#0077B5" for LinkedIn blue)
- **YOUR_FONT** → Your preferred font (e.g., "Poppins")
- **YOUR_TAGLINE** → Your professional tagline
- **YOUR_COMPANY** → Your current company/affiliation (if applicable)

---

## How It Works

You have a Python script at `scripts/generate_carousel.py` that takes a JSON config and produces a polished PDF. Your job is to:

1. Read your post content
2. Structure it into slides using a JSON config
3. Run the script to generate the PDF
4. Save the PDF to your outputs folder

The script handles all the visual design — fonts, spacing, pagination dots, accent bars, number badges, and your branding.

---

## Brand Defaults

These are baked into the script but can be overridden per-carousel via the `style` object:

- **Background**: White (`#FFFFFF`)
- **Text color**: Dark navy (`#1A1A2E`)
- **Accent color**: YOUR_BRAND_COLOR
- **Font**: YOUR_FONT (Light / Regular / Medium / Bold)
- **Author**: YOUR_NAME
- **Handle**: YOUR_X_HANDLE
- **Tagline**: YOUR_TAGLINE
- **Format**: 1080×1080 square (carousels) or 1080×1350 portrait (single images)

---

## Aspect Ratios

The JSON config supports an `aspect_ratio` field:

- **`square`** (default) — 1080×1080. Use for multi-slide carousels.
- **`portrait`** — 1080×1350. Use for single-image posts.

---

## Slide Types

### Carousel Slide Types

#### `cover` — Title/hook slide (always first)

```json
{
  "type": "cover",
  "title": "How to BEAT\nthe algorithm\nin 10 steps",
  "subtitle": "The playbook for getting reach",
  "title_size": 72
}
```

Use `\n` for manual line breaks. Aim for 3-5 lines max.

#### `list` — Numbered items (great for tips, steps, rules)

```json
{
  "type": "list",
  "title": "Steps 1–5",
  "start_number": 1,
  "items": [
    "Do this first",
    "Then do this",
    "Finally do this"
  ],
  "item_size": 30
}
```

Best for 4-6 items per slide. If you have more, split across multiple list slides.

#### `content` — Title + body text or bullets

```json
{
  "type": "content",
  "title": "Why This Matters",
  "number": 3,
  "body": "Explanation text here.",
  "body_size": 30,
  "title_size": 52
}
```

The optional `number` field adds a circled number badge. Use `bullets` instead of `body` for bullet-point layouts.

#### `quote` — Highlighted quote or key insight

```json
{
  "type": "quote",
  "quote": "The real insight\n\nstated powerfully",
  "text_size": 40,
  "attribution": "— Optional source"
}
```

Features a large decorative quote mark. Use `\n\n` for paragraph breaks.

#### `cta` — Call-to-action (always last)

```json
{
  "type": "cta",
  "title": "Found this helpful?",
  "body": "Follow for more insights like this.",
  "cta_text": "Follow"
}
```

Closing slide with follow/repost prompts and your branding.

---

## JSON Config Template

```json
{
  "aspect_ratio": "square",
  "slides": [
    {
      "type": "cover",
      "title": "Main Headline",
      "subtitle": "Subheading",
      "title_size": 72
    },
    {
      "type": "list",
      "title": "The Points",
      "items": ["Point 1", "Point 2", "Point 3"],
      "start_number": 1
    },
    {
      "type": "content",
      "title": "Deep Insight",
      "body": "Explanation here.",
      "number": 2
    },
    {
      "type": "quote",
      "quote": "Key takeaway stated\n\npowerfully",
      "attribution": "— You"
    },
    {
      "type": "cta",
      "title": "Found this valuable?",
      "body": "Follow for more like this."
    }
  ],
  "style": {
    "background_color": "#FFFFFF",
    "text_color": "#1A1A2E",
    "accent_color": "YOUR_BRAND_COLOR",
    "font": "YOUR_FONT"
  }
}
```

---

## Slide Structure Best Practices

### For 5-slide carousels (ideal):
1. Cover slide (hook, title)
2. List slide (3-4 points)
3. Content slide (deeper insight)
4. Quote or content slide (key takeaway)
5. CTA slide (follow/share)

### For 7-slide carousels:
1. Cover
2. List (1-4)
3. Content
4. List (5-7) or more content
5. Content or quote
6. Content or quote
7. CTA

### For single-image posts:
Use `aspect_ratio: "portrait"` and keep to 1-2 slides max.

---

## Slide Content Guidelines

- **Titles:** 5-8 words max. Punchy and clear.
- **Body text:** Short paragraphs. 2-3 sentences max per slide.
- **Lists:** 4-6 items per slide max. Avoid long bullets.
- **Quotes:** Striking, memorable, standalone meaningful (not dependent on context).
- **Numbers/metrics:** Always cite source if claimed.

---

## Generation Workflow

1. **Collect your post content** (paste or write draft)
2. **Identify the core argument or lesson** (what's the ONE thing?)
3. **Break into 5-7 slides** (cover, 3-4 body slides, 1 CTA)
4. **Create the JSON config** (structured data for each slide)
5. **Run the script:** `python scripts/generate_carousel.py output.pdf config.json`
6. **Save to outputs folder**
7. **Test by opening the PDF** — check text size, spacing, and readability

---

## Customization Options

Within the `style` object, you can override:
- `background_color`: Any hex color
- `text_color`: Any hex color
- `accent_color`: Your brand color
- `font`: Font family (Poppins recommended)
- `margin`: Slide margins in pixels
- `border_radius`: Rounded corners on elements
- `line_height`: Spacing between lines

---

## Tips for Great Carousels

1. **One idea per carousel.** Don't mix unrelated topics.
2. **Use numbers when possible.** "5 Ways" beats "Ways".
3. **Lead with the hook.** The cover slide determines swipes.
4. **Avoid jargon.** Even if you're an expert, clarity wins.
5. **Use white space.** Don't cram text onto every slide.
6. **Keep fonts minimal.** 2-3 font weights max (light, regular, bold).
7. **Color sparingly.** Accent color on 2-3 key elements, not every slide.
8. **Test readability.** Open the PDF on your phone and read each slide.

---

## What NOT to do

- Don't mix more than 2 main topics in one carousel
- Don't use more than 8 slides (sweet spot is 5-7)
- Don't cram too much text onto any slide
- Don't use rare/custom fonts (stick with Poppins or system fonts)
- Don't make the accent color overwhelm the content
- Don't forget your name/handle on the cover or CTA slide
