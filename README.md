# Token Optimization &mdash; reusable training module

A **two-hour crash course** in what an AI-assisted request actually costs and what to do about it:
where the tokens really go, how to choose a model by use case rather than by reputation, cheap-first
cascades, budgeting the context window, and the levers that live in your own code &mdash; the last
of which is not calling the model at all.

> This module was called **`token-economics`** until September 2026, when it grew from a single
> five-slide briefing into five tiers with labs. Tier 0 is that original briefing, re-cut and
> restyled; everything after it is new.

**Start at [`index.html`](index.html)**, the tier picker. On GitHub, start at
[`hands-on/`](hands-on/README.md).

```
token-optimization/
  index.html                       module home and tier picker
  token-learning-outcomes.md       the outcomes, where they came from, and the facts that age
  presentation/                    5 decks, one per tier
  hands-on/                        5 Markdown lab guides + their landing page
  solutions/                       reference answers for every lab - look whenever you want
  trainer/                         delivery map, timings, room map, the deck check
  assets/                          one theme file, one slide runner
```

The practice codebase is a **separate repository**,
[meridian-freight](https://github.com/ghewaredevopsai/meridian-freight) &mdash; shared with the
Prompt &amp; Context Engineering module, so a room doing both clones it once.

## The tiers

| Tier | Deck | Talk | Lab |
|---|---|:--:|---|
| 0 &mdash; What one request really costs | [`t0-what-a-request-costs.html`](presentation/t0-what-a-request-costs.html) | 12 min | [Lab 0](hands-on/lab-0-count-your-own-bundle.md) &middot; 6 min |
| 1 &mdash; Choosing the model, by use case | [`t1-model-selection.html`](presentation/t1-model-selection.html) | 10 min | [Lab 1](hands-on/lab-1-one-matrix-row.md) &middot; 16 min |
| 2 &mdash; Routing and cascades | [`t2-routing-cascades.html`](presentation/t2-routing-cascades.html) | 9 min | [Lab 2](hands-on/lab-2-the-cascade.md) &middot; 15 min |
| 3 &mdash; A budget per turn | [`t3-context-budget.html`](presentation/t3-context-budget.html) | 8 min | [Lab 3](hands-on/lab-3-the-40-percent-audit.md) &middot; 19 min |
| 4 &mdash; The levers in your own code | [`t4-code-levers.html`](presentation/t4-code-levers.html) | 7 min | [Lab 4](hands-on/lab-4-delete-a-model-call.md) &middot; 12 min |

46 minutes of talk, 68 minutes of lab, 6 minutes of buffer.

**Tier 0 also stands alone** as a 12-minute briefing for a room that only needs to know what a seat
costs and why the bill moves. It has been delivered that way and works.

## Run it

```bash
python3 -m http.server 8080          # http://localhost:8080 - the decks
```

Decks: arrow keys or space move, **T** index, **N** speaker notes, **F** fullscreen, **Esc** closes.
Print to PDF straight from the browser.

## House rules for this module

- **Derive, do not assert.** Every number on a slide traces to a measured run. Anything priced at a
  published rate is labelled a what-if, at that rate, uncached &mdash; never presented as an invoice.
- **At most five slides a tier.** Anything more goes into speaker notes or another tier. Held since
  the module was one deck.
- **No slide names a model.** The roster changes monthly. Tier 1's grid is **shaded, not numbered**,
  for exactly this reason, and it says so on the slide.
- **Two instruments, never in the same row.** `ctxmeter` proves the cut; the billing page proves the
  money. Conflating them is the fastest way to lose a finance audience.
- **Solutions ship with the module.** [`solutions/`](solutions/README.md) has a reference answer for
  every lab, including the exact cascade sweep and audit numbers.
- **Measured, not scored.** No pass marks. Each lab records a number before and after and ends with a
  committed `lab-N-record.md`.
- **Labs 0 and 2 need no assistant and no network**, so they work on a locked-down machine and give
  every participant identical numbers.
- **Client-neutral.** No client is named anywhere in this repository. The measured day is reported as
  "one workshop day" with participants shown as A, B, C and D, and no cohort, sandbox, gateway or
  provider is identifiable.

## Who owns what

| Owned by | The line |
|---|---|
| **This module** | Where tokens go, the credit mechanism, model selection by use case, routing and cascades, context budgeting and eviction, the levers in your own code |
| **Prompt &amp; Context Engineering** | How to ask: the six parts, output contracts, the pattern set, grounding, failure and drift |
| **Copilot as Assistant** | Credits, plans and budgets from the seat-holder's side &mdash; **and the three-model comparison lab**, which this module analyses rather than repeats |
| **MCP**, Tier 6 | What a tool list costs per turn, cost-per-tool-call traces, caps and fallback |
| *Evaluation*, when built | The cost-regression gate in CI |

## Where the content came from

- **Tier 0** is the original `token-economics` briefing: one measured workshop day of gateway
  counters, plus the published credit mechanism. Re-cut from 30 minutes to 12, restyled, and its
  distribution slide moved into Tier 3 where it motivates eviction.
- **Course One, Module 1** &mdash; the re-discovery tax, the context window as a budget, the
  placement rule, and session hygiene.
- **The framework modules** &mdash; the habit of reading spend off a run rather than asserting it.
- Written new, September 2026: the selection grid and task frontier, routing and cascades, the
  eviction order, the lever ladder, and all five labs.

---

&copy; Gheware DevOps &amp; Agentic AI &middot; devops.gheware.com &middot; training@gheware.com &middot; +91-9606795215
