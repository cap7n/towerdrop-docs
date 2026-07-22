# ⚡ The Revamp (temporary tab)

**Tutorial slows down. Main game speeds up and hits players with meaningful choices, fast.**

Born from dev-to-dev playtest feedback (2026-07-19): players spend their attention decoding systems instead of fighting; choices don't land with the impact the audience expects; nobody realizes they can upgrade. The reference point we keep coming back to: **Vampire Survivors** — one character, and within the first 20 seconds you've killed things and been handed a 3-way build choice. In-your-face. That's the energy the main game needs. The gentle easing-in moves to a dedicated Tutorial.

This is a **two steps back, five steps forward** operation, so it gets its own tab while it's live. When both tracks are done, fold the results into the [Backlog](backlog.md) / [Decision Log](decisions.md) and delete this page.

**Status tags:** <span class="pill done">Done</span> shipped · <span class="pill wip">WIP</span> in progress · <span class="pill todo">Todo</span> not started · <span class="pill idea">Idea</span> undecided · <span class="pill check">Check</span> may already be done.

---

## Track 1 — The Tutorial build (slow, its own thing)

The guided intro we already built becomes a dedicated MODE, and the main game gets stripped clean.

- <span class="pill done">Done</span> **1. Duplicate the level**: copy `level_1.tscn` → `tutorial_level.tscn`, near enough 1:1 — we already have most tutorial bits (TutorialDirector beats, rock-first gate, shop onboarding) and they simply move house.
- <span class="pill done">Done</span> **2. Start-menu "Tutorial" button** → launches the tutorial level with the guided intro FORCED on (a `Globals.tutorial_run` flag the TutorialDirector reads).
- <span class="pill done">Done</span> **3. Strip the main game**: `FORCE_EACH_RUN = false`; TutorialDirector goes fully dormant outside tutorial runs. Decide per beat: does it die in the main game or move to the tutorial? (The rock-first element gate is the big one — in the main game it likely DIES in favour of the opening element pick below.)
- <span class="pill todo">Todo</span> **4. Slow the tutorial down** — the full curriculum below, one lesson block per wave, target **10–15 minutes total**. No game-over pressure (tower can't die, or respawns free).
- <span class="pill todo">Todo</span> **5. Completion flag**: `tutorial done` saved to disk; first launch nudges new players toward the Tutorial button (but never forces it).

### The tutorial curriculum (target 10–15 min)

Everything a player must be SHOWN, grouped into wave-sized lessons. Ticks = wired into the tutorial level.

**Block 0 — Grace period (~1 min): you and the cart**

- <span class="pill todo">Todo</span> 1. **Movement**: ride the rail with A/D.
- <span class="pill todo">Todo</span> 2. **Camera tilt**: hold S to look down the wall (the beat exists in the director already — keep it here).

**Block 1 — Wave 1 (~2 min): fight**

- <span class="pill todo">Todo</span> 3. **Dropping materials**: tap Space = one item, hold = pour the bin.
- <span class="pill todo">Todo</span> 4. **Inventory selection**: scroll wheel picks which item drops next (the red-rimmed slot).
- <span class="pill todo">Todo</span> 5. **Enemy telegraph**: the flare + the windrose point at where the wave comes from — get your cart to that side.
- <span class="pill todo">Todo</span> 6. **Dealing with enemies**: rocks on heads; climbers get knocked off the wall (fall damage is your friend).
- <span class="pill todo">Todo</span> 7. **Gold**: kills drop coins that arc to the pile on the tower top — the pile IS your bank.

**Block 2 — Rest 1 (~2 min): spending**

- <span class="pill todo">Todo</span> 8. **The wave rhythm**: rest = shopping time; Enter calls the next wave early; the Auto next checkbox exists.
- <span class="pill todo">Todo</span> 9. **TAB shop**: tower & cart upgrades (buy one, feel it), and the Wizards button bridges to the wizard shops.
- <span class="pill todo">Todo</span> 10. **Wizard shop**: W or click a wizard; pick your element (rock-first gate teaches the flow here); buy Bigger Rock; the profile cards + Stats page.

**Block 3 — Wave 2 + Rest 2 (~3 min): defense**

- <span class="pill todo">Todo</span> 11. **Brick armour**: let a few spiders dig in — bricks tint, crack and tumble; the wall soaks damage before your HP bar.
- <span class="pill todo">Todo</span> 12. **Repair tool**: R or the button, click the damaged bricks, 1 gold per HP.
- <span class="pill todo">Todo</span> 13. **Aimed throw**: right-click lobs the selected item at the ground, left-click at a hovered enemy (60° arc).

**Block 4 — Wave 3 + Rest 3 (~3 min): the toys**

- <span class="pill todo">Todo</span> 14. **Artifacts**: grant one mid-wave — read the tooltip in the ✦ bag, place it on the tower, move it with the mover, double-click back to the bag. Mention the brick-break risk (wall = close to the action, top = safe).
- <span class="pill todo">Todo</span> 15. **Second wizard**: hire from the TAB shop once gold allows; different wizards can take different elements (respec refunds 75%).

**Block 5 — Wave 4 (~2 min): graduation**

- <span class="pill todo">Todo</span> 16. **Ultimates**: buy the ultimate node in a tree, watch it get scribed into the Spellbook, cast it.
- <span class="pill todo">Todo</span> 17. **Enemy diversity**: an immune/resistant enemy shows up telegraphed — resistance icons, why one damage type stops working, answer = switch what you throw.
- <span class="pill todo">Todo</span> 18. **Combo teaser**: one scripted moment of Frost then Rock (the brittle oneshot) — "elements combine; go find the rest."
- <span class="pill todo">Todo</span> 19. **Wrap-up card**: "waves keep coming and grow endless, rest is for spending, good luck" → continue playing or return to menu.

**Deliberately NOT taught** (discovery is the fun): deep combos (oil+fire, lure stacking), enemy specials (pillbug charge, snail shells), trap-family artifacts, the balance-note key.

## Track 2 — The main game (all out, asap)

Attention spans are short. No easing in. The first minute must jingle the car keys.

- <span class="pill done">Done</span> **1. Kill the slow open**: shorten the grace period; wave 1 spawns within seconds. Small wave, instant action. *(The real dead air turned out to be the 110–150 m approach march, ~80 s of walking — fixed with the **far-sprint**: enemies charge at 3× walk speed while beyond 40 m from the tower, easing to normal by 15 m. Arrival ≈ 30 s, combat at the wall unchanged, no spawn pop-in. Grace was already short at 3 s.)*
- <span class="pill done">Done</span> **2. The opening choice (the VS moment)**: after the first handful of kills (~20-30 seconds in), the game PRESENTS a **3-card choice**. DECIDED 2026-07-22: the cards are **3 artifacts, not elements** — all six elements stay open in the main game, and your first build-defining pick is an artifact. This is the "here's who you are this run" moment. (Realized via the wave-1 artifact draft in step 3.)
- <span class="pill done">Done</span> **3. The artifact DRAFT**: ~~end-of-wave-1 only~~ — upgraded to **after EVERY wave** (decided 2026-07-21: the recurring draft IS the meta-loop, leaning into artifacts as the game's build identity). Three cards slam centre-screen (`CardPick`, reusable BOOM component: slow-mo 5%, dark vignette, punch-in cards, hover grow, pick pops + losers fade), player picks 1 into the bag. Wave 1 always offers the Coin Maker; duplicates welcome (stacking + hoard-of-many philosophy). Replaced the fixed Coin Maker grant.
- <span class="pill todo">Todo</span> **4. Telegraph upgrades loudly**: flashing/glowing shop entry points when gold sits unspent, arrows/highlights on first-time surfaces, a rest-phase nudge line ("Spend your gold: TAB for tower, W for wizards"). Flash until first use, subtle after.
- <span class="pill wip">WIP</span> **5. Make early choices HIT**: first upgrade tiers cheap and dramatic (big visible deltas, not +1 increments) so every pick visibly changes the run. Re-tune waves 1–5 for pressure — they were paced around the old tutorial drip.
- <span class="pill todo">Todo</span> **6. Re-check the system reveal order**: with the tutorial gone, decide what the first 3 waves EXPOSE (spellbook already hides unearned spells; artifacts arrive end of wave 1 as the choice moment; TAB tree = telegraphed by the gold nudge). Rule of thumb: a player who ignores a system should still feel good for three waves.

## Open decisions along the way

- ~~Does the opening 3-card choice offer 3 random elements of the 6, or a fixed starter trio (Rock always included)?~~ **DECIDED 2026-07-22: neither — the opening choice offers 3 ARTIFACTS, not elements. All six elements stay open in the main game.**
- ~~Does the relic choice repeat or stay a wave-1 moment?~~ **DECIDED 2026-07-21: it repeats after EVERY wave — the draft is the meta-loop.**
- **Do artifact UPGRADES ever join the draft cards** (e.g. "Coin Maker II" as a card)? Current lean: **no** — the game is about hoarding, many artifacts beats few-but-powerful (Cap7n 2026-07-21). Parked here in case the late-game draft needs more depth.
- What happens to the rock-first gate philosophy (cheap reliable baseline) — folded into the card copy ("Rock: the reliable one")?
- Wave 1 tuning: how many enemies before the element choice interrupts?

## Status log

- **2026-07-19** — page created; feedback recorded in the [Backlog](backlog.md) (fellow-dev section).
- **2026-07-20 (1 AM)** — Track 1 steps 1+2 DONE: tutorial_level.tscn duplicated (1:1, own uid pending editor save) + a Tutorial button on the start menu under Play (code-built, sets Globals.tutorial_run and loads the tutorial level). The TutorialDirector still runs in BOTH modes until step 3 strips the main game.
- **2026-07-20 (1:30 AM)** — Track 1 step 3 DONE: the guided intro runs ONLY in tutorial runs (Globals.tutorial_run gates _maybe_begin); the ROCK-FIRST ELEMENT GATE is tutorial-only too — the main game opens all six elements immediately (the interim all-out state until Track 2's 3-card opening pick). Telegraphs (flare, windrose, conjure circles, W-reach hint, repair labels) untouched — they are gameplay, not tutorial.
- **2026-07-21 (later still)** — **THE DRAFT landed** (Track 2 step 3, upgraded in scope): built the reusable **CardPick** BOOM component (autoload, centre-screen 3-card chooser, slow-mo + punch-in + pick animation, no skip button — choosing IS the game) and wired the **after-every-wave artifact draft** through it (ArtifactInventory on `wave_completed`; wave 1 always offers the Coin Maker; fixed grant removed). The opening ELEMENT pick (step 2) can now reuse CardPick as-is. Headless-verified end to end (present, pick, callback, time-scale restore, wave-1 offer contents, grant-on-pick).
- **2026-07-21 (later)** — **THE CANNON landed** (the floated right-click idea from the same chat, built same day): right-click is now **hold = arm / release = fire**. Arming magically pulls the item out of the cart (higher + forward, hovers in the bubble, leans toward your aim; `arm_time_mult` = the future reload-speed upgrade hook); release fires a **flat cannon shot** (`throw_shot_speed` 30 m/s, the old lob-up deleted, small residual arc kept on purpose). The old 60° arc gate is **deleted** — replaced by a **magical trajectory line** (only while arming) that traces the real flight path, purple = clear, cut + red where it would strike the tower, and it's **advisory only**: the player reads it and risks the shot (item hitboxes vary; a bad shot bonks the wall, honest physics). Cancel = release over tower/sky, item returns to the bin. "VFX for aim line" backlogged. Per-element impact identities (drop = damage, shot = impact; rock pushback first) = the agreed next step for "make choices HIT".
- **2026-07-21** — Track 1 steps 4+5 (tutorial refinement) **PARKED** until the main game settles — more systems are coming, and the tutorial level gets re-copied from level_1 afterwards anyway. Track 2 **pacing pass landed**: the far-sprint (step 1 above); **gold now scales with enemy HP** (1 HP = 1 g, +2 g per HP up to 4 HP, +3 g per HP from 5: 1/3/5/7/10/13… a 15-HP capped spider pays 40 g — big kills rain physical coins, wave 1 stays 1 g fodder); **Enter = 0.5 s blink telegraph** (asked-for waves start near-instantly; normal/auto waves keep the full 4 s flare announce); **Auto next defaults ON** (saved cfg choice still wins); **Coin Maker halved** to a coin per 2.5 s. Decisions from the same chat: prices stay where they are (income is the lever, hoarding gold is part of the game); waves 1–3 keep their drip until a post-buff tuning pass; the 3-card choice uses the proven card pattern, no reinvention. Floated, not yet decided: right-click becomes an element CANNON shot (aimed = impact identity per element, e.g. rock pushback) replacing the plain throw.
