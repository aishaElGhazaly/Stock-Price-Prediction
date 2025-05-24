# Stock Price Prediction

![Python](https://img.shields.io/badge/python-3.12-blue)
![Platform](https://img.shields.io/badge/platform-Windows-lightgrey)
![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-Active-brightgreen)

This project implements a robust, end-to-end pipeline for forecasting stock returns of S&P 500 companies using machine learning. It covers data retrieval, feature engineering, dataset assembly, model training, and prediction reporting, with a focus on reproducibility and extensibility.

---

## Project Structure

```
.
├── 01 - Data Retrieval (V1).ipynb      # Download price & financial data for a single stock
├── 01 - Data Retrieval (V2).ipynb      # (Optional) Batch data retrieval version
├── 02 - Feature Engineering.ipynb      # Compute technical & fundamental features
├── 03 - Dataset Assembley.ipynb        # Merge all stocks into a unified dataset
├── 04- Predictions.ipynb               # Model training, forecasting, and reporting
├── S&P_500_Companies.csv               # S&P 500 constituents metadata
├── dataset/
│   ├── dataset.csv                     # Final merged dataset for modeling
│   └── Stocks/                         # Per-stock raw & feature files
├── AutoGluon_Models/                   # Saved AutoGluon models
├── config.py                           # API keys and configuration
└── spp-env/                            # Python virtual environment
```

---

## Pipeline Overview

### 1. Data Retrieval

- **Source:** Yahoo Finance (`yfinance`) and Alpha Vantage APIs
- **Data:** Daily price (adjusted/unadjusted), annual/quarterly financial statements
- **Output:** Cleaned CSVs per stock in `dataset/Stocks/<SYMBOL>/`

### 2. Feature Engineering

- **Technical Indicators:** SMA, EMA, MACD, RSI, Bollinger Bands, ATR, OBV, ROC, Stochastic Oscillator, etc.
- **Fundamental Ratios:** ROE, P/E, P/B, EPS growth, D/E, EV/EBITDA, FCF yield, etc.
- **Output:** Full feature set CSV per stock

### 3. Dataset Assembly

- **Merge:** All stock feature sets into a single DataFrame
- **Annotate:** Add sector/industry metadata
- **Output:** `dataset/dataset.csv` (ready for modeling)

### 4. Modeling & Prediction

- **Framework:** [AutoGluon](https://auto.gluon.ai/) TimeSeriesPredictor
- **Target:** Future log return over X-business-day horizon
- **Splitting:** Chronological train/validation/test split
- **Model:** PatchTST (Transformer-based time series model)
- **Evaluation:** RMSE (overall & per-symbol), prediction report

### 5. Reporting

- **Output:** CSV report with predicted returns, confidence intervals, and forecasted prices for each stock and date

---

## How to Run

1. **Install dependencies**  
   Create a virtual environment and install required packages:
   ```sh
   python -m venv spp-env
   source spp-env/bin/activate  # or .\spp-env\Scripts\activate on Windows
   pip install -r requirements.txt
   ```

2. **Configure API Keys**  
   Add your Alpha Vantage API key to a `.env` file (do **not** commit this file to version control):
   ```
   API_KEY=YOUR_ALPHA_VANTAGE_KEY
   ```
   In `config.py`, load the API key securely using `os.getenv()` (optionally with `python-dotenv`):
   ```python
   from dotenv import load_dotenv
   import os

   load_dotenv()
   API_KEY = os.getenv("API_KEY")
   ```
   > **Note:**  
   > - The `.env` file should be listed in your `.gitignore` to keep your credentials private.
   > - Never hard-code credentials in your codebase.

3. **Run Notebooks in Order**  
   - `01 - Data Retrieval (V1).ipynb` – Download data for each stock
   - `02 - Feature Engineering.ipynb` – Generate features per stock
   - `03 - Dataset Assembley.ipynb` – Merge all stocks into one dataset
   - `04- Predictions.ipynb` – Train model and generate forecasts

4. **View Results**  
   - Final dataset: `dataset/dataset.csv`
   - Model: `AutoGluon_Models/log_return_Xd/`
   - Prediction report: `predictions_report.csv`

---

## Key Features

- **Reproducible:** All steps are modular and save intermediate outputs
- **Extensible:** Add new features, models, or data sources easily
- **Interpretability:** Prediction reports include both returns and price forecasts
- **Scalable:** Designed for S&P 500 but adaptable to other universes

---

## Requirements

- Python 3.10+
- [AutoGluon](https://auto.gluon.ai/)
- [yfinance](https://github.com/ranaroussi/yfinance)
- [Alpha Vantage API](https://www.alphavantage.co/)
- pandas, numpy, matplotlib, talib, etc.

---

## License

See [Stock-Price-Prediction/LICENSE](Stock-Price-Prediction/LICENSE).

---

## Acknowledgements

- [AutoGluon](https://auto.gluon.ai/)
- [yfinance](https://github.com/ranaroussi/yfinance)
- [Alpha Vantage](https://www.alphavantage.co/)
