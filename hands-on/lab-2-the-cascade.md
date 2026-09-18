# Lab 2 &mdash; The cascade, and what it saved

**Tier 2 &middot; Routing and cascades** &nbsp;|&nbsp; ~15 minutes &nbsp;|&nbsp;
No assistant, no network &nbsp;|&nbsp; measured, not scored

## Objective

Find the gate: the rule that decides when a cheap answer is not good enough - and
find out what a badly set one costs.

By the end you should be able to:

- name four gates that can be automated and two that cannot;
- find the escalation rate where a cascade matches the strong model's accuracy, and
  the rate where it costs more than not cascading at all;
- check whether a cheap model's confidence tracks its correctness, and say what
  follows if it does not.

## What to watch for

- **The plateau, not the point.** 0.60 to 0.70 all cost the same. A threshold in the
  middle of a flat region is robust; one on a cliff edge is fitted to your sample.
- **The last row.** Escalating everything reaches 100% accuracy and costs *more* than
  always-strong. Nothing about the answers would tell you - only the cost column does.
- **Confidence when right against confidence when wrong.** If those two were equal the
  whole pattern collapses, and almost nobody checks.
## The situation

Twenty triage cases. Two "models" that are ordinary Python functions with a known
accuracy and a known price, so the arithmetic is the only thing under discussion and
every row in the room is comparable.

Your job is the **gate**: the rule that decides when the cheap answer is not good
enough.

**Twenty triage cases, two "models" that are ordinary Python functions**, and one
decision to make: the gate. Nothing here calls a model or touches the network, so
every number in the room is identical. Work straight down this page.

---

## Step 1 &mdash; Run it unmodified

```bash
cd ~/meridian-freight
python3 tools/cascade.py
```

You escalate nothing, so you get the cheap model's accuracy at the cheap model's
price &mdash; plus a message telling you exactly that.

---

## Step 2 &mdash; Read the two baselines

From that output:

- **always cheap:** 70% accurate, cost 80
- **always strong:** 100% accurate, cost 960

Everything you do from here lives between those two rows.

---

## Step 3 &mdash; Write the blunt gate

Open `tools/cascade.py` and replace the body of `decide()` with:

```python
return confidence < 0.6
```

Then run it again:

```bash
python3 tools/cascade.py
```

---

## Step 4 &mdash; See the whole curve at once

```bash
python3 tools/cascade_report.py
```

```
gate < 0.40        70%       80          0%
gate < 0.50        75%      128          5%
gate < 0.60       100%      368         30%   cheapest at full accuracy
gate < 0.65       100%      368         30%   same cost - the plateau
gate < 0.80       100%      560         50%   full accuracy, 1.5x the cost of 0.60
gate < 1.01       100%     1040        100%   costs MORE than always-strong
```

**Find the cheapest threshold that still reaches 100%.** Note also that 0.60 to 0.70
is a *plateau*, not a point: a threshold sitting in the middle of a flat region is
robust, one perched on a cliff edge is fitted to your sample.

This tool does not edit `cascade.py` &mdash; your gate from Step 3 stays yours. It
just saves you running the script six times and copying numbers.

---

## Step 5 &mdash; Look hard at the last row

`gate < 1.01` escalates every case. Compare its cost with always-strong's 960.

**It costs more.** Not the same &mdash; *more*. You bought the cheap answer twenty
times and threw it away twenty times, and the accuracy column still reads 100%, so
nothing about the answers would ever tell you. **Only the cost column does.**

Write that number down. It is the most important row in the lab.

---

## Step 6 &mdash; Check the assumption the pattern rests on

Does the cheap model's confidence actually track whether it is right?

```bash
python3 - <<'PY'
import json
cases = json.load(open("data/triage-cases.json"))["cases"]
right = [c["cheap_confidence"] for c in cases if c["cheap_answer"] == c["true_answer"]]
wrong = [c["cheap_confidence"] for c in cases if c["cheap_answer"] != c["true_answer"]]
print("when right: %.2f avg over %d" % (sum(right)/len(right), len(right)))
print("when wrong: %.2f avg over %d" % (sum(wrong)/len(wrong), len(wrong)))
PY
```

If those two numbers were equal, your gate would be a coin toss that costs money.

---

## Step 7 &mdash; Record

```bash
python3 tools/cascade_report.py --record
```

That writes `lab-2-record.md` with your chosen threshold, the saving against
always-strong and the confidence figures already in it. **You fill in the sweep rows
and the last question.**

Or write the sheet by hand:

```bash
cat > lab-2-record.md <<'EOF'
# Lab 2

threshold   accuracy   cost   escalated
0.40        ____       ____   ____
0.60        ____       ____   ____
0.80        ____       ____   ____
1.01        ____       ____   ____

Cheapest threshold at 100% accuracy: ______
Cost there vs always-strong: ______%  saving

Escalate-everything cost ______ against always-strong's 960.

Confidence when right ______ vs when wrong ______.
Would this cascade work if those two numbers were equal?  ______
EOF
```

Fill in the blanks in any editor, then commit it &mdash; the sheet is the
deliverable, not your memory of the run:

```bash
git add lab-2-record.md && git commit -m "lab 2: the cascade"
```

## Key takeaways

- **There is a threshold where you get the strong model's accuracy for about a third
  of its price.** That is the pattern working, and it is a bigger saving than any
  prompt technique in either module.

- **Escalate everything and you pay more than always-strong.** Not the same &mdash;
  *more*. You bought the cheap answer and then threw it away, twenty times. The
  accuracy column still reads 100%, so nothing about the answers would ever tell you.
  Only the cost column does, which is the argument for having one.

- **Step 6 is the check nobody does.** If confidence-when-wrong equals
  confidence-when-right, your gate is a random sampler that costs money, and the
  cascade is worse than either model alone. A cheap model that is *confidently* wrong
  cannot be cascaded &mdash; only replaced.

- **Nothing here called a model.** Every number in your sheet is reproducible by
  anyone in the room. When you take this pattern to real models, that stops being
  true, and your threshold becomes a thing to re-measure rather than a constant.

## Stretch

Change `COST_STRONG_MINOR` from 48 to 12, so the strong model is only three times the
price rather than twelve. Re-run your sweep. Does your chosen threshold still make
sense &mdash; and at what price ratio does the cascade stop being worth its
complexity at all? That ratio, not the threshold, is what you should carry to work.
