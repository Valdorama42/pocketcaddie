# PocketCaddie ⛳️

PocketCaddie is an early-stage golf web app that helps golfers make better **club selection decisions** on the course.

The idea is simple:
instead of guessing between clubs, PocketCaddie combines your **own carry distances** with **shot conditions** (wind, lie, elevation, feel) and suggests a club that fits the situation.

This project is currently in **active development** and is being built publicly.

---

## 🎯 Motivation

Most missed greens are not caused by bad swings, but by **poor decisions before the shot**.

On the course, golfers rarely have access to:
- their true carry distances
- a calm, consistent way to account for wind, lie, or elevation
- objective decision support under pressure

PocketCaddie aims to act like a **personal caddie**:
not over-analytical, not sensor-based, just focused on helping you choose the right club in the moment.

---

## 🧠 Core Concept

PocketCaddie is built around a simple loop:

1. **Bag setup**
The user inputs their club carry distances once.

2. **Shot input**
Before each shot, the user answers a few quick questions about conditions.

3. **Club recommendation**
The app calculates a “plays-like distance” and suggests a club.

4. **Shot feedback**
The user logs where the shot ended up (green, short, long, left, right).

5. **Insights**
Over time, the app shows simple statistics like most common misses and GIR%.

The focus is on **decision-making**, not swing mechanics.

---

## 🚧 Project Status

This repository currently contains:

- A **public landing page** (email waitlist)
- Early UI drafts for:
- shot input
- analytics
- Initial planning for:
- recommendation logic
- data models
- local persistence

The MVP goal is to implement the full decision loop locally (no backend initially).

---

## 🛠 Tech Stack (current & planned)

- **Frontend:** SvelteKit
- **Styling:** Tailwind CSS
- **Language:** TypeScript
- **State & persistence:** localStorage (initial MVP)
- **Deployment:** Vercel

The project intentionally avoids heavy dependencies at this stage to keep iteration fast.

---

## 📈 Roadmap (high-level)

- [x] Landing page + waitlist
- [ ] Bag setup & persistence
- [ ] Shot condition input
- [ ] Club recommendation engine
- [ ] One-tap shot feedback
- [ ] Basic analytics (GIR %, common misses)
- [ ] Small closed beta

Features like AI-based learning, GPS/maps, or weather APIs are **intentionally postponed** until the core loop is validated.

---

## 📬 Feedback

If you have thoughts, ideas, or just want to follow the project:
- join the waitlist via the landing page (live soon)
- or open an issue with feedback

---

⛳️
