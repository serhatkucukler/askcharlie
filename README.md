# AskCharlie —  Mental Model Analysis System inspired by Charlie Munger

A Claude Code skill that runs structured 3-layer analysis of any situation, decision, or challenge using Charlie Munger's mental models.

## How it works

When you invoke `/askcharlie` with a situation, it:

1. **Selector Agent** — reads your mental model index, selects the most relevant models across 3 layers (Core Question, Blind Spot, Growth)
2. **Analyst Agent** — generates 3 analysis blocks, each through a different lens
3. **Saves to Obsidian** — creates a structured note in your vault
4. **Logs the result** — maintains a running log with rating averages and improvement suggestions

## Files in this repo

| File | Description |
|---|---|
| `askcharlie.md` | The Claude Code skill |
| `charlie-munger-index.md` | 86 mental models with questions, blind spots, and development areas |

## Requirements

- [Claude Code](https://claude.ai/code) with a skill plugin (e.g. [Superpowers](https://github.com/anthropics/claude-code-superpowers))
- An Obsidian vault (or any markdown-based knowledge base)
- The `charlie-munger-index.md` placed somewhere Claude can read

## Setup

1. Place `askcharlie.md` in your Claude Code skills folder
2. Place `charlie-munger-index.md` somewhere Claude can read (e.g. `YOUR_VAULT_PATH/charlie-munger-index.md`)
3. In `askcharlie.md`, replace all placeholders:

| Placeholder | Replace with |
|---|---|
| `YOUR_VAULT_PATH` | Absolute path to your Obsidian vault |
| `YOUR_PERSONAL_PROFILE.md` | A markdown file describing yourself (optional but recommended — helps model selection feel personal) |
| `YOUR_LOG_PATH` | Where you want the log file saved (e.g. `~/.claude/logs`) |
| `YOUR_FALLBACK_PATH` | Fallback write path if primary is unavailable |

4. Create the folder `NO CONTEXT ASK CHARLIE/` inside your vault. This is where analysis notes are saved.

## Usage

Invoke `/askcharlie` followed by a **problem**, **situation**, or **condition** you want analyzed.

**Problem** — something you're actively struggling with:
```
/askcharlie

I keep starting new books, courses, and projects but rarely finish them.
I've abandoned 4 different learning paths in the last year alone.
I genuinely want to change this pattern. What do you suggest?
```

**Situation** — a decision or crossroads you're facing:
```
/askcharlie

I've been working on a side project for 8 months. It has 12 paying users
but growth has stalled for 3 months. I'm spending 15 hours/week on it.
Should I double down or let it go?
```

**Condition** — a recurring pattern or state you want to understand:
```
/askcharlie

My team keeps shipping features but the product isn't growing.
We've released 11 updates in 6 months. Users sign up but churn within 2 weeks.
I don't know if this is a product problem or a market problem.
```

Then rate the analysis: `ac1` through `ac5` (or just `1`–`5`).

## The 3-Layer Model

| Layer | Purpose |
|---|---|
| **Core Question** | Names the real underlying question — what you're actually deciding |
| **Blind Spot** | Surfaces what you likely can't see from where you're standing |
| **Growth** | Points toward what needs to change, with a concrete first step |

## The mental model index

`charlie-munger-index.md` contains 86 models organized into 4 columns:
- Model name
- What question(s) it helps solve
- What blind spot it reveals
- What development area it strengthens

You can extend this index with your own models — the Selector Agent reads the full table.

## Personal profile (optional)

The skill can read a personal profile file before selecting models. This makes the selection feel personal rather than generic. The file can be a simple markdown note about yourself: your tendencies, recurring challenges, decision-making patterns, goals.

## Log file

After each analysis, `askcharlie-log.md` is updated with:
- The anchor (one-sentence framing of the situation)
- Which models were selected across 3 layers
- Your rating
- A running summary: what's working, what to improve, suggestion for next time

---

Created by [Serhat Kücükler](https://www.linkedin.com/in/serhatkucukler/)
