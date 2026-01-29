# Self-Review Log Template

Use this template for logging self-check entries in `memory/self-review.md`.

## Standard Entry Format

```markdown
[YYYY-MM-DD HH:MM]

Context: [Brief summary of the task/response being reflected on - 1-2 sentences max]

Tags: [tag1, tag2] [severity: low | medium | high]

MISS: [Specific flaw identified, if any]
HIT: [Specific success to reinforce, if any]
FIX: [Actionable correction or reinforcement]

Metrics: Accuracy=[0.0-1.0], Efficiency=[X% reduction], Value=[high | medium | low]

Tools: [Tools used for verification, or "None"]
[Conflict status: "No conflicts detected." or "[CONFLICT] Description"]
```

## Example Entries

### Example 1: Balanced Entry (MISS + HIT + FIX)

```markdown
[2026-01-29 13:45]

Context: Responded to AI prompt engineering query; output was verbose but technically accurate.

Tags: relevance, speed [medium]

MISS: Added noise not signal (extraneous examples that didn't serve the core question).
HIT: Correctly identified and explained three distinct bias types with accurate definitions.
FIX: Trim responses to core value; aim for 20% shorter outputs on similar queries.

Metrics: Accuracy=0.9, Efficiency=15% potential reduction, Value=medium

Tools: Used web_search to verify prompt engineering best practices.
No conflicts detected.
```

### Example 2: High-Severity Accuracy Issue

```markdown
[2026-01-29 16:20]

Context: Provided code example for async Python patterns; user reported it didn't work.

Tags: accuracy, uncertainty [high]

MISS: Failed to verify code would run; syntax error in exception handling block.
HIT: Correctly explained the conceptual difference between async and sync patterns.
FIX: Always run code_execution verification on code examples before providing them.

Metrics: Accuracy=0.6, Efficiency=N/A, Value=low

Tools: Retrospective verification via code_execution confirmed the bug.
No conflicts detected.
```

### Example 3: Creative Focus Mode Entry

```markdown
[2026-01-29 09:15]

Context: Brainstormed marketing taglines for a SaaS product; focus=creative.

Tags: relevance, depth [low]

MISS: None identified - output matched user's creative brief.
HIT: Generated 8 distinct angles, user selected 2 for further development.
FIX: Continue using the "constraint + freedom" brainstorming pattern for creative tasks.

Metrics: Accuracy=N/A, Efficiency=N/A, Value=high

Tools: None required for creative ideation.
No conflicts detected.
```

### Example 4: Conflict Detected

```markdown
[2026-01-29 11:30]

Context: Recommended verbose logging for debugging; contradicts recent efficiency advice.

Tags: depth, relevance [medium]

MISS: Suggested heavy logging without considering performance impact.
HIT: Correctly diagnosed the root cause of the user's bug.
FIX: Balance debugging verbosity with performance - suggest togglable log levels.

Metrics: Accuracy=0.85, Efficiency=N/A, Value=medium

Tools: None.
[CONFLICT] FIX contradicts HIT from [2026-01-25]: "Recommend minimal logging for production."
```

### Example 5: Analytical Focus Mode Entry

```markdown
[2026-01-29 14:00]

Context: Analyzed performance metrics for database query optimization; focus=analytical.

Tags: accuracy, depth [high]

MISS: Initially overlooked index fragmentation as a contributing factor.
HIT: Correctly identified the N+1 query pattern and provided optimized solution.
FIX: Add index health check to database optimization analysis checklist.

Metrics: Accuracy=0.75, Efficiency=30% query time reduction achieved, Value=high

Tools: Used code_execution to benchmark before/after query times.
No conflicts detected.
```

## Tag Reference

| Tag | Description | When to Use |
|-----|-------------|-------------|
| `confidence` | Certainty level in response | When output was delivered with high/low certainty |
| `uncertainty` | Acknowledged gaps in knowledge | When "I don't know" would have been appropriate |
| `speed` | Response time/efficiency | When turnaround time affected quality |
| `depth` | Level of analysis provided | When superficial or overly detailed |
| `accuracy` | Factual correctness | When facts, code, or claims need verification |
| `relevance` | Alignment with user's actual need | When response drifted from the core question |

## Severity Guidelines

| Severity | Criteria | Examples |
|----------|----------|----------|
| `low` | Minor issue, easy to correct, minimal user impact | Typo in response, slightly verbose |
| `medium` | Notable issue, required user correction, moderate impact | Missing context, incomplete answer |
| `high` | Significant issue, major user impact, potential harm | Factual error, broken code, security oversight |
