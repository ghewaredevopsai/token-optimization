# Lab 3 — worked example

                              est. tokens   % of baseline   answer still right?
baseline (naive)              37,476        100%            no
cut 1: no data files          15,800        42%             yes
cut 2: six lines, not docs/   4,100         11%             yes
cut 3: new chat               3,600         10%             yes
cut 4: manifest.py alone      1,300         3%              NO

Largest cut that kept the answer: 90%
The cut that broke it, and why: cut 4 - with only manifest.py there is nothing to
compare against and no rule to settle it, so it reported the file looked fine.

Did ctxmeter refuse a percentage? yes
Why? the data share went 64% -> 0%, so its error stops cancelling between the
two numbers and a single headline percentage is not supportable.

What the meter could NOT see on any of these runs:
the vendor system prompt, the editor's own retrieval, and the two files the agent
opened by itself after I attached the first one. Every number above is a floor.
