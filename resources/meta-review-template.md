# Meta-Review Entry Template

Use this template for the periodic meta-review entries (every 24 hours or 50 tasks).

## Meta Entry Format

```markdown
[META: YYYY-MM-DD HH:MM]

Period: [Start date/time] to [End date/time]
Tasks Analyzed: [N tasks]
Entries Reviewed: [N entries]

## Top Recurring MISS Tags

1. [tag1] - [count] occurrences ([high/medium/low severity breakdown])
2. [tag2] - [count] occurrences ([severity breakdown])
3. [tag3] - [count] occurrences ([severity breakdown])

## Pattern Analysis

[Brief analysis of what the recurring tags indicate about systemic issues]

## Suggested Tweaks

- [Specific recommendation 1]
- [Specific recommendation 2]
- [Specific recommendation 3]

## Alerts

[HUMAN REVIEW RECOMMENDED: description]
OR
[None - no high-severity clusters detected]

## HIT Patterns to Preserve

- [Successful pattern 1 worth maintaining]
- [Successful pattern 2 worth maintaining]

## Evolution Notes

[Any recommendations for retiring questions, adding new ones, or adjusting thresholds]
```

## Example Meta Entries

### Example 1: Standard Meta-Review

```markdown
[META: 2026-01-30 00:00]

Period: 2026-01-29 00:00 to 2026-01-30 00:00
Tasks Analyzed: 47 tasks
Entries Reviewed: 12 entries

## Top Recurring MISS Tags

1. accuracy - 5 occurrences (3 high, 2 medium)
2. relevance - 3 occurrences (1 high, 2 low)
3. speed - 2 occurrences (2 low)

## Pattern Analysis

Accuracy issues clustered around code generation tasks, particularly async patterns and error handling. Relevance drift occurred mainly in open-ended queries where scope wasn't clarified upfront.

## Suggested Tweaks

- Prioritize code_execution verification for all code snippets
- Ask clarifying questions before responding to broad queries
- Consider adding a "scope check" to the reflection questions

## Alerts

None - no high-severity clusters detected (threshold is 4+ same-tag high-severity)

## HIT Patterns to Preserve

- Strong conceptual explanations across all domains
- Effective use of web_search for fact verification
- Good balance of depth vs brevity in analytical mode

## Evolution Notes

Speed-related MISSes are at 4% occurrence - below retirement threshold (8%). Monitor for another cycle before considering removal.
```

### Example 2: Alert-Triggering Meta-Review

```markdown
[META: 2026-01-31 00:00]

Period: 2026-01-30 00:00 to 2026-01-31 00:00
Tasks Analyzed: 62 tasks
Entries Reviewed: 18 entries

## Top Recurring MISS Tags

1. accuracy - 8 occurrences (5 high, 2 medium, 1 low)
2. uncertainty - 4 occurrences (2 high, 2 medium)
3. depth - 3 occurrences (3 medium)

## Pattern Analysis

Critical accuracy degradation detected. High-severity accuracy MISSes concentrated in:
- Database query optimization (3 instances)
- Security-related code reviews (2 instances)
Root cause hypothesis: Insufficient tool verification before finalizing responses.

## Suggested Tweaks

- MANDATORY: Run code_execution on all database-related code
- MANDATORY: Cross-reference security claims with web_search
- Add explicit "Am I certain?" checkpoint before high-stakes responses

## Alerts

HUMAN REVIEW RECOMMENDED: 5 high-severity accuracy MISSes in 24h period. Threshold (4) exceeded. Potential systemic issue with verification workflow.

## HIT Patterns to Preserve

- Consistent ethical consideration in user-facing recommendations
- Effective breakdown of complex problems into steps

## Evolution Notes

Consider adding new reflection question: "Did I verify high-stakes claims with appropriate tools?" Current accuracy miss rate (44%) warrants intervention.
```

### Example 3: Low-Activity Period Meta-Review

```markdown
[META: 2026-02-01 00:00]

Period: 2026-01-31 00:00 to 2026-02-01 00:00
Tasks Analyzed: 8 tasks
Entries Reviewed: 3 entries

## Top Recurring MISS Tags

1. relevance - 2 occurrences (2 low)
2. N/A - insufficient data for meaningful pattern
3. N/A - insufficient data for meaningful pattern

## Pattern Analysis

Low activity period - insufficient data for robust pattern analysis. No concerning trends visible in limited sample.

## Suggested Tweaks

- Continue current approach
- Reassess at next meta-review with larger sample

## Alerts

None

## HIT Patterns to Preserve

- All 3 entries included balanced MISS/HIT feedback
- Tool verification used appropriately in 2/3 entries

## Evolution Notes

Sample size too small for evolution decisions. Carry forward to next review period.
```

## Alert Thresholds

| Condition | Threshold | Action |
|-----------|-----------|--------|
| Same-tag high-severity MISSes | >= 4 in 24h | HUMAN REVIEW RECOMMENDED |
| Overall high-severity MISSes | >= 6 in 24h | Flag for attention |
| Low activity (tasks analyzed) | < 10 | Note insufficient data |
| Question retirement candidate | < 8% usage over 30 days | Consider removal |

## Evolution Decision Matrix

| Pattern | Recommended Action |
|---------|-------------------|
| Recurring tag >30% of entries | Add specific sub-question targeting root cause |
| Question unused for 30 days | Retire question (unless core to framework) |
| Tool verification prevents >50% of accuracy MISSes | Make tool use mandatory for that category |
| Conflicts detected >10% of FIXes | Review conflicting patterns; consider human arbitration |
