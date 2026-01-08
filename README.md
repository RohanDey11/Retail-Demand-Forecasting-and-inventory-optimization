# Retail Sales Forecasting & Inventory Optimization

## Project Overview

This project focuses on forecasting weekly sales for a UK-based online retailer to optimize inventory levels. The primary business goal is to predict demand 6-8 weeks in advance, allowing the supply chain team to minimize the risk of stockouts (lost revenue) and overstocking (holding costs).

By accurately modeling demand and analyzing the distribution of forecast errors, this solution aims to reduce Safety Stock requirements by approximately 10%.

## The Business Problem

- **Objective:** Forecast weekly sales revenue to support inventory ordering decisions.
- **Challenges:** The data exhibits strong multiplicative seasonality (increasing variance over time) and significant fluctuations due to holidays (e.g., Christmas peaks).
- **Evaluation Metrics:**
  - **RMSE (Root Mean Squared Error):** Selected to penalize large errors heavily.
  - **MAPE (Mean Absolute Percentage Error):** Used for business interpretability.
  - **Residual Normality:** Critical for calculating accurate Safety Stock buffers.

## Methodology

The analysis followed a structured time series workflow:

1.  **Exploratory Data Analysis:**

    - Identified multiplicative seasonality where variance increased with the trend.
    - Applied Log-Transformation to stabilize variance and satisfy homoscedasticity assumptions.
    - Confirmed stationarity using the Augmented Dickey-Fuller (ADF) test.

2.  **Model Development:**

    - **Baseline:** Implemented a Naive Forecast (predicting next week's sales equals this week's sales) as a performance benchmark.
    - **Statistical (SARIMA):** Modeled using Seasonal ARIMA with order (1, 0, 1) x (1, 1, 1, 52). While accurate, it struggled with store closures.
    - **Machine Learning (Prophet):** Implemented Facebook Prophet with custom holiday regressors to account for UK public holidays and seasonal shutdowns.

3.  **Optimization:**
    - Performed Grid Search Cross-Validation on Prophet to tune `changepoint_prior_scale` and `seasonality_prior_scale`.

## Model Comparison & Results

| Model       | Performance Note     | Issue Identified                                                                                                          |
| :---------- | :------------------- | :------------------------------------------------------------------------------------------------------------------------ |
| **SARIMA**  | Lowest MAPE          | **Bimodal Residuals:** The model failed to account for store closures (zero sales), creating a biased error distribution. |
| **Prophet** | Slightly higher MAPE | **Unbiased Residuals:** Effectively handled holidays and closures, resulting in a normal error distribution.              |

## Conclusion & Business Decision

Although SARIMA achieved a slightly lower raw error rate (MAPE), **Prophet was selected as the final production model.**

**Reasoning:** In inventory management, Safety Stock is calculated based on the standard deviation of forecast errors.

- SARIMA produced bimodal residuals (two peaks), which artificially inflated the standard deviation.
- Prophet produced normally distributed residuals (Gaussian bell curve).
- **Impact:** By using the unbiased Prophet model, we achieved a cleaner error profile, allowing the business to reduce Safety Stock buffers by roughly 10% without increasing the risk of stockouts.

## Tech Stack

- **Language:** Python
- **Time Series:** Statsmodels (SARIMA), Prophet, PMDARIMA (Auto-ARIMA)
- **Analysis:** Pandas, NumPy, Scikit-learn (Metrics)
- **Visualization:** Matplotlib, Seaborn (ACF/PACF plots, Residual Analysis)

## How to Run

To replicate this analysis:

1.  Clone the repository:

    ```bash
    git clone [https://github.com/RohanDey11/Retail-Demand-Forecasting-and-inventory-optimization.git](https://github.com/RohanDey11/Retail-Demand-Forecasting-and-inventory-optimization.git)
    ```

2.  Install dependencies:

    ```bash
    pip install -r requirements.txt
    ```

3.  Run the notebook:
    ```bash
    jupyter notebook notebooks/Retail Demand Forecasting and inventory optimization.ipynb
    ```
