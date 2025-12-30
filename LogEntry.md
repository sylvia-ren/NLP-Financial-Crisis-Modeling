###Log Entry 1: Project Inception & Hypothesis
**Goal:** Determine if public financial news can predict market "regimes" better than price history alone.
- **Hypothesis:** News sentiment is a leading indicator for the KBE (Bank ETF) volatility.
- **Initial Stack:** Python, Pandas, NLTK VADER.
- **First Results:** Worked on a small sample of 83 headlines. Initial plots showed a visual correlation, but the sample was too small for statistical significance.

##Log Entry 2: Scaling to Big Data 
**Update:** Shifted from 80 headlines to a massive dataset of **307,000 headlines**.
- **Technical Hurdle:** Encountered date formatting problems (YYYYMMDD integer format).
- **Optimization:** To handle 300k+ entries efficiently, I implemented pre-computation filtering. By restricting the dataset to the 2006-2008 window before running the NLP analysis, I significantly reduced the computational overhead. Additionally, I used explicit date parsing (converting YYYYMMDD integers to datetime objects) to ensure high-speed data ingestion.
- **Achievement:** Successfully processed the full 2006-2008 period. The "Law of Large Numbers" began to smooth out the sentiment signal.

##Log Entry 3: From Visuals to Correlation
**Update:** Moving beyond "it looks similar" to mathematical proof.
- **Finding:** Calculated a **60-day Rolling Correlation**. 
- **Discovery:** The correlation is highly non-stationary. I identified a major **-0.8 peak in August 2007**, marking the start of the liquidity crisis.
- **Analysis of Anomalies:** Observed correlation "flips" (up to +0.65) during early 2008.   - - **Hypothesis**: These periods correspond to government intervention rumors where "good news" actually increases market agitation/volatility.

##Log Entry 4 : Analysis limitation
The current analysis uses VADER as a baseline and a 20-day rolling volatility to explore preliminary patterns. We recognize that VADER is not finance-specific, and the 20-day window may smooth short-term reactions. These limitations motivate the next phase: applying finance-specific NLP models (FinBERT / Loughran-McDonald), testing different volatility horizons, and employing models such as Set-Sequence or Markov-switching to capture regime-dependent effects.


##Log Entry 5 : NLP Pipeline & Granger Validation
Features & Fixes:NLP Model: Swapped VADER for pysentiment2.LM (Loughran-McDonald).Preprocessing: Fixed datetime index misalignment between Sentiment (daily) and Financial data (trading days).
Stats Engine: Implemented statsmodels pipeline for Stationarity (ADF) and Causality (Granger).Key Results: Alpha Detected: Granger Test confirms sentiment predicts volatility changes at Lag 1 ($p=0.016$) and Lag 14 ($p=0.003$).
Correlation: Confirmed negative correlation on average, but identified non-stationary relationship (Regime Switch) during the Lehman collapse.