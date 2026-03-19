# March Madness 2026

**Author:** Arnav Gupta

XGBoost model predicting win probabilities for all possible NCAA tournament matchups.

## Model
- Trained on tournament games from 2010-2025
- 22 features: Torvik efficiency stats, shooting stats, game-to-game consistency residuals, pre-tournament ratings, seed differential
- 5-fold CV AUC: 0.878

## Data Sources
- Kaggle: tournament results, seeds, team IDs
- Barttorvik: season stats, shooting stats, game logs, Selection Sunday time machine ratings

## Usage
Run `model_clean.ipynb`. Requires Torvik CSVs in `data/torvik/` and Kaggle files in `data/kaggle/`.

## AI Usage
Used Claude (Anthropic) for debugging, assistance with code as well as data pipeline development 