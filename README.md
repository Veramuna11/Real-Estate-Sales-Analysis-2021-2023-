# 🏡 Real Estate Sales Analysis (2021–2023)

# 📚 Table of Contents

- [🏡 Project Overview](#-project-overview)
- [🎯 Objective](#-objective)
- [🗂️ Data Source](#️-data-source)
- [🛠️ Tools & Techniques Used](#️-tools--techniques-used)
- [🧹 Data Cleaning & Preparation](#-data-cleaning--preparation)
- [📊 Exploratory Data Analysis (EDA)](#-exploratory-data-analysis-eda)
  - [Top Towns by Number of Sales](#top-towns-by-number-of-sales)
  - [Top Towns by Total Sale Value](#top-towns-by-total-sale-value)
  - [Average Sale Amount by Property Type](#average-sale-amount-by-property-type)
  - [Sales Trend Over Time (2021–2023)](#sales-trend-over-time-2021–2023)
  - [Sale Amount Distribution (Log Scale)](#sale-amount-distribution-log-scale)
  - [Sale Amount vs Assessed Value](#sale-amount-vs-assessed-value)
  - [Geographical Sales Concentration](#geographical-sales-concentration)
- [📋 Tableau Dashboard](#-tableau-dashboard)
- [🧠 Insights & Recommendations](#-insights--recommendations)
- [✅ Conclusion & Decision-Making Insights](#-conclusion--decision-making-insights)
- [🔭 Future Enhancements](#-future-enhancements)
- [📚 References](#-references)

A data analysis and visualization project focused on property sales trends across towns and property types, using Python and Tableau.

---

## 📄 Project Overview

This project analyzes real estate sales in Connecticut from 2021 to 2023 using an extended version of a government-sourced dataset. The goal is to explore how property type, location, and timing influence sale amounts and market trends.

Python was used for data cleaning and feature engineering, while Tableau was used to build an interactive dashboard showing top towns by sales, pricing trends by property type, and sales performance over time. The final output helps investors, analysts, and planners better understand local market behavior as well help in making data-driven decisions in the housing market.



## 📈 Objective
- Clean and preprocess raw real estate sales data using Python (Pandas).
- Develop key business insights through Exploratory Data Analysis (EDA).
- Build a professional Tableau dashboard to visualize real estate sales trends.
- Focus analysis specifically on sales recorded between **2021 and 2023**.

---

## 🗂️ Data Source

- Original Dataset: [Data.gov - Real Estate Sales (2001–2018)](https://catalog.data.gov/dataset/real-estate-sales-2001-2018)
- File Format: **CSV** (`.csv`)
- Note: This analysis specifically focuses on sales recorded between **2021 and 2023**, extracted from an updated version of the dataset (`Real_Estate_Sales_2001-2022_GL.csv`).


---

## 🛠️ Tools & Techniques Used

- **Python**: Used for data loading, cleaning, transformation, and feature engineering (via Pandas and NumPy).
- **Jupyter Notebook**: Served as the development environment for step-by-step data exploration and EDA.
- **Matplotlib**: Used to visualize distributions and trends before dashboarding.
- **Tableau**: Built the final interactive dashboard to present KPIs, sales patterns, and geospatial insights.
- **GitHub**: Version control and documentation for code, data, and visuals.
- **Markdown**: For writing this project documentation in a clean, readable format.


---

## 🧹 Data Cleaning & Processing
- Loaded dataset with 100,416 records and 14 columns.
- Converted `Date Recorded` to datetime format.
- Created `Recorded Year` and `Recorded Month` columns.
- Cleaned missing values:
  - Dropped sparse columns like `OPM remarks` and `Assessor Remarks`.
  - Filled missing values in `Residential Type` and `Non Use Code` with "Unknown."
- Extracted `Latitude` and `Longitude` from `Location` column for geospatial analysis.
- Created new engineered features:
  - `Price per $1k Assessed`
  - `Log Sale Amount` (for better distribution visualization)

---
## Data Analysis
**Load the Dataset and Inspect Basic Info**
```Python
import pandas as pd

# Load dataset
df = pd.read_csv("Real_Estate_Sales_2001-2022_GL.csv")

# Preview the first few rows
df.head()
```

** Convert Date Column & Clean Up Data Types**
```Python
# Convert 'Date Recorded' to datetime
df['Date Recorded'] = pd.to_datetime(df['Date Recorded'], errors='coerce')

# Trim string columns
str_cols = df.select_dtypes(include='object').columns
df[str_cols] = df[str_cols].apply(lambda x: x.str.strip())

# Extract Year and Month from 'Date Recorded'
df['Recorded Year'] = df['Date Recorded'].dt.year
df['Recorded Month'] = df['Date Recorded'].dt.month
```

** Handle Missing Values**
```Python
# Check percentage of missing values
missing_percent = df.isnull().mean().round(4) * 100
print("Missing percentage per column:\n", missing_percent)

# Drop low-value columns with excessive missing data
df.drop(columns=['OPM remarks', 'Assessor Remarks'], inplace=True)

# Fill missing 'Residential Type' and 'Non Use Code' with 'Unknown'
df['Residential Type'] = df['Residential Type'].fillna('Unknown')
df['Non Use Code'] = df['Non Use Code'].fillna('Unknown')
```

** Feature Engineering**
```Python
import numpy as np

# 1. Price per $1000 Assessed Value
df['Price per $1k Assessed'] = df['Sale Amount'] / (df['Assessed Value'] / 1000)

# 2. Log of Sale Amount (for visualizations)
df['Log Sale Amount'] = np.log1p(df['Sale Amount'])

# 3. Extract Latitude and Longitude from the 'Location' column
df[['Longitude', 'Latitude']] = df['Location'].str.extract(r'POINT \((-?\d+\.\d+)\s(-?\d+\.\d+)\)').astype(float)
```




## 📊 Exploratory Data Analysis (EDA)

During the exploratory phase, several key questions were investigated to better understand Connecticut's real estate market between 2021 and 2023.

---

### 🔹 Key Analytical Questions:

- What are the top towns by the number of sales and total sale value?
- How do average sale amounts differ across property types?
- What is the overall trend in sales volume over the years?
- How is the distribution of sale amounts shaped — is it skewed or uniform?
- How do sales amounts relate to assessed values?
- Where are sales geographically concentrated?

---

### 1️⃣ What are the top towns by number of sales and total sale value?


![top_towns_by_sales](https://github.com/user-attachments/assets/6d40896f-416b-4f9b-8836-2d88069764c5)


This chart highlights the towns with the highest volume of real estate transactions from 2021 to 2023:

- **Waterbury**, **Stamford**, and **Bridgeport** top the list, suggesting a highly active housing market in these urban centers.
- Other towns like **Norwalk**, **New Haven**, and **Hamden** also show strong transaction activity, likely due to their population size, rental markets, or accessibility.
- These areas represent **high-turnover zones**, where homes are bought and sold more frequently, possibly indicating affordability or higher mobility.

**Insight**:  
These towns are key hotspots for agents, rental investors, or developers looking for active markets with steady transaction flow.




![top_towns_by_total_value](https://github.com/user-attachments/assets/6b09c2b2-610a-4f74-8862-1c7d843476a4)

### 💰 Top 10 Towns by Total Sale Value

This chart focuses on where the **most money was exchanged** in real estate sales:

- **Stamford**, **Greenwich**, and **Westport** dominate in total sales value, signaling high property prices and a wealthier buyer base.
- Towns like **Darien**, **New Canaan**, and **Norwalk** follow closely — areas known for upscale residential neighborhoods.
- These towns may not lead in transaction volume but make up for it with **fewer, high-priced properties**.

**Insights**:  
Ideal for investors targeting **luxury real estate**, high returns per property, or long-term appreciation in affluent markets.


---

### 2️⃣ How do average sale amounts differ across property types?

![avg_sale_by_property_type](https://github.com/user-attachments/assets/2b68f41d-7ec6-4cdd-85b5-e4f92c31870e)

- **Apartments** had the highest average sale amounts, followed by **Industrial** and **Commercial** properties.
- **Residential** and **Vacant Land** types were more frequent but had significantly lower sale averages.
- This suggests that higher-end properties are concentrated in specialized property types like Apartments and Commercial real estate.


---

### 3️⃣ What is the overall trend in sales volume over the years?

![sales_per_year](https://github.com/user-attachments/assets/cf1267d5-e9d9-432d-b382-577b9627b740)


- Sales volume **peaked in 2022**, possibly due to economic rebound after COVID restrictions and favorable interest rates.
- **2021** had the lowest volume, and **2023** saw a slight decline, suggesting possible market normalization or affordability constraints.


---

### 4️⃣ How is the distribution of sale amounts shaped — is it skewed or uniform?

![log_sale_amount_distribution](https://github.com/user-attachments/assets/7516d6f7-ef33-45f9-a316-65cf994e2431)


- The **log distribution** of sale amounts forms a roughly **bell-shaped curve**, but with a **right-skew**, meaning most properties fall under the mid-range, with a few high-value outliers.
- This highlights a need to use log transformations when modeling or summarizing the data to reduce the effect of outliers.

---

### 5️⃣ How do sales amounts relate to assessed values?

![sale_vs_assessed_value](https://github.com/user-attachments/assets/c4bfcfb8-b80c-49d4-9aaa-0a0d3d3c4a42)


- A **positive correlation** is visible: properties with higher assessed values tend to sell for more.
- However, there is a **wide spread**, indicating potential discrepancies in property valuations or negotiation power.
- Useful for identifying **undervalued or overvalued** properties.
---

### 6️⃣ Where are sales geographically concentrated?

![sales_geographical_concentration](https://github.com/user-attachments/assets/fefbfc57-95fb-40d3-aa74-5512aa7572b7)


- Sales are **highly concentrated in the southern and coastal towns**, especially around urban hubs like **Stamford**, **Bridgeport**, and **Norwalk**.
- Central and rural parts of Connecticut show fewer transactions, consistent with population density and development levels.

---


## ✅ Conclusion & Decision-Making Insights

This project provided a comprehensive analysis of real estate sales trends in Connecticut from 2021 to 2023. Using Python for data preprocessing and Tableau for visualization, several critical insights emerged that can inform investment, development, and market strategies:

- 🏘️ **Top Towns**: Stamford, Greenwich, and Westport dominate the high-value market segment, while Waterbury and Bridgeport drive transaction volume. Decision-makers can prioritize marketing, development, or investment efforts based on whether they target high turnover or high ticket value.

- 🏢 **Property Types**: Apartments command the highest average sale prices, indicating strong demand for multifamily housing. Developers and investors may consider focusing future projects on apartments and industrial properties for higher returns.

- 📅 **Temporal Trends**: The peak in sales activity during 2022 signals strong post-pandemic demand but also suggests potential cooling into 2023. Strategic timing around market cycles can maximize opportunities.

- 📈 **Pricing Insights**: The log distribution of sale amounts reveals a highly skewed market where a small number of luxury transactions can distort average values. Analysts and investors should prefer medians over means when assessing typical sale behavior.

- 🏷️ **Assessed vs Sale Value Relationship**: While there is a general positive correlation between assessed and actual sale values, inconsistencies suggest opportunities for identifying undervalued properties through deeper valuation modeling.

- 🌍 **Geographical Focus**: Urban and coastal towns are the epicenters of real estate activity, making them critical regions for expansion, new project launches, or infrastructure investments.

---

**Final Recommendation**:  
By aligning strategies with top-performing towns, focusing on high-yield property types like Apartments, and monitoring shifts in sales momentum post-2022, stakeholders can make informed, data-driven decisions to optimize their investments, development projects, or municipal planning.

This analysis establishes a solid foundation for deeper predictive modeling and market forecasting in future phases.

---

## 📋 Tableau Dashboard

The final dashboard presents interactive insights on:
- Total sales value and volume
- Trends over time (2021–2023)
- Property type averages
- Top towns by sales and value
- Distribution of sale amounts (log scale)



🔗 (https://public.tableau.com/app/profile/purrisima.matthew/viz/RealEstateSalesAnalysis-Visualization/Dashboard)


---

## 🚀 Future Improvements
- Analyze external factors (e.g., mortgage rates, economic indicators) alongside sales.
- Predict future sale amounts using machine learning models.
- Expand analysis to more years and compare pre-pandemic vs post-pandemic trends.

---

## 📚 References
- [Real Estate Sales Data - Data.gov](https://catalog.data.gov/dataset/real-estate-sales-2001-2018)
- Internal documentation and sources used during analysis

---

