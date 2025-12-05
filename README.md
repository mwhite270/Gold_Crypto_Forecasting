# Gold and Crypto Forecasting

This notebook explores time series forecasting for the prices of Gold (via Tether Gold), Bitcoin, and Ethereum using Python.

![Graphs of gold, bitcoin, and ethereum prices over time](https://github.com/user-attachments/assets/fd4869bf-5475-486f-9197-75edd0aaa9a6)

## Project Objective

*   Observe and analyze price trends of gold, Bitcoin, and Ethereum.
*   Implement and compare various time series forecasting and statistical models.
*   Evaluate the performance of these models in predicting highly volatile asset prices.

## Data Source

All pricing data is sourced from [CoinGecko.com](https://www.coingecko.com/).

*   **Tether Gold (XAUT)**: Used as a proxy for actual gold trading price due to its design to mirror gold's value.
*   **Bitcoin (BTC)**
*   **Ethereum (ETH)**

For consistency, the analysis focuses on data from 2020 onwards.

## Methodology

The forecasting process involved several steps:

1.  **Data Loading and Preparation**:
    *   Individual CSV files for Gold, Bitcoin, and Ethereum were loaded into pandas DataFrames.
    *   A `unique_id` column was added to each DataFrame for identification.
    *   DataFrames were concatenated into a single DataFrame.
    *   Columns were renamed to `ds` (for datetime) and `y` (for price) to align with `statsforecast` and `utilsforecast` requirements.
    *   Data was filtered to include prices from 2020-01-01 onwards.
    *   Data was resampled to ensure a consistent daily frequency, filling missing values.

2.  **Baseline Models Evaluation**:
    *   **Naive**: Forecasts the next value as the last observed value.
    *   **Historic Average**: Uses the average of all past values.
    *   **Window Average**: Uses the average of a defined window size (e.g., 7 days).
    *   **Seasonal Naive**: Uses the value from the same period in the previous season (e.g., 7 days ago).
    *   These models were trained on a training set and evaluated on a test set (last 7 days).
    *   Mean Absolute Error (MAE) was used as the evaluation metric.

3.  **ARIMA Models Evaluation**:
    *   **AutoARIMA**: Automatically finds the best ARIMA model parameters.
    *   **Seasonal AutoARIMA (SARIMA)**: Similar to ARIMA but accounts for seasonal components.
    *   These models were also trained and evaluated using MAE.

4.  **Cross-Validation**:
    *   Cross-validation was performed on the best-performing baseline models (Naive, ARIMA, SARIMA) to assess their robustness over multiple time windows.
    *   `StatsForecast`'s `cross_validation` function was used with `h=7` (7-day horizon) and `n_windows=8`.

## Key Findings

*   **High Volatility**: The cryptocurrency markets (Bitcoin and Ethereum) exhibit significant volatility, making accurate long-term forecasting challenging. Even gold, while steadier, shows unpredictable movements.
*   **Model Performance**: Across all evaluated models (Naive, Historic Average, Window Average, Seasonal Naive, ARIMA, SARIMA), Mean Absolute Error (MAE) values remained relatively high, indicating the inherent difficulty in predicting these markets.
*   **Naive Model Competitiveness**: The Naive model often performed comparably to more complex models like ARIMA and Seasonal ARIMA in terms of overall MAE, especially in cross-validation. This suggests that for highly volatile series, simpler models can sometimes be just as effective (or ineffective) as more complex ones, while being significantly faster to train and predict.
*   **Historic Average Inefficiency**: The Historic Average model consistently showed the highest error, indicating it's not suitable for forecasting these types of volatile assets.

![High mean absolute error for models](https://github.com/user-attachments/assets/2d1409ca-89a9-4ec7-9d7b-3552e7f02922)

## Tools and Libraries

*   **Python 3**
*   **pandas**: For data manipulation and analysis.
*   **matplotlib**: For plotting and visualization.
*   **utilsforecast**: For plotting time series data and forecasts.
*   **statsforecast**: For implementing various time series forecasting models (Naive, HistoricAverage, WindowAverage, SeasonalNaive, AutoARIMA).

## Setup and Usage

To run this notebook, ensure you have the necessary libraries installed:

```bash
pip install statsforecast utilsforecast pandas matplotlib
```

Then, execute the cells sequentially in a Google Colab environment or any Jupyter-compatible environment.

Data files have been provided. Point the notebook to the appropriate saved location.
