# Lab 2 &mdash; The cascade, and what it saved

**Tier 2 &middot; Routing and cascades** &nbsp;|&nbsp; ~15 minutes &nbsp;|&nbsp;
No assistant, no network &nbsp;|&nbsp; measured, not scored

## The situation

Twenty triage cases. Two "models" that are ordinary Python functions with a known
accuracy and a known price, so the arithmetic is the only thing under discussion and
every row in the room is comparable.

Your job is the **gate**: the rule that decides when the cheap answer is not good
enough.

## Do

1. **Run it unmodified.**

   ```bash
   cd meridian-freight
   python3 tools/cascade.py
   ```

   You escalate nothing, so you get the cheap model's accuracy at the cheap model's
   price, plus a message telling you exactly that.

2. **Read the two baselines.** Always-cheap: 70% accurate, cost 80. Always-strong:
   100% accurate, cost 960. Everything you do now lives between those two rows.

3. **Write the blunt gate.** In `tools/cascade.py`, replace the body of `decide()`:

   ```python
   return confidence < 0.6
   ```

   Run it again.

4. **Find your threshold.** Try several. A quick sweep:

   ```bash
   for t in 0.40 0.50 0.60 0.70 0.80 0.90; do
     sed -i "s/confidence < [0-9.]*/confidence < $t/" tools/cascade.py
     echo -n "$t  "; python3 tools/cascade.py | grep "your cascade"
   done
   ```

   Find the cheapest threshold that still reaches 100%.

5. **Now break it deliberately.** Set the threshold to `1.01` so every case escalates.
   Look at the cost column and compare it with always-strong. **Write that number
   down** &mdash; it is the most important row in the lab.

6. **Check the assumption the whole pattern rests on.** Does the cheap model's
   confidence actually track whether it is right?

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

## Record

```markdown
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
```

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
