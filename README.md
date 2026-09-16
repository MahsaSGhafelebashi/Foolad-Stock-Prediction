# Foolad Stock Price Forecasting

This project focuses on forecasting the next-day stock price of **Foolad Mobarakeh Isfahan (Foolad)** using time-series analysis, machine learning, and deep learning techniques.

The main objective is to build and compare different forecasting models for predicting the next trading day's adjusted closing price based on historical stock market data.

## Project Objectives

* Predict the next-day adjusted closing price.
* Compare classical time-series, machine learning, and deep learning models.
* Evaluate model performance using multiple forecasting metrics.
* Develop a forecasting pipeline while avoiding data leakage.

## Dataset

The dataset contains historical trading data for Foolad Mobarakeh Isfahan stock.

The main variables include:

* Open price
* High price
* Low price
* Close price
* Last price
* Trading volume
* Trading value
* Price-to-Earnings (P/E) ratio

Historical prices were adjusted to account for important corporate events, including:

* Capital increases
* Cash dividends

This adjustment provides a more consistent historical price series for forecasting.

## Data Preprocessing and Feature Engineering

The preprocessing pipeline includes:

* Sorting the data chronologically
* Converting and organizing trading dates
* Handling missing and invalid values
* Adjusting historical prices
* Detecting outliers
* Creating lag features
* Calculating moving averages
* Generating technical indicators

The engineered features include:

* Moving averages
* Price returns and percentage changes
* Lagged prices
* Rolling statistics
* Exponentially weighted moving averages
* RSI
* MACD
* Bollinger Bands
* ATR
* Stochastic indicators

## Models

The following models were implemented and compared:

1. **Naive Baseline**
2. **Autoregressive (AR(1))**
3. **Autoregressive Moving Average (ARMA(1,1))**
4. **ARIMA(0,1,1)**
5. **Ridge Regression**
6. **XGBoost**
7. **Multi-Task LSTM**

### Model Comparison

The models were evaluated using **MAE, RMSE, SMAPE, and Directional Accuracy**. Time-based validation was used to preserve the chronological structure of the financial data and reduce the risk of data leakage.

| Model              |     MAE |    RMSE |    SMAPE | Directional Accuracy |
| ------------------ | ------: | ------: | -------: | -------------------: |
| Naive Baseline     | 33.7666 | 55.8011 |  1.6137% |               60.15% |
| AR(1)              | 34.1143 | 55.8724 |  1.6293% |               45.74% |
| ARMA(1,1)          | 33.1787 | 57.4920 |  1.5989% |               65.04% |
| ARIMA(0,1,1)       | 33.4542 | 56.0659 |  1.6045% |               62.91% |
| Ridge Regression   | 33.2058 | 54.9904 |  1.5868% |               61.39% |
| XGBoost            | 32.6461 | 54.8623 |  1.5599% |               63.54% |
| Multi-Task LSTM V2 |       — |       — | 2.2554%* |              54.66%* |

* Validation results for the Multi-Task LSTM V2 experiment.

### Ridge Regression

Ridge Regression was optimized using time-series cross-validation with **45 features**. The best regularization parameter was:

```text
Best Alpha = 100
Mean CV SMAPE = 1.5359%
```

The final test results were:

```text
MAE   = 33.2058
RMSE  = 54.9904
SMAPE = 1.5868%
Directional Accuracy = 61.39%
```

### XGBoost

XGBoost was used to capture nonlinear relationships between the engineered features and the next-day price movement.

The model was evaluated using a chronological train/test split:

```text
Total samples = 4137
Training samples = 3309
Test samples = 828
```

The final test results were:

```text
MAE   = 32.6461
RMSE  = 54.8623
SMAPE = 1.5599%
Directional Accuracy = 63.54%
```

The model predicts the next-day return, which is then converted into the predicted next-day closing price.

### Multi-Task LSTM

A separate deep learning dataset containing **4,136 observations and 46 features** was used for the Multi-Task LSTM experiment.

The model was designed to jointly address price forecasting and directional prediction.

The target direction distribution was:

```text
Down / No Increase = 53.63%
Up                 = 46.37%
```

The validation results were:

```text
Validation SMAPE = 2.2554%
Directional Accuracy = 54.66%
Balanced Accuracy = 55.23%
Best Classification Threshold = 0.48
```

The mean predicted probability of an upward movement was **48.60%**.

## Model Evaluation

The following metrics are used to evaluate forecasting performance:

* **MAE (Mean Absolute Error)**
* **RMSE (Root Mean Squared Error)**
* **MAPE (Mean Absolute Percentage Error)**
* **SMAPE (Symmetric Mean Absolute Percentage Error)**
* **Directional Accuracy**
* **Balanced Accuracy** for classification-based experiments

SMAPE is emphasized because it provides a more stable percentage-based error measure for comparing forecasting performance.

**Directional Accuracy** measures whether the predicted price movement has the same direction as the actual movement.

**Balanced Accuracy** is used in the Multi-Task LSTM experiment to account for the distribution of the two direction classes.

## Validation and Overfitting Assessment

Because stock-market data is time-dependent, random train/test splitting was avoided.

The project uses:

* Chronological train/test splitting
* Walk-forward validation for time-series models
* Time-series cross-validation
* Training vs. testing error comparison
* Residual analysis for time-series models
* Hyperparameter optimization using Optuna

These methods were used to reduce the risk of data leakage and investigate model generalization.

## Technologies

The project was developed using Python and the following libraries:

* Python
* Pandas
* NumPy
* Matplotlib
* Scikit-learn
* Statsmodels
* XGBoost
* TensorFlow / Keras
* Optuna

## Project Structure

```text
├── data/
│   └── dataset.csv
├── notebooks/
│   ├── data_preprocessing.ipynb
│   ├── exploratory_data_analysis.ipynb
│   ├── time_series_models.ipynb
│   ├── machine_learning_models.ipynb
│   └── deep_learning_models.ipynb
├── results/
├── figures/
└── README.md
```

## Results

The experiments demonstrate the differences between classical statistical models, machine learning approaches, and deep learning approaches for next-day stock price forecasting.

The results also highlight the difference between **price prediction accuracy** and **directional prediction accuracy**. A model can achieve a relatively low price prediction error while having a different level of performance in predicting whether the next-day price will increase or decrease.

Among the regression experiments, the XGBoost model achieved an MAE of **32.6461**, an RMSE of **54.8623**, and an SMAPE of **1.5599%**, with a Directional Accuracy of **63.54%** on the test set.

The Multi-Task LSTM experiment was evaluated separately using a validation set and achieved a validation SMAPE of **2.2554%** and Directional Accuracy of **54.66%**.

## Disclaimer

This project was developed for **educational and research purposes** as an undergraduate project.

The predictions generated by the models should not be considered financial or investment advice. Stock prices are affected by many factors that cannot be fully captured by historical data alone.
