# Football Prediction Engine

**An AI-Assisted Development Case Study**

An honest record of what happened when a casino IT guy tried to build a self-learning decision engine in 33 days using AI coding tools. The system was built in 33 days — but the learning never stopped. It's still running, still calibrating, still improving from every match result.

This project is less about the predictions and more about the method: what happens when you pair domain expertise with AI-assisted engineering? The system doesn't know if it's "right" yet — and that honesty is part of the story.

> **Status**: Live paper trading. The system is still learning. The World Cup is the final exam.
>
> **Source code**: Released progressively alongside my [LinkedIn series](https://www.linkedin.com/in/sky-leong-6a559290/). Follow the series to see the code evolve.

---

## Why This Project Exists

With the 2026 World Cup approaching, I wanted to explore a question: *Can someone with deep industry knowledge but no traditional software engineering background build a production-grade system using AI coding tools?*

My background is casino IT application systems — working closely with every department (Table Games, Slots, Marketing, Cage, Finance, Surveillance, Junket/Premium) but focused on how casino application systems connect, integrate, and enforce rules across the operation. I'd never built a full software application from scratch. A football domain expert collaborator provided the analytical judgment.

Claude Code became the development partner that translated domain knowledge into working software.

This isn't about the specific domain. It's a method story. The same approach — domain expertise + AI tools + Harness Engineering — could apply to any field where human judgment matters.

---

## What Makes This Different

This is not another ML model that predicts match outcomes from historical data. Instead, it's a **decision engine** that combines:

- **Human judgment** — domain expertise about when markets misprice, which leagues behave predictably, when to pass on a match entirely
- **AI reasoning** — Claude analyzes live statistics, news, odds movements, and historical context for each match, then outputs structured assessments
- **Automated quality gates** — the system grades its own signals, checks data quality before analysis, and learns from past mistakes

The human decides *what questions to ask*. The AI processes *more data than a human can*. The system enforces *discipline that neither human nor AI maintains alone*.

---

## Architecture Overview

```
┌──────────────────┐
│   Telegram Bot    │  User interaction layer
│   (18+ commands)  │  Chinese-language interface
└────────┬─────────┘
         │
┌────────▼─────────┐     ┌──────────────────┐
│    Scheduler      │     │   Deep Analysis   │
│  (automated scan) │     │  (on-demand, per  │
│                   │     │   match analysis)  │
└────────┬─────────┘     └────────┬──────────┘
         │                        │
┌────────▼────────────────────────▼──────────┐
│            Decision Engine                  │
│                                             │
│  Data Collection ─► Core Analysis ─► Signal │
│  (5 API sources)   (Claude API)    Grading  │
│                                    & Filter │
└────────┬────────────────────────────────────┘
         │
┌────────▼─────────┐     ┌──────────────────┐
│  Signal Pipeline  │────►│  Self-Learning    │
│  (grade, filter,  │     │  Loop             │
│   deduplicate)    │     │  (calibrate,      │
└───────────────────┘     │   reflect, adapt) │
                          └──────────────────┘
```

**Data flows in, gets quality-checked, analyzed by Claude, graded across multiple dimensions, filtered, and either recommended or rejected. After settlement, the system reflects on what it got right and wrong — and adjusts.**

---

## Key Engineering Practices

### Harness Engineering

The most important lesson from this project: **rules enforced by tests survive; rules enforced by documentation decay.**

Early on, I wrote detailed rules about code quality, module boundaries, and configuration management. Claude (the AI) would follow them — sometimes. After enough incidents where documentation drifted from reality, I shifted to encoding every critical rule as an automated test.

If a rule matters, it has a test. If it doesn't have a test, it will eventually be violated.

The framework: **Prompt gives direction. Context provides the map. Harness is the vehicle** — mechanically ensuring the AI does it correctly through tests, linters, contracts, and hooks.

### Single Source of Truth

All numerical thresholds, feature flags, and tunable parameters live in one file. No magic numbers scattered across the codebase. When a threshold changes, automated tests verify that documentation stays in sync.

### Self-Learning Loop

Three layers of feedback:

1. **Calibration** — tracks predicted probabilities vs. actual outcomes, adjusts confidence over time
2. **Reflection** — after each result (win or loss), an AI analyzes what went right or wrong and identifies patterns
3. **Wisdom** — recurring patterns (approved by a human) become permanent adjustments to future analysis

The human stays in the loop for anything that changes system behavior.

---

## What I Learned (Living Document)

These are real lessons from 22 versions and 501 tests — each one triggered by something that went wrong. This list grows as the system learns. Last updated: 2026-03-31.

- **AI coding tools are force multipliers, not replacements.** Claude Code wrote ~95% of the code, but every architectural decision came from human judgment. The AI is fast but has no taste — it will happily build the wrong thing perfectly.

- **The biggest risk in AI-assisted development is speed.** You can ship features so fast that you outrun your own understanding of the system. By v10 I was shipping 3 versions per day. That's when things started silently breaking. Process and discipline exist specifically to counteract this.

- **AI fails confidently, not loudly.** On March 16th, I found 10+ contradictions between documentation and code — because the AI answered from memory instead of checking the source. It never said "I'm not sure." It just gave the wrong answer with full confidence. This led directly to building automated consistency checks.

- **Data quality matters more than model quality.** The system's worst predictions came from matches with incomplete data, not from bad analysis. Building a "data passport" — a pre-check that evaluates information quality before any analysis begins — eliminated an entire category of bad signals.

- **Every bug should leave behind a test.** This became a core rule: fix the code AND add a test that prevents the same bug from returning. 501 tests exist because 501 things went wrong. Each test is a scar from a real incident.

- **Configuration drift is the silent killer.** In casino IT, when a parameter changes in one place but not another, the system doesn't crash — it just quietly makes wrong decisions. The exact same thing happened with AI-generated code. Automated cross-checks between config and documentation caught drift that human review missed.

- **Domain expertise can't be replaced by data.** My partner's football judgment (85% win rate from manual filtering) consistently outperformed the system's unfiltered output (48%). The system got better not by becoming smarter, but by learning when to defer to human judgment.

- **The system is still learning.** Every result — win or loss — feeds back into the learning loop. The system learns from the human. The human learns from the system's data. Both are students. The World Cup will be the real test.

---

## Current Status

| Metric | Value |
|--------|-------|
| Version iterations | 22+ |
| Automated tests | 501+ |
| Status | Live paper trading, self-learning active |
| Source code | Releasing progressively with LinkedIn series |

> These numbers are shared for transparency, not as a claim of success. 55% is modest. The system is learning. The World Cup is the exam.

---

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Language | Python 3.12+ |
| Package Manager | uv |
| Match Data | Sportmonks API |
| Odds Data | OddsPapi (aggregated bookmaker odds) |
| AI Analysis | Claude API (claude-sonnet-4-6) |
| User Interface | Telegram Bot (python-telegram-bot) |
| Database | SQLite (local) |
| Testing | pytest (501+ tests) |
| Scheduling | APScheduler |

---

## Project Structure

```
.
├── decision_engine/       # Core prediction engine (facade pattern)
│   ├── __init__.py        #   Public API (facade)
│   ├── _core.py           #   Main analysis logic
│   ├── _data_collection.py#   Multi-source data fetching
│   ├── _signal_processing.py# Signal grading & filtering
│   └── _claude_api.py     #   Claude API integration
├── bot/                   # Telegram command handlers
├── src/
│   ├── api/               # API clients (Sportmonks, OddsPapi)
│   ├── intelligence/      # Calibration, reflection, wisdom
│   └── utils/             # Team registry, market filters
├── scripts/               # Settlement, monitoring, backups
├── tests/                 # 501+ tests
├── scheduler.py           # Automated scanning & push notifications
└── telegram_bot.py        # Bot entry point
```

> **Note**: Configuration files and methodology documentation are intentionally not included in this public repository. Source code will be released alongside the LinkedIn article series.

---

## The Story Behind This Project

I'm documenting this journey in a LinkedIn series: **"33 Days to Build a System"** — an honest record of building a system with AI, what went wrong, what I learned, and what the casino industry taught me about managing AI.

The system was built in 33 days. But the learning is ongoing — and so is the series.

If you're interested in AI-assisted development, human-AI collaboration, or applying domain expertise through AI tools, I'd welcome the conversation.

**LinkedIn**: [Sky Leong](https://linkedin.com/in/sky-leong-6a559290) | **Series hashtag**: #33DaysBuildASystem

---

## Disclaimer

This project is shared for **educational and research purposes only**.

- It is **not betting advice**. Do not use this system or its outputs to make real wagers.
- Past or simulated performance does not indicate future results.
- Sports betting involves significant financial risk.

The value of this project lies in its engineering approach and the lessons learned about human-AI collaboration — not in its predictions.

---

## License

- **Source code**: [MIT License](LICENSE) — free to use, modify, and distribute with attribution.
- **Articles, case studies, and written content** (including the LinkedIn series "33 Days to Build a System"): © 2026 Sky Leong. All rights reserved. No reproduction or republication without written permission. Sharing original links is welcome.

---

*Built with [Claude Code](https://claude.ai/claude-code) as a case study in AI-assisted development.*
*© 2026 Sky Leong*
