# Lab 2 &mdash; The cascade, and what it saved

**Tier 2 &middot; Routing and cascades** &nbsp;|&nbsp; ~15 minutes &nbsp;|&nbsp;
No assistant, no network &nbsp;|&nbsp; measured, not scored

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

## Step 4 &mdash; Sweep for your threshold

```bash
for t in 0.40 0.50 0.60 0.70 0.80 0.90; do
  sed -i "s/confidence < [0-9.]*/confidence < $t/" tools/cascade.py
  echo -n "$t  "; python3 tools/cascade.py | grep "your cascade"
done
```

Find the **cheapest threshold that still reaches 100%**. Write it down.

---

## Step 5 &mdash; Break it deliberately

Set the threshold to `1.01`, so every case escalates:

```bash
sed -i "s/confidence < [0-9.]*/confidence < 1.01/" tools/cascade.py
python3 tools/cascade.py
```

**Look at the cost column and compare it with always-strong's 960.** Write that number
down &mdash; it is the most important row in the lab.

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

One paste creates the sheet:

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

## Notice

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
