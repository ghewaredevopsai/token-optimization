# Lab 3 &mdash; The 40% token audit

**Tier 3 &middot; Context budgeting** &nbsp;|&nbsp; ~19 minutes &nbsp;|&nbsp;
Assistant needed for the re-checks &nbsp;|&nbsp; measured, not scored

## The situation

This is the flagship. You will cut a context bundle by at least 40% **without losing
the answer** &mdash; and the second half of that sentence is the hard part, because
an audit that only measures tokens proves you sent less, which was never in doubt.

**The task, held constant through every cut:**

> Why does the manifest total differ from the quote total?

The right answer names the fuel drift: `manifest.py` applies fuel to the base alone,
`rating.py` applies it to base plus surcharges. It should also notice that
`manifest.py` omits two surcharges entirely.

**Four cuts, in order, re-running the task after every one.** The target is 40%; most
rooms reach far more. Work straight down this page.

The task, held constant through every cut:

> Why does the manifest total differ from the quote total?

The right answer names the fuel drift: `manifest.py` applies fuel to the base alone,
`rating.py` applies it to base plus surcharges. A good answer also notices that
`manifest.py` omits two surcharges entirely.

---

## Step 1 &mdash; Baseline

```bash
cd ~/meridian-freight
python3 tools/ctxmeter.py count --absolute $(grep -v '^#' tools/bundles/naive.txt)
```

Then attach that same set of files in a **new chat** and ask the task question.

Record two things: the token total, and whether the answer was right.

---

## Step 2 &mdash; Cut 1: drop the data files

Attach the same set **minus `data/*.json`**.

Meter it, re-run the task in a new chat, record the token total and whether the
answer is still right.

---

## Step 3 &mdash; Cut 2: replace the documents with the lines that decide it

Instead of attaching `docs/`, paste in just the **Charging order** section:

```bash
sed -n '/^## Charging order/,/^## Clocks/p' docs/ops-runbook.md > six-lines.txt
python3 tools/ctxmeter.py count --absolute six-lines.txt
```

Attach the code files, paste those lines, re-run, record.

---

## Step 4 &mdash; Cut 3: start a new chat

You have been continuing one conversation. Start a **fresh** one with the same
attachments as Cut 2 and ask again.

The saving here is the whole conversation band from the Tier 0 diagram.

---

## Step 5 &mdash; Cut 4: go too far

Attach **only `manifest.py`**. Not `rating.py`, not the runbook.

Meter, re-run, record. **You are expected to break the answer here.** If you did not,
you have not cut hard enough, and you have learned less than the person next to you
who did.

---

## Step 6 &mdash; Compute the cut

```bash
python3 tools/ctxmeter.py diff tools/bundles/naive.txt tools/bundles/minimal.txt
```

**It will refuse to print a percentage.** Read the message it gives you. Then decide
whether to re-run it with `--allow-mixed`, and be able to say out loud why that is or
is not honest here.

---

## Step 7 &mdash; Record

One paste creates the sheet:

```bash
cat > lab-3-record.md <<'EOF'
# Lab 3

                              est. tokens   % of baseline   answer still right?
baseline (naive)              ______        100%            ___
cut 1: no data files          ______        ____%           ___
cut 2: six lines, not docs/   ______        ____%           ___
cut 3: new chat               ______        ____%           ___
cut 4: manifest.py alone      ______        ____%           ___

Largest cut that kept the answer: ____%
The cut that broke it, and why: ____________________________________

Did ctxmeter refuse a percentage? ___  Why? ____________________

What the meter could NOT see on any of these runs:
____________________________________________________________
EOF
```

Fill in the blanks in any editor, then commit it &mdash; the sheet is the
deliverable, not your memory of the run:

```bash
git add lab-3-record.md && git commit -m "lab 3: the 40% audit"
```

## Notice

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
