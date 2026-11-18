# CO2 Emission by Countries Year Wise (1750–2020)

## 1. Environmental Problem
Rising anthropogenic carbon dioxide ($CO_2$) emissions are the primary driver of global climate change and temperature anomalies. Understanding the underlying structural, demographic, and geographic factors that govern national emission levels is critical for designing equitable, long-term decarbonization policies rather than relying solely on historical emission trends.

## 2. Dataset Source
- **Dataset:** [CO2 Emission by Countries Year Wise (1750–2020) — Kaggle](https://www.kaggle.com/datasets/moazzimalibhatti/co2-emission-by-countries-year-wise-17502022)
- **Scope & Features:** Filtered to the modern industrial era (`1960–2020`), encompassing national records across `Year`, `Population(2020)`, `Area`, `% of World`, `Density(km2)`, and the engineered demographic pressure index `Pop_Area_Ratio`.

## 3. Research Questions
1. How accurately can national $CO_2$ emissions be modeled using solely structural, geographic, and demographic variables without relying on historical emission lag features?
2. Which structural and demographic factors exert the strongest influence on national $CO_2$ emissions across countries?
3. How do model interpretations (via SHAP) translate into actionable environmental and policy insights?

## 4. Methods
1. **Data Cleaning & Preprocessing:** Filtered records from 1960 onwards, removed non-positive emission entries, standardized string columns (`% of World`, `Density(km2)`), applied median imputation, and standardized features (`StandardScaler`).
2. **Feature Engineering & Leak Prevention:** Created `Pop_Area_Ratio` (`Population / Area`) and applied `log1p` transformation to normalize heavy target skewness. Deliberately excluded autoregressive lag features (`CO2_Lag1`) to prevent target leakage and isolate true structural drivers.
3. **Chronological Train-Test Split:** Split data temporally into Training (`1960–2013`) and Testing (`2014–2020`) sets to prevent temporal data leakage.
4. **Model Comparison & Tuning:** Evaluated 6 regression algorithms (Ridge, KNN, Decision Tree, Random Forest, Gradient Boosting, and XGBoost) and tuned XGBoost via `GridSearchCV` with `TimeSeriesSplit`.
5. **Explainable ML (XAI):** Conducted built-in feature importance ranking and SHAP (SHapley Additive exPlanations) summary analysis.

## 5. Main Results & Best Model
- **Best Model:** **Tuned XGBoost Regressor** (`learning_rate=0.03`, `max_depth=3`, `n_estimators=100`)
- **Test Set Performance (2014–2020):**
  - **MAE:** `1.4618` (Log scale)
  - **RMSE:** `1.8155` (Log scale)
  - **R² Score:** `0.6160`

![Actual vs Predicted](Actual%20vs.%20Predicted%20Log%20CO2%20emissions.png)

## 6. Feature Importance
Based on XGBoost importance scores and SHAP summary analysis, the primary structural drivers ranked by impact are:
1. **`Area` (Importance: ~0.29):** The strongest predictor of total national emissions.
2. **`Population(2020)` (Importance: ~0.21):** The second largest contributor to emission volume.
3. **`% of World` (~0.15) & `Pop_Area_Ratio` (~0.14):** Reflect national scale and demographic pressure.
4. **`Density(km2)` (~0.11) & `Year` (~0.10):** Capture urbanization concentration and temporal industrial growth.

![Feature Importance](Feature%20Importance.png)
![SHAP Summary Plot](SHAP%20Summary%20Plot.png)

## 7. Environmental Interpretation
- **Scale & Resource Footprint (`Area` & `Population`):** Countries with larger landmasses and populations exhibit substantially higher baseline $CO_2$ emissions due to extensive transportation networks, heating/cooling requirements, industrial infrastructure, and aggregate energy demand.
- **Demographic Pressure (`Pop_Area_Ratio` & `Density`):** High population density amplifies localized energy consumption and urban industrial activity, pushing emissions upward.
- **Policy Implication:** Because structural scale inherently drives higher baseline emissions, climate mitigation strategies in large, populous nations must prioritize energy-grid decarbonization, public transit electrification, and industrial efficiency rather than relying solely on population-level per-capita caps.

## Repository Structure
- `6971047221_Project_2.ipynb`: Full Google Colab notebook with code, outputs, and visualizations.
- `Actual vs. Predicted Log CO2 emissions.png`: Model evaluation plot.
- `Feature Importance.png`: XGBoost feature importance bar chart.
- `SHAP Summary Plot.png`: SHAP feature impact distribution plot.
