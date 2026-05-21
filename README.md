# UK Real Estate Market Analysis: Socio-Economic Nexus
### A Cluster-Based Econometric Study of UK Top-50 Housing Prices (2017-2025)

## Project Overview

This project identifies the macro and socio-economic drivers of housing prices across the **Top 50 UK cities, 2017-2025**, through a clustered Panel OLS approach. It is an *explanatory* study — it does not make forecasts. Its central contribution is an empirical finding about how the Elite London market actually works.

**Headline result.** The Elite London housing market is **not** structurally insulated from UK macro fundamentals, as the literature usually suggests. It is **two structurally different markets glued together at 2024Q1** — pre-2024 dominated by British residential investors responsive to UK rates and inflation, post-2024 dominated by foreign cash capital that does not respond to UK rates and reads UK inflation as a *negative* sterling signal. The same Chow F-test cannot reject that the two halves share a regression structure (F = 4.59, p = 0.0001). The CPI coefficient flips sign from +0.56 to −2.13 across the break — the smoking gun. Discussed formally in **§9.6**, recapped in **§10.3**.

## Key Findings — Where Each Result Lives in the Notebook

### In-sample fit per cluster (§8, §10.1)

| Cluster | Cities | n_obs | R²_adj | What it captures |
| :--- | ---: | ---: | ---: | :--- |
| Mid-Market | 15 | 465 | **0.77** | Mortgage-driven affordability — most rate-sensitive tier |
| Value | 28 | 868 | **0.72** | Affordable end; rate + inflation + migration all matter |
| Premium | 4 | 140 | 0.59 | Wealthier commuter belt; slower rate transmission |
| Elite | 3 | 105 | 0.35 | **Composition artefact — see §9.6** (resolves to R²_adj = 0.60 on the pre-2024 sub-sample) |

Mid and Value are the high-confidence fits: large n, theory-consistent coefficients, brute-force optimum lag structures, and signal-to-noise ratios of **19-24×** above a permutation noise floor (§7.11). Elite and Premium are smaller cross-sections (n ≈ 100), with weaker signal-to-noise (~6-7×) — discussed honestly in **§7.13**.

### Headline coefficients (§8, §10.2)

| Channel | Mid β | Premium β | Value β | Elite β |
| :--- | ---: | ---: | ---: | ---: |
| BoE Rate (1 pp hike → annual price growth) | **−2.1 pp** | −1.85 pp | −1.4 pp | −1.0 pp |
| CPI Inflation (1 pp → annual price growth) | **+1.08 pp** | +0.69 pp | +1.15 pp | +0.59 pp |

All coefficients of theory-consistent sign; most significant at the 1 % level; all robust against permutation tests (§7.11), top-20 lag-stability checks (§7.9), and a formal collinearity filter that constrains the Rate-CPI pair to |r| < 0.4 (§7.14).

### The Elite Paradox — formally resolved (§6 → §9.6 → §10.3)

- **§6 hypothesis.** Elite London's weak R² reflects structural insulation from UK macro — it follows global capital flows, not domestic fundamentals.
- **§9.1 Bai-Perron sup-F.** Locates a structural break in the Elite cluster at **2024Q1**.
- **§9.6 formal test.** Re-fitting the §8 Elite specification separately pre- and post-break:
  - Pre-2024 (n = 81): R²_adj = **+0.60**, all macro coefficients significant at p < 0.001.
  - Post-2024 (n = 24): R²_adj = **−0.09**, no coefficient significant.
  - **Chow F = 4.59, p = 0.0001** — formal rejection of structural equality.
  - **CPI coefficient sign-flips** from +0.56 (inflation hedge for British residential investors) to −2.13 (foreign capital reading UK CPI as a negative sterling signal).
- **Economic interpretation.** The 2023Q3 BoE rate peak at 5.25 % made leveraged UK property purchases uneconomic relative to gilt yields. British residential investors rotated out; foreign cash capital became the marginal price-setter at the top of London. The same macro inputs now drive opposite-sign price responses — which is why a single-regime fit averages to weak R².

The "Elite Paradox" is therefore not a structural property of Elite housing — it is a **composition artefact** of pooling two regimes with very different macro-sensitivity. The §8 Elite specification should be read as a characterisation of British residential investors' reaction function during the period when British investors actually set prices, not as a generic "UK macro → Elite prices" production function.

## Notebook Structure

The notebook is **91 cells (~1.5 MB)**, fully pre-rendered (every code output is embedded — readers do not need to execute anything).

