# Bitcoin Volatility Modelling with GARCH-Family Models

### Asymmetry, Heavy Tails and Short-Horizon Volatility Forecasting

A comparative econometric study of **Bitcoin return volatility** using GARCH-family models under alternative distributional assumptions.

The analysis evaluates whether explicitly modelling **asymmetric volatility responses** and **heavy-tailed returns** improves the statistical fit of Bitcoin volatility models.

The study compares:

* GARCH(1,1) with Normal innovations
* GARCH(1,1) with Student's t innovations
* EGARCH(1,1) with Normal innovations
* EGARCH(1,1) with Student's t innovations

The reported results identify **EGARCH(1,1) with Student's t innovations** as the strongest specification among the evaluated models according to the model-selection criteria used in the study.

---

## Research Paper

This repository accompanies the research paper:

**Capturing Asymmetry and Heavy Tails in Bitcoin Price Volatility: A Comparative GARCH Family Analysis**

**Authors:** Suha Roy and Abhishek Gupta
**Programme:** M.Sc. Computer Science — Big Data Analytics
**Central University of Rajasthan**

[**Read the full research paper**](Capturing_Asymmetry_and_Heavy_Tails_in_Bitcoin_Price_Volatility___A_Comparative_GARCH_Family_Analysis.pdf)

---

## Research Objective

Financial return series frequently exhibit characteristics that are poorly represented by constant-variance Gaussian models.

Bitcoin is particularly relevant because its return behaviour can exhibit:

* volatility clustering;
* periods of elevated market uncertainty;
* heavy-tailed return distributions;
* non-normal shocks;
* asymmetric responses to market movements.

The central question explored in this work is:

> Does incorporating asymmetric volatility dynamics and heavy-tailed innovations provide a better representation of Bitcoin return volatility than a conventional Gaussian GARCH model?

---

## Dataset

The repository includes weekly Bitcoin market data in:

```text
Bitcoin_5.csv
```

The dataset covers approximately:

```text
March 2020 → March 2025
```

and contains:

| Variable   | Description              |
| ---------- | ------------------------ |
| `Date`     | Weekly observation date  |
| `Close`    | Weekly closing price     |
| `Open`     | Weekly opening price     |
| `High`     | Weekly high              |
| `Low`      | Weekly low               |
| `Vol.`     | Trading-volume field     |
| `Change %` | Weekly percentage change |

The modelling workflow primarily uses the **closing-price series** to construct returns and volatility measures.

---

## Analysis Pipeline

```text
Weekly Bitcoin prices
        │
        ▼
Data cleaning
        │
        ▼
Closing-price series
        │
        ├── Percentage returns
        │
        └── Log returns
        │
        ▼
Exploratory analysis
        │
        ├── Return behaviour
        ├── Rolling volatility
        ├── Distribution analysis
        └── Autocorrelation
        │
        ▼
Stationarity testing
       ADF
        │
        ▼
GARCH-family estimation
        │
   ┌────┴───────────────────────┐
   │                            │
   ▼                            ▼
GARCH(1,1)                 EGARCH(1,1)
   │                            │
 ┌─┴──────┐                  ┌──┴─────┐
 │        │                  │        │
Normal  Student-t          Normal  Student-t
 │        │                  │        │
 └────────┴──────────┬───────┴────────┘
                    ▼
             Model comparison
                    │
          AIC · BIC · Log-Likelihood
                    │
                    ▼
            Volatility forecasts
```

---

## Feature Construction

The analysis constructs several financial time-series measures from the closing-price series.

### Percentage Return

$$
R_t =
\frac{P_t-P_{t-1}}{P_{t-1}}
$$

where \(P_t\) represents the Bitcoin closing price at time \(t\).

### Log Return

$$
r_t =
\ln\left(
\frac{P_t}{P_{t-1}}
\right)
$$

Log returns are used as the primary input for volatility modelling.

### Rolling Volatility

The exploratory workflow also calculates a four-week rolling standard deviation of log returns:

$$
\sigma_t =
SD(r_{t-3},\ldots,r_t)
$$

This provides a descriptive view of how volatility evolves over time.

---

## Exploratory Analysis

The notebook includes exploratory analysis of:

* Bitcoin weekly closing prices;
* percentage returns;
* log returns;
* rolling four-week volatility;
* volatility distribution;
* autocorrelation in returns;
* autocorrelation in rolling volatility.

These diagnostics provide the empirical motivation for conditional-variance modelling.

---

## Stationarity Analysis

