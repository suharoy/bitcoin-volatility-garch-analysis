# Bitcoin Price Volatility: A Comparative GARCH Family Analysis

This repository hosts the research paper:

*"Capturing Asymmetry and Heavy Tails in Bitcoin Price Volatility – A Comparative GARCH Family Analysis"*  
by *Suha Roy and Abhishek Gupta* 

M.Sc. Computer Science (Big Data Analytics), Central University of Rajasthan  


---

## Abstract

This study analyzes Bitcoin’s weekly returns from March 2020 to March 2025 using a suite of GARCH-family models, including:

- GARCH(1,1) with Normal and Student’s t-distributions
- EGARCH(1,1) with Normal and Student’s t-distributions

The results show that *EGARCH(1,1) with Student’s t-distribution* provides the best overall fit. It captures both asymmetric volatility and fat tails in the return distribution, yielding more conservative and realistic short-term risk forecasts.

---

## Paper Access

*[Click here to download the research paper (PDF)](./Capturing_Asymmetry_and_Heavy_Tails_in_Bitcoin_Price_Volatility___A_Comparative_GARCH_Family_Analysis.pdf)*

---

## Motivation

Bitcoin's extreme price behavior and market sentiment-driven volatility make it an ideal candidate for volatility modeling using econometric techniques. This study aims to evaluate the efficacy of traditional models like GARCH against their more nuanced variants, especially under non-normal return distributions.

---

## Highlights

- *Dataset*: Weekly closing prices of Bitcoin (March 2020 – March 2025)
- *Models Evaluated*:
  - GARCH(1,1) – Normal vs. Student’s t
  - EGARCH(1,1) – Normal vs. Student’s t
- *Evaluation Metrics*:
  - Akaike Information Criterion (AIC)
  - Bayesian Information Criterion (BIC)
  - Log-Likelihood
- *Output*:
  - 8-week out-of-sample volatility forecasts
  - Emphasis on asymmetric volatility and fat tails

---

## Key Findings

- *Student’s t-distribution* better captures fat tails in return distributions than the Normal.
- *EGARCH model* accounts for leverage effects—how bad news impacts volatility more than good news.
- *EGARCH(1,1) with Student’s t* delivers the best statistical fit (lowest AIC/BIC, highest Log-Likelihood).

---

## Tools Used

- LaTeX (for academic paper formatting)
- Python / Excel (for initial data handling and plotting)
- GitHub (for version control and hosting)

---

## Folder Structure

```text
bitcoin-volatility-garch-analysis/
├── README.md
├── LICENSE
├── Capturing_Asymmetry_and_Heavy_Tails_in_Bitcoin_Price_Volatility.pdf
├── data/
│   └── Bicoin_5.csv 
├── scripts/
│   └── crypto_volatility_prediction_model.py         
