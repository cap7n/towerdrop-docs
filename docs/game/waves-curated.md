# Curated Waves 1–50

The wave-by-wave design sheet. **Waves 1–10 are implemented** (the `CURATED` array in
`Levels/Scripts/wave_manager.gd` is the code truth). **Waves 11–50 are seeded designs**
(Claude first pass, 2026-07-27) — untested numbers meant to be tuned by feel first, then by
the balance CSVs. The generator currently running waves 11+ is the thing this sheet replaces:
its linear budget is why runs go immortal past wave 10.

## The tuning loop

1. **Seed** — this table (done).
2. **Implement** a block of waves into `CURATED` (10 at a time is a good bite).
3. **Playtest with the balance logger on.** The KPI is the `tower_dmg` column of
   `balance_waves_*.csv` against the **Target dmg** band below (percent of 500 tower HP lost
   at par skill). Duration and `hp_destroyed/duration_s` (player DPS proxy) come free.
4. **Tune** counts/HP/powerups until logged damage lands in band.
5. **Fit the model**: once 1–50 log in-band, regress the tuned pressure curve
   (`hp_spawned`, powerup density, set-piece cadence vs wave number) and refit the
   generator's knobs from it — wave 51+ then scales from measured reality instead of guesses.

!!! note "Curated powerups: SHIPPED 2026-07-27"
    `CURATED` groups take an optional 4th entry, e.g. `[scene, count, hp, {"imm": 0.2, "shield": 0.1}]`
    — per-enemy roll, shield wins the roll, applied at spawn. Unlike the generator (spiders only),
    curated groups may powerup ANY enemy type. Swarm set-pieces sit at **15/25/35/45 on purpose**:
    `_is_swarm_wave`'s every-5-from-15 cadence already flips the fast spawn interval on those
    indices, so the flood rate comes free. (Boss waves 20/30/40/50 also land on that cadence;
    irrelevant for a 1-unit boss, and a fast-streaming escort is fine.)

!!! note "Block 1 (waves 11–20) implemented 2026-07-27"
    In `CURATED`, untuned — the Target bands below are the tuning goal. W20 is a **Queen
    rematch placeholder** (same Queen + escort) until Queen II is designed. Caveat from
    playtesting: **snails currently read as a nuisance, not a threat** (cost/behaviour
    rebalance pending), so the SNAIL WALL waves (18/34/47) are provisional designs.

## Reading the sheet

- **Sp / Pb / Sn** = spiders / pillbugs / snails spawned. **SpHP** = the spider group's HP
  override ("roll" = the size-driven roll; pillbugs and snails always keep their scene stats).
- **Powerups** = share of that wave's spider group (or the named group, for the walls) rolling
  an elemental immunity (`imm`) or a breakable pure-shield (`sh`).
- **Target dmg** = tower HP the wave should cost at par skill. This is the number tuning aims
  for and the CSV measures.
- Escalation is deliberately multi-axis: counts plateau at ~70 (perf pillar: 200–250 alive
  max), so late pressure comes from HP, powerup density, and composition instead of bodies.

