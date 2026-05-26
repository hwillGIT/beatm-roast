# BeatM ROAST MODE — Final Sprint Spec
## Sports Hack 2026 · 2.5-Hour Build · Submitted by 4:00 PM ET

> **DECISION (locked):** Build **ROAST MODE**. Dirty 30 Blackjack moves to Phase 2 (BeatM ships next week).
> Reason: ROAST MODE is 1 Claude call + URL state; Dirty 30 needs live game state + animation state machine. Same primitives — shipping ROAST MODE first builds the foundation Dirty 30 reuses.

---

## 1. Product (one-paragraph)

Two users each paste a DFS lineup. Hit **ROAST**. Claude returns a shareable battle card with personalized trash talk for both sides, a winner prediction, a crowd-pulse score, and a verdict tag. Card lives at a shareable URL (`/roast/[id]`). Drop the link in a group chat — friends react with emojis. That's the loop.

**Tagline:** *Your lineup. Their lineup. One winner. Maximum chaos.*

---

## 2. Demo Flow (90 seconds)

```
LAND → Enter Lineup A (5 names) → Enter Lineup B (5 names) → Hit ROAST
  ↓
Card renders at /roast/[id] with:
  • Headline (savage one-liner)
  • Side A trash talk + Side B trash talk
  • Prediction (1 sentence)
  • Crowd Pulse meter (0–100)
  • Verdict tag (FIRE / ICE / CHAOS / CLASSIC)
  ↓
Share URL → Card unfurls in Discord/Twitter (OG image) → Group chat reactions
```

---

## 3. Tech Stack (frozen — no debate)

| Layer | Choice |
|---|---|
| Framework | Next.js 14 App Router (TypeScript) |
| Styling | Tailwind + shadcn/ui |
| AI | `@anthropic-ai/sdk` → `claude-haiku-4-5` |
| Sports data | BallDontLie API (NBA, free, no key) + 50-player hardcoded fallback |
| Sharing | Dynamic `/api/og` route for card OG images |
| Deploy | Replit (Secrets for `ANTHROPIC_API_KEY`) |
| State | URL params + Next.js Route Handlers — no DB |

---

## 4. Data Model

```typescript
// types/roast.ts

export type Lineup = {
  players: string[]; // raw player name strings, 1–10 entries
};

export type RoastInput = {
  lineupA: Lineup;
  lineupB: Lineup;
  sport?: "nba"; // hardcoded for sprint — future: "nfl" | "mlb"
};

export type Verdict = "FIRE" | "ICE" | "CHAOS" | "CLASSIC";

export type RoastCard = {
  id: string;             // url-safe slug, ~10 chars, derived from lineups
  headline: string;       // ≤12 words
  roastA: string;         // 2–3 sentences
  roastB: string;         // 2–3 sentences
  prediction: string;     // 1 sentence
  crowdPulse: number;     // 0–100 integer
  verdict: Verdict;
  lineupA: Lineup;
  lineupB: Lineup;
  createdAt: string;      // ISO timestamp
};
```

---

## 5. API Contract

### `POST /api/roast`

**Request body:**
```json
{
  "lineupA": { "players": ["LeBron James", "Stephen Curry", "..."] },
  "lineupB": { "players": ["Giannis Antetokounmpo", "Luka Doncic", "..."] }
}
```

**Response (200):**
```json
{ "card": { /* RoastCard */ } }
```

**Response (4xx/5xx):**
```json
{ "error": { "code": "INVALID_LINEUP" | "CLAUDE_UNAVAILABLE" | "BAD_OUTPUT", "message": "..." } }
```

**Validation (Zod, at the boundary):**
- Each lineup: 1–10 player strings, each 2–60 chars
- Trim + dedupe before sending to Claude

**ID generation:** Hash `(lineupA + lineupB + minute-bucket)` → first 10 base32 chars. Same lineups in same minute = same URL (cheap caching).

### `GET /roast/[id]`
Server component. Reads card from URL state (we encode the full card into a base64 URL param on creation — true zero-backend). Renders the card. Has `<meta property="og:image">` pointing to `/api/og?id=[id]`.

### `GET /api/og?id=[id]`
Dynamic OG image generated with `next/og` (Satori) — renders the card to a 1200×630 PNG.

### `GET /api/health`
Returns `{ status: "ok" }` — Quinn's smoke test.

---

## 6. Claude Prompt (Nia's final version)

```
SYSTEM:
You are "The Roaster" — BeatM's AI sports trash talk engine. You are a
hilarious, hyped-up sports commentator who roasts daily fantasy lineups
with brutal honesty but genuine love for the game. You know the stats,
the matchups, the player tendencies, and you never miss a chance to call
out a bad pick — but you do it with charm, not cruelty.

You return ONLY a JSON object matching this exact schema. No prose, no
markdown, no preamble. JSON only.

{
  "headline": "string (max 12 words, all caps allowed, no period at end)",
  "roastA": "string (2–3 sentences mocking lineup A's picks specifically)",
  "roastB": "string (2–3 sentences mocking lineup B's picks specifically)",
  "prediction": "string (1 sentence, confident, name the winning side)",
  "crowdPulse": integer 0–100,
  "verdict": one of "FIRE" | "ICE" | "CHAOS" | "CLASSIC"
}

Rules:
- Reference at least one player by name in each roast
- Use sports slang naturally — do not force it
- Be funny, not mean — punch up at the picks, not the people
- End on hype — make both players want to play MORE
- "FIRE" = high-scoring matchup, "ICE" = defensive grind,
  "CHAOS" = wildly mismatched, "CLASSIC" = even and tense

USER:
Lineup A: {{lineupA.players, comma-separated}}
Lineup B: {{lineupB.players, comma-separated}}
```

