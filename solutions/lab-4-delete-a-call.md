# Lab 4 — reference answer

## The deterministic half already exists

```python
from meridian import report, store, tariffs
rows = report.exceptions(store.load(), tariffs.load())
print(report.summarise(rows))
```

```
{'total_consignments_with_exceptions': 16,
 'counts': {'MF-01': 2, 'MF-02': 2, 'MF-03': 8, 'MF-04': 3, 'MF-05': 0, 'MF-06': 2}}
```

Exactly the same numbers every time, in milliseconds, for zero tokens.

**`report.summarise()` was already in the codebase.** Somebody wrote the deterministic version, and
somebody else later wrote a prompt that duplicates it. That is not a story about AI — it is an
ordinary discoverability failure, and it is why the first question in this lab is "does this already
exist?"

## Grouping by depot too, if you want to extend it

```python
import collections
by_depot = collections.Counter(r["destination"] for r in rows)
for depot, n in by_depot.most_common():
    print("%-5s %2d" % (depot, n))
```

## The numbers

| | est. tokens | wall time | 3 runs identical? |
|---|--:|---|:--:|
| prompt as it stands | ~1,900 per run | 4–15 s | **no — 3 different groupings** |
| Python | 0 | <0.1 s | yes |
| short prompt, counts supplied | ~450 | 3–8 s | groupings yes, wording no |

## The argument is the variance column

The token saving is real and it is not the point. **Three model runs give three groupings; three
Python runs give one.** You were paying a premium for non-determinism in arithmetic — and nobody
would choose that if it were put to them as a choice. It never is. It arrives as "let's use AI for
the morning report".

## What to keep for the model

Not the counting. The judgement:

```text
Here are this morning's exception counts:
  MF-01: 2   MF-02: 2   MF-03: 8   MF-04: 3   MF-05: 0   MF-06: 2

Eight unrouted consignments is unusual for a Tuesday. Suggest what the duty
supervisor should work first and why, in under 80 words. If the counts alone
do not justify a recommendation, say so rather than guessing.
```

That question genuinely needs a model: it depends on the day, the depot, who is on shift and what is
unusual. It is also a fifth of the tokens, because you stopped asking it to count.

**This is not an argument against using a model.** It is an argument for sending it the part that is
actually a judgement.

## The stretch — `scripts/nightly_sync.py`

At least four problems, and only some are about money:

1. **A hardcoded bearer token in source.** The one you never want an assistant to learn as a pattern
   from your codebase.
2. **`os.system` with string interpolation** — a shell injection waiting for a filename with a space
   in it.
3. **Money in a float**, `weight_g * 0.042`, against the runbook's *Money* section.
4. **It re-implements pricing** — a third copy, after `rating.py` and `manifest.py`, and this one
   does not even use the tariff.

Also: `datetime.now()` in a nightly job that is supposed to be re-runnable, which is the *Clocks*
rule.
