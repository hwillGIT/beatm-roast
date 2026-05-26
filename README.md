# BeatM ROAST MODE

> **Your lineup. Their lineup. One AI roast. Maximum chaos.**

An AI-powered social trash talk battle card generator for Daily Fantasy Sports lineups. Built for the **Cursor Boston × Hult Sports Hack 2026** (May 26, 2026).

---

## The Game

**ROAST MODE is a 1-vs-1 trash-talk battle.**

Two friends each pick a Daily Fantasy Sports lineup. They challenge each other through the app. Claude — playing the role of a hyped-up, brutally honest sports commentator — reads both lineups and writes a personalized **roast card**: 2–3 sentences of savage takedown for each side, a verdict tag (`🔥 FIRE` / `❄️ ICE` / `💥 CHAOS` / `⚖️ CLASSIC`), a crowd-pulse score, and a winner pick.

The card has its own shareable URL. You drop it in the group chat. Your friends pile on with emojis. Tomorrow, the loser builds a revenge lineup. The cycle never ends.

**It turns DFS — usually a solo statistical exercise — into a social spectacle.**

### How a round plays out (under 60 seconds)

| Beat | What happens |
|---|---|
| **1. Challenge** | You text a friend: *"Bet your lineup against mine."* Drop the ROAST link in the chat. |
| **2. Submit** | Both of you paste your 5-player lineups into the page. Any sport, any positions, names only. |
| **3. Roast** | Hit the giant 🔥 **ROAST** 🔥 button. Claude reads both lineups in ~3 seconds. |
| **4. React** | The card appears. You read the roast aloud. Someone laughs. Someone slaps the table. *That moment is the product.* |
| **5. Share** | Copy the URL, drop it back in the chat. Tomorrow you come back for revenge. |

### The stakes

No money. No accounts. No leaderboard (yet). The stakes are **pure bragging rights** — the card is screenshot-worthy and the roast is public. You "lose" when your friend's roast is funnier than yours and they shared it before you did.

### Why it's a game, not just a tool

| Game element | How ROAST MODE has it |
|---|---|
| **Players** | Two rivals — friends, coworkers, group-chat enemies |
| **Skill** | Lineup construction — picking players the AI will roast favorably (or hilariously) |
| **Surprise** | The AI's tone shifts night to night · you don't know how hard it'll hit |
| **Resolution** | One verdict tag, one winner, one card |
| **Replay loop** | Lose tonight? Build a revenge lineup tomorrow. |

### Why it wins the hackathon

- ✅ Directly serves BeatM's social-first DFS thesis — DFS should be loud and communal, not isolated and transactional
- ✅ AI-native — without Claude, the joke doesn't exist. The AI **is** the product.
- ✅ Visually shareable — every card is a screenshot-worthy unit of viral content
- ✅ Demo-able in 90 seconds — the whole pitch is a real laugh on camera

---

## What's In This Repo

| Doc | What it is |
|---|---|
| [`docs/SPEC.md`](docs/SPEC.md) | **Start here.** Final product spec: tech stack, data model, API contract, Claude prompt, demo content, submission checklist |
| [`docs/TEAM.md`](docs/TEAM.md) | The 7-agent build team (Zara, Kai, Nia, Marco, Aisha, Sofia, Quinn) + 2.5-hour sprint plan + code-quality gates |
| [`docs/BRAINSTORM.md`](docs/BRAINSTORM.md) | World-class 4-phase brainstorm methodology + 7 scored product concepts + 2 real-world sports voices (NBA vet + DFS pro) |
| [`docs/VIDEO.md`](docs/VIDEO.md) | Sofia Ferrante's 90-second director's guide for the demo film |

---

## Tech Stack

```
Next.js 14 (App Router, TS)  •  Tailwind + shadcn/ui  •  Claude Haiku 4.5
BallDontLie API (NBA)        •  Replit deploy         •  URL-state (no DB)
```

---

## Team

| Agent | Role |
|---|---|
| **Zara Okonkwo** | Tech Lead · Scaffold · Deploy |
| **Kai Yamamura** | Frontend · UI builder · Cursor power user |
| **Dr. Nia Osei** | Claude API · Prompt architecture · Output quality |
| **Marco Salvatore** | Sports APIs · Real-time data · Fallback architecture |
| **Aisha Diallo** | Visual design · Tailwind system · Shareable cards |
| **Sofia Ferrante** | Demo video · Shot list · Edit |
| **Quinn Nakashima** | Code quality · TypeScript discipline · 4-gate review |

---

## The Build Window

| Time (ET) | Phase |
|---|---|
| 10:00 | Scaffold (Zara) · API tests (Marco) · Prompt v1 (Nia) · Shot list (Sofia) |
| 10:15 | Core build — landing, card page, `/api/roast` (Kai + team) |
| 11:15 | Quality sweep (Quinn) — types, lint, docs, runtime |
| 11:30 | Polish (Kai + Aisha) · Final Replit deploy (Zara) |
| 11:45 | Video shoot (Sofia leads) |
| 12:10 | Video edit (Sofia) |
| 12:25 | Submit — PR open, `meta.json` updated |
| **2:30** | **Demo to judges** |
| **4:00** | **Hard deadline** |

---

## Status

- [x] Product concept locked: **ROAST MODE** (Dirty 30 Blackjack → Phase 2)
- [x] Spec written
- [x] Team assembled
- [x] Demo video plan written
- [ ] Scaffold deployed
- [ ] Claude integration wired
- [ ] Card UI shipped
- [ ] Demo video shot + edited
- [ ] Submission PR open

---

*Built with [Cursor](https://cursor.com) × [Claude](https://claude.com) at the Cursor Boston Sports Hack 2026.*
