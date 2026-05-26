# UI Flow & Screen Mockups

---

## The Three Screens

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   LANDING       │    │   ROAST CARD    │    │   SHARED VIEW   │
│                 │    │                 │    │                 │
│  Two lineups +  │───▶│  Battle card    │───▶│  Standalone     │
│  ROAST button   │    │  + share/again  │    │  card (friend   │
│                 │    │                 │    │  arrives here)  │
└─────────────────┘    └─────────────────┘    └─────────────────┘
       /                  /roast/[id]              /roast/[id]
```

Same URL pattern for screens 2 and 3 — the only difference is whether the visitor created the card (they see "Roast again" CTA) or arrived from a share link (they see "Roast your own" CTA bouncing back to /).

---

## Screen 1: Landing — `/`

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│                  ▶  BeatM ROAST MODE  ◀                         │
│         Your lineup. Their lineup. One AI roast.                │
│                                                                 │
│  ┌───────────────────────┐      ┌───────────────────────┐       │
│  │  SIDE A   🟠 ORANGE   │      │  SIDE B   🔵 ICE BLUE │       │
│  │  ─────────────────    │  VS  │  ─────────────────    │       │
│  │  LeBron James         │      │  Jayson Tatum         │       │
│  │  Stephen Curry        │      │  Jaylen Brown         │       │
│  │  Draymond Green       │      │  Jrue Holiday         │       │
│  │  Andrew Wiggins       │      │  Kristaps Porzingis   │       │
│  │  Anthony Davis        │      │  Derrick White        │       │
│  │                       │      │                       │       │
│  │  + Add player         │      │  + Add player         │       │
│  └───────────────────────┘      └───────────────────────┘       │
│                                                                 │
│              ┌─────────────────────────────┐                    │
│              │      🔥  R O A S T  🔥      │  (huge button)     │
│              └─────────────────────────────┘                    │
│                                                                 │
│   Quick battles ›                                               │
│   [ Coast War ]  [ Greek vs Slovenian ]  [ Old vs New Wave ]    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Design notes (Aisha):**
- Background: `#0A0A0F` (near-black, arena-at-night)
- Side A accent: `#FF6B2B` (electric orange) — its card glow, its border, its label
- Side B accent: `#00D4FF` (ice blue) — same treatment, opposite color
- Headline font: `Space Grotesk` (geometric, bold)
- Body font: `Inter` (clean, fast)
- ROAST button: orange-to-blue gradient, pulses on hover, lifts on press

---

## Screen 2: Roast Card — `/roast/[id]`

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│  🔥 FIRE                                  CROWD PULSE  87/100   │
│                                           ████████████████░░░   │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │                                                           │  │
│  │       COAST WAR: BAY BREAKS BEANTOWN                      │  │
│  │                                                           │  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                 │
│  ┌───────────────────────────┐   ┌──────────────────────────┐   │
│  │  SIDE A   🟠              │   │  SIDE B   🔵             │   │
│  │  ──────────────────       │   │  ──────────────────      │   │
│  │  LeBron · Curry · Dray    │   │  Tatum · Brown · Holiday │   │
│  │  Wiggins · AD             │   │  Porzingis · White       │   │
│  │                           │   │                          │   │
│  │  "Locking three 35-year-  │   │  "Five Celtics like      │   │
│  │  olds and praying jet     │   │  that's a strategy and   │   │
│  │  lag works for you.       │   │  not a fan moment. KP    │   │
│  │  Curry's still cooking    │   │  is one minute restric-  │   │
│  │  but Draymond's stat      │   │  tion away from a DNP    │   │
│  │  line has been a flat     │   │  and you stacked him     │   │
│  │  circle for three weeks.  │   │  next to White, who      │   │
│  │  This lineup peaked       │   │  scores when no one's    │   │
│  │  in 2022."                │   │  watching. Bold."        │   │
│  │                           │   │                          │   │
│  └───────────────────────────┘   └──────────────────────────┘   │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │ THE CALL ›  Boston's defense smothers Bay's over-30 club. │  │
│  │             Side B by 9.                                  │  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                 │
│   [ 📋  COPY SHARE LINK ]       [ 🔥  ROAST AGAIN ]             │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Verdict tag visual treatment:**
| Verdict | Color | Vibe |
|---|---|---|
| 🔥 FIRE | Orange/red glow | High-scoring shootout |
| ❄️ ICE | Blue/white glow | Defensive grind |
| 💥 CHAOS | Magenta/purple | Wildly mismatched |
| ⚖️ CLASSIC | Gold/cream | Even & tense |

