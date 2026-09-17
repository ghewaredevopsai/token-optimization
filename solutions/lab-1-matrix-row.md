# Lab 1 — reference answer

> **Read this after you have written your prediction down.** The whole lab is the gap between the
> two, and you are the only person who would know if you skipped it.

## The aggregate table

| task | fast | default | reasoning |
|---|---|---|---|
| explain-code | kept 3/4 · 0.4 cr | kept 4/4 · 1.1 cr | kept 4/4 · 6.4 cr |
| write-docstring | kept 4/4 · 0.3 cr | kept 2/4 · 0.9 cr | kept 4/4 · 5.6 cr |
| mechanical-rename | kept 3/4 · 0.5 cr | kept 4/4 · 1.2 cr | kept 3/4 · 7.5 cr |
| write-unit-test | kept 0/4 · 0.7 cr | kept 3/4 · 1.6 cr | kept 3/4 · 8.5 cr |
| find-subtle-bug | kept 1/4 · 0.6 cr | kept 3/4 · 1.7 cr | kept 4/4 · 9.5 cr |
| multi-file-feature | kept 1/4 · 1.1 cr | kept 3/4 · 2.9 cr | kept 4/4 · 11.9 cr |

**Synthetic data, labelled as such in the file.** Reason about the shapes; replace it with your own
runs before quoting a row.

## What it says

**The fast tier is fine for explaining, documenting and renaming** — 3 or 4 kept out of 4 at roughly
a fifteenth of the reasoning tier's cost. Most teams' highest-volume work is here, which is where
the money is.

**The fast tier collapses on tests.** 0 of 4 kept on `write-unit-test`. Not "slightly worse" —
useless, and cheap uselessness is not cheap.

**`write-unit-test` is the clearest value row.** Default keeps 3/4 at 1.6 credits; reasoning keeps
3/4 at 8.5. Five times the price for no improvement.

**The reasoning tier loses on `mechanical-rename`** — 3/4 against default's 4/4, at six times the
cost. It over-thinks and touches files nobody asked about. Almost nobody predicts this: people
expect the expensive model to be weakly better everywhere.

**`write-docstring` default at 2/4 is noise.** Four runs cannot separate it from fast's 4/4 or
reasoning's 4/4 on a task all three can do. Where a cell looks anomalous and the effect is small,
n=4 is the explanation.

## A worked defensible row

```markdown
For unit tests and multi-file changes we use the default tier,
because in our runs it kept 3 of 4 diffs at about a fifth of the
reasoning tier's cost, which kept the same 3 of 4.
We revisit this when the model roster changes or when our kept-rate
on tests drops below 3 in 4 for a fortnight.
```

Three properties make it defensible: it names the tasks it applies to, it cites a number rather than
a reputation, and **it has an expiry**. A selection rule with no review trigger is how a team ends
up on last year's defaults.

Compare with what people usually write: *"Use the reasoning model for anything important."*
"Important" is every task, to the person doing it — so this is a rule that spends the frontier rate
on docstrings.

## Most people get one of three

That number is about you, not about the models, and it is the reason the prediction step exists. The
technique transfers to any decision where somebody already has an opinion: **write the guess down
before you open the evidence**, and you find out whether you learned something or merely confirmed
something.
