# Football (Soccer) Betting Example

This mirrors the NCAA Basketball tutorial in `docs/tutorials/ncaab.rst`,
adapted for football (soccer) using **SportFlow** (`sflow`).

`data/FOOTBALL_game_scores_1g.csv` is **real** Turkish Super Lig data:
694 matches from the 2023-24 and 2024-25 seasons, with real final
scores and real Bet365 Asian handicap lines (`line`, negative favors
the home team). The `over_under` column is a fixed 2.5 total, since
that is the standard market this data's odds are quoted against
rather than a per-match line. Source: [Club Football Match Data
2000-2025](https://github.com/xgabora/Club-Football-Match-Data-2000-2025)
(MIT licensed), itself aggregated from
[football-data.co.uk](https://www.football-data.co.uk/).

The target is `won_on_spread`: did the home team cover the line?

Run from this directory (train on everything before the date, test
on everything after):

```
sflow --pdate 2025-03-01
```

Predict mode scores fixtures on/after the date with the saved model
(run the training command above first):

```
sflow --predict --pdate 2025-03-01
```

On this real data, ROC AUC lands around 0.50-0.57 on the test set,
in the same range the NCAAB tutorial reports — a real bookmaker's
line is priced to be close to a coin flip to cover, so getting any
edge at all is the hard part.

Depending on the date and how the grid search happens to land, the
best model can be `RF`, `XGB`, or the blended stack of both (`BLEND`).
Predict mode handles all three: when a blend wins, AlphaPy needs the
underlying RF/XGB estimators (not just the blending regression) to
score new fixtures, which the `BlendEstimator` wrapper in
`alphapy/model.py` takes care of.
