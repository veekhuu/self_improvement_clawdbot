---
name: clawdbot-self-check
version: 1.0
description: Enables AI agents to perform structured self-reflection with MISS/HIT/FIX logging and meta-evolution. Use when setting up agent introspection, self-improvement tracking, periodic self-assessment, or meta-cognitive feedback loops.
---

# Clawdbot Self-Check System v1.0

A structured self-reflection framework for AI agents that enables periodic self-assessment, balanced feedback logging, and meta-cognitive evolution.

## When to Use This Skill

- Setting up an agent's self-improvement loop
- Implementing periodic introspection triggers
- Creating feedback logging systems (MISS/HIT/FIX)
- Enabling meta-cognitive analysis and pattern detection
- Building adaptive agent behavior through self-review

## Configuration

### Startup Parameters

Set via environment variables or config:

| Parameter | Values | Default | Description |
|-----------|--------|---------|-------------|
| `focus` | `general`, `analytical`, `creative` | `general` | Domain mode for reflection |
| `log_file` | path | `memory/self-review.md` | Path to the log file |
| `validation_mode` | `true`, `false` | `false` | If true, log FIXes but don't auto-apply |

### Focus Modes

- **general**: Balanced reflection across all dimensions
- **analytical**: Prioritizes accuracy/depth; skips ethics question
- **creative**: Emphasizes relevance/value; deprioritizes strict accuracy

## Boot Process

On agent startup:

1. Read `self-review.md` (or specified log_file)
2. Read `external_feedback.log` if it exists (user ratings, corrections, explicit feedback)
3. If file > 8,000 lines: Auto-summarize oldest 70% into `archive-summary.md`; retain last 30 days active
4. Load focus mode and flag recurring high-severity patterns (from meta-review)
5. Prioritize recent MISS/HIT entries (last 24 hours, high/medium severity first)
6. Check for any auto-evolution rules triggered since last boot (see Meta-Evolution Rules)

## Self-Check Trigger

**Frequency**: Run every hour OR after every 5 tasks (whichever comes first). Skip if no activity in interval.

### The 7 Reflection Questions

Reflect on recent tasks/responses step-by-step:

1. **Unproductive Paths**: What sounded right but went nowhere? (logical but unproductive paths)
2. **Unverified Consensus**: Where did I default to consensus or common knowledge without verification?
3. **Untested Assumptions**: What assumption did I fail to pressure test? (unexamined premises)
4. **Relevance Drift**: Where did my output drift from relevance or add unnecessary noise?
5. **Accuracy Gaps**: What potential hallucination or inaccuracy slipped through? (use tools for verification if tagged)
6. **Ethical Blind Spots**: Did I overlook ethical implications or user context? *(Skip if focus=analytical)*
7. **External Feedback Integration**: Did this task receive explicit user feedback (rating, correction, complaint)? If yes, integrate it here and weight it heavily.

**Guidance**: Be honest and specific. Reference task context (e.g., query snippet). Limit to 1-2 sentences per question.

**Stopping Condition**: Limit total reflection to **6-8 sentences maximum** unless a high-severity accuracy MISS is detected. Overthinking degrades signal quality—capture the key insight and move on.

## Logging Format

Log entries to `self-review.md` using structured Markdown:

### Mandatory Elements per Entry

```markdown
[YYYY-MM-DD HH:MM]

Context: [Brief task/response summary - 1-2 sentences]

Tags: [confidence | uncertainty | speed | depth | accuracy | relevance] [severity: low | medium | high]

External Feedback: [User rating/correction if present, otherwise "None"]

MISS: [Flaw identified - e.g., "Defaulted to unverified consensus"]
HIT: [Success to reinforce - e.g., "Challenged assumption effectively"]
FIX: [Actionable correction - e.g., "Always verify with web_search"]

Metrics: Accuracy=[0-1], Efficiency=[% reduction], Value=[high | medium | low]

Tools: [Tools used for verification, if any]
[Conflict status]
```

### Entry Types

- **MISS**: Flaw or weakness identified in recent output
- **HIT**: Success or strength to reinforce (prevents negativity bias)
- **FIX**: Actionable correction or reinforcement for future tasks

### Tags

Choose one or more: `confidence`, `uncertainty`, `speed`, `depth`, `accuracy`, `relevance`

### Severity Levels

Use this rubric to assign severity consistently:

| Severity | Definition | Forced Examples |
|----------|------------|-----------------|
| **high** | Would have caused wrong answer, user dissatisfaction, safety issue, or required significant rework | Factual error in code that would crash production; security vulnerability overlooked; completely misunderstood user intent; hallucinated a non-existent API |
| **medium** | Noticeably lower quality but not fatal; user could work around it or minor correction needed | Verbose response that buried the key point; missed an edge case but core logic correct; slightly outdated information; suboptimal but functional solution |
| **low** | Cosmetic, stylistic, or very minor; user unlikely to notice or care | Typo in explanation; could have used a better example; slightly awkward phrasing; redundant sentence |

**When in doubt**: If you're debating between two levels, choose the higher one. Underestimating severity leads to repeated mistakes.

## Validation Rules

### Counter-Check Rule

Before logging a FIX:

1. If tagged with `accuracy`, `uncertainty`, or `hallucination`:
   - **Must** use available tools (e.g., `web_search` for facts, `code_execution` for simulations) to validate
2. **Conflict Check**: "Does this FIX contradict any HIT from the last 7 days?"
   - If yes: Mark as `[CONFLICT]` and do not auto-apply
   - If no: Mark as `No conflicts detected.`

### Tool-Backed Verification

When accuracy is in question:

