# Solutions

Reference answers for all five labs. **Look whenever you want** — nothing here is a spoiler, because
none of these labs is scored.

Two of the five labs are pure arithmetic against supplied data, so their answers are exact and
everyone's should match. The other three depend on an assistant, so the answers below describe what
usually happens and why, not what must happen.

| File | What is in it |
|---|---|
| [`lab-0-two-instruments.md`](lab-0-two-instruments.md) | The bundle and turn numbers, and the three things that can happen when you look for a credit figure |
| [`lab-1-matrix-row.md`](lab-1-matrix-row.md) | The full aggregate table, what it actually says, and a worked defensible row |
| [`lab-2-cascade.md`](lab-2-cascade.md) | The gate, the whole threshold sweep, and the row that costs more than doing nothing |
| [`lab-3-audit.md`](lab-3-audit.md) | The four cuts with numbers, and the cut that breaks the answer |
| [`lab-4-delete-a-call.md`](lab-4-delete-a-call.md) | The deterministic replacement, and the shorter prompt for the half that needs judgement |

## Exact numbers you should reproduce

| Measurement | Value |
|---|---|
| `everything.txt` | ~52,000 est. tokens |
| `naive.txt` | ~38,400 |
| `minimal.txt` | ~2,200 |
| `cascade.py`, no gate | 70% accuracy, cost 80 |
| always-strong | 100%, cost 960 |
| threshold 0.60–0.70 | **100%, cost 368, 6 of 20 escalated** |
| threshold 1.01 | 100%, **cost 1040** |
| confidence when right / wrong | 0.85 / 0.53 |

## One warning about Lab 1

`data/model-runs.json` in the practice repo is **synthetic**, and says so in its own provenance
block. It exists so the lab runs offline on a locked-down seat and so every room reasons about the
same table. The shapes are plausible; **no row is an observation.** Replace it with your own recorded
runs before quoting any of it as evidence.