The workflow applies the **Augmented Dickey-Fuller test** to the log-return series.

The analysis reports strong evidence against non-stationarity in the return series, supporting its use in the subsequent volatility-modelling stage.

The complete statistical output is retained in the notebook.

---

## GARCH(1,1)

The standard GARCH(1,1) specification models conditional variance as:

$$
\sigma_t^2 =
\omega
+
\alpha\epsilon_{t-1}^{2}
+
\beta\sigma_{t-1}^{2}
$$

where:

* \(\omega\) represents the variance constant;
* \(\alpha\) captures the effect of recent shocks;
* \(\beta\) captures volatility persistence.

Two innovation distributions are evaluated:

```text
Normal
Student's t
```

The Student's t specification allows greater probability mass in the tails than a Gaussian distribution.

---

## EGARCH(1,1)

The Exponential GARCH model represents the logarithm of conditional variance.

A simplified EGARCH formulation is:

$$
\log(\sigma_t^2)
=
\omega
+
\beta\log(\sigma_{t-1}^{2})
+
\alpha
\left(
\frac{|\epsilon_{t-1}|}
{\sigma_{t-1}}
\right)
+
\gamma
\left(
\frac{\epsilon_{t-1}}
{\sigma_{t-1}}
\right)
$$

The asymmetric term allows positive and negative shocks to affect future volatility differently.

This makes EGARCH useful when volatility responds asymmetrically to market movements.

---

## Distributional Assumptions

Each volatility structure is estimated under two innovation distributions.

### Normal Distribution

Provides the conventional Gaussian benchmark.

### Student's t Distribution

Provides heavier tails and is therefore more appropriate when extreme return observations occur more frequently than a Normal distribution would imply.

This allows the analysis to address two separate modelling questions:

```text
Does asymmetric volatility matter?
               +
Do heavy-tailed innovations matter?
```

---

## Model Comparison

The fitted models are compared using:

### Akaike Information Criterion

$$
AIC = 2k - 2\ln(L)
$$

### Bayesian Information Criterion

$$
BIC = k\ln(n) - 2\ln(L)
$$

### Log-Likelihood

Higher log-likelihood indicates stronger statistical fit, while lower AIC and BIC are preferred when comparing competing models.

The evaluated specifications are:

| Model        | Volatility Structure | Innovation Distribution |
| ------------ | -------------------- | ----------------------- |
| GARCH-N      | GARCH(1,1)           | Normal                  |
| GARCH-t      | GARCH(1,1)           | Student's t             |
| EGARCH-N     | EGARCH(1,1)          | Normal                  |
| **EGARCH-t** | **EGARCH(1,1)**      | **Student's t**         |

According to the results reported in the study, **EGARCH(1,1) with Student's t innovations provides the strongest overall statistical fit among the evaluated specifications**.

---

## Key Findings

### Heavy-tailed innovations improve model fit

The Student's t specifications better accommodate the heavy-tailed behaviour observed in Bitcoin returns than the corresponding Gaussian models.

### Asymmetric volatility dynamics are relevant

EGARCH provides a mechanism for positive and negative shocks to have different effects on future volatility.

### Combining asymmetry and heavy tails produced the strongest specification

The study reports that **EGARCH(1,1) with Student's t innovations** achieved the preferred combination of AIC, BIC and log-likelihood among the models evaluated.

### Volatility forecasting is not price forecasting

The models estimate and forecast **conditional variance**, not the future direction or price level of Bitcoin.

This distinction is important when interpreting the results.

---

## Volatility Forecasting

The fitted models are used to generate **eight-week volatility forecasts**.

The workflow compares forecasts from:

* GARCH Normal
* GARCH Student's t
* EGARCH Normal
* EGARCH Student's t

For multi-step forecasts, simulation-based forecasting is used where required.

Forecast conditional variances are converted into volatility through:

$$
\sigma =
\sqrt{\sigma^2}
$$

The resulting trajectories are compared visually.

---

## Repository Structure

```text
.
├── Bitcoin_5.csv
├── Crypto_Volatility_Prediction_Model.ipynb
├── crypto_volatility_prediction_model.py
├── Capturing_Asymmetry_and_Heavy_Tails_in_Bitcoin_Price_Volatility___A_Comparative_GARCH_Family_Analysis.pdf
├── .gitignore
├── LICENSE
└── README.md
```

### Primary Analysis

[`Crypto_Volatility_Prediction_Model.ipynb`](Crypto_Volatility_Prediction_Model.ipynb)

