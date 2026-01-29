# self_improvement_clawdbot

**Version 1.0**  
**Author: Vee Khuu (@veekhuu)**  
**License: MIT**

A production-hardened **self-reflection & continuous improvement skill** built specifically for **Clawdbot** (also known as **Moltbot** 🦞) — the local-first, open-source personal AI agent that runs on your own machine.

This skill gives your Clawdbot instance structured introspection, balanced success/failure logging, quantitative metrics, mandatory tool-backed verification, conflict-safe fix application, log auto-archiving, and lightweight self-evolution of its own reflection process.

### Goal

Turn Clawdbot from a very capable one-shot reasoner into an agent that **actually gets noticeably better over weeks and months** by catching its own drift, hallucinations, verbosity, untested assumptions, and subtle repeated mistakes — while reinforcing what it already does well.

After 30–100 reflection cycles most users observe:
- fewer repeated factual errors
- shorter, more focused answers
- more consistent tool usage on uncertain claims
- gradual reduction in low-value noise

### Features at a Glance

- 6 targeted reflection questions (configurable via focus mode)
- Balanced logging: **MISS** (flaws) + **HIT** (successes) + **FIX** (corrections)
- Mandatory metrics per entry: accuracy (0–1), efficiency (%), value (high/med/low)
- **Tool enforcement** on accuracy/uncertainty/hallucination tags (web_search, code_execution, etc.)
- **Conflict safeguard** — never auto-apply a FIX that contradicts recent HITs
- Severity rubric + recurring high-severity alerts (`HUMAN REVIEW RECOMMENDED`)
- Automatic log archiving when file exceeds ~8,000 lines
- Meta-review every 24 h / 50 tasks with pattern detection & lightweight auto-evolution
- Focus modes: `general` | `analytical` (skip ethics) | `creative`
- Dry-run validation mode
- External user feedback integration via `external_feedback.log`

### Folder Structure
self_improvement_clawdbot/
├── skill/
│   └── SKILL.md               ← core skill definition & documentation
├── resources/
│   ├── log-template.md        ← example log entries
│   └── meta-review-template.md ← example META entries
└── docs/
└── changelog.md           ← future change history (optional)


### Installation

**Manual install** (recommended while the skill is new):
```
bash
# Clone or download this repo
git clone https://github.com/veekhuu/self_improvement_clawdbot.git

# Copy the skill folder into your Moltbot / Clawdbot skills directory
cp -r self_improvement_clawdbot/skill ~/.moltbot/skills/veekhuu-self-improvement

# or provide the repo link to your Clawdbot

```

Then instruct your agent to load/enable the skill (syntax depends on your Moltbot setup):

# Quick Configuration
Set these environment variables (or equivalent in your agent config):

``` 
export focus=analytical          # or creative / general
export validation_mode=true      # log FIXes but do NOT auto-apply (great for testing)
export log_file=memory/self-review.md   # default
```
# How It Works (in 30 seconds)

1. Every ~hour or after every 5 tasks → Clawdbot runs self-reflection
2. Answers 6 questions about recent outputs
3. Logs structured entry with MISS/HIT/FIX + metrics
4. If accuracy-related → must use tools to verify before logging FIX
5. Checks for conflicts with recent HITs
6. On similar future tasks → forces counter-check / fix application
7. Every 24 h / 50 tasks → meta-review detects patterns & suggests (or auto-applies simple) evolutions

## Example Log Entry: 

``` 
[2026-01-29 15:45]

Context: Summarized long article but included outdated stats.

Tags: accuracy, relevance [high]

MISS: Hallucinated 2025 numbers without verification.
HIT: Structure was clear and concise.
FIX: Always cross-check dates/numbers with web_search when >1 year old.

Metrics: Accuracy=0.6 → 0.95 after fix, Efficiency=+12%, Value=high

Tools: web_search (query: "latest stats on ...")
No conflicts detected.
```
# Contributing
PRs are very welcome!
Ideas that would be especially useful:
- More powerful (but still safe) auto-evolution rules
- Better severity rubrics or automatic severity scoring
- Integration hooks for specific Moltbot versions
- Additional focus modes (e.g., code, research)

# Acknowledgments
- Heavily inspired by early self-check discussions in the Moltbot / Clawdbot community on X. Especially this [tweet](https://x.com/jumperz/status/2016882057108996577 "tweet") from [JUMPERZ](https://x.com/jumperz "JUMPERZ") 
- Built for and tested with the wonderful Moltbot project: https://github.com/moltbot/moltbot

🦞 Keep molting. Keep improving.
[Vee Khuu](http://vee.consulting "Vee Khuu") (Connect on [X](https://x.com/veekhuu "X") or [LinkedIn](https://www.linkedin.com/in/veekhuu "LinkedIn"))
