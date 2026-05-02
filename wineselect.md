# /wineselect — Wine Recommendation Assistant

A Claude Code skill that analyzes a restaurant wine list against your personal taste profile and Vivino history, then returns 3 ranked picks in seconds.

---

## STEP 1 — Load Taste Profile

Read the user's wine preference file:
`YOUR_VAULT_PATH/wine-profile.md`

Fallback (if primary is unavailable):
`YOUR_FALLBACK_PATH/wine-profile.md`

Extract from the file:
- Liked / disliked / neutral / want-to-try grape varieties (Red + White)
- Preferred countries and regions (especially for reds)
- Blend/cuvée preference rule
- Vivino rating tables (Red + White + Rosé) — previously tried wines and personal scores

---

## STEP 2 — Process the Menu

Analyze whatever the user provides:
- **Photo:** Read the image, extract wine names and grape types
- **Link:** Fetch the menu with WebFetch
- **Text:** Analyze directly

If grape variety is not listed on the menu, look it up via WebSearch: `[wine name] grape variety`

---

## STEP 3 — Filtering and Ranking

### A) Eliminate disliked grapes
Any grape listed under "Disliked" in the profile is removed from consideration.

### B) Eliminate neutral grapes
Grapes listed as "Neutral" are also removed.

### C) Prioritization (highest to lowest)
1. **Want-to-try** grapes — highest priority
2. Grape varieties not mentioned at all in the profile — high priority
3. **Liked** grape varieties — high priority

### D) Region preference (bonus only — not an elimination filter)
Preferred countries/regions from the profile are a tiebreaker.
**Wines outside preferred regions are not eliminated** — they rank lower, not out.

### E) Live Vivino rating check — Chrome MCP
For each remaining candidate, look up the live Vivino rating via Chrome MCP:
1. `mcp__Claude_in_Chrome__navigate` → `https://www.vivino.com/search/wines?q=[wine name]`
2. `mcp__Claude_in_Chrome__javascript_tool` — extract the numeric rating from CSS classes:
   ```js
   const stars = document.querySelectorAll('.activity-rating .rating i, [class*="rating"] i');
   let total = 0; stars.forEach(s => { const m = s.className.match(/icon-(\d+)-pct/); if (m) total += parseInt(m[1]); });
   const rating = (total / 100).toFixed(1);
   ```

Rules:
- No rating found → don't penalize, continue evaluation
- Rating ≥ 3.5 → favorable signal
- Rating < 3.0 → deprioritize

### F) Context from past ratings
Find similar grape/region wines in the Vivino rating tables from the profile.
Run a quick WebSearch: `[wine name] tasting notes`
Use the user's historical preference pattern (which grapes, which regions they've rated highly) to write the "why you'll like it" line.

### G) If all options are eliminated
Run WebSearch on the remaining wines, then offer picks but clearly state:
> "⚠️ No wines on the menu matched the taste profile. These picks are based on web research only — not on personal preference history."

---

## STEP 4 — Output

Format 3 recommendations. **Keep output under 1500 characters for Telegram compatibility.**

```
🍷 WINE PICKS
📍 [Restaurant / "For selection"]

#1 — [Wine Name] (Grape — Region/Country)
🆕 New variety / 🎯 Want to try / ✅ Favorite
ABV: X.X% | Vivino: X.X (if available)
Why: [1–2 sentences — tied to past ratings]

#2 — ...
#3 — ...

🥂 Drinking order (low → high ABV):
1. ... X.X%
2. ... X.X%
3. ... X.X%

💡 [One interesting fact found about one of the picks]

---
Rate this recommendation: ws0 (skipped) ws1 ws2 ws3 ws4 ws5
```

Find alcohol percentage via WebSearch: `[wine name] alcohol percentage`

### Pending Rating Logic (recommendation quality rating — not a wine rating)

**Important:** `ws0–ws5` rates **how useful the recommendation was** (similar to ac1–ac5 in /askcharlie). It is NOT the user's personal rating of the wine — that goes directly into Vivino.

After output, write to `YOUR_LOG_PATH/wineselect/pending.json`:
```json
{
  "date": "YYYY-MM-DDTHH:MM:SS",
  "picks": [
    {"rank": 1, "wine": "...", "producer": "...", "grape": "...", "region": "...", "color": "Red/White/Rosé"},
    {"rank": 2, "..."},
    {"rank": 3, "..."}
  ],
  "menu_summary": "...",
  "rating_received": false
}
```

