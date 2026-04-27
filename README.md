# Quantifying the Environmental Cost of Agricultural Intensification in Sri Lanka

A Predictive Analysis of Deforestation and Land-Use Efficiency

This repository contains a comprehensive data science pipeline to analyze and model the relationship between agricultural expansion and forest cover loss in Sri Lanka. The project moves from raw data cleaning and hypothesis testing to high-performance ensemble modeling.

## Step 1: Problem Definition

While food security is improving, it appears to be at the direct expense of critical ecosystem services. This study aims to model the threshold where forest loss begins to yield diminishing returns for agricultural output. Current agricultural expansion relies on converting "marginal" lands (forests and shrublands) into productive fields. This leads to increased human-wildlife conflict, loss of biodiversity, and soil degradation, which eventually threatens the very yields the expansion intended to boost.

The "Sustainability Gap" in Sri Lanka suggests that food security gains are currently achieving a zero-sum relationship with forest conservation. This study aims to quantify the threshold where agricultural expansion leads to critical ecosystem degradation. We specifically investigate if increased productivity (yield/machinery) can be decoupled from horizontal land expansion (deforestation).

## Step 2: Data Collection

For the 70 percent of the world's poor who live in rural areas, agriculture is the main source of income and employment. But depletion and degradation of land and water pose serious challenges to producing enough food and other agricultural products to sustain livelihoods here and meet the needs of urban populations. Data presented here include measures of agricultural inputs, outputs, and productivity compiled by the UN's Food and Agriculture Organization.

    Time period of the dataset : 01 January 1960 - 31 December 2025
    Dataset Added on HDX : 19 November 2019
    Modified : 2 March 2026
    Expected update frequency : Every month

Source: World Bank Agriculture and Rural Development Indicators for Sri Lanka via HDX.
URL: https://data.humdata.org/dataset/world-bank-agriculture-and-rural-development-indicators-for-sri-lanka

The dataset includes longitudinal indicators (1961–2022) covering:

    Forest area (sq. km)
    Agricultural land (% and sq. km)
    Fertilizer consumption
    Cereal yield and Food Production Indices

    Indicators: Access to electricity, Agricultural land, Agricultural raw materials exports, Agricultural raw materials imports, Agriculture, Annual freshwater withdrawals, Arable land, Average precipitation in depth, Cereal production, Cereal yield, Crop production index, Employment in agriculture, Fertilizer consumption, Food production index, Forest area, Land area, Land under cereal production, Livestock production index, Permanent cropland, Rural land area, Rural land area where elevation is below 5 meters, Rural population, Rural population growth, Rural population living in areas where elevation is below 5 meters, Surface area

## Step 3: Data Understanding

Initial inspection revealed a high-dimensional time-series dataset. Key features were identified as heavily right-skewed with significant temporal dependencies. Indicator-specific coverage varies, requiring a pivoted structure to align all variables by year for multivariate analysis.

## Step 4: Data Cleaning and Preprocessing

A multi-tiered cleaning strategy was implemented to maintain data integrity:

    1. Imputation Strategy: * Low Missing (<10%): Linear interpolation to maintain time-series trends.
    2. Medium Missing (10-50%): Validated interpolation.
    3. High Missing (>50%): Forward/Backward fill to preserve the most recent policy-driven shifts.
    4. Outlier Handling: Outliers were retained as they represent genuine "shocks" (e.g., the 2021 organic policy shift).
    5. Normalization: StandardScaler was applied via ColumnTransformer to ensure scale-sensitive models (Linear Regression/Ridge)  performed optimally alongside tree-based models.

## Step 5: Exploratory Data Analysis

EDA highlighted a near-perfect inverse correlation (r ~ -0.98$) between the Food Production Index and Forest Area.
Time-series plots demonstrated that while cereal yields have stabilized, land expansion continues to encroach on forest boundaries, indicating a shift from "vertical" to "horizontal" growth.

## Step 6: Feature Selection

Features were selected based on their relevance to the "Sustainability Gap" hypothesis:

    Target: Forest area (sq. km)

Features: Food Production Index, Fertilizer consumption (kg/ha), Agricultural machinery (tractors), and Agricultural land area.

## Step 7: Hypothesis Testing

    Null Hypothesis (H_0): Changes in the Food Production Index have no relationship with, or do not precede, changes in Forest Area.

    Alternative Hypothesis (H_A): Increases in Food Production significantly correlate with and "Granger-cause" a reduction in Forest Area.

    H1 (Fertilizer Intensity): A T-test confirmed a statistically significant increase in chemical intensity post-2000 ($p < 0.05$) with a large Cohen's $d$ effect size.

    H2 (Land Expansion): Spearman Rho ($r_s > 0.9$) confirmed a strong monotonic upward trend in agricultural land area over time, directly correlating with forest decline.

## Step 8: Model Development

A diverse set of regressors were trained and compared:

    Baseline: Mean Predictor.
    Linear Models: Linear Regression and Ridge.
    Ensemble Models: Random Forest and Gradient Boosting Regressor.

## Step 9: Model Evaluation

Performance was benchmarked using four key metrics:

    R2 Score: Measures variance explanation (Target: >0.90).
    RMSE: Quantifies prediction error in sq. km.
    MAE: Average absolute error.
    MAPE: Percentage error to assess reliability across different decades.

## Step 10: Interpretation of Results

The Random Forest model emerged as the definitive winner.
While Gradient Boosting performed well in specific splits, Random Forest demonstrated superior Cross-Validation stability (lower CV-RMSE).
This indicates that Random Forest is better at generalizing across different "eras" of Sri Lankan agricultural policy and is less sensitive to the high volatility of the 2021–2022 economic shocks.

## Step 11: Conclusion

The analysis confirms that Sri Lanka's agricultural growth currently operates as a structural driver of deforestation. To mitigate this, the study advocates for Sustainable Intensification—shifting from horizontal expansion (clearing land) to vertical growth (optimizing yield on existing plots through technology). This transition is essential to decouple national food security from environmental degradation.

🛠 Tech Stack
    Languages: Python 3.x

    Data Science: pandas, numpy, scipy

    Visualization: matplotlib, seaborn

    Machine Learning: scikit-learn

    Preprocessing: StandardScaler, ColumnTransformer, Pipeline

    Models: RandomForestRegressor, GradientBoostingRegressor, Ridge, LinearRegression

    Metrics: r2_score, mean_squared_error, mean_absolute_error, mean_absolute_percentage_error
