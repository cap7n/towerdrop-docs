# ⚡ The Revamp (temporary tab)

**Tutorial slows down. Main game speeds up and hits players with meaningful choices, fast.**

Born from dev-to-dev playtest feedback (2026-07-19): players spend their attention decoding systems instead of fighting; choices don't land with the impact the audience expects; nobody realizes they can upgrade. The reference point we keep coming back to: **Vampire Survivors** — one character, and within the first 20 seconds you've killed things and been handed a 3-way build choice. In-your-face. That's the energy the main game needs. The gentle easing-in moves to a dedicated Tutorial.

This is a **two steps back, five steps forward** operation, so it gets its own tab while it's live. When both tracks are done, fold the results into the [Backlog](backlog.md) / [Decision Log](decisions.md) and delete this page.

---

## Track 1 — The Tutorial build (slow, its own thing)

The guided intro we already built becomes a dedicated MODE, and the main game gets stripped clean.

- [ ] **1. Duplicate the level**: copy `level_1.tscn` → `tutorial_level.tscn`, near enough 1:1 — we already have most tutorial bits (TutorialDirector beats, rock-first gate, shop onboarding) and they simply move house.
- [ ] **2. Start-menu "Tutorial" button** → launches the tutorial level with the guided intro FORCED on (a `Globals.tutorial_run` flag the TutorialDirector reads).
- [ ] **3. Strip the main game**: `FORCE_EACH_RUN = false`; TutorialDirector goes fully dormant outside tutorial runs. Decide per beat: does it die in the main game or move to the tutorial? (The rock-first element gate is the big one — in the main game it likely DIES in favour of the opening element pick below.)
- [ ] **4. Slow the tutorial down**: longer grace, gentler waves, one system per wave — drop/pour → shop + first upgrade → element trees → repair → artifact placement → spellbook. No game-over pressure (tower can't die, or respawns free).
- [ ] **5. Completion flag**: `tutorial done` saved to disk; first launch nudges new players toward the Tutorial button (but never forces it).

## Track 2 — The main game (all out, asap)

Attention spans are short. No easing in. The first minute must jingle the car keys.

- [ ] **1. Kill the slow open**: shorten the grace period; wave 1 spawns within seconds. Small wave, instant action.
- [ ] **2. The opening choice (the VS moment)**: after the first handful of kills (~20-30 seconds in), the game PRESENTS a **3-card element choice** — pick your starting wizard's element, free, right now. Replaces the rock-first gate in the main game. This is the build-defining "here's who you are this run" moment.
- [ ] **3. End-of-wave-1 relic choice**: upgrade the fixed Coin Maker grant into a **pick 1 of 3 artifacts** card moment. Same pattern, second hit of build identity. (The grant hook + bag already exist; this is a 3-card UI on top.)
- [ ] **4. Telegraph upgrades loudly**: flashing/glowing shop entry points when gold sits unspent, arrows/highlights on first-time surfaces, a rest-phase nudge line ("Spend your gold: TAB for tower, W for wizards"). Flash until first use, subtle after.
- [ ] **5. Make early choices HIT**: first upgrade tiers cheap and dramatic (big visible deltas, not +1 increments) so every pick visibly changes the run. Re-tune waves 1–5 for pressure — they were paced around the old tutorial drip.
- [ ] **6. Re-check the system reveal order**: with the tutorial gone, decide what the first 3 waves EXPOSE (spellbook already hides unearned spells; artifacts arrive end of wave 1 as the choice moment; TAB tree = telegraphed by the gold nudge). Rule of thumb: a player who ignores a system should still feel good for three waves.

## Open decisions along the way

- Does the opening 3-card choice offer 3 random elements of the 6, or a fixed starter trio (Rock always included)?
- Does the relic choice repeat (every N waves = a recurring VS-style draft?) or stay a wave-1 moment? A recurring draft could become THE meta-loop.
- What happens to the rock-first gate philosophy (cheap reliable baseline) — folded into the card copy ("Rock: the reliable one")?
- Wave 1 tuning: how many enemies before the element choice interrupts?

## Status log

- **2026-07-19** — page created; feedback recorded in the [Backlog](backlog.md) (fellow-dev section). Nothing built yet.
