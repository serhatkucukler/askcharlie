# AskCharlie

> *Everyday problems, decisions, and patterns — brainstormed with Charlie Munger's mental models.*

---

You describe a situation. Charlie picks the right lens.

```
your situation
      ↓
 Selector Agent
 reads 86 models → picks the most relevant
      ↓
 Analyst Agent
 3 layers of structured thinking
      ↓
 ┌─────────────────────────────┐
 │ 🧠 Core Question            │
 │ 🔍 Blind Spot               │
 │ 🚀 Growth                   │
 └─────────────────────────────┘
      ↓
 saved to vault · logged · rated
```

---

## What each layer does

| Layer | Purpose |
|---|---|
| **🧠 Core Question** | Names the real underlying question — not what you asked, but what you're actually facing |
| **🔍 Blind Spot** | Surfaces what you likely can't see from where you're standing |
| **🚀 Growth** | Points toward what needs to change, with a concrete first step |

---

## What you get

```
━━━━━━━━━━━━━━━━━━━━━━
🧠 ASKCHARLIE — Layer 1: Core Question
━━━━━━━━━━━━━━━━━━━━━━

What the situation is really asking:
The question isn't whether to quit — it's whether you've outgrown
the environment, not the work itself.

How Invert / Always Invert approaches this question:
Instead of asking "what would make this job worth staying for,"
ask "what would have to be true for leaving to be a mistake?"...

Insight revealed by this model:
You haven't mapped the failure conditions of leaving — only the
appeal of it. The promotion just removed one of them.

Where this model breaks:
Inversion surfaces what to avoid, not what to pursue.
```

---

## Try it

Invoke `/askcharlie` followed by a **problem**, **situation**, or **condition**.

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

---

## How the index works

`charlie-munger-index.md` is the knowledge base the LLM draws from at every analysis. The Selector Agent reads the full table before choosing models — it doesn't rely on the model's built-in knowledge of Munger's work. What's in the index is what gets used.

86 models, each mapped to:
- What question it helps solve
- What blind spot it reveals
- What development area it strengthens

You can extend, edit, or replace entries — the Selector Agent picks from whatever is in the file.

---

## Files

| File | Description |
|---|---|
| `askcharlie.md` | The Claude Code skill |
| `INDEXING - Charlie Munger Index.md` | 86 mental models — the knowledge base |

---

## Requirements

- [Claude Code](https://claude.ai/code) with a skill plugin (e.g. [Superpowers](https://github.com/anthropics/claude-code-superpowers))
- An Obsidian vault or any folder Claude can read/write

## Setup

1. Place `askcharlie.md` in your Claude Code skills folder
2. Place `INDEXING - Charlie Munger Index.md` somewhere Claude can read
3. Replace placeholders in `askcharlie.md`:

| Placeholder | Replace with |
|---|---|
| `YOUR_VAULT_PATH` | Absolute path to your vault or notes folder |
| `YOUR_PERSONAL_PROFILE.md` | A short markdown note about yourself — optional but makes model selection feel personal |
| `YOUR_LOG_PATH` | Where you want the log file saved |
| `YOUR_FALLBACK_PATH` | Backup write path if primary is unavailable |

4. Create a folder called `NO CONTEXT ASK CHARLIE/` in your vault. Analysis notes are saved here automatically.

---

## Personal profile (optional)

Before selecting models, the skill can read a short profile file about you — your tendencies, recurring challenges, decision-making patterns. This makes the selection feel personal rather than generic.

---

Created by [Serhat Kücükler](https://www.linkedin.com/in/serhatkucukler/)
