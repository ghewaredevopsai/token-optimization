# Lab 4 &mdash; Delete a model call

**Tier 4 &middot; The levers in your own code** &nbsp;|&nbsp; ~12 minutes &nbsp;|&nbsp;
Assistant optional &nbsp;|&nbsp; measured, not scored

## The situation

`prompts/summarise-exceptions.md` is a prompt somebody on the desk runs every
morning. Part of what it asks for is a judgement. The rest is counting.

You are going to take the counting back.

## Do

1. **Meter the prompt as it stands.**

   ```bash
   cd meridian-freight
   python3 tools/ctxmeter.py count --absolute prompts/summarise-exceptions.md
   python3 -m meridian exceptions > /tmp/exceptions.txt
   python3 tools/ctxmeter.py count --absolute /tmp/exceptions.txt
   ```

   The sum of those two is roughly what one morning costs, before the answer.

2. **Run it three times** through your assistant, with the report pasted in. Save all
   three answers. Are the three groupings identical? Are the counts?

3. **Write the deterministic half.** About twenty-five lines of standard library that
   produce the counts per exception code from the same data:

   ```bash
   python3 - <<'PY'
   from meridian import report, store, tariffs
   rows = report.exceptions(store.load(), tariffs.load())
   print(report.summarise(rows))
   PY
   ```

   That already exists &mdash; `report.summarise()`. The lab is noticing that it
   exists, and that a model was being paid to do it anyway. Extend it if you like:
   group by destination depot as well, and print it as a table.

4. **Run your Python three times.** Compare the three outputs.

5. **Decide what is left for the model.** Counting is settled. What remains is *what
   to do about each group* &mdash; a judgement that depends on the day, the depot and
   who is on shift. Write a much shorter prompt that asks only for that, with the
   counts supplied.

6. **Meter the new prompt.**

## Record

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