| W | Status | Sp | Pb | Sn | SpHP | Powerups | Target dmg | Intent / set piece |
|---|--------|----|----|----|------|----------|------------|--------------------|
| 1 | <span class="pill done">Done</span> | 6 | 0 | 0 | roll | | 0–2% | Gentle intro, spiders only |
| 2 | <span class="pill done">Done</span> | 8 | 0 | 0 | roll | | 0–3% | Still easing in |
| 3 | <span class="pill done">Done</span> | 8 | 0 | 0 | 3 | | 2–5% | THE TEACHING WAVE: 3 hits bare, oneshot at Bigger Rock 2 |
| 4 | <span class="pill done">Done</span> | 18 | 3 | 0 | roll | | 5–12% | Pillbugs introduced (crack them) |
| 5 | <span class="pill done">Done</span> | 22 | 4 | 0 | roll | | 5–12% | Climb |
| 6 | <span class="pill done">Done</span> | 26 | 5 | 2 | roll | | 5–15% | Snails introduced (shelled elites) |
| 7 | <span class="pill done">Done</span> | 30 | 6 | 2 | roll | | 5–15% | Climb |
| 8 | <span class="pill done">Done</span> | 34 | 7 | 3 | roll | | 8–15% | Climb |
| 9 | <span class="pill done">Done</span> | 38 | 8 | 3 | roll | | 8–15% | Last normal wave before the boss |
| 10 | <span class="pill done">Done</span> | — | — | — | — | | 20–35% | **BOSS: Spider Queen.** She is the wave (brood + eggs) |
| 11 | <span class="pill done">Done</span> | 40 | 8 | 4 | roll | | 10–18% | Back to normal waves, no breather in the slope |
| 12 | <span class="pill done">Done</span> | 44 | 9 | 4 | 2 | imm 10% | 10–18% | Immunities become a fixture, not a surprise |
| 13 | <span class="pill done">Done</span> | 46 | 10 | 5 | 2 | imm 10% | 10–18% | Climb |
| 14 | <span class="pill done">Done</span> | 50 | 10 | 5 | 3 | imm 10% | 12–20% | Climb |
| 15 | <span class="pill done">Done</span> | 90 | 6 | 0 | 1 | | 15–25% | **SWARM I**: volume exam, ultimates earn their slot |
| 16 | <span class="pill done">Done</span> | 48 | 12 | 6 | 3 | imm 10%, sh 10% | 12–20% | Shields become a fixture |
| 17 | <span class="pill done">Done</span> | 52 | 12 | 6 | 4 | imm 15%, sh 10% | 15–22% | Climb |
| 18 | <span class="pill done">Done</span> | 20 | 0 | 20 | 3 | sh 20% | 15–25% | **SNAIL WALL**: armor-cracking exam |
| 19 | <span class="pill done">Done</span> | 56 | 14 | 6 | 4 | imm 20%, sh 10% | 15–25% | Last push before the rematch |
| 20 | <span class="pill done">Done</span> | 24 | 0 | 0 | 4 | | 20–35% | **BOSS: Queen II** (design open: HP x2, faster eggs, reuse SummonBrood/LayEggs) |
| 21 | <span class="pill todo">Todo</span> | 55 | 14 | 7 | 5 | imm 20%, sh 15% | 15–25% | Climb |
| 22 | <span class="pill todo">Todo</span> | 30 | 24 | 0 | 4 | imm 20% | 18–28% | **PILLBUG STAMPEDE**: charge-bonk chaos |
| 23 | <span class="pill todo">Todo</span> | 58 | 15 | 7 | 6 | imm 25%, sh 15% | 18–28% | Climb |
| 24 | <span class="pill todo">Todo</span> | 60 | 15 | 8 | 6 | imm 25%, sh 20% | 18–28% | Climb |
| 25 | <span class="pill todo">Todo</span> | 110 | 8 | 0 | 2 | | 20–30% | **SWARM II** |
| 26 | <span class="pill todo">Todo</span> | 60 | 16 | 8 | 7 | imm 30%, sh 20% | 20–30% | Climb |
| 27 | <span class="pill todo">Todo</span> | 62 | 16 | 8 | 8 | imm 30%, sh 20% | 20–30% | Climb |
| 28 | <span class="pill todo">Todo</span> | 40 | 10 | 14 | 8 | sh 30% | 20–30% | Armor-heavy mix |
| 29 | <span class="pill todo">Todo</span> | 65 | 17 | 9 | 9 | imm 35%, sh 25% | 22–32% | Last push before boss 3 |
| 30 | <span class="pill todo">Todo</span> | 30 | 0 | 0 | 6 | | 25–40% | **BOSS 3** (design fully open) |
| 31 | <span class="pill todo">Todo</span> | 65 | 17 | 9 | 10 | imm 35%, sh 25% | 20–30% | Climb |
| 32 | <span class="pill todo">Todo</span> | 34 | 28 | 0 | 8 | imm 25% | 22–32% | **STAMPEDE II** |
| 33 | <span class="pill todo">Todo</span> | 68 | 18 | 9 | 11 | imm 40%, sh 30% | 22–32% | Climb |
| 34 | <span class="pill todo">Todo</span> | 24 | 0 | 24 | 10 | sh 40% | 22–32% | **SNAIL WALL II** |
| 35 | <span class="pill todo">Todo</span> | 130 | 10 | 0 | 3 | | 25–35% | **SWARM III** |
| 36 | <span class="pill todo">Todo</span> | 68 | 18 | 10 | 12 | imm 40%, sh 30% | 22–32% | Climb |
| 37 | <span class="pill todo">Todo</span> | 70 | 19 | 10 | 12 | imm 45%, sh 30% | 25–35% | Count ceiling reached; HP and powerups carry it now |
| 38 | <span class="pill todo">Todo</span> | 70 | 19 | 10 | 13 | imm 45%, sh 35% | 25–35% | Climb |
| 39 | <span class="pill todo">Todo</span> | 70 | 20 | 10 | 13 | imm 50%, sh 35% | 25–35% | Last push before boss 4 |
| 40 | <span class="pill todo">Todo</span> | 35 | 0 | 0 | 8 | | 30–45% | **BOSS 4** (design fully open) |
| 41 | <span class="pill todo">Todo</span> | 70 | 20 | 11 | 14 | imm 50%, sh 35% | 25–38% | Climb |
| 42 | <span class="pill todo">Todo</span> | 36 | 30 | 0 | 10 | imm 30% | 28–38% | **STAMPEDE III** |
| 43 | <span class="pill todo">Todo</span> | 70 | 20 | 11 | 14 | imm 50%, sh 40% | 28–38% | Climb |
| 44 | <span class="pill todo">Todo</span> | 70 | 21 | 12 | 15 | imm 50%, sh 40% | 28–38% | Spider HP at the cap; density is the only lever left |
| 45 | <span class="pill todo">Todo</span> | 150 | 12 | 0 | 4 | | 30–40% | **SWARM IV**: the perf-budget stress test |
| 46 | <span class="pill todo">Todo</span> | 70 | 21 | 12 | 15 | imm 55%, sh 40% | 28–40% | Climb |
| 47 | <span class="pill todo">Todo</span> | 26 | 0 | 26 | 15 | sh 50% | 28–40% | **SNAIL WALL III** |
| 48 | <span class="pill todo">Todo</span> | 70 | 22 | 12 | 15 | imm 55%, sh 45% | 30–42% | Climb |
| 49 | <span class="pill todo">Todo</span> | 70 | 22 | 13 | 15 | imm 60%, sh 45% | 30–42% | Everything at ceiling, the last normal wave |
| 50 | <span class="pill todo">Todo</span> | 40 | 10 | 10 | 10 | imm 40%, sh 30% | 35–50% | **FINALE BOSS** (design fully open) + full-spectrum escort |

## Design notes on the seeds

- **Why the run goes immortal today**: the generator adds a *linear* +10 budget per wave while
  player power compounds (tree levels, an artifact after every wave, spikes/regen, ultimates,
  and gold income that scales with kills). Spider HP caps at 15 and bodies cap at 70, so past
  ~wave 12 the generator has nothing left to spend on. The seeds above escalate on the axes
  the generator underuses: **HP overrides toward the cap, powerup density, and composition
  set-pieces.**
- Set-piece cadence: swarm at 15/25/35/45, elite exam (wall or stampede) roughly every 4–6
  waves, boss every 10. Normal "climb" waves in between are the tunable filler.
- When new enemies ship (termite / bombardier / centipede, see backlog), they slot into climb
  waves as substitutions rather than reshaping the sheet.
- Waves 11–50 replacing the generator does not delete it: it stays as the 51+ endless tail,
  refit from this sheet's tuned data (step 5 above).