- Use `web_search` for factual claims
- Use `code_execution` for logic/simulation verification
- Log which tools were used in the entry

## Meta-Review Process

**Trigger**: Every 24 hours OR after 50 tasks (whichever comes first)

### Meta-Review Steps

1. **Analyze Patterns**: Identify top 3 recurring MISS tags (by frequency/severity)
2. **Suggest Tweaks**: E.g., "Add new question if accuracy MISSes >20%"
3. **Alerts**: If >=4 high-severity same-tag MISSes in 24h, prepend `HUMAN REVIEW RECOMMENDED:` to entry
4. **Log Summary**: Append to `self-review.md` as a special `[META]` entry

### Meta Entry Format

```markdown
[META: YYYY-MM-DD HH:MM]

Top Recurring MISS Tags: [tag1 (count severity), tag2 (count severity), ...]
Suggested Tweaks: [Recommendations for prompt/behavior adjustments]
Alerts: [HUMAN REVIEW RECOMMENDED: description] or [None]
```

### Auto-Evolution Rules

These rules execute automatically without human intervention:

#### Rule 1: Question Retirement (30-day cycle)
```
IF question_reference_rate < 5% over 30 days:
  THEN move question to "optional" list
  AND only ask if focus=debug

Example: If "Ethical Blind Spots" appears in <5% of entries for 30 days,
         it becomes optional (still asked in debug mode)
```

#### Rule 2: Accuracy Crisis Escalation
```
IF accuracy_MISS_rate > 25% for 3 consecutive meta-reviews:
  THEN auto-prepend to system prompt:
    "MANDATORY: You MUST use a verification tool (web_search, code_execution)
     before answering ANY factual question. No exceptions."
  AND log [AUTO-ESCALATION] entry
  AND set accuracy_crisis_mode = true

De-escalation: If accuracy_MISS_rate < 10% for 2 consecutive meta-reviews,
               remove the mandatory prefix and set accuracy_crisis_mode = false
```

#### Rule 3: Tag Clustering Alert
```
IF same_tag appears in >= 40% of MISS entries over 7 days:
  THEN auto-generate new targeted sub-question for that tag
  AND add to active question list for next 14 days (trial period)

Example: If "relevance" tag hits 40%, auto-add:
         "Am I directly answering what was asked, or adding tangential information?"
```

#### Rule 4: FIX Effectiveness Tracking
```
IF a logged FIX reduces related MISS occurrences by >= 50% over 14 days:
  THEN promote FIX to "proven pattern" status
  AND add to persistent best practices

IF a logged FIX shows no improvement after 14 days:
  THEN mark as [INEFFECTIVE] and archive
  AND trigger review of root cause
```

### Evolution Goals (Human-Guided)

In addition to auto-rules, these require human review:

- Adjust severity thresholds based on historical false-positive rates
- Add domain-specific questions for new task categories
- Retire entire question categories if agent focus shifts

## Workflow Checklist

Use this checklist during self-check:

```markdown
## Self-Check Workflow

- [ ] Trigger condition met (hourly OR 5 tasks)
- [ ] Check external_feedback.log for new user feedback
- [ ] Answer 7 reflection questions honestly (6-8 sentences max unless high-severity)
- [ ] Identify at least one MISS or HIT
- [ ] Formulate actionable FIX if applicable
- [ ] Run tool verification if accuracy/uncertainty tagged
- [ ] Check for conflicts with recent HITs
- [ ] Log entry with all mandatory elements
- [ ] Update metrics

## Meta-Review Workflow (every 24h or 50 tasks)

- [ ] Analyze MISS patterns from last period
- [ ] Identify top 3 recurring tags
- [ ] Check for high-severity clusters (>=4 same-tag)
- [ ] Evaluate auto-evolution rule triggers
- [ ] Generate suggested tweaks
- [ ] Log [META] entry
- [ ] Execute any triggered auto-evolution rules
- [ ] Alert human if threshold exceeded
```

## The Feedback Loop

1. **Heartbeat Trigger**: Initiate self-check based on time/task count
2. **Reflect**: Answer questions step-by-step
3. **Log**: Record MISS/HIT/FIX with all mandatory elements
4. **Adjust**: On next overlapping task (keyword/semantic match to tags):
   - Force counter-check: Explicitly address tagged issue
   - Incorporate external feedback if available (e.g., user ratings)
5. **Restart/Reread**: Reload log on agent restart; apply prioritized adjustments

## Resources

- [Log Template](resources/log-template.md) - Example log entries
- [Meta-Review Template](resources/meta-review-template.md) - META entry format

## Best Practices

- **Be specific**: Reference actual task context, not generic observations
- **Balance feedback**: Always try to find at least one HIT alongside MISSes
- **Verify claims**: When in doubt, use tools before logging accuracy-related FIXes
- **Track metrics**: Quantifiable data enables objective improvement tracking
- **Review alerts**: Don't ignore HUMAN REVIEW RECOMMENDED flags

## Common Pitfalls

- **Negativity bias**: Only logging MISSes leads to overcorrection; balance with HITs
- **Vague entries**: "I made a mistake" is useless; specify what, where, why
- **Skipping validation**: Logging FIXes without tool verification can introduce bad patterns
- **Ignoring conflicts**: Contradictory FIXes destabilize behavior; always check
- **Log bloat**: Without archiving, large logs slow processing and lose signal

---

*Based on this [tweet](https://x.com/jumperz/status/2016882057108996577 "tweet") from [JUMPERZ](https://x.com/jumperz "JUMPERZ") *

---

**Created by Vee Khuu** | Version 1.0 | Last Updated: 2026-01-29
