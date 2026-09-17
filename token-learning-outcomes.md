# Learning Outcomes &mdash; Token Optimization

*A two-hour crash course &middot; 5 tiers &middot; 5 labs*

Tick a box (`- [x]`) when you can do the thing **without looking it up**.

This module assumes you use an AI assistant at work and have a seat on a paid plan or a gateway key.
It does not teach prompting &mdash; see *Boundary* at the foot.

## Progress

| Tier | Outcomes | Lab | Done |
|---|:--:|---|:--:|
| 0 &mdash; What one request really costs | 5 | Lab 0 | &#9744; |
| 1 &mdash; Choosing the model, by use case | 5 | Lab 1 | &#9744; |
| 2 &mdash; Routing and cascades | 5 | Lab 2 | &#9744; |
| 3 &mdash; A budget per turn | 5 | Lab 3 | &#9744; |
| 4 &mdash; The levers in your own code | 4 | Lab 4 | &#9744; |

## Tier 0 &middot; What one request really costs

- [ ] Write the credit formula from memory and say which of the three token prices is the big one.
- [ ] Name what is metered and what is not, and say what Auto changes.
- [ ] State what happens at zero on an individual plan versus in an organisation &mdash; and say
      which default surprises people.
- [ ] Explain why input dominates a bill by two orders of magnitude, in terms of what an agent turn
      re-sends.
- [ ] Say why "be concise" is not a cost lever, with a number.

**Lab evidence**
- [ ] Lab 0 &mdash; two bundles metered, the growth of a ten-turn conversation recorded, and you know
      whether your own seat shows you a credit figure at all.

## Tier 1 &middot; Choosing the model, by use case

- [ ] Name the six selection criteria, and say which one is not a property of the tier at all.
- [ ] Explain why the grid is shaded rather than numbered.
- [ ] Place a task on the frontier using what being wrong costs and how often the cheap model is
      already right &mdash; and say why placing tasks outlives placing models.
- [ ] Pre-register a prediction before opening evidence, and say what that protects you from.
- [ ] Write a selection rule that says what it is *not* for, and includes its own review trigger.

**Lab evidence**
- [ ] Lab 1 &mdash; three predictions written before the data, scored against it, and one row of a
      model-per-task guide you would defend.

## Tier 2 &middot; Routing and cascades

- [ ] Draw the three topologies and give the cost formula for each in one line.
- [ ] Say why a cascade pays the cheap call even when it escalates, and what that implies.
- [ ] Name four gates that can be automated and two that cannot.
- [ ] Find the escalation rate at which a cascade matches the strong model's accuracy, and the rate
      at which it costs more than not cascading at all.
- [ ] Check whether a cheap model's confidence tracks its correctness, and say what follows if it
      does not.

**Lab evidence**
- [ ] Lab 2 &mdash; accuracy and cost at four thresholds, including the one that costs more than
      always-strong, and the confidence-when-right against confidence-when-wrong figures.

## Tier 3 &middot; A budget per turn

- [ ] Explain why the average user tells you nothing about a token bill, using the distribution.
- [ ] Give the eviction order: what goes first, what goes second, what never goes.
- [ ] Say what caching rewards, and what a daily edit to an instructions file costs you.
- [ ] Run a context audit that re-checks the answer after every cut, and say why one that does not
      is worthless.
- [ ] State what a context meter cannot see, without prompting.

**Lab evidence**
- [ ] Lab 3 &mdash; four cuts with tokens and correctness recorded for each, the cut that broke the
      answer identified, and a sentence about what the meter could not see.

## Tier 4 &middot; The levers in your own code

- [ ] Rank the levers by saving against effort, and say which two are free.
- [ ] Say why `max_tokens` is on every checklist and near the bottom of this one &mdash; and why it
      is still worth setting.
- [ ] Recognise a model call that is really a computation, and replace it.
- [ ] Argue the variance case, not just the token case, for that replacement.

**Lab evidence**
- [ ] Lab 4 &mdash; a prompt's counting half replaced with standard library, with tokens, wall time
      and whether three runs agreed, plus a statement of what you kept for the model and why.

## Where these outcomes came from

| Source | What it contributed |
|---|---|
| The original `token-economics` T0 briefing | The credit mechanism, plan defaults, and one measured workshop day: 42.7M tokens, 99.2% input, 128:1, 46% of spend in one hour, and the distribution in which one participant sent 54%. Re-cut from 30 minutes to 12; the distribution moved to Tier 3. |
| Course One, Module 1 | The re-discovery tax, the context window as a budget, the placement rule, session hygiene, and choosing per task rather than per person. |
| `copilot-assistant/` T6 and its Lab 6 | Auto against fast against reasoning, credit mechanics from the seat-holder's side, and the three-model comparison this module analyses rather than repeats. |
| `mcp/` T6 | A tool list is context you pay for on every turn. One slide here, cross-linked; that module owns it. |
| `crewai/` and `google-adk/` T2 | The habit of reading spend off a run, and the finding that framework choice changes token counts on identical work. |
| Written new, September 2026 | The selection grid and the task frontier, routing and cascades with a measurable gate, the eviction order, the lever ladder, and all five labs. |

## Facts that age

Checked 18 September 2026. **Re-read before every delivery** &mdash; this module ages faster than
any other in the catalogue.

- **Credit rates and plan allowances** change. The formula and the shape of the three prices have
  been stable; the numbers have not. Verify Tier 0 slide 2 against the vendor's current published
  pricing before you show it.
- **The organisation default** &mdash; additional usage on, individual plans off &mdash; is the line
  most worth re-checking, because it is the one that costs a client money if it has changed.
- **No slide names a model**, and Tier 1's grid is shaded rather than numbered, precisely so that a
  roster change does not invalidate the deck. Keep it that way when someone asks you to add names.
- **The measured workshop day is a fixed historical observation**, not a forecast. It ran on an
  open-weight model through a gateway, so its absolute spend is not a seat invoice and is never
  quoted as one. The 99.2% input split is the transferable part.
- **`data/model-runs.json` in the practice repo is SYNTHETIC**, and says so in its own provenance
  block. It exists so Lab 1 runs offline on a locked-down seat. **Replace it with recorded runs
  before anyone quotes a row as evidence.**
- **`ctxmeter`'s calibration** was measured 18 September 2026 and lives in the practice repo's
  `tools/calibration.md`. Re-run it after any large change to that repository.

## Boundary

This module owns cost and choice. **How to ask** is the **Prompt &amp; Context Engineering** module.
**Credits and plans from the seat-holder's side, and the three-model comparison lab**, are
**Copilot as Assistant** Tier 6. **What a tool list costs per turn** is **MCP** Tier 6. A
**cost-regression gate in CI** belongs to the evaluation module when it is built, not here.
