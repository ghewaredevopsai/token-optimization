# Lab 0 &mdash; Count your own bundle

**Tier 0 &middot; What one request costs** &nbsp;|&nbsp; ~6 minutes &nbsp;|&nbsp;
No assistant needed &nbsp;|&nbsp; measured, not scored

## The situation

You are about to spend two hours cutting a number. First you need to know what you
can actually measure &mdash; and, more importantly, what you cannot.

You will finish this lab holding **two instruments that do not agree and are not
supposed to**: one that answers instantly and counts only what you listed, and one
that is authoritative about money and arrives late.

**You will finish this lab holding two instruments that disagree** &mdash; one
deterministic and instant, one authoritative about money and late. Work straight down
this page.

---

## Step 1 &mdash; Get the practice repo

Skip this if you already have it from the Prompt &amp; Context Engineering module.

```bash
cd ~
git clone https://github.com/ghewaredevopsai/meridian-freight
cd meridian-freight
```

---

## Step 2 &mdash; Diff two bundles

```bash
python3 tools/ctxmeter.py diff tools/bundles/everything.txt tools/bundles/naive.txt
```

Read the **itemised list** before the total. Answer two questions for yourself:

- which two files dominate?
- is either of them about how the desk prices freight?

---

## Step 3 &mdash; Watch a conversation grow

```bash
python3 tools/ctxmeter.py turns tools/bundles/naive.txt --turns 10
```

Compare what **turn 1** sends with what **turn 10** sends, and look at the cumulative
column &mdash; that is what a ten-turn agent session costs to send.

---

## Step 4 &mdash; Now the other instrument

Open your **GitHub billing and usage settings** and look for today's AI credit figure.

Three things can happen. **All three are findings** &mdash; write down which one you
got:

1. **You see a number.** Note it, and note when it was last updated.
2. **You see a monthly aggregate but nothing for today.**
3. **You see nothing at all.** Common on a company seat, where usage is often visible
   only to an administrator.

If you got (3), you have just discovered the constraint this whole module is designed
around &mdash; and three of its five labs need no credit visibility whatsoever.

---

## Step 5 &mdash; Read the calibration

```bash
less tools/calibration.md      # or open it in your editor
```

Read the **error table** and the **last section**. You are about to quote this tool's
numbers all morning; spend ninety seconds knowing how wrong it is, and in which
direction.

---

## Step 6 &mdash; Record

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
