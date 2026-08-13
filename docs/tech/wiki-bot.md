# The Task Bot (local LLM)

Design record for the small local-LLM helper that sits beside [Ingenui Tasks](https://github.com/) and this wiki. Designed 2026-08-10 (overnight session, Cap7n + Claude). **Nothing is built yet.** This page is the spec and, more importantly, the reasoning, so the next person to touch it does not relearn the traps.

!!! info "What it runs on"
    Ollama is installed on the main PC with models on `D:\ollama-models`: `qwen3-coder:30b` (18GB, MoE, the heavy one) and `qwen3:8b` (5.2GB, the small one). The 8B is the size the ingenui box's RTX 2070 can host, so the bot is designed against **8B capability**, not 30B. See the local-weights memory for hardware limits: 32k context is the practical ceiling on a 24GB card, less on the 2070.

---

## What it is for

Three jobs, ranked by actual value. This ranking is deliberate: the job we designed first turned out to be the weakest.

1. **Intake: messy notes into task cards.** <span class="pill todo">Todo</span> The real bottleneck. A playtest generates a dozen scrappy notes from different people in varying English; converting them into well-formed cards is what kept this wiki's backlog off the board for months. High volume, genuinely needs comprehension, and **errors are cheap** because a card is read before anyone works on it.
2. **Duplicate and contradiction flagging.** <span class="pill todo">Todo</span> The one whose value *grows*. 96+ cards, two teams feeding one board, more wiki imports coming. A missed duplicate costs real work (two people doing one job).
3. **The logger: done cards into wiki entries.** <span class="pill todo">Todo</span> Runs nightly at 23:00, half an hour before the box suspends. Weakest business case (it serves one user who already does the job well by hand) but it has a genuine second purpose: teammates who write two-word summaries produce nothing this wiki can use, and the Done prompt below is the fix for that.

---

## The one hard rule

**A model invents whenever the output you ask for is more specific than the input you gave it.**

That sentence is the whole design. Everything below follows from it.

### The evidence

Tested 2026-08-10 with `qwen3:8b`, given the brick-repair task card and the house style rules. Format compliance was flawless: correct pill tag, past tense, one paragraph, no bullets, no emojis, and it obeyed the no-dashes rule without a slip. It kept every technical specific correctly.

Then it wrote this:

> The hold-to-repair mechanic with a repair_interval of ~0.3s **was implemented** as a follow-up to the per-click base build, prioritizing incremental progress.

The card says hold-to-repair *"is the follow-up, build per-click base first"*. **It does not exist.** The model converted a planned feature into shipped history, in fluent confident prose, in the middle of an otherwise accurate paragraph, and invented a rationale ("prioritizing incremental progress") that nobody wrote.

Read that in six months and you would go looking for code that was never written.

### Why this matters more here than elsewhere

This wiki is the channel that briefs Claude at the start of every session. That creates an asymmetry which drives every decision on this page:

- An entry that says **too little** costs almost nothing. Someone asks, or reads the code.
- An entry that says something **false** is acted on confidently by both a human and an assistant.

**So: optimise for a boring wiki, not a complete one.** Design every prompt so the failure mode is terseness, never invention. "If you do not know, leave it out" is a rule an 8B model can follow. "Be accurate" is not.

---

## Architecture

### The split

Give the model the work where being *approximately* right is fine and a bad result is obvious on sight. Give code the work where being *exactly* right matters and a mistake fails silently.

| Deterministic code | The model |
|---|---|
| Which tasks are done, which are already processed | Turning free-form human text into wiki register |
| Which file and section, where to insert | Understanding that "chitin instead of dust" overrides the card |
| Dates, task IDs, git commit | Compressing a rambling note into one line |
| The output template itself | Nothing else |

The model writes no connective tissue. Connective tissue is exactly where `was implemented` came from.

### Resolution cap

The output must never be more specific than the input. Concretely: **the bot does not name scripts, functions, particle systems or variables** unless those words were handed to it. Script names live in the code, and the code is always right about itself. This wiki is a design record, not an API reference.

Good: `Added VFX on bonk (chitin particles on tower impact)`
Bad: anything naming the scene, the emitter or the script.

Giving the bot repo access to "look up the real names" was considered and **rejected**: it is a lot of machinery to buy a detail this wiki does not want, small models are markedly weaker at multi-step tool use, and it *inverts* the risk rather than removing it (a real filename attributed to the wrong thing is more convincing and therefore worse).

### Layers, weakest link last

1. Extraction and rephrasing, never composition.
2. A template assembles the final text.
3. **Token check:** every technical token in the output (names, numbers, filenames) must appear in the input. Anything invented fails automatically and is held for review. Will not catch a wrong tense; does catch fabricated specifics.
4. **Git diff for a human.** The bot commits to a branch or staging file. It never writes to main. The review is thirty seconds and it is the layer that caught `was implemented`.

---

## The Done prompt

The card is the **plan**. The wiki should record **what happened**. Those differ more often than not, so the Done button asks one question at the moment the person still remembers the answer.

The question must be **specific and generated from the card**, not a blank box:

> The card said "something with dust." What did you end up using?

A blank "describe what you did" gets answered with one word. A pointed question gets answered with four useful ones.

**If it is skipped, the entry gets shorter, never more imaginative.** Title, date, done. That is a perfectly good wiki line.

!!! warning "The two-word problem"
    Teammate commit history runs to things like `garage rescaling`. Two words cannot be expanded into a paragraph without inventing the paragraph. The model can rephrase information; it cannot create it. The Done prompt is the *only* place that information can be captured, which is the real argument for building it.

### Two sources, not one

The bot always has **two** inputs, and this is what makes a terse Done phrase workable (Cap7n's correction, and it is right):

1. **The card** - written carefully in advance, detailed and accurate. This is a structural advantage: the bot's main source is high quality by construction.
2. **The Done phrase** - terse confirmation, plus any deviation from the plan.

Worked example:

| Source | Content |
|---|---|
| Card | "Rescale garage so it can fit the car, my best estimate 20%" |
| Done phrase | "garage rescaled" |
| Entry | *Silvio, 02/02/26: Rescaled the garage. The task estimated 20% would be needed to fit the car.* |

Every element traces to a source. Nothing is invented. The bot merged two real inputs rather than filling a void, which is a different operation from the `was implemented` failure above.

### Grammar attributes claims to their source

With two sources the risk stops being invention and becomes **false confirmation**: the plan's numbers appearing in a sentence about the outcome. If Silvio actually rescaled 50%, an entry reading *"rescaled around 20%"* is wrong information wearing honest clothes.

**The rule:** anything drawn from the card stays in planned/estimated voice. Only what came from the Done phrase may be stated as accomplished.

- Bad: `Rescaled the garage, estimated rescale around 20% to fit the car` (implies the result was 20%)
- Good: `Rescaled the garage. The task estimated 20% would be needed to fit the car.` (the actual figure is simply not recorded, which is true)

### Make input quality visible

A thin Done phrase should produce a visibly thin entry, and that is a feature. If the richness of the log obviously tracks the richness of the answer, people who care will start writing better ones. That teaches faster than any instruction, and it keeps the bot honest about how much it was actually told.

---

## Preset prompts for teammates

Teammates will not write good prompts and should not have to learn. Put an icon beside a card with a few buttons: **Find duplicates**, **Find in wiki**, **Other**. The user types something casual ("whatever is related to the pillbug") and it is slotted into a strict scaffold they never see.

## Retrieval: use the cheap tool first

- **Literal lookup** ("articles mentioning pillbugs") is a `ripgrep` job. Five milliseconds, perfect, every time. Never page a model through data to do what a search does.
- **Semantic lookup** (the paragraph describing "curl-morph roll" that never says *pillbug*) is where a model is needed. Either precompute embeddings once with `nomic-embed-text` (274MB, runs on CPU, no GPU needed) or crawl.
- **Crawling is viable at current scale.** The wiki is ~204,000 characters across 32 files, roughly 51k tokens, so a full sweep is about **three calls** at 32k context. A scratchpad file carries findings between calls; the model itself remembers nothing.
- **The crawl's real advantage is judgment, not retrieval:** *"does anything in the wiki contradict this new entry?"* No index answers that. A false flag costs ten seconds of reading.

!!! note "The model never learns"
    Weights are frozen. What improves over time is the *notes handed to it*. Same goldfish, better reference sheet. If output quality improves it is because input improved, which means it can be debugged.

## Self-building index

Rather than hand-maintaining a task-type to wiki-section map (which rots the first time the wiki is reorganised, then silently misfiles things), the bot **proposes** a destination and the approved choice is **stored**. Past confirmations become evidence for the next similar card. The index builds itself out of decisions actually made, so it cannot drift from reality. The review step is therefore not just a safety net, it is the training signal.

---

---

## The two prompts

The Done flow uses **two** prompts at different moments. Neither asks the model to write prose.

### 1. Question generator (runs when Done is clicked)

```
You read a task card and write ONE short question for the person who just finished it.
Ask about the single most likely place reality differed from the plan.
Prefer a detail the card guessed at, called an estimate, or left open.
Maximum 15 words. One question. No pleasantries.

CARD TITLE: {title}
CARD DESCRIPTION: {description}

Output only the question.
```

### 2. Entry extractor (runs nightly)

```
You extract facts from a finished task. You never add information.

TWO SOURCES. Keep them separate:
[PLAN] {card title + description}
[DONE] {done note - may be empty or very short}

Rules:
- Facts from [DONE] may be stated as accomplished.
- Facts from [PLAN] may ONLY be described as planned or estimated, never as outcomes.
- Never name a script, function, file, variable or particle system unless
  that exact word appears above.
- If a field has no support in the sources, return "". Empty is correct and expected.
- Do not explain, suggest, or add rationale.

Return JSON only:
{
  "did":        "",  // accomplished, from [DONE]. One sentence.
  "planned":    "",  // intent/estimates from [PLAN], in estimated voice, or empty.
  "deviation":  "",  // only if [DONE] contradicts [PLAN], else empty.
  "still_open": ""   // only if explicitly stated, else empty.
}
```

The JSON shape enforces the grammar rule **structurally**: `did` and `planned` are separate fields, so plan content physically cannot leak into the outcome sentence. Code assembles the final line from the fields; the token check runs on the values before assembly.

---

## Duplicate detection

**Automatic, not human-triggered** (decided 2026-08-10). A nightly pass finds them, tags them, and people filter by the tag rather than being interrupted.

Two conditions make it work:

- **Embeddings are a dependency, not an extra.** All-pairs LLM comparison does not scale: 96 cards is 4,560 pairs, growing quadratically. `nomic-embed-text` finds each card's nearest neighbours in milliseconds and the model only adjudicates the handful of close pairs. Only *new* cards need checking against the existing set.
- **Dismissals must be remembered.** Without a "not a duplicate" store, the nightly pass re-flags the same false positive forever, people learn to ignore the tag, and the feature is dead.

The tag names the pair (`possible duplicate of #47`), never a bare "dup found" that sends someone hunting.

---

## Implementation checklist

### Tasks app, data
- `done_note` - the Done-phrase answer
- `done_question` - the generated question, stored alongside so the answer keeps its context
- `wiki_synced_at` - nullable, drives "what is new" for the nightly run
- `wiki_target` - page + section. Bot proposes, human confirms, value is then reused (see self-building index)
- `dup_of` - nullable task id
- dismissed-pairs store

### Tasks app, UI
- <span class="pill done">Done</span> Done flow: click, ask, capture answer or skip, mark done. **Deployed 2026-08-10** (v1 asks a fixed question; the card-specific generated question waits until a model runs on the box). Cancel aborts the action so a misclick on Done is recoverable; saving an empty box means "nothing to add".
- Duplicate tag display plus a filter
- Preset prompt buttons (Find duplicates / Find in wiki / Other)
- <span class="pill done">Done</span> **Bot button + settings (2026-08-10 evening):** header **Bot** button opens a modal with on/off, run hour, log page path, and the last run's report. Config lives server-side in `data/bot.json` (`GET /bot`, `PATCH /bot/config`, bot reports via `POST /bot/status`) so any teammate steers it from the app - nobody needs SSH. The log path is validated (repo-relative `.md` only, no `..`): the bot joins it onto a clone while holding a write-capable deploy key, so the team key must not be able to aim it outside the repo. Cron pings hourly (8-23); the script exits silently unless enabled and the clock matches the configured hour - that is how the app controls the run hour without anything ever editing crontab.

!!! danger "Use the in-app modals"
    The question modal **must** use the existing `ask()` / `askText()` pattern. A native `prompt()` or `confirm()` reintroduces the Electron bug where the window stops accepting keyboard input until it loses and regains focus. All native dialogs were deliberately removed from this app.

### The bot
- <span class="pill done">Done</span> **BUILT AND LIVE 2026-08-10.** Master copy `Ingenui\Tasks\bot\wiki-logger.js` (Node, zero deps), deployed to `~/wiki-bot/` on ingenui, cron `0 23 * * *`. First run produced 5 stub entries (pre-note-feature tasks) on branch `bot/log` in `~/wiki-bot/towerdrop-docs`. Implementation facts:
    - Filters to **Tower Drop tasks only** - other projects' done tasks stay unsynced until they have a wiki of their own.
    - Idempotent two ways: re-run with empty queue is a no-op, and a run that died between commit and mark re-marks without re-appending (searches the log for `task #ID`).
    - Commit per task, mark per task, commit BEFORE mark: a duplicate in a reviewed diff beats a silently lost entry. A failed mark aborts the run.
    - Guard failure or model failure falls back to quoting the Done note **verbatim** - the human's own words are always safe.
    - Push failure is tolerated and logged (commits accumulate locally, retried next run) - proven on the first run, before the deploy key existed.
    - `temperature: 0` for the extraction - same input should give the same entry.
- Server got one new route for it: `POST /tasks/:id/wiki-synced` (team key only, no user identity) - the server stays the single DB writer.
- **Deploy key**: `~/.ssh/wiki-bot` on the box; the public key must be added to the GitHub repo (Settings > Deploy keys, write access) before pushes land.

### Dedup
- Build and incrementally update the embedding index
- New card, nearest neighbours, adjudicate close pairs only
- Write `dup_of` via the API, respect dismissals

### Infrastructure
- Ollama plus `qwen3:8b` and `nomic-embed-text` on ingenui. **Nothing is installed there yet** - only the main PC has Ollama.
- A clone of `towerdrop-docs` wherever the bot runs, plus push credentials

---

## Decided: append-only log, no placement

<span class="pill done">Done</span> **The bot does not decide where entries belong.** It appends dated entries to a single chronological log page. Sorting them into topic pages is a separate, optional, later pass (weekly, by a human, from a list the bot produces) and may never happen at all.

Why this is the right call:

- **Placement was the hardest part of the job.** Inferring a location inside a 372-line nested document is harder than writing the entry. Removing the requirement removes the risk.
- **Capture is irreversible, sorting is not.** An entry filed in the wrong place can be moved next week. An entry never written because the bot could not choose a destination is gone, along with whatever the author still remembered that evening.
- **An unsorted log is fully usable.** The whole wiki is ~51k tokens, so an assistant reads it end to end in about three calls, and a dated list is browsable enough for a human.
- **It matches the split this wiki already has:** curated pages hold *why things are the way they are*; the log holds *what happened*. Different jobs, different lifespans.

This removes from v1: placement inference, the `wiki_target` field, and the self-building index. Embeddings remain, for duplicate detection only. **v1 is: extract, assemble, append, push branch.**

## Decided: runs on the box

<span class="pill done">Done</span> The bot runs on ingenui, not the main PC. It is meant for the team as well as Cap7n, and the board already lives there. The 23:00 slot sits inside the box's waking window (suspends 23:30, wakes 16:45 weekdays / 08:45 weekends), and this is self-consistent: if the box is asleep, nobody can be marking tasks done either.

!!! warning "Mark each task synced as it completes"
    The run starts 23:00 and the box suspends 23:30. A large backlog could run past the window and be frozen mid-job. Write `wiki_synced_at` per task as each one finishes, never as one batch at the end, so a mid-run suspend simply resumes the next evening instead of losing or double-writing entries.

## Measured on the box (2026-08-10)

Ollama 0.32.7 runs on ingenui as a **user-space install** (`~/ollama/`, no root, `@reboot` cron, models in `~/ollama-models`, listening on 127.0.0.1 only since the bot runs on the same box). Models pulled: `qwen3:8b` (5.2 GB) and `nomic-embed-text` (274 MB). **CPU-only for now** - the RTX 2070 is invisible until the NVIDIA driver lesson happens (deliberately deferred, needs sudo + likely a reboot).

Real numbers from a realistic extraction prompt on the i7-4770K:

| Operation | Measured | Verdict |
|---|---|---|
| Embedding one card | 0.34 s | Dedup is trivially viable on CPU. Full board ~35 s once, then incremental. |
| 8B extraction, one task | ~26 s wall (prompt 18.8 tok/s, output 3.8 tok/s) | Nightly logger fine: a 5-task evening is ~2 minutes, well inside the 23:00-23:30 window. |
| Interactive question at Done-click | would be 15-25 s | Too slow for UX on CPU. This is why v1 ships a **fixed question**; the card-specific generated question waits for the GPU driver. |

Two implementation facts learned from the same test:

- **Use `format: json`** (Ollama's constrained decoding). The output was structurally valid JSON on the first try; the format is enforced at the decoder, not begged for in the prompt.
- **Normalize empty-meaning literals.** Told to return `""` for unsupported fields, the model wrote the *words* `"None"` and `"False"` instead. The assembler must canonicalize none/false/n-a/nothing to empty before the token check, or stubs will carry junk.

## Open questions

- `qwen3:8b` has thinking mode on by default and emits a long visible reasoning monologue before its answer. The bot must strip everything before the final output, or disable thinking.
- Does the token check produce too many false holds in practice? Unknown until it runs on real cards.
- Does the log need periodic curation before it grows past the point anyone reads it? A problem for month six, not for v1. The weekly sorting pass is the answer if it materialises.
