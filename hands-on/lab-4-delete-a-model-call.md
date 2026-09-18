# Lab 4 &mdash; Delete a model call

**Tier 4 &middot; The levers in your own code** &nbsp;|&nbsp; ~12 minutes &nbsp;|&nbsp;
Assistant optional &nbsp;|&nbsp; measured, not scored

## The situation

`prompts/summarise-exceptions.md` is a prompt somebody on the desk runs every
morning. Part of what it asks for is a judgement. The rest is counting.

You are going to take the counting back.

**A prompt somebody runs every morning does two jobs: counting and judging.** You are
going to take the counting back. Work straight down this page.

---

## Step 1 &mdash; Meter the prompt as it stands

```bash
cd ~/meridian-freight
python3 tools/ctxmeter.py count --absolute prompts/summarise-exceptions.md
python3 -m meridian exceptions > exceptions.txt
python3 tools/ctxmeter.py count --absolute exceptions.txt
```

The sum of those two is roughly what one morning sends, before any answer comes back.

---

## Step 2 &mdash; Run it three times

Paste `prompts/summarise-exceptions.md` into a **new chat**, with the contents of
`exceptions.txt` underneath. Do that **three times, in three new chats**.

Save all three replies, then answer two questions:

- are the three **groupings** identical?
- are the three **counts** identical?

---

## Step 3 &mdash; Run the deterministic version

```bash
python3 - <<'PY'
from meridian import report, store, tariffs
rows = report.exceptions(store.load(), tariffs.load())
print(report.summarise(rows))
PY
```

That function already existed in the codebase. **Somebody wrote it, and somebody else
later wrote a prompt that duplicates it.** Noticing that is most of this lab.

---

## Step 4 &mdash; Run the Python three times

Same command, three times. Compare the three outputs with each other.

---

## Step 5 &mdash; Decide what is left for the model

Counting is settled. What remains is *what to do about each group* &mdash; which
depends on the day, the depot and who is on shift.

Write a much shorter prompt that asks only for that, with the counts supplied:

```text
Here are this morning's exception counts:
  MF-01: 2   MF-02: 2   MF-03: 8   MF-04: 3   MF-05: 0   MF-06: 2

Eight unrouted consignments is unusual for a Tuesday. Suggest what the duty
supervisor should work first and why, in under 80 words. If the counts alone do
not justify a recommendation, say so rather than guessing.
```

---

## Step 6 &mdash; Meter the new prompt

Save it as `prompt-short.txt`, then:

```bash
python3 tools/ctxmeter.py count --absolute prompt-short.txt
```

---

## Step 7 &mdash; Record

One paste creates the sheet:

```bash
cat > lab-4-record.md <<'EOF'
# Lab 4

                          est. tokens   wall time   3 runs identical?
prompt as it stands       ______        ______      ___
Python                    0             ______      ___
short prompt, counts fed  ______        ______      ___

Tokens saved per morning: ______   Per year, at one run a day: ______

The part I kept for the model, and why it needs a model:
____________________________________________________________
EOF
```

Fill in the blanks in any editor, then commit it &mdash; the sheet is the
deliverable, not your memory of the run:

```bash
git add lab-4-record.md && git commit -m "lab 4: delete a model call"
```

## Notice

- **The variance column is the argument, not the token column.** Three model runs give
  three groupings; three Python runs give one. You were paying for
  non-determinism in arithmetic. Nobody would choose that if it were framed as a
  choice, and it never is &mdash; it arrives as "let's use AI for the morning report".

- **The saving compounds quietly.** One run a day is not a budget item. The same
  pattern across forty internal prompts is, and forty is not an unusual number for a
  department eighteen months in.

- **This is not an argument against using a model.** Step 5 is the point: the judgement
  half is genuinely hard, genuinely valuable, and worth every credit. Sending the
  counting along with it is the waste.

- **`report.summarise()` was already in the codebase.** Somebody wrote the
  deterministic version, and somebody else later wrote a prompt that duplicates it.
  That is not a story about AI &mdash; it is an ordinary discoverability failure, and
  it is why the first question in this lab is "does this already exist?".

## Stretch

Look at `scripts/nightly_sync.py`. It computes an amount per consignment with a
hardcoded multiplier and a float. Ask your assistant what is wrong with that file,
then read its answer against `docs/ops-runbook.md` yourself. There are at least four
problems and only some of them are about money &mdash; and one of them is the kind
of thing you never want an assistant to have learned from your codebase as a pattern.
