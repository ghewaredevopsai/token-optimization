# Lab 0 &mdash; Count your own bundle

**Tier 0 &middot; What one request costs** &nbsp;|&nbsp; ~6 minutes &nbsp;|&nbsp;
No assistant needed &nbsp;|&nbsp; measured, not scored

## The situation

You are about to spend two hours cutting a number. First you need to know what you
can actually measure &mdash; and, more importantly, what you cannot.

You will finish this lab holding **two instruments that do not agree and are not
supposed to**: one that answers instantly and counts only what you listed, and one
that is authoritative about money and arrives late.

## Do

1. **Clone the practice repo** (skip if you already have it from the prompt module):

   ```bash
   git clone https://github.com/ghewaredevopsai/meridian-freight
   cd meridian-freight
   ```

2. **Diff two bundles.**

   ```bash
   python3 tools/ctxmeter.py diff tools/bundles/everything.txt tools/bundles/naive.txt
   ```

   Read the itemised list before the total. Which two files dominate, and is either
   of them about how the desk prices freight?

3. **Watch a conversation grow.**

   ```bash
   python3 tools/ctxmeter.py turns tools/bundles/naive.txt --turns 10
   ```

   The cumulative column is what a ten-turn agent session sends. Compare it with the
   total for turn 1.

4. **Now the other instrument.** Open your GitHub billing and usage settings and find
   today's AI credit figure.

   Three things can happen, and all three are findings:

   - You see a number. Write it down, and note what time it was last updated.
   - You see an aggregate for the month but nothing for today.
   - **You see nothing** &mdash; on a company seat this is common, because usage is
     often visible only to an administrator.

   Write down which of the three you got. If it is the third, you have just discovered
   the constraint this whole module is designed around.

5. **Read `tools/calibration.md`.** Specifically the error table and the last section.
   You are about to quote this tool's numbers; spend ninety seconds knowing how wrong
   it is and in which direction.

## Record

One paste creates the sheet:

```bash
cat > lab-0-record.md <<'EOF'
# Lab 0

everything.txt      ______ est. tokens
naive.txt           ______ est. tokens
turn 1 sends        ______      turn 10 sends ______     10 turns cumulative ______

Credits visible on my seat?   yes / month only / no
If yes, last updated: ______

ctxmeter's measured error on code files: ______  (from calibration.md)
The thing it can never see: ____________________________________
EOF
```

Fill in the blanks in any editor, then commit it &mdash; the sheet is the
deliverable, not your memory of the run:

```bash
git add lab-0-record.md && git commit -m "lab 0: two instruments"
```

## Notice

- **The two data files are most of the repository** and neither is about pricing
  logic. Every "just use my codebase" request sends them.

- **Turn 10 sends far more than turn 1 although you typed less.** Nothing about that
  is a bug; it is the shape of every agent conversation, and it is why "start a new
  chat" is the second-widest bar on the Tier 4 ladder.

- **The meter is a floor.** It counts what you listed. The hidden system prompt, the
  editor's retrieval, and whatever the agent decided to open are invisible to it. Any
  number you take from this module is "at least this much".

- **A token is not a credit.** Credits depend on the model's rates and on how much of
  your context was cached. `ctxmeter` will not print a money figure unless you supply
  a rate, and then it labels it a what-if. That is not excessive caution: a token
  count quoted as a bill is the single easiest way to lose an audience's trust.

## Stretch

Run the diff again with `--rate 2.50`. Now you have a rupee-ish figure per turn.
Ask yourself what you would have to know before putting that number in an email to
your head of engineering &mdash; there are at least three things, and two of them are
in the footer.
