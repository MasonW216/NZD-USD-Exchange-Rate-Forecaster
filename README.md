# NZD/USD Exchange-Rate Forecasting

This project builds and evaluates forecasting models for the NZD/USD exchange rate using historical daily exchange-rate data.

The goal is not only to test forecasting models, but to follow a realistic time-series modeling workflow: data cleaning, transformation, baseline forecasting, stationarity analysis, ARIMA modeling, supervised machine-learning forecasting, and out-of-sample model evaluation.

## Project overview

The dataset uses the FRED `DEXUSNZ` exchange-rate series, which represents:

> U.S. dollars per 1 New Zealand dollar

This means higher values indicate a stronger New Zealand dollar relative to the U.S. dollar, while lower values indicate a weaker New Zealand dollar relative to the U.S. dollar.

The project evaluates whether ARIMA-style models and supervised machine-learning models can improve forecast accuracy compared with simple baseline forecasts.

## Repository structure

```text
NZD-USD-Exchange-Rate-Forecaster/
│
├── data/
│   ├── raw/
│   │   └── USD to NZD Exchange Rate.csv
│   └── processed/
│       └── cleaned/transformed NZD/USD dataset
│
├── notebooks/
│   ├── 01_data_audit_and_transformation.ipynb
│   ├── 02_baseline_forecasting.ipynb
│   ├── 03_stationarity_and_autocorrelation.ipynb
│   ├── 04_arima_modeling.ipynb
│   ├── 05_machine_learning_forecasting.ipynb
│   └── 06_project_summary.ipynb
│
├── README.md
└── requirements.txt
```

## Workflow

### 1. Data audit and transformation

The raw exchange-rate data was inspected for structure, date ordering, duplicate dates, missing values, and plausibility.

Key steps included:

* Converting observation dates to datetime format
* Sorting the data chronologically
* Removing missing exchange-rate observations
* Creating a log exchange-rate series
* Creating daily log returns
* Saving a cleaned processed dataset for modeling

The project keeps the raw data unchanged and saves transformed data separately.

### 2. Baseline forecasting

Simple baseline models were created before fitting more complex models.

The main baseline approaches were:

* Naïve forecast: use the previous exchange-rate value as the next forecast
* Drift forecast: adjust the previous value by the average historical change

The baseline models established a minimum performance threshold that ARIMA and machine-learning models needed to beat.

### 3. Stationarity and autocorrelation analysis

The project analyzed the exchange-rate level, log exchange-rate level, and daily log returns using visual inspection, autocorrelation plots, and Augmented Dickey-Fuller tests.

Main findings:

* The raw exchange-rate level was highly persistent and likely nonstationary.
* The log exchange-rate level was also likely nonstationary.
* Daily log returns appeared stationary.
* Daily returns showed weak autocorrelation, suggesting limited linear predictability from recent returns.

These findings informed the ARIMA modeling strategy.

### 4. ARIMA modeling

ARIMA-style models were fitted to the log NZD/USD exchange-rate series.

The initial model was:

```text
ARIMA(0, 1, 0)
```

This model acts as a random-walk model on the log exchange rate.

Additional ARIMA specifications were tested, including:

```text
ARIMA(1, 1, 0)
ARIMA(0, 1, 1)
ARIMA(1, 1, 1)
ARIMA(2, 1, 0)
ARIMA(0, 1, 2)
```

In the fixed-origin multi-step test setup, the best ARIMA model produced only a very small improvement over the random-walk ARIMA model and did not outperform the drift baseline.

### 5. Supervised machine-learning forecasting

The time series was converted into a supervised learning dataset by creating a next-period target variable:

```text
target_nzd_usd_next
```

Lagged and rolling features were created from the exchange-rate level and return series.

Feature examples included:

* Current exchange rate
* Daily log return
* Lagged exchange-rate values
* Lagged returns
* Rolling means
* Rolling standard deviations

The models tested were:

* Linear regression
* Ridge regression
* Random forest regression

The supervised models were evaluated using a one-step-ahead forecasting setup.

## Key results

### Fixed-origin ARIMA test-period comparison

| Model                                |      MAE |     RMSE |
| ------------------------------------ | -------: | -------: |
| Naïve baseline                       | 0.029734 | 0.033495 |
| Drift baseline                       | 0.020696 | 0.025189 |
| ARIMA(0, 1, 0)                       | 0.029734 | 0.033495 |
| Best ARIMA candidate: ARIMA(1, 1, 1) | 0.029702 | 0.033464 |

The drift baseline performed best in the fixed-origin ARIMA test setup.

### One-step-ahead machine-learning comparison

| Model             |      MAE |     RMSE |
| ----------------- | -------: | -------: |
| Naïve baseline    | 0.002688 | 0.003580 |
| Drift baseline    | 0.002686 | 0.003579 |
| Linear regression | 0.002728 | 0.003627 |
| Ridge regression  | 0.002726 | 0.003626 |
| Random forest     | 0.003458 | 0.004415 |

The supervised machine-learning models did not outperform the one-step-ahead baseline forecasts.

## Important evaluation note

The ARIMA and machine-learning notebooks use different forecasting setups.

The ARIMA notebook uses a fixed-origin multi-step forecast, where models are trained up to the training cutoff and then forecast across the full test period.

The machine-learning notebook uses a one-step-ahead supervised learning setup, where each row uses current and past information to predict the next observed exchange rate.

Because of this difference, ARIMA and machine-learning metrics should not be compared directly without considering the forecasting setup. Within each setup, models are compared against appropriate baselines.

## Main conclusion

Across the project, simple baseline models remained difficult to beat.

The NZD/USD exchange-rate level was highly persistent, meaning the current exchange rate was usually the strongest predictor of the next observed exchange rate. Stationarity and autocorrelation analysis showed that daily returns had weak linear dependence, which helped explain why more complex models struggled to add predictive value.

The ARIMA models produced only small improvements over a random-walk specification and did not beat the drift baseline in the fixed-origin test setup. The supervised machine-learning models also failed to outperform one-step-ahead naïve and drift baselines.

Overall, the project demonstrates an important forecasting principle:

> More complex models are only useful if they improve out-of-sample performance against strong baselines.

## Skills demonstrated

This project demonstrates:

* Time-series data cleaning
* Missing-value handling
* Log transformations
* Return calculation
* Exploratory time-series visualization
* Baseline forecasting
* MAE and RMSE evaluation
* Autocorrelation analysis
* Augmented Dickey-Fuller stationarity testing
* ARIMA modeling
* Supervised learning feature engineering
* Linear regression
* Ridge regression
* Random forest regression
* Model comparison against baselines
* Interpretation of forecasting results

## Technologies used

* Python
* pandas
* NumPy
* Matplotlib
* statsmodels
* scikit-learn
* Jupyter Notebook

## Resume-ready summary

Built an end-to-end NZD/USD exchange-rate forecasting project using historical daily exchange-rate data. Cleaned and transformed raw time-series data, created baseline forecasts, tested stationarity and autocorrelation, fitted ARIMA models, engineered lagged and rolling features for supervised learning, and evaluated linear, regularized, and tree-based models against naïve and drift baselines. Found that simple baselines outperformed more complex models, highlighting the importance of rigorous out-of-sample evaluation in financial forecasting.