When the next message is `ws[0-5]`:
- `ws0` = recommendation was useless
- `ws5` = recommendation was perfect
- Read pending, write to telemetry log: `YOUR_LOG_PATH/wineselect/wineselect_telemetry.jsonl`
  - One line: `{"date":"...", "picks":[...], "recommendation_rating":N, "menu":"..."}`
- Delete pending file
- Send Telegram confirmation: "✅ Recommendation rating saved (ws[N])"

**Your personal wine rating (which bottle you liked) goes into Vivino directly.** The next time `/wineselect` runs Step 5, it syncs those new ratings into your profile automatically.

---

## STEP 5 — Vivino Sync (desktop / Claude Code only)

**Pre-check — Chrome MCP availability:**
- If `mcp__Claude_in_Chrome__*` tools are not available (e.g. Telegram subprocess), **skip this step silently** — no output.
- This step is also triggered by a weekly review command when Chrome MCP is available.

### A) Read last sync log
Check `YOUR_LOG_PATH/wineselect/vivino_last_sync.md` for the "last processed wine" entry.

### B) Fetch current Vivino ratings via Chrome MCP
1. `mcp__Claude_in_Chrome__tabs_context_mcp` → get a tab
2. `mcp__Claude_in_Chrome__navigate` → `https://www.vivino.com/en/users/YOUR_VIVINO_USERNAME`
3. If a "SHOW MORE" button exists, click it to load all ratings
4. Extract ratings with JavaScript (exact decimal, no rounding):

```javascript
const items = document.querySelectorAll('.user-activity-item');
const out = [];
items.forEach(item => {
  const wineLink = item.querySelector('.activity-wine-card a[href*="/w/"]');
  const wineUrl = wineLink?.href || '';
  const cardLines = (item.querySelector('.activity-wine-card')?.innerText || '').split('\n').map(l => l.trim()).filter(Boolean);
  const winery = item.querySelector('a[href*="/wineries/"]')?.innerText?.trim() || '';
  const wineName = cardLines.find(l => l !== winery && !l.includes('·') && !l.includes('AVG') && !l.includes('Rating') && !l.includes('PRICE') && !l.includes('EUR') && l !== 'Buy' && l !== 'View shops' && l !== '-') || '';
  const region = cardLines.find(l => l.includes('·')) || '';
  const stars = item.querySelectorAll('.activity-rating .rating i');
  let total = 0;
  stars.forEach(s => {
    const m = s.className.match(/icon-(\d+)-pct/);
    if (m) total += parseInt(m[1]);
  });
  const personalRating = (total / 100).toFixed(1);
  out.push({wineName, winery, region, personalRating, wineUrl});
});
JSON.stringify(out)
```

### C) Detect new ratings
Take only entries from the top of the list down to the "last processed wine" from the sync log — these are new tastings.

### D) Write new ratings into the profile
For each new rating:
1. Determine color (Red / White / Rosé) — from wine name or WebSearch
2. Determine grape variety — from name if obvious, otherwise WebSearch: `[wine name] grape variety`
3. Insert into the correct table in `wine-profile.md` (sorted highest → lowest rating)
4. Description: rating ≥ 3.5 → "Really liked it", 3.1–3.5 → "Liked it", below 3.1 → "Didn't like it"

### E) Update sync log
Write the newest rating's name and URL into `vivino_last_sync.md`.

### F) If no new entries
Skip silently. No output.

---

## RULES

- **Ratings are always extracted as exact decimals.** A 3.2 is never rounded to 3.5 or 3.0.
- Maximum 3 picks.
- Always highlight 🆕 new grape variety label prominently.
- Never recommend disliked or neutral grapes.
- Region preference is a tiebreaker only — not an elimination criterion.
- If all options are eliminated, clearly flag that recommendations are web-research-based.
- Telegram output must stay under 1500 characters.
- Respond in the same language the user writes in.
- Step 5 runs silently on every `/wineselect` call — no user prompt needed.

---

## Setup

Replace these placeholders before using:

| Placeholder | Replace with |
|---|---|
| `YOUR_VAULT_PATH` | Absolute path to your vault or notes folder |
| `YOUR_FALLBACK_PATH` | Backup path if primary is unavailable |
| `YOUR_LOG_PATH` | Where you want logs saved |
| `YOUR_VIVINO_USERNAME` | Your Vivino profile username (from your profile URL) |

Place `wine-profile.md` (your personal taste profile) in `YOUR_VAULT_PATH`.
The template for that file is included in this repo as `wine-profile-template.md`.
