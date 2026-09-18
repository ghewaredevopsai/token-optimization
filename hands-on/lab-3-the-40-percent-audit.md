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

## Do

1. **Baseline.** Meter the naive bundle and run the task with it attached.

   ```bash
   python3 tools/ctxmeter.py count --absolute $(grep -v '^#' tools/bundles/naive.txt)
   ```

   Record the number and whether the answer was right.

2. **Cut 1 &mdash; drop the data files.** Remove `data/*.json` from what you attach.
   Meter, re-run the task, record.

3. **Cut 2 &mdash; replace the documents with the lines that decide it.** Instead of
   `docs/`, paste the six lines under **Charging order** in `docs/ops-runbook.md`.
   Meter, re-run, record.

4. **Cut 3 &mdash; new chat.** You have been continuing one conversation. Start a
   fresh one with the same attachments. The saving is the whole conversation band from
   the Tier 0 diagram.

5. **Cut 4 &mdash; go too far.** Attach only `manifest.py`. Not `rating.py`, not the
   runbook. Meter, re-run, record. **You are expected to break the answer here.**
   If you did not, you have not cut hard enough and you have learned less than the
   person next to you who did.

6. **Compute the cut.**

   ```bash
   python3 tools/ctxmeter.py diff tools/bundles/naive.txt tools/bundles/minimal.txt
   ```

   **It will refuse to print a percentage.** Read the message. Then decide whether to
   pass `--allow-mixed`, and say out loud why that is or is not honest here.

## Record

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
