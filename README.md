# 🏪 Superstore Sales Analysis Dashboard  

## 🎯 Project Overview  
This project provides a comprehensive analysis of retail sales performance using *Power BI*.  
It explores revenue patterns, customer behavior, logistics efficiency, and product performance across multiple U.S. states — empowering stakeholders with data-driven insights for better decision-making.  

---

## 🎯 Primary Goals  
- Identify *top revenue-generating products* across all sales channels.  
- Analyze *state and city sales performance* to determine profitable locations.  
- Compare *customer segment sales trends* to optimize inventory distribution.  
- Recognize *seasonal sales trends* and *peak shopping periods*.  
- Evaluate *product categories* contributing most to total revenue.  
- Assess *logistics efficiency* across different *shipping modes*.  

---

## 🚩 Business Problem  
The Superstore operates across multiple states in the U.S., generating vast amounts of transactional data.  
However, several challenges affect efficiency and profitability:  

- *Revenue Optimization:* Lack of visibility into which products and locations contribute most to revenue.  
- *Customer Demand Trends:* Difficulty in predicting customer preferences and demand fluctuations.  
- *Store Performance Analysis:* Fragmented data hinders comparison of store performance.  
- *Inventory Management:* Inefficient tracking of high-demand vs. slow-moving products.  
- *Sales Channel Effectiveness:* Need to understand contributions from online and physical channels.  
- *Logistics Efficiency:* Limited visibility into shipping performance and product returns.  

This in-depth analysis provides clarity on key revenue drivers and operational performance.  

---

## ✅ Expected Outcomes  
- A *well-structured retail sales database*.  
- *Interactive Power BI dashboards* delivering actionable business intelligence.  
- Improved understanding of *sales trends, product performance, and logistics efficiency*.  

---

## 🧰 Technology Stack  
- *Excel:* Initial data exploration and preprocessing.  
- *Power BI:*  
  - Data Cleaning (Power Query)  
  - Data Modeling (DAX)  
  - Interactive Visualization and Reporting  

---

## 📊 Data Source  
- *Dataset:* Superstore Sales Dataset  
- *Structure:* 23 columns, 5,902 rows  

---

## 🧩 Project Workflow  

### 1️⃣ Data Preparation (Microsoft Excel)  
- Explored and understood dataset structure.  
- Verified column headers, data types, and consistency.  
- Imported .csv file into *Power BI Power Query Editor* for transformation.  

---

### 2️⃣ Data Cleaning & Transformation (Power BI – Power Query)  

#### Step 1: Remove Unnecessary Columns  
Dropped surplus columns (e.g., Ind1, Ind2) not relevant to project objectives.  

#### Step 2: Remove Duplicates  
- Removed duplicates using *Order ID* as the unique identifier.  
- Verified row count before and after deduplication.  

#### Step 3: Standardize Data Formats  
Ensured numeric fields (Sales, Profit, Quantity) were in decimal or currency format:  
```m
= Table.TransformColumnTypes(#"Removed Columns", {{"Profit", Currency.Type}, {"Sales", Currency.Type}})
```
Converted date fields (Order Date, Ship Date) to locale-specific formats:
```m
= Table.TransformColumnTypes(#"Added Custom", {{"Order Date", type date}, {"Ship Date", type date}}, "en-GB")
```
### 🧹 Step 4: Handle Missing Values  
- Checked each column for null values using filters.  
- Ensured data completeness in key fields.  

---

### 💾 Step 5: Load Clean Data  
Applied all transformations and loaded the cleaned dataset into *Power BI* for further modeling and visualization.  

---

## 🧩 3️⃣ Data Modeling  
- Designed an efficient schema optimized for analytical performance.  
- Established relationships:  
  - Superstore_Sales[Order Date] → DateTable[Date] (Active)  
  - Superstore_Sales[Ship Date] → DateTable[Date] (Inactive)  
- Kept the schema *denormalized* for simplicity and direct reporting.

<img width="2115" height="1389" alt="image" src="https://github.com/user-attachments/assets/1933225a-f5e5-4513-900a-49b7de11b3b7" />


## 📊 Key DAX Measures  

### 💰 Revenue  
```dax
Revenue = 
CALCULATE(SUM(SuperStore_Sales[Sales]))
```

