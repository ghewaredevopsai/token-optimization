# Five labs, one number at a time

Every lab runs against [Meridian Freight
Desk](https://github.com/ghewaredevopsai/meridian-freight) &mdash; the same practice
repository the Prompt &amp; Context Engineering module uses. Clone it once.

## How these labs work

- **Measured, not scored.** No pass marks. Each lab records a number before and a
  number after, and the comparison is the result.
- **The record sheet is the deliverable.** Each lab ends with a committed
  `lab-N-record.md`. That sheet, not your memory of the session, is what makes a cost
  claim defensible to somebody who was not in the room.
- **Two labs need no assistant at all.** Labs 0 and 2 are pure arithmetic against
  supplied data, so they work on a locked-down machine and give every participant the
  same numbers.
- **The instruments do not agree, and are not meant to.** `ctxmeter` answers instantly
  and counts only what you listed. Your billing page is authoritative about money and
  arrives late and aggregated. **The counter proves the cut; the billing page proves
  the money; they never go in the same row of a table.**

## No git, or no visible credits?

Neither blocks a lab. If `git` is restricted, copy the folder instead of branching.
If your seat shows no credit figure &mdash; common on a company seat, where usage is
often visible only to an administrator &mdash; Lab 0 tells you to record that fact and
carry on. Every other measurement in this module is deliberately independent of it.

## The labs

| Lab | Tier | Time | What you do | What you record |
|---|---|:--:|---|---|
| [0](lab-0-count-your-own-bundle.md) | 0 | 6 min | Meter two bundles; find out what your seat shows you | Bundle sizes, turn growth, credits visible or not |
| [1](lab-1-one-matrix-row.md) | 1 | 16 min | Predict first, then read 72 recorded runs | How many of three predictions survived |
| [2](lab-2-the-cascade.md) | 2 | 15 min | Write the gate; find the threshold; then break it | Accuracy and cost at four thresholds |
| [3](lab-3-the-40-percent-audit.md) | 3 | 19 min | Four cuts, re-checking the answer after each | Tokens and correctness at every cut |
| [4](lab-4-delete-a-model-call.md) | 4 | 12 min | Replace the counting half of a prompt with Python | Tokens, time, and whether three runs agree |

## Getting started

```bash
git clone https://github.com/ghewaredevopsai/meridian-freight
cd meridian-freight
python3 tools/ctxmeter.py diff tools/bundles/everything.txt tools/bundles/naive.txt
```

You need **Python 3.11+**. Labs 1, 3 and 4 also want an assistant in your editor.
Nothing is installed and nothing reaches the network.

> **One file in the practice repo is synthetic and says so.** `data/model-runs.json`
> carries a provenance block declaring it illustrative reference data rather than a
> recorded measurement. Lab 1 uses it deliberately, and tells you to replace it with
> your own runs before quoting a row as evidence.
