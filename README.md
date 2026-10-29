# USD/THB Forecasting
**Chetsada Phummee** | Academic project

Compare classical baselines, gradient boosting, neural models, and pretrained time-series models for next-observation USD/THB forecasting. Two notebooks separate the univariate and multivariate experiments.

- [Univariate notebook](USD_THB_Univariate.ipynb): exchange-rate history only; includes five-step forecasts from 30 September 2026.
- [Multivariate notebook](USD_THB_Multivariate.ipynb): exchange-rate history, interest-rate differential, inflation differential, Brent oil prices, and VIX. Random Walk, Chronos, and TimesFM remain univariate reference models.

## Experimental design
689 observations from 2 January 2024 through 30 September 2026. Use 501 actual observations per rolling training window, 103 validation targets, and 85 test targets. XGBoost and neural models refit from scratch at every origin; scalers fit inside each window. Choose hyperparameters and overall model using aggregate Validation RMSE, then keep settings fixed during Test. Earlier actuals can enter later windows once observed.

Models: Random Walk, XGBoost, RNN, LSTM, Transformer, Chronos Bolt, and TimesFM. The two foundation models use pinned pretrained checkpoints without fine-tuning.

## Recorded results
Saved notebook outputs, not a fresh full rerun of the public copies:

| Experiment / model | Validation RMSE | Test RMSE |
|---|---:|---:|
| Univariate Random Walk | 0.181736 | 0.125773 |
| Univariate RNN (validation-selected) | 0.180181 | 0.128986 |
| Multivariate Random Walk | 0.181736 | 0.125773 |
| Multivariate RNN (validation-selected) | 0.178708 | 0.129301 |

Units are THB per USD. The selected RNN does not beat Random Walk on Test RMSE in either experiment. Other models' test rankings do not replace the validation selection. Small observed differences do not establish statistical significance or trading profitability.

## Run
Open either notebook in a fresh Google Colab environment with a GPU recommended, or use a local Python environment. Run its installation cell, then the remaining cells in order. Full rolling hyperparameter searches may take substantial time. `RUN_MODE="smoke"` provides a reduced execution check; it is not final evaluation. Set `RUN_FOUNDATION_MODELS=False` if you only want the non-foundation models. Pretrained model downloads require internet access.

The default `DATA_MODE="snapshot"` uses the embedded supplied-data snapshot. No personal Drive links are required. Optional path/upload modes allow replacement workbook data. No model checkpoints or result files are written by these notebooks; save the notebook to preserve displayed outputs.

## Data provenance and limits
The embedded snapshot comes from the workbook supplied for this academic project. The original upstream sources and redistribution licenses are not independently documented here; no open-data license or ownership of the underlying data is asserted. Source macro publication timestamps and revision histories are absent, so historical row lags alone do not establish real-time feature availability. Brent/VIX missing or invalid values use past-only forward filling, never future interpolation.

The test period was revised after earlier 2026 results had been inspected, so it is not an untouched final holdout. Foundation-model pretraining overlap is unverified. Business-day labels in the five-step forecast omit weekends but do not adjust all source holidays. This study is not a production trading model or evidence of investment performance.

## Implementation
Both public notebooks passed an execution smoke check on Python 3.13.7 with PyTorch 2.13.0 and XGBoost 3.3.0: two targets per split for Random Walk, XGBoost, RNN, LSTM, and Transformer. Foundation models were disabled for this check; their retained outputs come from the original full run. Smoke results are not replacements for the recorded comparison table.

Python, pandas, NumPy, scikit-learn, XGBoost, PyTorch, Chronos, and TimesFM. The notebook installation cells specify the intended dependency ranges and model revisions; requirements are not a fully locked environment. Source academic notebooks remain unchanged outside this repository.
