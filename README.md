# Atlas — Autonomous Training OS

A single-file, offline, agentic gym app. Open `index.html` in any modern browser — no backend, build step, API keys, or install. All data lives in the browser's `localStorage` and can be exported/imported as JSON.

## What it is

An "autonomous training operating system" that plans the week, conducts each session, adapts from real performance and recovery, and escalates to a human when its confidence or authority is insufficient. It follows the brief's architecture principle: **the language model is not responsible for every decision** — a deterministic training engine owns the safe limits; agents act under a policy layer; every decision is written to an audit trail with a confidence band.

## Features (member-facing)

| Feature | What it does |
|---|---|
| **Mission Mode** *(flagship)* | Describe your situation in free text ("38 min, gym's crowded, slept badly, don't want to aggravate my shoulder") → a single session that honors time, crowding, fatigue, injuries and your programme, with a confidence score and what it deferred. |
| **Programme Director** | Turns a goal + days/week + equipment into a multi-week plan (full-body / upper-lower / PPL), with double-progression and scheduled deloads. |
| **Live Conductor** | Runs the session hands-free: rest-timer ring, per-set logging, RPE-based auto-regulation (adjusts the next set within safe limits), voice logging ("80 for 8"). Auto vs Suggest per your policy. |
| **Gym Navigator** | Queue-aware substitution — swaps an occupied/unavailable station for an equivalent that preserves the muscle stimulus, never duplicating a movement and never violating an injury restriction. |
| **Recovery Governor** | Daily check-in → readiness estimate with an explicit **confidence band** (not a verdict) → maintain / reduce / redistribute. |
| **Digital Twin** | Simulate 3 vs 4 vs 5 days, a 2-week holiday, a calorie deficit, or a strength bias — clearly labelled as directional estimates. |
| **Habit-Rescue** | Predicts an at-risk session and offers a proportional intervention (20-min express / defer / mobility + walk) instead of a nag. |
| **Human Escalation** | Builds a structured handoff for a trainer/physio. Never auto-sends. |
| **Agents & Policy** | Per-agent autonomy: **auto / suggest / off**. Full audit trail with inputs, decision, reason, confidence. |

## Architecture (in `index.html`)

1. **Domain data** — exercises tagged by movement pattern, muscles, equipment, difficulty, and contraindicated joints (`contra`).
2. **Training engine** — `availableEx` / `substitutes` / `pickForSlot` / `prescribe` / `generateProgram` / `generateMission` / `computeReadiness` / `applyWeek`. Pure functions, no LLM.
3. **Agent + policy + audit layer** — `agentAct` gates every side-effecting decision on the agent's autonomy mode; `logAgent` records it. This is the trust surface.
4. **Views** — dashboard, mission, programme, progress, recovery, twin, escalation, agents, settings. Server-component-style: mostly stateless render functions.
5. **Live conductor** — overlay with timers, set entry, auto-regulation, swap, pause/resume.

Intent parsing in Mission Mode is deterministic and offline; `//LLM:` comments mark where a language model would widen the phrasings understood without taking authority over what's *safe*.

## Deliberately out of scope (offline build)

Computer-vision technique coaching, real wearable/HealthKit/Health-Connect sync, live gym-floor occupancy, and connected-machine control require hardware/cloud and are noted in the UI where they'd slot in. The engine and data model are shaped to accept them.

## Packaging as a Windows app

Like the trade-journal app, this can be wrapped as a Tauri v2 desktop `.exe`: point the Tauri `distDir` at this folder (or copy `index.html` into `src`), keep `localStorage` for persistence, and build. No other changes needed — it's already a self-contained static page.

## Safety & privacy

Health/recovery data is sensitive. Atlas keeps everything on-device by default (localStorage), offers export/erase, and stays clearly within fitness/wellness — it does not diagnose, predict conditions, or treat.
