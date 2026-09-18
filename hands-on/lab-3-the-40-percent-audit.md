# Lab 3 &mdash; The 40% token audit

**Tier 3 &middot; Context budgeting** &nbsp;|&nbsp; ~19 minutes &nbsp;|&nbsp;
Assistant needed for the re-checks &nbsp;|&nbsp; measured, not scored

## Objective

Cut a context bundle by at least 40% **without losing the answer** &mdash; and find
where the floor is by going past it.

By the end you should be able to:

- run a context audit that re-checks the answer after every cut, not just at the end;
- name the cut that breaks it, and say why;
- state what the meter could not see, which is what makes your number defensible.

## The situation

This is the flagship. You will cut a context bundle by at least 40% **without losing
the answer** &mdash; and the second half of that sentence is the hard part, because
an audit that only measures tokens proves you sent less, which was never in doubt.

**The task, held constant through every cut:**

> Why does the manifest total differ from the quote total?

The right answer names the fuel drift: `manifest.py` applies fuel to the base alone,
`rating.py` applies it to base plus surcharges. It should also notice that
`manifest.py` omits two surcharges entirely.

## What to watch for

- **The correctness column, not the token column.** An audit that only measures
  tokens proves you sent less, which was never in doubt.
- **Whether cut 2 makes the answer *better*.** Dropping `docs/` also drops the stale
  notes file that agrees with the bug.
- **Where it breaks.** If nothing you do breaks the answer, you did not cut hard
  enough and you learned less than the person who did.

The task, held constant through every cut:

> Why does the manifest total differ from the quote total?

The right answer names the fuel drift: `manifest.py` applies fuel to the base alone,
`rating.py` applies it to base plus surcharges.

---

## Step 1 &mdash; See all four cuts before you run any of them

```bash
cd ~/meridian-freight
python3 tools/audit_report.py
```

```
           what changed                 est. tok  of baseline   answer still right?
------------------------------------------------------------------------------
baseline   what most people attach         38360       100%   ___
cut 1      drop the two data files         11969        31%   ___
cut 2      the rule, not all of docs/      10158        26%   ___
cut 3      start a new chat                 same       same   ___
cut 4      one file, far too little          938         2%   ___
```

The arithmetic is done. **The last column is yours**, and it is the only one that
matters. The bundles for each cut are in `tools/bundles/`.

---

## Step 2 &mdash; Baseline

Attach everything in `tools/bundles/naive.txt` in a **new chat** and ask the task
question.

Did it name the fuel drift? Mark the baseline row.

---

## Step 3 &mdash; Cut 1: drop the data files

Attach the set in `cut1-no-data.txt` &mdash; the same files minus `data/*.json`.

New chat, same question, mark the row.

---

## Step 4 &mdash; Cut 2: the rule, not the whole docs folder

Attach the set in `cut2-rule-not-docs.txt`. That swaps the `docs/` folder for
`six-lines.txt`, which `audit_report.py` generated for you &mdash; just the
**Charging order** section.

New chat, same question, mark the row.

---

## Step 5 &mdash; Cut 3: start a new chat

No attachment changes at all. Same set as cut 2, but a **fresh conversation**.

The saving is the conversation you stop re-sending, which no bundle can show. The
script prints what six turns were costing you.

Ask again, mark the row.

---

## Step 6 &mdash; Cut 4: go too far

Attach **only `meridian/manifest.py`**. Not `rating.py`, not the runbook.

**You are expected to break the answer here.** Mark the row and write down *why* it
broke &mdash; that sentence is the most valuable line on your sheet.

---

## Step 7 &mdash; Watch the meter refuse a percentage

```bash
python3 tools/ctxmeter.py diff tools/bundles/naive.txt tools/bundles/minimal.txt
```

**It will not print a headline percentage.** Read the message. Then decide whether to
re-run with `--allow-mixed`, and be able to say out loud why that is or is not honest
here.

---

## Step 8 &mdash; Record

```bash
python3 tools/audit_report.py --record
```

That writes `lab-3-record.md` with every token figure already in it. **You fill in the
correctness column and the three questions at the foot.**

```bash
git add lab-3-record.md && git commit -m "lab 3: the 40% audit"
```

## Key takeaways

- **40% is the floor.** Most rooms reach 70&ndash;85% while keeping the answer. If you
  stopped at 40% you stopped early.

- **Cut 2 usually improves the answer**, not just the price. Removing `docs/` also
  removes `tariff-2026-notes.md`, which is stale and says fuel applies to the base
  alone &mdash; agreeing with the bug. A wrong document in the bundle is worse than no
  document, and this is that fact with a number attached.

- **`ctxmeter` refusing the percentage is the tool working.** The naive bundle is
  about two-thirds data by tokens; the minimal one is all code. The estimator's error
  does not cancel across that change, so a single headline percentage is not
  supportable without saying so. Passing `--allow-mixed` is fine &mdash; passing it
  without reading the message is not.

- **The last line of the record sheet is the one that matters at work.** Your 40% is
  a floor measured on what you listed. The hidden system prompt, the editor's
  retrieval and whatever the agent opened on its own are not in it. Somebody senior
  will ask; have the sentence ready.

## Stretch

Build your own bundle file &mdash; `tools/bundles/mine.txt` &mdash; that is the
smallest set of paths that still gets the right answer every time over three runs.
Commit it. That file is a reusable artifact, not an exercise: it is the answer to
"what should I attach for this kind of question", written down once for everyone
who asks it next.