---

## Screen 3: Shared View — `/roast/[id]` (visitor arrived via share link)

Identical to Screen 2 — **but the bottom CTA changes:**

```
   [ 📋  COPY THIS LINK ]   [ 🔥  ROAST YOUR OWN  →  ]
                                  (bounces to /)
```

This is the conversion mechanism. Every shared card is a recruiting funnel.

---

## State Machine

```
                          [idle]
                            │
              user types in either lineup
                            │
                            ▼
                       [composing]
                            │
                  user clicks ROAST
                            │
                            ▼
                       [loading]  ← shimmer animation on card area
                            │
                  Claude responds in ~3s
                            │
        ┌───────────────────┴───────────────────┐
        │                                       │
   success: 200                         failure: 4xx/5xx
        │                                       │
        ▼                                       ▼
   [card_shown]                          [error_shown]
        │                                       │
   ┌────┼────────┐                       retry once
   ▼    ▼        ▼                              │
[copy] [again] [share]                          ▼
        │                                  [fallback_card]
        │                            (hardcoded "demo never dies")
        └─ back to [idle]
```

---

## URL Schema (zero-backend trick)

A roast card lives entirely in its URL:

```
/roast/eyJoZWFkbGluZSI6IkNPQVNUIFdBUjogQkFZIEJSRUFLUyBCRUFOVE9XTiIs...
```

The `[id]` segment is a base64-encoded JSON payload of the `RoastCard` object. The page is statically generated server-side from the URL — **no database, no backend storage, no expiry.**

Trade-offs:
- ✅ Cards are permanent and shareable forever
- ✅ No infra cost, no rate limits
- ✅ Works offline once the page is loaded
- ❌ URLs are long (~500 chars) — fine for sharing, ugly to type
- ❌ Card content is reconstructable by anyone (acceptable — not sensitive)

Phase 2 fix: short-link service that maps `/r/AB12C` → full URL.

---

## Mobile (375px width)

Side A and Side B stack vertically. Everything else compresses. The ROAST button stays huge — it's the only thing that matters on mobile.

```
┌──────────────────────┐
│   BeatM ROAST MODE   │
│                      │
│  ┌────────────────┐  │
│  │ SIDE A 🟠      │  │
│  │ [lineup]       │  │
│  └────────────────┘  │
│                      │
│         VS           │
│                      │
│  ┌────────────────┐  │
│  │ SIDE B 🔵      │  │
│  │ [lineup]       │  │
│  └────────────────┘  │
│                      │
│  ┌────────────────┐  │
│  │  🔥 ROAST 🔥   │  │
│  └────────────────┘  │
└──────────────────────┘
```

---

## Animations (the polish layer, T+1:30)

| Moment | Animation |
|---|---|
| Page load | Headline fades in (200ms) · cards lift from below (300ms staggered) |
| ROAST button hover | Pulse · glow intensifies · cursor changes to "fire" |
| ROAST button click | Press-down · brief flash · button collapses into a loading shimmer |
| Card reveal | Headline types out letter-by-letter (300ms) · roasts fade in side-by-side (400ms) · verdict tag pops in last (200ms with bounce) |
| Crowd pulse meter | Fills from 0 → final value over 800ms |
| Copy share link | Button briefly changes to "✓ Copied" (1.5s) |

Aisha's rule: **no animations until the layout is locked.** Animation on a broken layout is lipstick on a pig.
