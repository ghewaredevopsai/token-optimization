# Lab 3 — reference answer

## The four cuts

| | est. tokens | % of baseline | answer still right? |
|---|--:|--:|:--:|
| baseline (`naive.txt`) | ~37,500 | 100% | often no |
| cut 1 — no `data/*.json` | ~15,800 | 42% | usually |
| cut 2 — six lines, not `docs/` | ~4,100 | 11% | yes |
| cut 3 — new chat | ~3,600 | 10% | yes |
| cut 4 — `manifest.py` alone | ~1,300 | 3% | **no** |

**The target was 40%. The honest answer is about 90%**, and the answer got *better* on the way down
until cut 4 broke it.

## Why cut 2 improves the answer

Dropping `docs/` removes `tariff-2026-notes.md`, which is stale and says fuel applies to the base
alone — agreeing with the bug in `manifest.py`. Until you remove it, you are paying tokens to tell
the model something false.

**A wrong document in the bundle is worse than no document.** Cut 2 is that sentence with a number
attached, and it is the most quotable result in this module.

## Why cut 4 breaks it

With only `manifest.py` attached there is nothing to compare against. The model can see the fuel
calculation but not that `rating.py` does it differently, and not the rule that settles which is
right. It will usually say `manifest.py` looks fine.

**If nobody in your room broke the answer, nobody cut hard enough.** The cut that breaks it is the
most valuable line on the sheet, because it is the only one that tells you where the floor is.

## What `ctxmeter diff` does, and why

```
saved 34534 est. tokens
NOT printing a percentage: the 'data' share of the bundle moved
by more than 25 points, so the estimator's error does not cancel
between these two numbers. Re-run with --allow-mixed if you accept that.
before mix: data 64%, code 36%
after  mix: code 100%
```

`naive.txt` is about two-thirds data by tokens; `minimal.txt` is all code. The estimator's error is
roughly multiplicative and cancels across a before-and-after **of similar content** — and does not
cancel here.

**This is the tool working.** Passing `--allow-mixed` is fine; passing it without reading the
message is not. The refusal is a better lesson than the percentage would have been.

## The sentence that makes your number defensible

> Our 40% is measured on what we listed. It does not include the vendor's system prompt, the
> editor's retrieval, or files the agent opened on its own — so the real bundle is larger and the
> real saving is at least this much.

Somebody senior will ask. Have it ready.

## The stretch is the actual deliverable

A committed `tools/bundles/mine.txt`: the smallest set of paths that answers this class of question
every time. That file is reusable — it is the written-down answer to "what should I attach for this
kind of question", which otherwise gets re-derived by every person who asks it.
