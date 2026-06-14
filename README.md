# NZD-USD-Exchange-Rate-Forecaster

## Overview

This project analyzes and prepares historical NZD/USD exchange-rate data for time-series forecasting. The project is motivated by a practical use case: understanding exchange-rate movements that matter for New Zealand international students and families managing money across NZD and USD.

The current version focuses on the first stage of the project: **data auditing, cleaning, transformation, and exploratory visualization**. Forecasting models have not been built yet.

## Current Project Stage

Completed so far:

* Loaded historical exchange-rate data from FRED.
* Audited the raw dataset structure.
* Confirmed the meaning of the FRED `DEXUSNZ` series.
* Checked the date range, date formatting, chronological ordering, and duplicate dates.
* Investigated missing exchange-rate observations.
* Removed missing exchange-rate rows for the cleaned working dataset.
* Created a log-transformed exchange-rate series.
* Created daily log returns.
* Produced initial visualizations of:

  * the NZD/USD exchange-rate level;
  * the log NZD/USD exchange rate;
  * daily NZD/USD log returns.
* Saved a cleaned and transformed dataset for later modeling.

## Data Source

The dataset comes from FRED and uses the series:

`DEXUSNZ`

This series represents:

> U.S. dollars per 1 New Zealand dollar

In other words:

```text
1 NZD = x USD
```

This project currently uses `DEXUSNZ` directly, so the project is focused on modeling the **NZD/USD exchange rate**, not the inverted USD/NZD rate.

## Dataset

The raw dataset contains two columns:

```text
observation_date
DEXUSNZ
```

After cleaning and transformation, the processed dataset contains:

```text
observation_date
DEXUSNZ
log_nzd_usd
nzd_usd_return
```

Column meanings:

| Column             | Meaning                                       |
| ------------------ | --------------------------------------------- |
| `observation_date` | Date of the exchange-rate observation         |
| `DEXUSNZ`          | U.S. dollars per 1 New Zealand dollar         |
| `log_nzd_usd`      | Natural log of `DEXUSNZ`                      |
| `nzd_usd_return`   | Daily log return of the NZD/USD exchange rate |

## Data Audit Summary

The raw dataset covers observations from:

```text
2016-06-06 to 2026-06-05
```

The dataset originally contained:

```text
2,610 rows
```

The `DEXUSNZ` column contained:

```text
112 missing values
```

After removing missing exchange-rate observations, the cleaned dataset contains:

```text
2,498 usable observations
```

The missing values appeared to occur mostly on holidays or non-publication days, rather than from obvious data corruption.

## Initial Findings

The `DEXUSNZ` exchange-rate values ranged from approximately:

```text
0.55 to 0.75
```

This is plausible for an NZD/USD exchange-rate series.

The average exchange rate in the cleaned sample was approximately:

```text
0.652 USD per 1 NZD
```

The daily log return series had an average close to zero, which is typical for daily financial return data.

The exchange-rate level series is persistent and moves gradually over time, while the daily return series is much noisier. This suggests that short-term exchange-rate forecasting may be difficult and should be evaluated carefully against simple baseline models.

## Visual Analysis Completed

Three main plots were created:

1. **NZD/USD exchange-rate level over time**
   Shows the broad movement of the exchange rate across the sample.

2. **Log NZD/USD exchange rate over time**
   Preserves the shape of the original series while preparing the data for return calculations.

3. **NZD/USD daily log returns over time**
   Shows day-to-day exchange-rate changes. The return series fluctuates around zero and contains occasional spikes, suggesting periods of higher volatility.

## Current Repository Structure

The project is organized around separating raw data, processed data, and analysis notebooks.

Expected structure:

```text
NZD-USD-Exchange-Rate-Forecaster/
  data/
    raw/
    processed/
  notebooks/
    01_data_audit_and_transformation.ipynb
  README.md
```

## Next Steps

The next stage of the project will focus on baseline forecasting.

Planned next notebook:

```text
02_baseline_forecasting.ipynb
```

The goal of the next stage is to build simple benchmark forecasts before trying more complex models.

The most important baseline will be the naïve/random-walk forecast:

```text
Tomorrow's exchange rate = today's exchange rate
```

This baseline is especially important for exchange-rate forecasting because financial time series are often difficult to beat with more complex models.

Future work may include:

* random-walk baseline forecasting;
* drift baseline forecasting;
* train/test split using time order;
* walk-forward validation;
* forecast accuracy metrics such as MAE and RMSE;
* ARIMA modeling;
* comparison of model forecasts against baseline forecasts;
* dashboard development for currency-planning use cases.

## Project Goal

The final goal is to build a credible NZD/USD exchange-rate forecasting and analysis project that emphasizes:

* correct financial-data interpretation;
* clean time-series methodology;
* honest baseline comparison;
* clear communication of uncertainty;
* practical relevance for international students and families dealing with NZD/USD exchange-rate risk.
