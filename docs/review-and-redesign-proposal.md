# Friction Wars: review and redesign proposal

Scope: `index.html` at commit `bd578b1`. Part 1 covers AI hallmarks in the visual design and the copy. Part 2 covers the teaching design. Part 3 is the proposed redesign. Figures in Part 2 come from running the engine in Node (200 to 500 seeded runs per figure; script method described in the appendix).

The reference for "Claudisms" is the Arize write-up (Bennett, September 2026), which groups them by rhetorical move: salience flag, contrast reframe, verdict intensifier, signpost, stock metaphor, gotcha framing. It also notes newer tics: bold lead-in bullets and three-part lists. Its main finding applies here: a phrase list catches few of these. Most of the problems below are moves, so fixing them means rewriting sentences. Find-and-replace will catch few of them.

---

## Part 1. AI hallmarks

### 1.1 Copy

The page contains 42 em dashes, 34 of them in text students or teachers will read. Every one should go. Most can become a full stop or a comma; a few hide a second clause that should be its own sentence.

Instances by move:

| Move | Where | Current text |
|---|---|---|
| Contrast reframe | L379 | "You don't beat that law with willpower — you exploit it." |
| Contrast reframe | L1184 | "the difference is **where** the friction was, not how much motivation was declared." |
| Contrast reframe | L599, L605, L637, L640 | "Willpower is not a friction lever", "Motivation display ≠ lower effort to act", "A gadget is not a friction lever", "Display ≠ lower effort to start" |
| Verdict / gotcha | L442 | "No hints, no labels: that judgement is the game." |
| Verdict | L1183 | "Equal budget, worse outcome:" |
| Signpost | L1172 | "The flywheel, in one line." |
| Salience flag | L600 | "small levers still count" |
| Aphorism | L1218 | "Robust designs — ones that removed the option upstream — survive it; marginal ones fold." |
| Stock metaphor | L1145, L1172, L1188, L598, L604, L639, L500 | "flywheel" (x3), "upstream", "design the choice out of the house entirely", "design the frictionless path out of existence", "stress-test" |
| Slogan headline | L378 | "Engineer the / path of least / resistance." |
| Unsupported claim | L1144 | "The same N pt on a real build lever would have shortened the morning bar enough to start the automaticity flywheel." This is printed unconditionally. For a 1-point pseudo spend it is false: the best 1-point real lever (shoes, -1.5) gives the habit a 2% chance of taking hold. |
| Decorative glyphs | L361, L500, L1069, L1143 | ◐ ☀ ⚡ ◆ ⚠, plus ▶ inside the "Run the week ▶" label |

Jargon a 14-year-old will not parse without help: "build automaticity", "break automaticity", "pseudo-lever", "cue", "drive", "body clock", "decision point", "budget efficiency", "baseline", "passive baseline". The scoring footnote (L504) and the "game-design parameters" footnote (L402) are written for a developer.

The code comments carry a separate set of tells that reveal generation from a design prompt: "Register: PLAYFUL", "SIGNATURE ELEMENT", "structural device that encodes the real flow", "differentiate structurally, NOT a grid of identical shadow-cards", "dark = re-derived ... not inverted", "clay / ochre, not pure yellow", "Replaces the old undecodable striped stack", and section references (§2, §5.6, §7, §8) to a spec that is not in the repo. Anyone reading the source will notice these.

### 1.2 Visual design

| Pattern | Where | Why it reads as generated |
|---|---|---|
| Uppercase, letter-spaced accent "eyebrow" above every H1 | `.eyebrow`; every screen | Standard template of AI-built landing pages |
| Numbered step rail "01 Set up … 05 Compare" in mono numerals | `.rail` | Same |
| Everything is a pill | buttons, badges, legend, meters, effort bars, progress bar, chips, "Winner" crown | Uniform `border-radius: 999px` on every element reads as a default setting |
| Thick coloured top or left border on rounded cards | `.team-panel`, `.stage`, `.target`, `.design-box`, `.d-item` | A very frequent pattern in generated UIs |
| Dashed-border "callout" boxes | `.callout` | Same |
| Purple accent on sage, accent focus ring, `color-mix` tints | `:root` | Purple/violet accents are a widely noticed default in generated UIs |
| Team names "Team Sage" and "Team Violet" | L770 | Named after the palette tokens. The class had no say in them |
| 800-weight geometric display face, clamp() hero at 74px with 0.95 leading | `.display-xl` | Marketing hero, out of place in a lesson tool |
| Mono numerals on every number | `.mono`, `.net`, `.term` | Dashboard styling with no data-dense reason |
| Light/dark toggle | top bar | Unneeded chrome on a projector tool |
| Two-column hero with radio cards ("Choose your game") | start screen | SaaS onboarding layout |

The larger problem is density. During the simulation each team panel shows a clock, a cue, a lever box (arrow, label, note, action tag per lever), two candidates (name, tag pill, bar, number, four to six equation chips, a "picked N% of the time" pill), and two meters. With two teams that is about 40 text elements, replaced every 1.65 seconds, on a projector. Nobody at the back of the room can read it, so the screen communicates nothing beyond "bars move".

