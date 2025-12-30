# Project Log: NLP-Driven Financial Crisis Modeling

## Log Entry 1: Project Inception & Hypothesis
**Goal:** Determine if public financial news can predict market "regimes" better than price history alone.
* **Hypothesis:** News sentiment is a leading indicator for the Banking Sector (KBE ETF) volatility during crisis periods.
* **Initial Stack:** Python, Pandas, NLTK (VADER Lexicon).
* **First Results:** Conducted a pilot study on a small sample of 83 headlines. Initial plots showed a visual correlation, but the sample size was too small to reject the null hypothesis.

## Log Entry 2: Data Engineering & Scaling
**Update:** Shifted from 80 headlines to a massive dataset of **307,000 headlines**.
* **Technical Hurdle:** Encountered date formatting inconsistencies (YYYYMMDD integer format vs. Datetime objects).
* **Optimization:** * Implemented **pre-computation filtering** to restrict the dataset to the 2006-2009 crisis window, significantly reducing memory overhead.
    * Used **vectorized date parsing** to ensure high-speed data ingestion.
* **Achievement:** Successfully processed the full 3-year period. The "Law of Large Numbers" began to smooth out the sentiment signal, reducing idiosyncratic noise.

## Log Entry 3: From Visuals to Correlation
**Update:** Moving beyond visual inspection to quantitative validation.
* **Methodology:** Calculated a **60-day Rolling Correlation** between Sentiment and Volatility.
* **Discovery:** The correlation is highly **non-stationary**. Identified a major **-0.8 correlation peak in August 2007**, accurately marking the onset of the liquidity crisis.
* **Anomaly Detection:** Observed "Correlation Flips" (positive correlation up to +0.65) during early 2008.
    * **Hypothesis:** These periods correspond to government intervention rumors (bailouts) where "good news" paradoxically increases market agitation.

## Log Entry 4: Critical Review & Model Limitation
**Update:** Assessing the validity of the baseline model.
* **Critique:**
    1.  **Lexicon Bias:** VADER is trained on social media data, not financial terminology. It lacks domain specificity.
    2.  **Lag Sensitivity:** The initial 20-day volatility window over-smoothed short-term market reactions.
* **Pivot:** These limitations motivate the next phase: applying finance-specific NLP models (**Loughran-McDonald**), testing different volatility horizons, and employing rigorous statistical testing.

## Log Entry 5: NLP Pipeline Upgrade & Statistical Validation
**Update:** Implementation of the V2 Pipeline correcting identified flaws.
* **NLP Model:** Swapped VADER for **Loughran-McDonald (LM)** via `pysentiment2` to capture finance-specific tone.
* **Data Wrangling:** Fixed datetime index misalignment between Sentiment (daily) and Financial data (trading days).
* **Statistical Engine:** Implemented `statsmodels` pipeline for Stationarity (ADF) and Causality (Granger).
* **Key Results:**
    * **Alpha Detected:** Granger Causality Test confirms sentiment predicts volatility changes significantly.
    * **Calibration (Sensitivity Analysis):**
    * Tested volatility calculation windows ranging from 3 to 14 days.
    * **Discovery:** Identified **6 days (one trading week)** as the optimal "news absorption" period ($p < 0.001$). This suggests a structural lag between news publication and its full impact on bank stock volatility.

## Log Entry 6: Robustness & Discriminant Validity Check
**Goal:** Challenge the model's specificity by comparing Sectorial Volatility (KBE) against a Market Benchmark (VIX).

* **Methodological Pivot:** Introduced the **VIX (CBOE Volatility Index)** as a control variable.
    * *Scientific Justification:* To determine if the NLP signal predicts *global market sentiment* (Implied Volatility) or strictly *banking sector turbulence* (Realized Volatility).
* **Comparative Results:**
    * **Target A (KBE - Banking Sector):** Strong Granger Causality found across lags 2-15. The sentiment signal clearly precedes the price turbulence.
    * **Target B (VIX - S&P 500 Benchmark):** No significant causality found. The VIX reacts instantaneously to news, leaving no predictive lag for the daily model.
* **Conclusion:** The discrepancy confirms the model's **Discriminant Validity**. The NLP pipeline successfully captures **idiosyncratic banking risk** (specific to the nature of the 2008 crisis) but is not a proxy for global implied volatility. The signal is genuine sector-specific alpha.