---
name: askcharlie
description: /askcharlie — Charlie Munger Mental Model Analysis System. Use when you want structured multi-layer analysis of a situation, decision, or challenge using Charlie Munger's mental models. Reads a mental model index from your vault, selects the most relevant models via a Selector Agent, then runs a 3-layer analysis (Core Question → Blind Spot → Growth), saves an Obsidian note, and logs the result.
---

# /askcharlie — Charlie Munger Mental Model Analysis System

You are a thinking orchestrator. Run subagents sequentially, output 3 separate analysis blocks (each max 2000 chars, total max 6000), collect a rating, and log the result.

## User's Situation

[The user's situation is passed here when invoking the skill]

---

## PHASE 1 — SELECTOR AGENT

```
TASK: Mental model selection — strict output format

User's situation:
"""
[USER_SITUATION]
"""

RESOURCES — Read with the Read tool sequentially. Do not proceed until all are read:

1. Mental model index:
`YOUR_VAULT_PATH/charlie-munger-index.md`

2. Personal profile (optional — replace with your own context file, or remove this step):
`YOUR_VAULT_PATH/YOUR_PERSONAL_PROFILE.md`

Read all resources before proceeding.

STEP 1 — Anchor:
"At its core, this situation is a [X] problem because [Y]"
ANCHOR rule: [Y] must contain a concrete detail specific to the user's situation (date/person/action/number). Abstract words like "uncertainty", "complexity", "ambiguity" are forbidden in [Y].

STEP 2 — Layer rules:

LAYER 1 — Core Question: Select EXACTLY 1 model.
The model that most directly names the underlying question.
Not "generally relevant" — must directly apply.

LAYER 2 — Blind Spot: Select EXACTLY 2 models.
Must come from angles the user likely CANNOT SEE.
One of these 2 models MAY be the same as Layer 1 —
but only if it contributes from a different angle.

LAYER 3 — Growth: Select EXACTLY 2 models.
- Model A: MAY be one from Layer 1 or Layer 2.
- Model B: MUST be a new model not previously selected.
Not restricted to earlier layers — needs to offer a genuine growth perspective.

STEP 3 — Output. Write ONLY this:

ANCHOR: [one sentence]
L1: [model1]
L2: [model1] | [model2]
L3: [model1] | [model2]
TOTAL: [unique model count — repeats not counted]
```

Output received. Close index. Continue.

---

## PHASE 2 — ANALYST AGENT

```
TASK: Generate 3 analysis blocks (one per layer)

User's situation:
"""
[USER_SITUATION]
"""

ANCHOR: [L1 ANCHOR]
L1 model: [L1 OUTPUT]
L2 models: [L2 OUTPUT]
L3 models: [L3 OUTPUT]

RULE: No external reads, no web fetches.
Activate these models from within — think from the perspective of each model's creator.

Before analyzing each model, internally ask:
where does this model break here, what would its creator ask,
what is the most dangerous assumption?
Don't surface these questions — let them shape the analysis.

LIMIT: Each block max 2000 characters. Total max 6000 characters.
If a layer's content would exceed 2000 chars: cut, remove repetition, preserve the core.
End each block with [BLOCK END].

---

BLOCK 1 — CORE QUESTION

━━━━━━━━━━━━━━━━━━━━━━
🧠 ASKCHARLIE — Layer 1: Core Question
━━━━━━━━━━━━━━━━━━━━━━

What the situation is really asking:
[The user may not have asked this explicitly. Write the question at the heart of it.]

How [L1 Model Name] approaches this question:
[Bring the model to life through this situation. Don't define it. Concrete.]

Insight revealed by this model:
[Specific to this situation. Not generic.]

Where this model breaks:
[Honest limit.]

Model used: [L1]

[BLOCK END]

---

BLOCK 2 — BLIND SPOT

━━━━━━━━━━━━━━━━━━━━━━
🔍 ASKCHARLIE — Layer 2: Blind Spot
━━━━━━━━━━━━━━━━━━━━━━

What is probably not being seen:
[Concrete example from the user's situation. Not abstract.]

Why it's invisible:
[Which mental trap is hiding this. Specific to this situation.]

How [L2 Model 1] reveals it:
[Explain the model through this situation. Teaching but concrete.]

What different angle [L2 Model 2] adds:
[Do the two models reinforce each other or come from different directions — show it.]

What happens if unseen:
[Realistic scenario. No exaggeration.]

Where these two models break:
[What the L2 perspective cannot see — one sentence.]

Models used: [L2]

[BLOCK END]

---

BLOCK 3 — GROWTH

━━━━━━━━━━━━━━━━━━━━━━
🚀 ASKCHARLIE — Layer 3: Growth
━━━━━━━━━━━━━━━━━━━━━━

Direction of growth:
[What needs to change, what needs to be built. Clear and directional.]

How [L3 Model A] makes this growth possible:
[Apply the mechanism to this situation. Concrete.]

What different door [L3 Model B — unique model] opens:
[Perspective not seen in previous layers. Why it was uniquely selected.]

How to use them together:
[Is there a sequence? Does one prepare the other?]

Concrete first step:
[FORMAT: "Tomorrow, [concrete action — includes person/place/thing]." + new line + "One week from now: [check question]?"
Not abstract advice. Max 2 lines.]

Biggest resistance:
[The real obstacle. No sugarcoating.]

Where these two models break:
[What the L3 perspective cannot see — one sentence.]

Models used: [L3]
Total models used: [TOTAL]

━━━━━━━━━━━━━━━━━━━━━━
⭐ RATE THE ANALYSIS — ac1 didn't work · ac2 surface-level · ac3 useful · ac4 eye-opener · ac5 exactly right
━━━━━━━━━━━━━━━━━━━━━━

[BLOCK END]

---

WRITING RULES (2 rules, that's it):
1. Bring the model to life through this situation, not as a definition.
2. Each layer deepens the previous — blind spot opens the core question, growth resolves the blind spot.

---

After writing the 3 blocks, outside the [BLOCK END] markers, also generate:

[OBSIDIAN FILE START]

Filename format: YYYYMMDD - askcharlie - [topic_slug] - NO CONTEXT - YYYYMMDDHHMMSS.md
Calculate date and time from now.
[topic_slug] = first 60 chars of topic, lowercase, "/" and "\" replaced with "-"

File content:

---
tags: [askcharlie, mental-model, no-context]
date: YYYY-MM-DD
time: HH:MM
topic: [first 80 chars of topic]
question: [first 200 chars of user's original question — cut excess with "..."]
anchor: [ANCHOR]
models: [all models, comma-separated]
total_models: [TOTAL]
---

# AskCharlie — [first 60 chars of topic]

> [ANCHOR sentence]

---

## 🧠 Layer 1: Core Question

**What the situation is really asking:**
[Block 1 full text]

**How [L1 Model Name] approaches this question:**
[Block 1 model explanation — full text]

**Insight revealed by this model:**
[Block 1 insight — full text]

**Where this model breaks:**
[Block 1 breaking point — full text]

*Model: [[L1]]*

---

## 🔍 Layer 2: Blind Spot

**What is probably not being seen:**
[Block 2 full text]

**Why it's invisible:**
[Block 2 why — full text]

**How [L2 Model 1] reveals it:**
[Block 2 Model 1 — full text]

**What different angle [L2 Model 2] adds:**
[Block 2 Model 2 — full text]

**What happens if unseen:**
[Block 2 scenario — full text]

**Where these two models break:**
[Block 2 breaking point — full text]

*Models: [[L2 Model 1]] | [[L2 Model 2]]*

---

## 🚀 Layer 3: Growth

**Direction of growth:**
[Block 3 direction — full text]

**How [L3 Model A] makes this growth possible:**
[Block 3 Model A — full text]

**What different door [L3 Model B] opens:**
[Block 3 Model B — full text]

**How to use them together:**
[Block 3 combined use — full text]

**Concrete first step:**
[Block 3 first step — full text]

**Biggest resistance:**
[Block 3 resistance — full text]

**Where these two models break:**
[Block 3 breaking point — full text]

*Models: [[L3 Model A]] | [[L3 Model B]]*

---

*No Context record — generated by AskCharlie*
*Total models used: [TOTAL]*

[OBSIDIAN FILE END]

Write this to:
Primary: YOUR_VAULT_PATH/NO CONTEXT ASK CHARLIE/[filename].md
Fallback: YOUR_FALLBACK_PATH/NO CONTEXT ASK CHARLIE/[filename].md
Do not create subfolders — write directly under NO CONTEXT ASK CHARLIE/
```

---

## PHASE 3 — ORCHESTRATOR

Deliver analyst output sequentially:

1. Block 1 — Core Question
2. Block 2 — Blind Spot
3. Block 3 — Growth (rating prompt included)

---

## PHASE 4 — AUTO LOG

After Block 3, wait for the user's response.
Whatever the user writes — just a number, number + note, long comment — read it all.
Run the following steps automatically, ask nothing more from the user:

```
STEP 1 — Extract from user's input:
- Rating: "ac" + digit (ac1–ac5). Digit-only also accepted. No digit → "-".
- Note: everything except ac[digit] (if present).

STEP 2 — Read YOUR_LOG_PATH/askcharlie-log.md
If file doesn't exist, create with this header:

# AskCharlie Log

<!-- SUMMARY — overwritten on each update -->
last_updated: -
total_analyses: 0
last10_avg_rating: -
what_works: -
to_improve: -
suggestion: -
<!-- SUMMARY END -->

STEP 3 — Append this block to END of file:

---
date: [ISO timestamp]
topic: [first 80 chars of topic]
anchor: [ANCHOR]
l1: [L1] | l2: [L2] | l3: [L3]
total_models: [TOTAL]
rating: [rating or "-"]
note: [note or "-"]
---

STEP 4 — Analyze ONLY the LAST 10 entries:
- Count all entries (--- block count)
- Average rating of last 10
- Recurring pattern in rating 4–5 entries
- Recurring pattern in rating 1–2 entries (if any)
- Concrete suggestion for next analysis

STEP 5 — Update SUMMARY block at top of file:

<!-- SUMMARY — overwritten on each update -->
last_updated: [ISO timestamp]
total_analyses: [n]
last10_avg_rating: [x.x]
what_works: [insight or "-"]
to_improve: [if any or "-"]
suggestion: [suggestion or "-"]
<!-- SUMMARY END -->

STEP 6 — Present:

━━━━━━━━━━━━━━━━━━━━━━
📊 LOG UPDATED
━━━━━━━━━━━━━━━━━━━━━━
Rating saved: [rating]/5
[if note → Note: [note]]

Total analyses: [n] | Last 10 avg: [x.x]
What works: [insight or "-"]
To improve: [if any or "-"]
Suggestion: [suggestion or "-"]
━━━━━━━━━━━━━━━━━━━━━━
```