---

## Part 2. Teaching design

The intended lesson is one sentence: Ada does whatever is easiest at the moment of choice, so change what is easy and the behaviour follows; repeat it and the habit carries it. The current build obscures that sentence in five ways.

**1. The core claim on screen is false.** The sim heading says "The agent always takes the shortest bar" (L470). The engine samples the choice with a softmax (L714 to L720), and also adds random noise to every bar. In balanced designs the longer bar is chosen in 12 to 16% of decisions; in designs that mix real and decoy levers, 24 to 25%. Students will watch the stated rule break roughly once every five frames, at the moment the rule is being taught.

**2. Luck decides close matches, and the two teams get different luck.** Each team gets a different seed (L941: `baseSeed + i*7919`), so they face different random draws. Running the same balanced design twice against itself, the two scores differ by more than 10 points in 31% of matches. A "Winner" crown on a coin flip teaches that design does not matter.

**3. The real "a-ha" exists in the model but the UI never shows it.** The model has a sharp tipping point. A single friction change on the morning run, nothing else:

| Friction removed from "Go for a run" | Runs where the habit takes hold (of 300) |
|---|---|
| 2.0 | 6% |
| 3.0 | 29% |
| 4.0 | 70% |
| 5.0 | 93% |

Small differences in friction produce all-or-nothing outcomes, because once the run is chosen a few times, habit lowers its effort further. This is the most surprising thing the simulation can show, and it is the thing students should leave with. The current UI shows it only as a percentage on two meters, frame by frame, with no view of the week as a whole.

**4. The decoys are too easy to spot, and the brief tells students they exist.** "Premium fitness tracker", "motivational poster" and "productivity quote as wallpaper" fail an obvious common-sense test, and the brief says "pseudo-levers only feel productive" before teams choose (L431). Teams that avoid the obvious decoys are not learning anything about friction. The model has a better decoy built in that the scenarios do not use: reward only affects habit growth after the action is chosen (L724). A card like "Treat yourself to a good coffee after every run" is attractive, sounds sensible, and does nothing until friction gets the first run to happen. Combined with a real friction card, it speeds the habit up. That is a second, subtler lesson, and it is true to the model.

**5. Too many concepts for one lesson.** Students meet base effort, friction, habit bonus, body clock, noise, choice probability, habit decay, reward, build and break automaticity, baseline, reduction %, budget efficiency, score out of 100, letter grade, and a stress-test rematch with a cut budget. Diminishing returns on stacked levers also changes outcomes but is never shown. Most of these should be hidden or removed.

Smaller issues:

- The two scenarios are the same numbers with different labels. That is fine, but the focus scenario's "Phone in another room" and "Website blocker" are modelled as making deep work 3 points easier, which is the wrong mechanism: they make the distraction harder. That blurs the distinction the lesson depends on (make the good thing easier vs make the bad thing harder).
- The alternatives "Skip the workout" and "Skip the snack" are non-actions. Students cannot picture choosing "skip the snack" as a thing that costs effort. Use a concrete rival: "Hit snooze", "Make a cup of tea and go to bed".
- Single break levers below +4 do almost nothing visible (with snack friction +3 the snacking habit still ends the week at 0.93, against 0.98 with no cards). A team that buys the snack tin alone will conclude friction does not work. The model is saying that strong habits need large friction; the debrief should say so explicitly, or the numbers should change.
- "Budget efficiency" (10 points for not buying decoys) scores the same mistake twice, since decoys already fail to move behaviour.
- The design screen says "Teacher enters each team's picks". With ten cards on a projector this is slow. Physical cards would let teams work at their tables.
- There is no prediction step. Asking students to commit to a forecast before the reveal is a cheap, well-established way to make the result surprising.

---

## Part 3. Proposal

### 3.1 Lesson structure (about 25 minutes)

**Round 0: Ada does nothing (3 min).** Show one morning: two bars, a long one for "Go for a run" and a short one for "Hit snooze". State the rule once: Ada does whichever is shorter. Ask the class: out of 14 mornings, how many times will Ada run? Run it. The calendar fills with 14 grey squares. Same for the 14 evenings of snacking: 14 red squares.

**Round 1: Design (8 min).** Each team gets eight printed cards and a budget of 10. They pick cards and write two predictions on the sheet: mornings run (of 14), evenings snacked (of 14). The teacher enters the picks.

**Round 2: The reveal (5 min).** Both teams run side by side under identical conditions. The view is a 14-day calendar per team, filling one day at a time, and one line per team showing the gap between the two bars over the fortnight. When the gap crosses zero, the line turns and keeps going, without any further help from the design. That moment is the a-ha, and the screen should mark it: "Day 4: running became easier than snoozing. Ada has run every morning since."

**Round 3: Debrief (8 min).** Flip each card to show what it changed. Then a single slider for the teacher: "remove this much friction from the run" from 0 to 5, rerunning the fortnight live. The class watches the calendar go from 0/14 to 13/14 across a small range. That shows the tipping point directly, independent of any team's choices.

