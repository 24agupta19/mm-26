# March Madness 2026

XGBoost model predicting win probabilities for all 2,278 possible NCAA tournament matchups.

## Model
- Trained on 988 tournament games (2010–2025)
- 22 features: Torvik efficiency stats, shooting stats, game-to-game consistency residuals, pre-tournament ratings, seed differential
- 5-fold CV AUC: 0.878

## Data Sources
- Kaggle: tournament results, seeds, team IDs
- Barttorvik: season stats, shooting stats, game logs, Selection Sunday time machine ratings

## Usage
Run `model_clean.ipynb` top to bottom. Requires Torvik CSVs in `data/torvik/` and Kaggle files in `data/kaggle/`.

## Acknowledgements
Built with assistance from Claude 