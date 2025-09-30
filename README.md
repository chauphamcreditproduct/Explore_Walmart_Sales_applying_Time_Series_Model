# 📈 Walmart_Holiday_Sales: Analyzing the Impact of Holidays on Weekly Sales Patterns

This project explores the effects of holidays on Walmart's weekly sales using time series analysis. By aggregating sales data and applying an Autoregressive (AR) model, we quantify seasonal patterns, holiday-driven spikes, and promotional impacts. The analysis focuses on data from 45 Walmart stores (2010–2012) and provides insights for retail optimization, such as inventory planning and targeted promotions.

The project includes a Jupyter notebook for data processing, modeling, and visualization, along with a comprehensive PDF report summarizing findings and recommendations.

---

## 🔍 Project Goals

- Investigate how holidays (e.g., Black Friday, Christmas) influence weekly sales compared to non-holiday periods.
- Quantify seasonal effects using time series modeling.
- Test the statistical significance of holiday vs. non-holiday sales.
- Provide actionable recommendations for retail strategies, including inventory management and promotions.

---

## 🛠️ Technologies Used

- Python (3.12), NumPy, Pandas, Seaborn, Matplotlib
- Datetime for date handling
- Jupyter Notebook (for code execution and visualization)
- CSV-based data ingestion (train.csv and features.csv from Kaggle)

---

## 📊 Dataset Overview

The dataset is sourced from the Walmart Sales Forecasting competition on Kaggle:
- **train.csv**: 421,570 records with Store, Dept, Date, Weekly_Sales, and IsHoliday (binary).
- **features.csv**: 8,190 records with Store, Date, Temperature, Fuel_Price, MarkDown1-5, CPI, Unemployment, and IsHoliday.
- Merged dataset: 6,435 records after aggregation by Store and Date, including derived features like IsMarkdown (binary indicator for promotions).

Key features: Weekly_Sales (target), IsHoliday, Temperature, Fuel_Price, CPI, Unemployment, IsMarkdown.

---

## 🔎 Exploratory Data Analysis (EDA)

The EDA process includes:
- Data quality checks (no duplicates in train.csv; handled missing values in features.csv).
- Aggregation of sales by Store and Date to focus on store-level trends.
- Descriptive statistics and visualizations (e.g., average weekly sales bar plots across stores).
- Time series plots for representative stores (e.g., Store 34 as the median performer).
- Identification of sales volatility tied to holidays (e.g., peaks in November–December).

![Weekly Sales for Store 34](https://github.com/yourusername/Walmart_Holiday_Sales/blob/main/figures/figure2.png)  
*Figure 2: Weekly sales for Store 34, showing high volatility with peaks likely tied to holiday periods.*

---

## 🧹 Data Cleaning & Feature Engineering

- **Aggregation**: Summed Weekly_Sales by Store and Date; dropped Dept and redundant IsHoliday.
- **Missing Values**: Created IsMarkdown (True if any MarkDown1-5 is non-null); retained CPI/Unemployment for analysis (no imputation needed for core model).
- **Feature Creation**: Monthly dummy variables for seasonal effects; IsMarkdown for promotional impact.
- **Data Splits**: Full dataset used for AR modeling (143 weekly observations per store from 2010–2012).

---

## 🤖 Modeling Approach

We applied an **Autoregressive (AR(4)) model** to predict Weekly_Sales, incorporating four lags to capture short-term dependencies (one-month structure). Monthly dummies and IsMarkdown were included to isolate holiday and promotional effects.

### Highlights:
- Features: Lagged sales (L1–L4), monthly dummies (Feb–Dec, ref: Jan), IsMarkdown.
- Model fit on 143 observations (March 2010–October 2012).
- Focus on coefficients for seasonal interpretation.
- In-sample predictions with 95% confidence intervals for validation.

### Evaluation Metrics:
The model shows good alignment with trends but underestimates extreme holiday spikes (e.g., R-squared not computed directly; visual fit assessed via predictions).

![AR(4) Model Results](https://github.com/yourusername/Walmart_Holiday_Sales/blob/main/figures/figure3.png)  
*Figure 3: AR(4) model coefficients, highlighting significant holiday impacts.*

![Predicted Weekly Sales](https://github.com/yourusername/Walmart_Holiday_Sales/blob/main/figures/figure4.png)  
*Figure 4: In-sample predictions for Store 34, with actual sales (blue) and 95% CI (shaded).*

---

### ✅ Model Insights & Key Coefficients

| Month      | Coefficient | Std Err | z      | P>|z|  | Interpretation                  |
|------------|-------------|---------|--------|--------|--------------------------------|
| Feb        | 300,600     | 5,086   | 5.915  | 0.000  | Sharp increase (Super Bowl + Valentine's). |
| Mar        | 180,300     | 4,082   | 4.417  | 0.000  | Modest uplift (Easter/St. Patrick's). |
| Apr        | 220,300     | 4,330   | 5.083  | 0.000  | Easter-driven gains.            |
| May        | 212,200     | 4,354   | 4.874  | 0.000  | Mother's/Memorial Day.          |
| Jun        | 212,800     | 4,264   | 4.991  | 0.000  | Father's Day/summer start.      |
| Jul        | 174,100     | 4,334   | 4.018  | 0.000  | Independence Day (smaller bump).|
| Aug        | 221,100     | 4,556   | 4.852  | 0.000  | Back-to-school.                 |
| Sep        | 178,000     | 4,414   | 4.033  | 0.000  | Labor Day.                      |
| Oct        | 205,400     | 4,666   | 4.402  | 0.000  | Halloween.                      |
| Nov        | 331,000     | 4,764   | 6.948  | 0.000  | Thanksgiving/Black Friday.      |
| Dec        | 439,400     | 5,586   | 7.866  | 0.000  | Christmas peak.                 |
| IsMarkdown | 25,710      | 1,397   | 1.841  | 0.065  | Marginal promotional boost.     |

**Key Insights**:
- **November–December peaks**: Largest coefficients (331K–439K), driven by major holidays.
- **Secondary peaks**: February/April/August (200K–300K) from events like Valentine's and back-to-school.
- **Markdowns**: Modest +$25K/week, marginally significant (p=0.065); supportive but not primary driver.
- All monthly coefficients significant (p<0.001), confirming strong holiday effects.

---

### 🔍 Model Fit & Business Fit

In-sample predictions align well with trends, with most actual values within the 95% CI. However, the model underestimates sharp holiday spikes, indicating a need for additional event-specific variables. This conservative fit supports risk-averse retail planning, prioritizing predictable trends over outliers.

---

## 📈 Feature Importance

Top contributors (based on AR coefficients):
- Lagged sales (L1–L4): Short-term persistence (coefficients ~0.3–0.9).
- Monthly dummies: Strong seasonal/holiday signals (e.g., Dec: 439K).
- IsMarkdown: Minor role in boosting sales during promotions.

---

## 💡 Business Insight

> Holidays like Black Friday and Christmas are critical revenue drivers, contributing up to 40%+ lifts over baseline months. Retailers can optimize by scaling inventory/staffing in Q4 and using targeted promotions in secondary peaks (e.g., back-to-school). Markdowns enhance but don't replace holiday demand—focus on high-precision planning to maximize profitability while minimizing overstock risks.

---

*Note: Replace `yourusername` with your actual GitHub username and ensure the figure links are updated with the correct paths once uploaded.*