🧍‍♂ Top Customer
```dax
Top Customer =
VAR TopCust =
    TOPN(
        1,
        SUMMARIZE(
            SuperStore_Sales,
            SuperStore_Sales[Customer Name],
            "CustomerRevenue", [Revenue]
        ),
        [Revenue],
        DESC
    )
RETURN
    MAXX(TopCust, SuperStore_Sales[Customer Name])
  ```
  📅 Date Table
  ```dax
  DateTable =
ADDCOLUMNS(
    CALENDARAUTO(),
    "Year", YEAR([Date]),
    "Month Number", MONTH([Date]),
    "Month Name", FORMAT([Date], "MMM"),
    "Quarter", "Q" & FORMAT([Date], "Q"),
    "Weekday Number", WEEKDAY([Date], 2),  -- 2 means week starts on Monday
    "Weekday Name", FORMAT([Date], "ddd"),
    "Start of Month", EOMONTH([Date], -1) + 1,
    "Quarter No", FORMAT([Date], "Q")
)
```
💸 Profit Lost
```dax
Profit Lost =
CALCULATE(
    SUM(SuperStore_Sales[Profit]),
    SuperStore_Sales[Returns] = "1"
)
```
⏱ On-Time Threshold
```dax
On-Time Threshold =
IF(
    SuperStore_Sales[Ship Mode] = "Same Day", 0,
    IF(
        SuperStore_Sales[Ship Mode] = "First Class", 3,
        IF(
            SuperStore_Sales[Ship Mode] = "Second Class", 5,
            IF(
                SuperStore_Sales[Ship Mode] = "Standard Class", 7,
                BLANK()
            )
        )
    )
```
)
<img width="3835" height="2225" alt="Screenshot 2025-11-05 152122" src="https://github.com/user-attachments/assets/5e35c557-39a0-4161-ad5c-250dc8009fb8" />
<img width="3840" height="2178" alt="image" src="https://github.com/user-attachments/assets/e5804940-ab38-4283-9d06-ae92539e383c" />

### Key Analysis Visual

.Sales by state

<img width="816" height="591" alt="image" src="https://github.com/user-attachments/assets/fffd05d0-0a08-4301-8a6b-53eee403bb91" />

*California* stands out as the top-performing state in terms of total revenue, generating approximately *$206K*.  
The visual highlights its consistent strength, showing that it maintained the *#1 position* from the previous year (PY) to the current year (TY) — demonstrating stable performance and strong market dominance.

.👥 Customer Profitability Analysis

<img width="1197" height="639" alt="image" src="https://github.com/user-attachments/assets/d53cffa3-d8c0-4c88-aaa4-6b2f71b3a5b4" />  

This visual provides a breakdown of individual customers’ purchasing strength and the profit each customer contributes.  
It helps identify the most valuable customers and highlights how much revenue and profit the company generates from each one.

### 🚚 Average Shipping Duration  

<img width="909" height="651" alt="image" src="https://github.com/user-attachments/assets/2119235d-8c50-4f4f-baa2-c104ee759a97" />


This visual provides insights into the efficiency of the logistics team by showing how average shipping duration changes across months.  
It highlights periods of strong performance as well as months with slower delivery times, prompting further investigation into the factors affecting logistics efficiency and consistency.

## 📊 Dashboard Visuals Overview

To ensure the dashboard delivers *actionable insights* and supports *data-driven decisions*, the following key visuals are included:

### 1. Revenue and Profit Breakdown by Category, Subcategory, and Product
Provides a detailed analysis of financial performance across product hierarchies, helping identify high- and low-performing items.

### 2. Top Revenue-Generating States and Cities
Highlights the best-performing geographical locations, enabling targeted strategies for regional sales growth.

### 3. Customer Segmentation Sales Performance
Compares sales trends across the three customer segments to understand purchasing patterns and customer value.

### 4. Sales Trend Over Time
Reveals seasonal sales patterns and overall purchasing behavior, assisting in forecasting and demand planning.

### 5. Logistics Ship Mode Performance
Analyzes delivery efficiency across the four available shipping modes, identifying the most preferred options and evaluating performance against SLA (Service Level Agreement) time frames.

## Superstore Sales Insights

<img width="3096" height="1731" alt="image" src="https://github.com/user-attachments/assets/24d18fe5-ca4b-4b82-900b-b72437f10c5f" />

## Categories Performance Insight

<img width="3101" height="1754" alt="Screenshot 2025-11-05 201651" src="https://github.com/user-attachments/assets/1ba81df8-ee7b-4a1c-8e3a-280d2b41fdec" />

## Customer details Overview

<img width="3090" height="1731" alt="image" src="https://github.com/user-attachments/assets/0cd73204-6fd0-4948-bc48-b5767f3dad80" />

## Product Sales Insight

<img width="3102" height="1746" alt="image" src="https://github.com/user-attachments/assets/7a2e2e73-10a1-479d-ab32-8b6bc05c12ca" />

## Logistics Performance Insight

<img width="3141" height="1734" alt="image" src="https://github.com/user-attachments/assets/9e1127c9-faab-4117-a9f6-dce3c07fe526" />

## 🔗 Power BI Dashboard Link

Click Link to report:
<iframe title="Superstore Sales Analysis" width="600" height="373.5" src="https://app.powerbi.com/view?r=eyJrIjoiNzM2NDgxZjItZDAwYS00ZjE2LWE1ZmUtNjM0Y2EyZGUzZGVlIiwidCI6ImMwOTA5OWE3LWY0MjItNDMyOC04Nzg2LTBlODRmYmNkOTJmNCJ9" frameborder="0" allowFullScreen="true"></iframe>

### 📈 Key Insights
3D Systems Cube Printer, 2nd Generation, Magenta has generated the highest revenue so far.
Carlifonia is the top-performing state in terms of revenue.
Consumer segment is the highest revenue generating segment, showing consistent growth.
Sales always increases during the 4th quater, largely due to holiday shopping trends.
Office Supplies were the best-selling product categories across all regions.





