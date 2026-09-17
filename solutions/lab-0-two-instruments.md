# Lab 0 — reference answer

## The numbers

| | est. tokens |
|---|--:|
| `everything.txt` | ~46,700 |
| `naive.txt` | ~37,500 |
| difference | ~9,200 |

The two files that dominate are `data/tariff.json` (~10,100, about a fifth of the repository on its
own) and `data/consignments.json` (~9,700). **Neither has anything to do with how the desk prices
freight.** They are the freight. Every "just use my codebase" request sends them.

## The turn table

`ctxmeter turns --turns 10` shows the standing context re-sent in full every turn, with the
conversation growing on top. Turn 10 sends considerably more than turn 1 although you typed less —
and that is not a defect, it is the shape of every agent conversation.

It is also why "start a new chat" sits second from the bottom of the Tier 4 lever ladder: free,
instant, and nobody does it.

## Looking for a credit figure — all three outcomes are findings

1. **A number, with a timestamp.** Note how stale it is. It will not be live.
2. **A monthly aggregate and nothing for today.** Common. Enough for trend, useless for a
   before-and-after inside one session.
3. **Nothing at all.** Very common on a company seat, where usage is often visible only to an
   administrator.

If you got (3), you have discovered the constraint this module is designed around. **Three of the
five labs need no credit visibility whatsoever**, and that is deliberate rather than a compromise.

## What `calibration.md` says

`ctxmeter`'s constants were fitted against two real BPE tokenizers over every file in the practice
repo. Measured error:

| content | mean error | worst |
|---|--:|--:|
| code | +0.3% | +15.4% |
| data | −0.4% | −3.8% |
| prose | −0.8% | −2.4% |
| **whole repo** | **+0.1%** | |

Read it as: **reliable in aggregate, worth about ±15% on any single code file.** Use it for bundles
and deltas, not to pronounce on one file.

The first draft of the estimator used the familiar characters÷4 rule and over-counted by 26%. It
would still have been fine for a *ratio* — the bias largely cancels between a before and an after —
and indefensible the moment anyone read an absolute out loud. That is why the tool makes you pass
`--absolute`.

## The thing it can never see

The vendor's hidden system prompt, the editor's own retrieval and repository indexing, and whatever
the agent decides to open once it starts working. **Every number is a floor.**

That sentence is the one to have ready when somebody senior asks how you know.
