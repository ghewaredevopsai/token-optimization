# Token Optimization &mdash; delivery map

**Trainer-facing.** It ships in the repository: there are no lab spoilers in this module,
because the labs are measured rather than scored. The one genuinely private thing - the participant
key and the cohort snapshot - is called out in section 4 and lives outside.
Five tiers, five labs, two hours. Rewritten 18 September 2026, when `token-economics` (one tier,
five slides, no lab) became `token-optimization`.

---

## 1. Timing

| Tier | Talk | Lab | Running |
|---|:--:|:--:|:--:|
| 0 &mdash; What one request really costs | 12 | 6 | 0:18 |
| 1 &mdash; Choosing the model, by use case | 10 | 16 | 0:44 |
| 2 &mdash; Routing and cascades | 9 | 15 | 1:08 |
| 3 &mdash; A budget per turn | 8 | 19 | 1:35 |
| 4 &mdash; The levers in your own code | 7 | 12 | 1:54 |
| Buffer / questions | | 6 | **2:00** |

**46 min talk / 68 min lab / 6 min buffer &mdash; 40.4% theory on teaching time.**

⚠️ **T0 used to be a 30-minute briefing and is now 12.** If you are working from muscle memory you
will overrun the whole module in the first tier. The distribution material that used to close it
now lives in **T3 slide 2**, where it motivates eviction instead of standing alone.

## 2. Where it fits

| Room | Use | Notes |
|---|---|---|
| Developers on a paid seat (the default) | T0&ndash;T4, full | |
| Engineering leads and rollout owners | T0 and T1 talks, T3 slide 2 | budget defaults and the pooled-credit point |
| Leadership, 15 minutes | **T0 alone**, no lab | it still works as the standalone briefing it began as |
| An API or gateway course, not seat-based | T0 s3&ndash;s5, then T2 and T4 in full | the measured day is native to that audience; the plan table is not |
| A room that has just done Prompt &amp; Context Engineering | skip nothing, but say in T3 that the budget bar is the same picture | they have seen it from the accuracy side |

**T2 is the tier to protect.** It is the only one with a saving big enough to change a budget, and
its lab needs no assistant, so it survives a room with broken seats.

## 3. The 90-minute Course Three path

Day 1 is 09:00&ndash;19:00 with a fixed 4-hour block 1.3, so three full 2-hour blocks leave no lunch.
For that delivery:

1. Run **Lab 1 as a 6-minute discussion** rather than a 16-minute lab &mdash; do the prediction step
   live, on a whiteboard, then show the aggregate table from the front. Saves 10 minutes and keeps
   the pre-registration lesson, which is the part that matters.
2. Cut **T4 to its last slide** (*is the answer derivable?*) and run Lab 4 as a demo. Saves 12.
3. Keep T0, T2 and T3 and their labs intact.

That lands at ~90 minutes. **Do not buy time from Lab 3** &mdash; the 40% audit is the deliverable
participants quote afterwards.

## 4. The data behind T0 and T3

The measured day is one full delivery day of gateway counters, anonymised on the slides as
participants A, B, C and D.

⚠️ **The key that maps those letters to real identities is NOT in this repository.** It lives in
`../../trainer-private/` on the trainer's machine, together with the cohort snapshot JSON, which is
the only surviving copy of the data. That folder is the one thing about this module that is
deliberately not shareable: it carries real sandbox identifiers, the cohort number, real spend
figures, and the names of internal infrastructure.

Everything else a trainer needs is in this file.

## 5. Slide notes worth pre-reading

- **T0 s2:** the organisation default is *on*. Say it twice.
- **T0 s3:** the ratio is the lesson, not the dollars. 128:1 means shorter answers touch ~5% of the bill.
- **T0 s5, lever 5 (caching):** rests on the published price list only. **No Copilot cache hit rate
  has been measured.** Do not imply one.
- **T1 s2:** if someone asks you to put model names on the grid, decline and say why on the slide.
- **T3 s2:** the cause of participant A's spike was **not traced**. It is a hypothesis &mdash; an agent loop
  carrying a growing conversation &mdash; and never a finding. Say "not traced".
- **T2 s4 and T4:** every number is reproducible from the practice repo, so invite anyone who
  disagrees to re-run it in front of you.

## 6. Verified lab numbers

Deterministic and identical in every room:

| Where | Value |
|---|---|
| `cascade.py`, no gate | 70% accuracy, cost 80 |
| always-strong | 100%, cost 960 |
| threshold 0.60&ndash;0.70 | **100%, cost 368, 6 of 20 escalated** |
| threshold 1.01 (escalate all) | 100%, **cost 1040 &mdash; more than always-strong** |
| confidence when right / wrong | 0.85 / 0.53 &mdash; the gate is well founded |
| `everything.txt` / `naive.txt` / `minimal.txt` | ~52,000 / ~38,400 / ~2,200 est. tokens |
| practice repo test suite | 28 tests, exactly 1 failure |

⚠️ `ctxmeter diff naive minimal` **refuses to print a percentage** (the data share moves 64% &rarr; 0%).
That is the tool working. Read the refusal aloud in Lab 3 &mdash; it is a better lesson than the
percentage would have been.

**Re-measure these before a delivery**, and after any change to the practice repo or to
`ctxmeter`'s constants:

```bash
cd ../meridian-freight
python3 tools/ctxmeter.py repo --absolute | grep TOTAL
for b in everything naive minimal; do
  python3 tools/ctxmeter.py count --absolute $(grep -v '^#' tools/bundles/$b.txt) | grep TOTAL
done
```

⚠️ These figures moved once already: they were first written against the uncalibrated
estimator, and recalibrating it changed every one of them by 10-50%. A number quoted on
a slide and never re-derived is the exact failure this module teaches people to avoid.

## 7. Resolved, and still open

**RESOLVED &mdash; "how does a participant see their own spend?"** The old map recorded this as
blocking Lab 1. The answer was to stop depending on the vendor's meter: `ctxmeter` gives an instant
deterministic count of what you listed, the six counters give a behavioural proxy, and the billing
page is used only as a delayed sanity check. Lab 0 makes the participant hold both instruments and
notice they disagree. **Three of the five labs need no credit visibility at all.**

Still open:

- **The cause of participant A's spike** could be traced from the gateway spend logs if the claim is ever to
  become a finding. Until then it stays a hypothesis.
- **Copilot cache hit rate** in a real agent session is unmeasured.
- **`data/model-runs.json` in the practice repo is synthetic** and labelled so in the file and in
  Lab 1. Record real runs and replace it before anyone quotes a row as evidence.
- **Lab timings are estimates.** Labs 0 and 2 have been run end to end; 1, 3 and 4 have not been
  rehearsed against a live assistant.

## 8. Before you push

Run the leak scan from the trainer's local notes (`../../trainer-private/leak-scan.sh`). Its pattern
list names clients and internal systems, so the list itself does not ship with this repository.

Check authorship too: `git log --format='%an %ae'`.

⚠️ **The old README rule "never send the whole folder" is gone and must stay gone.** `trainer/` is
inside this repository on purpose - the labs are measured rather than scored, so there are no
spoilers. The only genuinely private material is the participant key and the cohort snapshot, which
live outside.

---

*Gheware DevOps &amp; Agentic AI &middot; devops.gheware.com &middot; training@gheware.com &middot; +91-9606795215*