1. **§1 — §5.** Data engineering, weak-baseline ML showcase, K-Means clustering, cluster-specific Panel OLS baseline.
2. **§6.** Formal statement of the Elite Paradox hypothesis (to be tested in §9).
3. **§7.** Fourteen subsections of specification validation: silhouette analysis, CCF lag selection, per-cluster CCF, multicollinearity discovery (r = 0.97 between Rate@4 and CPI@6), brute-force lag search (9⁶ ≈ 531 k specs per cluster), permutation tests, top-20 stability, collinearity-filtered re-search.
4. **§8.** Final per-cluster specifications with all four Panel OLS fits and diagnostic plots.
5. **§9.** Structural change analysis: Bai-Perron supF for all four clusters, pre-2024 out-of-sample validation for Elite/Premium, honest "why Mid/Value cannot be validated this way" explanation, and **§9.6 Elite composition-shift Chow test**.
6. **§10.** Conclusion: in-sample fit summary with bar charts and economic readings; coefficient comparison; Elite paradox resolution recap; what the study delivers and what it does not.
7. **Appendix.** Guided tour of every raw dataset (Price Index, CPI, Crimes, Asylum, Visas) with `head(10)` previews and explicit preprocessing documentation.

## Methodology Highlights

**Standard data pipeline.**
- 7 raw datasets aggregated into a quarterly panel of UK Top-50 cities (§1, Appendix).
- K-Means clustering on (AveragePrice, Average_Income) with `k = 4` chosen by economic theory rather than the elbow method (§3, §7.1 silhouette).
- Stationary transforms: year-over-year differencing for stocks, year-over-year percentage change for indices (§4).
- Cluster-specific Panel OLS with city fixed effects (§5, §8).

**Beyond-baseline econometrics.**
- **Cross-correlation function (CCF)** for lag selection per cluster and per predictor (§7.2, §7.5).
- **Multicollinearity diagnostics**: VIF, condition number, pairwise correlation map for the macro lag grid (§7.6, §7.14).
- **Brute-force lag optimisation** with within-demeaning trick — 9⁶ specs in ~4 min per cluster (§7.8).
- **Permutation test** for in-sample noise floor — 19-24× signal-to-noise for Mid/Value, 6-7× for Elite/Premium (§7.11).
- **Collinearity filter** on the brute-force winner: constrained re-search restricted to (Rate, CPI) pairs with |r| < 0.4 (§7.14).
- **Bai-Perron sup-F test** for structural breaks on within-demeaned panel data (§9.1).
- **Chow F-test** for formal parameter equality across regimes (§9.6).

## Tech Stack

- **Languages:** Python
- **Data engineering:** `pandas`, `numpy`
- **Machine learning (baselines only):** `scikit-learn` (K-Means, LinearRegression), `xgboost`, `Ridge` — used in §2 to motivate why a single ML pass on absolute prices is the wrong approach.
- **Econometrics:** `statsmodels` (OLS with `C(RegionName)` fixed effects, ARIMA)
- **Statistical testing:** `scipy.stats` (Chow F-test), manual implementation of Bai-Perron sup-F
- **Visualisation:** `matplotlib`, `seaborn`

## Repository Structure

- `UK_Real_Estate_ML_Full_Project.ipynb` — the primary notebook (91 cells, ~1.5 MB, fully pre-rendered).
- `example raw data/` — sampled raw datasets needed to re-execute the notebook locally.
- `reference/` — full original raw datasets (where licensing permits).

## What This Study Delivers and Does Not (recap of §10.4)

**Delivered.**

1. A robustness-tested **four-cluster segmentation** of UK Top-50 cities with cluster boundaries reflecting economic theory.
2. A **per-cluster macro specification** explaining 35-77 % of in-sample annual price-growth variation, with all coefficients of theory-consistent sign.
3. Formal demonstration that **Mid-Market and Value tiers respond to interest rates and inflation in textbook ways** (Rate β = −1.4 to −2.1, CPI β = +1.1 to +1.2 across the two clusters; n = 1 333 combined).
4. **The formal resolution of the Elite Paradox as a composition artefact** — the headline finding — with an empirically identified break point (2024Q1) and an economically substantive interpretation (British → foreign buyer rotation triggered by the BoE rate peak).

**Not delivered.**

1. **Out-of-sample forecasting** of future quarters. This is an explanatory study; no forecast claims are made about 2026+.
2. **A working model for post-2024 Elite.** UK-macro features are the wrong inputs; a proper post-2024 Elite model needs GBP, FTSE, and global-wealth proxies that this dataset does not contain.
3. **Single-city predictions** within a cluster — the model is identified at cluster level.
4. **Formal causal identification** — the model captures associations consistent with theory; it does not exploit an instrument or natural experiment.

## License & Data Attribution

This project uses public sector information licensed under the **Open Government Licence v3.0**. Data sources: HM Land Registry (UK House Price Index), Office for National Statistics (Consumer Price Inflation, Average Income), Bank of England (Base Rate), Home Office (Asylum Claims, Short-term and Long-term Visas).