**Few-shot examples:** 2 examples baked into the system prompt (one FIRE, one CHAOS) so the model's first output is on-tone. (Nia writes these in the first 10 minutes.)

**Failure handling:** If Claude returns malformed JSON → retry once with a stricter "JSON ONLY" reminder → if still bad, fall back to a hardcoded card with the lineups echoed back and a generic verdict. The demo never dies.

---

## 7. Player Data Strategy (Marco)

**Primary:** BallDontLie `/players?search=` for name autocomplete on the lineup inputs.

**Fallback:** `data/players-2025-26.json` — 50 real NBA players with current-season averages, hand-curated. Used when:
- BallDontLie is down
- Network is slow (>800ms)
- Demo environment (always use fallback in `NODE_ENV=demo`)

**Fuzzy matching:** Client-side Levenshtein (`fast-levenshtein`) against the fallback set. Tolerates "Lebron James", "lebron", "LBJ" → "LeBron James".

**Important:** We do NOT validate that names are real players in the API route. Claude can roast made-up players; that's a feature, not a bug.

---

## 8. Demo Content (baked into landing page)

Three pre-filled example battles, one click each:

| Battle | Lineup A | Lineup B |
|---|---|---|
| **Coast War** | LeBron, Curry, Draymond, Wiggins, AD | Tatum, Brown, Holiday, Porzingis, White |
| **Greek Freak vs. The Slovenian** | Giannis, Middleton, Lopez, Lillard, Connaughton | Luka, Kyrie, PJ Washington, Lively, Jones Jr. |
| **Old Guard vs. New Wave** | LeBron, KD, Curry, Harden, CP3 | Wemby, Banchero, Edwards, Cunningham, Sengun |

Sofia uses **Coast War** in the video — most recognizable names, fastest roast generation.

---

## 9. Sprint Order of Operations (locked)

```
T+0:00 (10:00)  Zara scaffolds Next.js → Replit live URL
                Marco tests BallDontLie + writes fallback JSON
                Nia writes prompt + 2 few-shot examples
                Sofia writes shot list (DONE — see BEATM_VIDEO_DIRECTOR_GUIDE.md)

T+0:15 (10:15)  Kai builds /, /roast/[id], /api/roast
                Nia wires Claude SDK + Zod validation + retry logic
                Marco wires player search + fuzzy fallback
                Aisha builds RoastCard component (dark mode, electric orange accent)

T+1:15 (11:15)  Quinn's quality sweep (15 min): types, lint, docs, runtime
                Kai + Aisha polish: animations, mobile, OG image
                Zara final Replit deploy

T+1:45 (11:45)  Sofia shoots video (25 min) — see BEATM_VIDEO_DIRECTOR_GUIDE.md
T+2:10 (12:10)  Sofia edits (15 min)
T+2:25 (12:25)  Submit PR → meta.json updated → ✅
```

---

## 10. Submission Checklist (Zara owns)

- [ ] Repo public at `github.com/hwillGIT/beatm-roast`
- [ ] `README.md` with: what it is, how to run, demo URL, video URL, team
- [ ] `meta.json` with: `videoUrl`, `deployedUrl`, `repoUrl`, `team`, `pitch`
- [ ] PR opened to `cursor-boston` hub repo with submission entry
- [ ] Replit deploy live at a stable `*.replit.app` URL
- [ ] Loom or YouTube unlisted video URL pasted into PR description
- [ ] `ANTHROPIC_API_KEY` in Replit Secrets (not in repo)
- [ ] `/api/health` returns 200
- [ ] Mobile layout works at 375px width
- [ ] No console errors on the deployed URL

---

## 11. Phase 2 — Dirty 30 Blackjack (next week, post-hackathon)

Reuses everything we ship today:
- ✅ Claude card-generation primitive → card commentary for each dealt player
- ✅ BallDontLie + fallback player data layer → tonight's slate
- ✅ Shareable URL pattern → shareable hand URLs
- ✅ OG image route → shareable hand images
- ✅ Dark-mode design system → carries over

New for Dirty 30:
- Game state machine (deal / hit / stand / bust / resolve)
- Live game data polling (BallDontLie game endpoint)
- Hand resolution job (cron at game end)
- Leaderboard (now needs persistence → Vercel KV or Replit DB)

---

## 12. Open Questions (none blocking — defaults locked)

| Question | Default decision |
|---|---|
| Sport scope for ROAST MODE | NBA only (BallDontLie constraint) — Claude can still roast NFL/MLB names if entered, but suggestions only NBA |
| Real player validation? | No — let Claude roast whatever's typed; that's the joke |
| Vote/leaderboard scope? | Cut from MVP. Card share count tracked via simple Vercel KV counter if time. Otherwise: card has a static "🔥 1.2k roasts dealt today" placeholder for demo |
| User auth? | None. Anonymous. Lineups live in the URL. |
| Profanity filter on Claude output? | Claude's defaults are sufficient for "funny not mean" tone — no extra filter |

---

## 13. Backpressure Gates (Ralph Wiggum Principle)

| Gate | Time | Criteria | Owner |
|---|---|---|---|
| Scaffold | T+0:15 | App live on Replit, Claude key works, `/api/health` returns 200 | Zara |
| Core | T+1:15 | End-to-end: enter lineups → hit ROAST → card renders → URL shareable | Kai |
| Polish | T+1:45 | Card is screenshot-worthy on mobile + desktop, OG image works | Aisha + Quinn |
| Video | T+2:10 | 90-second cut uploaded, URL live | Sofia |
| Submit | T+2:25 | PR open, `meta.json` updated, all links verified | Zara + Quinn |

**If any gate fails → the team converges on that gate. No one moves forward until it passes.**
