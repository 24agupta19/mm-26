# March Madness 2026 Prediction Model
**UW-Madison Sports Analytics Club — March Madness Data Challenge 2026**

## Overview

XGBoost classifier trained on 988 historical NCAA tournament games (2010–2025) to predict win probabilities for all 2,278 possible matchups among the 68-team 2026 field. The model outputs a probability (0–1) that the higher seed wins each matchup.

**Predicted champion: Michigan**
**Bold upset pick: Santa Clara over Kentucky (Round of 64)**

---

## Model

### Architecture
- **Algorithm:** XGBoost classifier (200 estimators, max depth 3, learning rate 0.05)
- **Training data:** 988 NCAA tournament games, 2010–2025
- **Cross-validation AUC:** 0.878 (5-fold)
- **Features:** 22 total (all expressed as higher seed minus lower seed differentials)

### Features

| Group | Features |
|---|---|
| Base Torvik (9) | adjoe, adjde, barthag, sos, WAB, ncsos, consos, Opp OE, Opp DE |
| Shooting (5) | eFG%, FTR, OR%, DR%, TO% |
| Consistency (2) | oe_std_resid, de_std_resid |
| Pre-tournament (5) | pre_adjoe, pre_adjde, pre_barthag, pre_sos, pre_WAB |
| Seeding (1) | seed_diff |

### Key Design Decisions

**Row flipping:** 50% of training rows are randomly flipped (seed 42) so the model learns from both team perspectives rather than always predicting from the winner's point of view.

**Residual consistency features:** Raw game-to-game variance in offensive/defensive efficiency was highly correlated with overall team quality (r = -0.798 with barthag). To fix this multicollinearity, efficiency variance was regressed against barthag using linear regression, and the residuals were used as features instead. This captures consistency *above or below what you'd expect given team quality*.

**Pre-tournament ratings:** Selection Sunday ratings from Torvik's time machine API capture teams at their true tournament-entry strength rather than full-season averages. Missing values (2013 file corrupted) fall back to season averages.

**Dropped features:** Tempo (adjt) was excluded due to excessive missing values in older Torvik files.

---

## Data Sources

| Source | Data |
|---|---|
| Kaggle | Tournament results, seeds, team IDs (2010–2025) |
| Torvik (`barttorvik.com`) | Season efficiency stats, shooting stats (fffinal), game-by-game logs (super_sked) |
| Torvik Time Machine | Selection Sunday ratings for each tournament year |

---

## Results

### Bracket Simulation (using submission predictions)
- **Champion:** Michigan
- **Final Four:** Michigan, Duke, Florida, Purdue
- **Elite Eight:** Michigan, Duke, Florida, Illinois, Arizona, Purdue, St. John's, Iowa St.
- **Notable upsets:** Santa Clara over Kentucky, Iowa over Clemson, Texas over BYU

---

## Reproducing Results

### Requirements
```
pandas
numpy
requests
xgboost
scikit-learn
joblib
```

### Data Setup
Download Torvik CSVs to `data/torvik/` and Kaggle files to `data/kaggle/`:
- `data/torvik/YEAR_team_results.csv` (2010–2026)
- `data/kaggle/MNCAATourneyCompactResults.csv`
- `data/kaggle/MNCAATourneySeeds.csv`
- `data/kaggle/MTeams.csv`

### Running
Open `model_clean.ipynb` and run all cells top to bottom. The notebook fetches fffinal and super_sked data directly from barttorvik.com at runtime. Output is saved to `submission_residual.csv`.

> **Note:** The submitted `submission.csv` was generated from a separately trained instance of the same model. Predictions may differ slightly on rerun due to differences in the data pipeline, but the model architecture and feature set are identical.
