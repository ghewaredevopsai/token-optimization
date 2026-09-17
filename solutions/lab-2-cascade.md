# Lab 2 — reference answer

## The gate

```python
def decide(answer: str, confidence: float, case: dict) -> bool:
    """Escalate when the cheap model is not confident enough to be trusted.

    0.6 is not a guess: it is where the sweep below stops buying accuracy.
    """
    return confidence < 0.6
```

## The whole sweep

Reproducible — these are exact:

| threshold | accuracy | cost | escalated |
|--:|--:|--:|--:|
| 0.40 | 70% | 80 | 0 |
| 0.50 | 75% | 128 | 1 |
| **0.60** | **100%** | **368** | **6** |
| 0.65 | 100% | 368 | 6 |
| 0.70 | 100% | 368 | 6 |
| 0.80 | 100% | 560 | 10 |
| 0.90 | 100% | 800 | 15 |
| 1.01 | 100% | **1040** | 20 |

Baselines: always-cheap 70% at 80, always-strong 100% at 960.

## What the table says

**0.60 is the answer: the strong model's accuracy for 38% of its price.** A 62% saving with nothing
given up. That is a bigger saving than any prompting technique in either module.

**0.60 to 0.70 is a plateau**, which matters more than the single best number. A threshold sitting
in the middle of a flat region is robust; one perched on a cliff edge is not. If your production
gate needs to be exactly 0.617 to pay off, you have fitted to your sample.

**1.01 is the row to write down.** Escalate everything and you pay **1040 against always-strong's
960**. Accuracy is a perfect 100%, so nothing about the answers would ever tell you. You bought the
cheap answer twenty times and threw it away twenty times. **Only the cost column reveals it** — which
is the argument for having a cost column at all.

**0.80 and 0.90 are the quiet waste.** Still 100%, but 1.5x and 2.2x the cost of 0.60 for nothing.
A gate that is merely "cautious" looks responsible and is pure spend.

## The check almost nobody does

```
when right: 0.85 avg over 14
when wrong: 0.53 avg over 6
```

Confidence separates cleanly, so a threshold between them can exist. **This is the assumption the
whole pattern rests on**, and it is worth checking before designing around it: if those two numbers
were equal, the gate would be a random sampler that costs money, and the cascade would be worse than
either model alone.

A cheap model that is *confidently* wrong cannot be cascaded. It can only be replaced.

## The stretch

Set `COST_STRONG_MINOR = 12` (three times the cheap model rather than twelve). Escalating everything
now costs 320 against always-strong's 240 — the trap is still there, but the *saving* at 0.60 shrinks
to about 33%. Somewhere below a 3x price ratio the cascade stops being worth its complexity at all.

**That ratio is what to carry to work, not the threshold.** The threshold is a property of these
twenty cases; the break-even ratio is a property of the pattern.
