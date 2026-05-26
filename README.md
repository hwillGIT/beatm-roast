# BeatM ROAST MODE

> **Your lineup. Their lineup. One AI roast. Maximum chaos.**

An AI-powered social trash talk battle card generator for Daily Fantasy Sports lineups. Built for the **Cursor Boston × Hult Sports Hack 2026** (May 26, 2026).

---

## The 30-Second Pitch

Two users paste their DFS lineups. Hit **ROAST**. Claude returns a shareable battle card with personalized trash talk for both sides, a winner prediction, a crowd-pulse score, and a verdict tag (`FIRE` | `ICE` | `CHAOS` | `CLASSIC`). Card lives at a shareable URL. Drop the link in a group chat — friends react with emojis. That's the loop.

**Why it wins:**
- Directly serves BeatM's social-first DFS thesis
- AI-native — Claude does the funny part
- Visually shareable — cards are screenshot-worthy
- Demo-able in 90 seconds — relies on a real laugh on camera

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
