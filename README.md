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
