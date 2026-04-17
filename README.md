# Multifamily Risk Score
## Class A/B/C Apartment Market Analysis USA
**Author:** Daksha Gummadi | Springboard Data Science Capstone Two

---

## Problem

Real estate investors who own or manage multifamily apartment properties need to know where rents are heading. If rents are about to grow they want to invest more. If rents are about to fall they want to reduce exposure early. Most investors currently rely on lagging reports that come out months after market changes have already occurred.

The goal of this project is to build a machine learning model that predicts **year over year rent growth** for US multifamily properties using economic and market indicators that are available before rent changes actually show up in the data.

---

## Data Sources

| Source | Description | Link |
|---|---|---|
| Zillow ZORI | Observed Rent Index for 544 US metros 2015-2025 | [Zillow Research](https://www.zillow.com/research/data/) |
| FRED Vacancy Rate | National rental vacancy rate quarterly | [RRVRUSQ156N](https://fred.stlouisfed.org/series/RRVRUSQ156N) |
| FRED Building Permits | New housing permits issued monthly | [PERMIT](https://fred.stlouisfed.org/series/PERMIT) |
| FRED Timber PPI | Producer Price Index for lumber | [WPU081](https://fred.stlouisfed.org/series/WPU081) |
| FRED CPI Inflation | Consumer Price Index all urban consumers | [CPIAUCSL](https://fred.stlouisfed.org/series/CPIAUCSL) |
| FRED Unemployment | National unemployment rate | [UNRATE](https://fred.stlouisfed.org/series/UNRATE) |

---

## Project Steps

### 1. Data Wrangling
- Loaded Zillow ZORI data covering 544 metro areas and reshaped from wide to long format
- Calculated national average rent and year over year growth
- Converted quarterly vacancy data to monthly using interpolation
- Merged all three datasets on date after aligning date formats
- Created Class A, B, and C property segments based on rent percentiles each month
- Calculated year over year growth rates for each property class

### 2. Preprocessing
- Checked for categorical variables requiring dummy encoding (none found)
- Standardized all numeric features using StandardScaler to mean 0 and standard deviation 1
- Split data into 80 percent training and 20 percent testing sets

### 3. Exploratory Data Analysis
- Analyzed univariate distributions, time series trends, and feature relationships
- Compared Class A, B, and C performance across the full 2020 to 2025 period
- Ran correlation analysis and inferential statistics across property classes
- Engineered lag features for vacancy rate, permits, and rent growth

### 4. Modeling
- Tested stationarity of all features using the Augmented Dickey Fuller test
- Applied first differencing to vacancy rate, permits, and inflation which failed the test
- Used a 12 month sliding window to augment the dataset. Each window predicts the 13th month
- Built and compared three models: Linear Regression, Ridge Regression, and Random Forest
- Used TimeSeriesSplit cross validation to respect the time ordering of data
- Selected Random Forest as the final model

---

## Key Findings

- The rental market went through a dramatic boom and bust cycle from 2020 to 2025
- Rent growth peaked at 10 to 14 percent in 2021 and 2022 then turned negative by late 2025
- Vacancy rate dropped to 5.6 percent during the boom then climbed above 7 percent by 2025
- Building permits surged in 2021 and 2022 and those units hitting the market in 2024 and 2025 caused the oversupply
- Class A peaked highest at 14.5 percent growth. Class C was the most stable with smaller swings
- Building permits and vacancy rate were the most powerful predictors of rent growth

---

## Model Results

| Model | Train R2 | Test R2 | Test MAE | Test RMSE |
|---|---|---|---|---|
| Linear Regression | 0.9940 | -2.8658 | 1.1337 | 1.3335 |
| Ridge Regression | 0.9749 | -1.2368 | 0.8391 | 1.0144 |
| **Random Forest** | **0.9743** | **0.0756** | **0.5604** | **0.6521** |

**Final Model: Random Forest**

Random Forest was the only model with a positive Test R2 meaning it was the only one that actually learned useful patterns from the data. Linear Regression and Ridge Regression both overfit severely with negative Test R2 scores.

**Best Hyperparameters**
- n_estimators: 100
- max_depth: 3
- min_samples_leaf: 2

---

## Recommendations

1. **Watch building permits as a leading signal.** Permits were the most important predictor by a wide margin. When permits start declining from elevated levels it signals future supply pressure will ease and rents may stabilize.

2. **Use vacancy rate as a real time risk gauge.** When vacancy climbs above 6.5 to 7 percent it is a strong signal that rent growth will slow or turn negative. Operators should consider locking in longer lease terms before vacancy rises.

3. **Consider Class C properties for lower volatility.** Class A had the biggest swings peaking at 14.5 percent and dropping to negative. Class C was more stable and predictable for investors seeking consistent returns.

---

## Project Files

| File | Description |
|---|---|
| [Data Wrangling](Multifamily_Risk_Score_Data_Wrangling.ipynb) | Data loading, cleaning, merging and feature creation |
| [EDA](Multifamily_Risk_Score_EDA.ipynb) | Exploratory analysis, visualizations and correlation analysis |
| [Preprocessing](Multifamily_Risk_Score_Preprocessing.ipynb) | Dummy variable check, standardization and train test split |
| [Modeling](Multifamily_Risk_Score_Modeling.ipynb) | Stationarity testing, sliding window, model building and evaluation |
| [Final Report](Capstone_Final_Report.pdf) | Full project report as PDF |
| [Model Metrics](model_metrics.txt) | Final model parameters and performance metrics |

---

*Springboard Data Science Capstone Two | 2025*
