# Lab 1 &mdash; One matrix row you would defend

**Tier 1 &middot; Model selection** &nbsp;|&nbsp; ~16 minutes &nbsp;|&nbsp;
Works with any seat, including Auto-only &nbsp;|&nbsp; measured, not scored

## The situation

Everybody in this room already has an opinion about which model to use for what.
This lab is designed to find out whether that opinion survives contact with data
&mdash; and the only way to find that out honestly is to **write it down first**.

## Step 1 &mdash; Predict. Sixty seconds. Do this before you open anything.

Name your team's **three commonest AI-assisted tasks**. For each, predict which
archetype wins on *value for money*: **fast**, **default** or **reasoning**.

```markdown
task 1: ____________________  my prediction: ______
task 2: ____________________  my prediction: ______
task 3: ____________________  my prediction: ______
```

Write it in the record sheet now. **Do not skip ahead and then fill this in.** The
whole lab is worthless if you do, and you are the only person who would know.

## Step 2 &mdash; Open the data

```bash
python3 - <<'PY'
import json, collections
runs = json.load(open("data/model-runs.json"))["runs"]
agg = collections.defaultdict(list)
for r in runs:
    agg[(r["task_type"], r["archetype"])].append(r)
print("%-20s %-10s %5s %7s %8s" % ("task", "archetype", "kept", "credits", "seconds"))
for (task, arch), rs in sorted(agg.items()):
    kept = sum(1 for r in rs if r["diff_kept"])
    cr = sum(r["approx_credits"] for r in rs) / len(rs)
    secs = sum(r["wall_seconds"] for r in rs) / len(rs)
    print("%-20s %-10s %2d/%-2d %7.1f %8.0f" % (task, arch, kept, len(rs), cr, secs))
PY
```

**Read `data/model-runs.json`'s provenance block first.** It says, in the file, that
these runs are **synthetic** &mdash; illustrative reference data, not a recorded
measurement. They exist so this lab runs offline on a locked-down seat and so every
room reasons about the same numbers. Treat the *shapes* as worth thinking about and
**replace the file with your own recorded runs before quoting any row as evidence.**

## Step 3 &mdash; Score your predictions

How many of your three survived? Most people get one.

## Step 4 &mdash; One live run

Pick **one** task type. Run it once, for real, on whatever your seat offers:

> Add a `--depot` option to `python3 -m meridian exceptions` that filters the report
> to one destination depot.

Record wall time, whether you kept the diff, and approximate credits if your seat
shows them. **One run.** You are not benchmarking; you are checking that the shape of
the reference data is not absurd for your setup.

## Step 5 &mdash; Write the row

One row of a model-per-task guide that you would defend to a colleague who disagrees:

```markdown
For ____________________ tasks we use the ______ tier,
because ____________________________________________
We revisit this when ________________________________
```

The last line is not optional. A selection rule with no expiry is how a team ends up
on last year's defaults.

## Record

```markdown
# Lab 1

predictions:  task 1 ______  task 2 ______  task 3 ______
survived the data: ___ of 3

The prediction I was most confident about, and what the data said:
____________________________________________________________

My live run: task ____________ time ____ kept? ___ credits ______

My defensible row:
____________________________________________________________
```

```bash
git add lab-1-record.md && git commit -m "lab 1: predict, then look"
```

## Notice

- **Pre-registering the guess is the entire technique**, and it transfers far beyond
  models. It costs sixty seconds and it converts "I knew it" into evidence or into a
  surprise, both of which are useful. Skipping it produces neither.

- **The reasoning tier loses somewhere in this data**, and it is usually on the
  mechanical task &mdash; it over-thinks a rename and touches files nobody asked it
  to. People expect the expensive model to be weakly better everywhere. It is not.

- **Four runs per cell is thin**, and the file says so. Where two archetypes are
  close, you cannot separate them at n=4 and should not pretend to. Where one is four
  times the cost for the same kept-rate, n=4 is plenty.

- **You did not re-run three models yourself, on purpose.** That lab exists in the
  Copilot as Assistant module and works well. Here the interesting thing was never
  the runs &mdash; it was your prediction.

## Stretch

Sort the data by credits per kept diff rather than by credits. Does any archetype
change position? That ratio &mdash; what you paid for work you actually shipped
&mdash; is closer to what your finance team means by cost than anything on the
billing page.
