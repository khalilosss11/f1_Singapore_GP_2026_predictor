# F1 Race-Winner Predictor: 2026 Singapore Grand Prix

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR_USERNAME/f1-singapore-gp-2026-predictor/blob/main/singapore_gp_predictor.ipynb)

A machine-learning model that predicts the winner and finishing order of the 2026 Singapore Grand Prix from qualifying, recent form, circuit history and championship standing.

## Result
Predicted before the race, using the real qualifying result.

| | Verstappen | Leclerc | Antonelli | Hamilton |
|---|---|---|---|---|
| Win probability | 57.5% | 13.8% | 5.0% | 1.6% |

The actual podium was Verstappen, Leclerc, Antonelli, the model's top three. The finishing-order model predicted VER, LEC, ANT, NOR, HAM, PIA; the race finished VER, LEC, ANT, NOR, PIA, HAM.

**Honest reading:** Verstappen won from pole, which a simple "pole wins" rule also gets right. The more informative calls were Antonelli over Hamilton for third, and the recoveries of Russell (started 22nd, predicted 7th, finished 9th) and Hadjar (started 21st, predicted 10th, finished 10th). This is one race, so it illustrates the model; it doesn't prove it.

## How it works
- **Data:** Kaggle F1 history (1950-2024) plus FastF1 data for 2025-2026, joined on driver code, team and race name.
- **Features:** qualifying and grid position, recent form (last 5 races, driver and team), circuit history, championship standing. All form features are shifted so a race never sees its own result.
- **Models:** a calibrated XGBoost classifier (win probability) and an XGBoost ranker (finishing order).
- **Validation:** trained on seasons before 2022, tested on 2022-2024, split by season (never randomly).

## Evaluation (held-out 2022-2024)
| | Top-1 accuracy |
|---|---|
| Model | [from Cell 10] |
| "Always pick pole" baseline | [from baseline cell] |

## Reproducibility
Set `CUTOFF_DATE` and `CIRCUIT_HISTORY_NAME` in Cell 0. Only races before the cutoff are used, and the FastF1 data is stored in `fastf1_2025_2026_snapshot.csv`, so results don't change as new races are added.

## Running it
Open it in Colab with the badge above. Download the Kaggle CSVs (`races`, `results`, `drivers`, `constructors`, `qualifying`) from the dataset linked below and upload them next to the notebook. Qualifying results are entered by hand in Cell 12.

## Limitations
- Grid position dominates the model's decisions, so it mostly follows the starting order.
- No weather, strategy or reliability inputs, and retirements aren't modelled.
- Sprint points aren't counted in the championship feature.
- Audi and Cadillac have no history, so they fall back to team and field averages.
- The Kaggle dataset ends in 2024 and isn't updated.

## Data and credits
- [Formula 1 World Championship dataset](https://www.kaggle.com/datasets/rohanrao/formula-1-world-championship-1950-2020) by Rohan Rao (Vopani), CC0
- [FastF1](https://github.com/theOehrly/Fast-F1)

Educational project. Not affiliated with Formula 1, and not betting advice.