The notebook is the **primary executable research artifact** and contains:

* data preprocessing;
* return construction;
* exploratory analysis;
* rolling volatility;
* autocorrelation analysis;
* ADF testing;
* GARCH estimation;
* EGARCH estimation;
* Normal vs Student's t comparisons;
* model-selection statistics;
* eight-week volatility forecasting.

### Python Export

[`crypto_volatility_prediction_model.py`](crypto_volatility_prediction_model.py)

This file is retained as a Python export of the original notebook workflow.

Because it was generated from a Google Colab notebook, it contains notebook-specific assumptions such as Colab paths and notebook commands.

The `.ipynb` file should therefore be treated as the primary reproducible implementation.

### Research Paper

[`Capturing_Asymmetry_and_Heavy_Tails_in_Bitcoin_Price_Volatility___A_Comparative_GARCH_Family_Analysis.pdf`](Capturing_Asymmetry_and_Heavy_Tails_in_Bitcoin_Price_Volatility___A_Comparative_GARCH_Family_Analysis.pdf)

Contains the full academic discussion of the methodology, model comparison, results and conclusions.

---

## Technology Stack

The analysis uses:

```text
Python
Pandas
NumPy
Matplotlib
Seaborn
Statsmodels
ARCH
Jupyter / Google Colab
```

---

## Running the Analysis

Clone the repository:

```powershell
git clone https://github.com/suharoy/bitcoin-volatility-garch-analysis.git
cd bitcoin-volatility-garch-analysis
```

Create a virtual environment:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Install the primary dependencies:

```powershell
pip install pandas numpy matplotlib seaborn statsmodels arch jupyter
```

Launch Jupyter:

```powershell
jupyter lab
```

Then open:

```text
Crypto_Volatility_Prediction_Model.ipynb
```

When running locally rather than in Google Colab, update the dataset path to:

```text
Bitcoin_5.csv
```

---

## Limitations

This work is an econometric volatility study rather than a cryptocurrency trading system.

Important limitations include:

* the sample covers a specific five-year market period;
* only weekly observations are modelled;
* the study compares a focused set of GARCH-family specifications;
* exogenous macroeconomic and market variables are not incorporated;
* regime-switching volatility models are not evaluated;
* model selection is based primarily on statistical fit criteria;
* volatility forecasts are not equivalent to directional price forecasts;
* cryptocurrency market structure can change substantially across periods.

The conclusions should therefore be interpreted within the scope of the evaluated dataset and model specifications.

---

## Future Work

Potential extensions include:

* GJR-GARCH and additional asymmetric models;
* skewed Student's t innovations;
* regime-switching volatility models;
* realized-volatility measures;
* higher-frequency Bitcoin data;
* rolling-window estimation;
* formal out-of-sample forecast-loss comparison;
* Value-at-Risk and Expected Shortfall evaluation;
* macroeconomic and liquidity covariates;
* comparison with neural volatility models.

---

## Research Perspective

The central result of this study is not that one model is universally optimal.

Rather, it demonstrates why **distributional assumptions and asymmetric volatility dynamics matter** when modelling financial assets with extreme and irregular return behaviour.

For the Bitcoin sample considered here, allowing both:

```text
asymmetric volatility response
            +
heavy-tailed innovations
```

produced the strongest specification among the models evaluated.

---

## Disclaimer

This repository is intended for **academic and research purposes**.

The analysis does not constitute investment advice, trading advice, or a recommendation to buy or sell cryptocurrency.

Historical model fit and volatility forecasts do not guarantee future market behaviour.

---

## Citation

If you use or build upon this work, please cite:

```bibtex
@misc{roygupta2025bitcoinvolatility,
  author       = {Suha Roy and Abhishek Gupta},
  title        = {Capturing Asymmetry and Heavy Tails in Bitcoin Price Volatility: A Comparative GARCH Family Analysis},
  year         = {2025},
  howpublished = {GitHub repository},
  url          = {https://github.com/suharoy/bitcoin-volatility-garch-analysis}
}
```

---

## License

The original code and documentation in this repository are released under the **MIT License**.

See [`LICENSE`](LICENSE).

The included dataset and third-party libraries remain subject to their respective source licenses and terms.

---

## Authors

**Suha Roy**
M.Sc. Computer Science — Big Data Analytics
Central University of Rajasthan

**Abhishek Gupta**
M.Sc. Computer Science — Big Data Analytics
Central University of Rajasthan

**GitHub:** https://github.com/suharoy