Drop the stress-test rematch, the letter grades and the score out of 100. The result is two numbers per team, compared to the team's own prediction: "Predicted 5 mornings. Ada ran 11."

### 3.2 Engine changes

- Make the choice deterministic: Ada takes the shorter bar, every time. The on-screen rule is then literally true.
- Replace random noise with a scripted fortnight that both teams share: a rainy Tuesday (+1 to the run), a late night Thursday (+1.5 to snacking the next evening), a weekend lie-in. The week still varies, but both teams face the same variation, so every difference between them comes from their cards.
- Run 14 days with two decisions per day (one morning, one evening). Same 28 decisions as now, half the concepts per day.
- Keep diminishing returns but show it on the card back ("a second card on the same action counts for less").

### 3.3 Card set (morning scenario, budget 10)

Each card does one of three things. The card back names which.

| Card | Cost | Effect | Type |
|---|---|---|---|
| Lay out your kit the night before | 2 | run -3 | Fewer steps |
| Sleep in your running clothes | 2 | run -2.5 | Fewer steps |
| Shoes by the front door | 1 | run -1.5 | Fewer steps |
| Arrange to run with a friend at 7:00 | 3 | run -2, snooze +2 | Both |
| Don't buy snacks on the weekly shop | 3 | snack +3.5 | More steps for the habit you want gone |
| Snacks in a closed tin on the top shelf | 2 | snack +2.5 | More steps for the habit you want gone |
| Treat yourself to a good coffee after every run | 2 | reward of running doubled | Reward: only works once the run is happening |
| Write a goal on the fridge | 1 | none | Changes nothing Ada has to do |

The coffee card replaces the tracker and poster. On its own it does nothing. Paired with a friction card, it makes the habit form faster. Teams that understand why will buy a friction card first and the coffee card second.

### 3.4 Visual direction

Treat it as a board game printed on paper, projected.

- Off-white background, near-black text, one colour per team. Use orange and blue, which stay distinct under the common forms of colour blindness. No purple, no sage, no gradients, no shadows.
- One typeface, chosen for legibility at distance: Atkinson Hyperlegible (Google Fonts). Regular and bold only. No mono except for the score.
- Corners at 4px or square. No pills. No coloured side or top borders on cards. Cards are white rectangles with a thin rule, laid out like playing cards, cost in the top corner.
- Minimum text size on the projected screens: 28px. If a label does not fit at 28px, cut the label.
- No eyebrows, no step rail, no dark-mode toggle, no glyph icons. "Step 2 of 4" in plain text if a progress cue is needed.
- Default team names "Team 1" and "Team 2", editable by the teacher.
- Simulation screen per team: the calendar, the gap line, and today's two bars with a single number each. Nothing else. The full effort breakdown opens on click, for the teacher.
- A print stylesheet that outputs the eight cards and a prediction sheet on A4.

### 3.5 Copy rules

Plain verbs, second person or Ada's name, one idea per sentence. No em dashes. No sentences built on "X, not Y". No metaphors (flywheel, upstream, path of least resistance, stress-test). No sentences whose only job is to say something is important. Numbers as counts of days, never percentages of "automaticity".

Examples:

| Current | Proposed |
|---|---|
| Engineer the path of least resistance. | Make the run easier than the snooze button. |
| Behaviour flows to the lowest-friction option at every cue. You don't beat that law with willpower — you exploit it. | Every morning Ada does whichever option takes less effort. Change the effort and you change what she does. |
| The agent always takes the shortest bar | Ada picks the shorter bar. |
| Build automaticity 87% | Ada ran 12 of 14 mornings. |
| ⚠ 3 pt spent on pseudo-levers. That budget moved almost nothing at the cue. | 3 points went on cards that changed nothing Ada had to do. |
| The flywheel, in one line. Lowering friction early let the good action get chosen… it compounds. | From day 4, running was easier than snoozing. Each run made the next one easier. |
| Willpower is not a friction lever — nothing at the cue changed. | Ada still has to do the same things at 7:00. |
| Removes the option upstream of the cue. | The snacks aren't in the house. |

### 3.6 Order of work

1. Engine: deterministic choice, shared scripted fortnight, 14 days. Smallest change, fixes problems 1 and 2.
2. New card set and the coffee mechanic.
3. Calendar and gap-line view; remove equation chips from the default view.
4. Prediction step and teacher slider.
5. Visual restyle and copy rewrite.
6. Print stylesheet for cards.

---

## Appendix: method

The `<script>` block was extracted and run in Node. Figures:

- Longer-bar choice rate: share of the 28 decisions in which the chosen option's effort exceeded the other's, averaged over 200 seeds per design.
- Luck spread: the balanced design (kit, sleep kit, shoes, cancel delivery, tin) run against itself with different seeds, 500 pairs.
- Tipping point: one zero-cost lever on "Go for a run" at each friction level, 300 seeds each, counting runs that ended with build habit above 0.5.
- Break-lever sweep: same method on the snack action, 300 seeds each.
