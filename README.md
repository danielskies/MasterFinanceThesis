# Does Corporate AI Talk Predict Stock Returns?
**Evidence from LLM-Based Disclosure Analysis and the AI-Washing Phenomenon**  

Master’s thesis repository — Vrije Universiteit Amsterdam  

---

## 📄 Thesis Abstract
This study tests whether the language U.S. public firms use to discuss artificial intelligence (AI) in annual 10-K filings predicts future stock performance. Using a balanced panel of **799 Russell 3000 firms (2020-2024)**, I score eight disclosure dimensions—strategic depth, sentiment, three risk categories, forward-looking statements, AI-washing, and talent investment—with three large-language models (OpenAI **GPT-4o mini**, Google **Gemini 1.5 Flash**, **Gemini 2.5 Flash**). Averaged scores form an **AI factor suite** plus an aggregate index. Cross-sectional regressions of 3-, 6-, 9-, and 12-month buy-and-hold excess returns—controlling for firm characteristics, Fama-French six-factor betas, and fixed effects—show that higher scores on most AI dimensions predict **significantly lower 12-month excess returns**. Investors appear to discount heavy AI rhetoric, correcting hype during the sample window.

---

## 📁 Project Structure

| Notebook | Purpose |
|---|---|
| `01_Data_Loading_and_Preparation.ipynb` | Build the balanced 10-K text panel |
| `02_AiScoring_ControlsDownload_Dataset_Builder.ipynb` | Score disclosures with LLMs and merge financial controls |
| `03_SummaryStatistics.ipynb` | Compute descriptive statistics and visualisations |
| `04_Regressions.ipynb` | Run portfolio sorts and cross-sectional regressions |

## 📦 Key Data Assets

| File | What’s Inside | Size / Format |
|---|---|---|
| **`perfect_balanced_ai_dataset.csv`** | Raw AI-score matrix — every firm-year (799 × 5) × 3 LLMs × 8 dimensions | ~30 MB, CSV |
| **`final_clean_dataset_filtered_with_corrected_factor_loadings.csv`** | Full modelling dataset — excess returns, AI factors, controls, dynamic betas | ~40 MB, CSV |
| **`panel_data_text_sample50.parquet`** | *Sample* of the original 10-K text panel: first 25 + last 25 observations (≈ 50 rows × two long-text columns) | ~5 MB, Parquet |

> **Why the sample file?**  
> The full `panel_data_charactermax200k.parquet` (~3 GB) is too large for GitHub.  
> Re-running **`01_Data_Loading_and_Preparation.ipynb`** will regenerate the full file if needed.

---

### 01_Data_Loading_and_Preparation.ipynb
* **Data acquisition:** Download 10-K filings (FY 2020-2024) for Russell 3000 firms from SEC EDGAR.  
* **Text extraction:** Pull *Item 1A – Risk Factors* and *Item 7 – MD&A* sections.  
* **Cleaning:**  
  - Keep only firms with filings in all five years (balanced panel).  
  - Retain latest amended filing.  
  - Drop filings > 200 k characters (LLM context sanity).  
* **Output:** `panel_data_charactermax200k.parquet`.

### 02_AiScoring_ControlsDownload_Dataset_Builder.ipynb
* **LLM scoring:** GPT-4o mini + Gemini Flash (1.5 & 2.5) → eight AI dimensions.  
* **Financial integration:** Merge Compustat & CRSP fundamentals; add Fama-French 6-factor data.  
* **Factor loadings:** Rolling 6-month betas; calculate momentum.  
* **Output:** Final modelling dataset.

### 03_SummaryStatistics.ipynb
* **Descriptive stats:** Means, medians, SDs for returns, AI factors, controls.  
* **Visuals:**  
  - Correlation heat-maps for AI factors.  
  - Sector distribution of AI scores.  
  - Inter-model agreement metrics (Kendall τ, Spearman ρ; exact/±1-grade matches).

### 04_Regressions.ipynb
* **Univariate sorts:** Quintile (and sector-neutral) portfolios on each AI score → excess returns.  
* **Multivariate regressions:** 3/6/9/12-m horizons, full control set, year & sector FE.  
* **Sector splits:** Separate regressions for each GICS sector.  
* **Robustness:** VIFs, Breusch-Pagan / White tests, firm-clustered SEs.

---

## ▶️ How to Run

1. **Clone** this repo and `cd` into it.  
2. Run notebooks sequentially:  
   1. **`01_Data_Loading_and_Preparation.ipynb`** → creates `panel_data_charactermax200k.parquet`.  
   2. **`02_AiScoring_ControlsDownload_Dataset_Builder.ipynb`**  
      *Requires* `OPENAI_API_KEY` and `GOOGLE_API_KEY` environment variables.  
   3. **`03_SummaryStatistics.ipynb`** → generates tables/plots.  
   4. **`04_Regressions.ipynb`** → outputs main empirical results.  

---

## 🔧 Requirements
* Python ≥ 3.10  
* Core libraries: `pandas`, `numpy`, `scikit-learn`, `statsmodels`, `matplotlib`, `seaborn`, `yfinance`, `openai`, `google-generativeai`  
* See `environment.yml` (or install manually with `pip`/`conda`).  

---

## 📜 License
MIT — see `LICENSE` file.

---

> **Straight talk:** The code is research-grade, not production-ready. Expect long runtimes and substantial API costs if you reproduce the LLM scoring step.




