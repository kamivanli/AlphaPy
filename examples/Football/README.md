# Football (Soccer) Betting Example

This mirrors the NCAA Basketball tutorial in `docs/tutorials/ncaab.rst`,
adapted for football (soccer) using **SportFlow** (`sflow`).

`data/FOOTBALL_game_scores_1g.csv` is a **synthetic** two-season dataset
(12 fictional Turkish Super Lig-style clubs, double round robin) with
realistic Poisson-distributed scorelines, a betting line (negative
favors the home team) and an over/under total. Swap in a real
results+odds dataset with the same columns
(`season,date,away.team,away.score,home.team,home.score,line,over_under`)
for genuine predictions.

The target is `won_on_spread`: did the home team cover the line?

Run from this directory (train on everything before the date, test
on everything after):

```
sflow --pdate 2024-11-15
```

Predict mode scores fixtures on/after the date with the saved model
(run the training command above first):

```
sflow --predict --pdate 2024-11-15
```

Because the synthetic line is generated directly from each match's
true expected goal difference, covering it is close to a coin flip
by construction (ROC AUC around 0.45-0.55 on the test set) — this
mirrors how an efficient real betting market behaves, and is the
same conclusion the NCAAB tutorial reaches with real data.
