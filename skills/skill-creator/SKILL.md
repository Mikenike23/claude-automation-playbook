---
name: skill-creator
description: >
  Meta-skill for creating, modifying, and optimizing Claude Cowork skills. Use this skill whenever
  you want to create a skill from scratch, edit or improve an existing skill, run evaluations to test
  a skill, benchmark skill performance, or optimize a skill's description for better triggering accuracy.
  Trigger on "create a skill", "modify skill", "optimize skill", "test skill", "skill evaluation",
  "skill benchmark", or any request to build, improve, or analyze a skill.
---

# Skill Creator

## Customization Guide

This is a generic skill-creation meta-skill. It requires no customization — use it as-is to create and optimize any skill.

---

## What this skill does

This skill helps you create new skills and iteratively improve them. The process goes like this:

1. **Decide what you want the skill to do** and roughly how it should do it
2. **Write a draft of the skill**
3. **Create a few test prompts** and run them against the skill
4. **Evaluate the results** qualitatively and quantitatively
5. **Rewrite the skill** based on feedback
6. **Repeat until satisfied**
7. **Expand the test set** and try again at larger scale

---

## Where you are in the process

When using this skill, I'll help you figure out where you are and progress through these stages:

- **Starting from scratch?** I'll help you narrow down what you mean, write a draft, write test cases, and run them.
- **Already have a draft?** I'll help with eval and iteration.
- **Already have tests?** I'll help with benchmarking and refinement.
- **Done and ready?** I'll help optimize the skill description for better triggering.

---

## Key workflow principles

### Capture Intent

Before creating a skill, understand:
1. What should this skill enable Claude to do?
2. When should this skill trigger? (user phrases/contexts)
3. What's the expected output format?
4. Should we set up test cases? (For objective outputs like file transforms, data extraction, code generation — yes. For subjective outputs like writing style — maybe not.)

### Interview and Research

Ask proactively about:
- Edge cases and error scenarios
- Input/output formats
- Example files or data
- Success criteria
- Dependencies and tools needed

### Write the SKILL.md

A skill file contains:
- **YAML frontmatter** with `name` and `description` (triggers go here)
- **Markdown instructions** describing how to use it
- **Optional bundled resources** (reference files, templates)

**Important:** The `description` field is the primary triggering mechanism. Include both what the skill does AND specific contexts for when to use it. Be a bit "pushy" — instead of "describes how to do X", write "Use this skill whenever the user mentions X, asks about X, or wants to accomplish X, even if they don't explicitly say 'skill'."

### Create Test Prompts

For objective skills (file transforms, data extraction, code generation):
- 5-10 varied test cases covering normal cases, edge cases, and error scenarios
- Each test should have a clear expected output you can verify

For subjective skills (writing, creative):
- You may still want tests, but they focus on style/tone rather than correctness

### Run Tests and Evaluate

1. **Qualitative:** Does the output look right? Is it helpful? Does it match the intent?
2. **Quantitative:** If applicable, measure success metrics (accuracy, output quality, formatting correctness)

### Iterate

Based on test results:
- Rewrite unclear instructions
- Add edge case handling
- Clarify expected inputs/outputs
- Improve triggering conditions

---

## Anatomy of a Well-Designed Skill

```
skill-name/
├── SKILL.md (required)
│   ├── YAML frontmatter (name, description)
│   └── Markdown instructions
├── references/ (optional)
│   ├── reference-file-1.md
│   └── reference-file-2.md
└── examples/ (optional)
    ├── example-input.txt
    └── example-output.txt
```

---

## Common Skill Types

### Type 1: Workflow Skills
"Automate this multi-step process"
- Examples: job-hunt, interview-prep, content-scanner
- Test with: realistic scenarios and edge cases

### Type 2: Analysis Skills
"Evaluate this and give me insights"
- Examples: interview-reviewer, career-coach
- Test with: varied inputs with known characteristics

### Type 3: Generation Skills
"Create X from my input"
- Examples: social-graphics, cover letters, carousel slides
- Test with: variety of input styles and tones

### Type 4: Engagement Skills
"Help me interact with platforms/systems"
- Examples: reply-guy, email-scanner
- Test with: different platform states and content types

### Type 5: Coordination Skills
"Manage multiple steps and hand off between systems"
- Examples: telegram-job-scorer, career-page-scanner
- Test with: large batches and edge cases

---

## Tips for Great Skills

1. **Be explicit about triggers.** Don't rely on the user to find the skill — list specific phrases and contexts in the description.

2. **Structure for clarity.** Use clear headings, numbered steps, and examples. Assume the skill will be used months later when you don't have context.

3. **Handle edge cases.** "What if the user provides incomplete info?" "What if the platform changes?" Build in defensive steps.

4. **Default to safe.** When in doubt, ask for confirmation or flag for manual review rather than proceeding with assumptions.

5. **Make it reusable.** A good skill works for multiple users and scenarios, not just one specific situation. Use placeholders for names, IDs, URLs, etc.

6. **Document dependencies.** If the skill relies on files, tools, or external systems, state that clearly at the top.

7. **Provide examples.** Show what good input looks like and what good output should be.

8. **Keep it focused.** One skill = one core capability. If it does 5 unrelated things, split it into 5 skills.

---

## Optimization for Triggering

After your skill is working well, optimize the description for better triggering:

- Lead with the primary use case
- List 5-8 specific trigger phrases
- Include synonyms and variations
- Mention when NOT to use it
- Keep it to 2-3 sentences max

Example:
**Bad:** "Use this skill to analyze interview transcripts."
**Good:** "Interview transcript analyzer. Use this whenever you want to review how you did in an interview, ask 'how did I do?', upload a transcript, or analyze interview performance. Also trigger on 'where am I weak', 'show me my progress', or any interview performance question. Provides archetype-aware scoring and tracks progress across multiple interviews."

---

## When to create vs. edit vs. optimize

- **Create:** New skill for a capability that doesn't exist
- **Edit:** Fix bugs, improve instructions, add edge case handling
- **Optimize:** Description is working but triggering is weak, or test results suggest refinement

---

## What NOT to do

- Don't create skills that are just thin wrappers around other tools
- Don't create skills that require constant manual intervention
- Don't put credential handling or sensitive data in skills
- Don't make skills so specialized they're only useful once
- Don't forget to test before declaring a skill "done"
