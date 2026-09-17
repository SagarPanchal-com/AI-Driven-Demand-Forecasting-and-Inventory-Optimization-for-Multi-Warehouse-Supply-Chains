# AI-Driven Demand Forecasting & Inventory Optimization for Multi-Warehouse Supply Chains

![Python](https://img.shields.io/badge/Python-3.11-blue)
![Machine Learning](https://img.shields.io/badge/Machine%20Learning-XGBoost-green)
![Supply Chain](https://img.shields.io/badge/Supply%20Chain-Analytics-orange)
![Inventory Management](https://img.shields.io/badge/Inventory-Optimization-red)

---

# Project Overview

Modern supply chains face two critical challenges:

- Demand uncertainty
- Inventory inefficiency

Poor demand planning can lead to:

- Stockouts
- Excess inventory
- High holding costs
- Reduced service levels

This project develops an end-to-end Supply Chain Analytics System that combines:

- Machine Learning based Demand Forecasting
- ABC Inventory Classification
- Pareto Analysis
- Safety Stock Optimization
- Reorder Point Determination
- Economic Order Quantity (EOQ)
- Kraljic Procurement Matrix

The objective is to transform historical demand data into actionable inventory and procurement decisions.

---

# Business Problem

Organizations operating multiple warehouses and managing thousands of SKUs must answer:

### Demand Planning

- How much demand will occur next week or next month?

### Inventory Control

- Which products should receive the highest inventory attention?

### Replenishment

- When should inventory be reordered?

### Procurement

- Which products represent the highest procurement risk?

This project provides a complete analytical framework to answer all of these questions.

---

# Dataset Summary

The project uses four datasets.

| Dataset | Description |
|----------|-------------|
| Daily_Demand_History | Historical demand transactions |
| Inventory_Master_FINAL | Inventory parameters and cost structure |
| Warehouse_Stock_FINAL | Warehouse stock availability |
| Supplier_Data_FINAL | Supplier performance and procurement information |

---

# Project Architecture

```text
Historical Demand Data
            │
            ▼
      Data Cleaning
            │
            ▼
   Feature Engineering
            │
            ▼
     Demand Forecasting
            │
            ▼
      Annual Demand
            │
            ▼
       ABC Analysis
            │
            ▼
Inventory Optimization
│           │          │
▼           ▼          ▼
EOQ    Safety Stock   ROP
            │
            ▼
      Procurement
     Risk Analysis
(Kraljic Matrix)
            │
            ▼
 Business Insights
```

---

# Exploratory Data Analysis

The project begins with comprehensive exploratory analysis.

### Performed Analysis

- Data Quality Checks
- Missing Value Analysis
- Duplicate Detection
- Demand Distribution
- Warehouse Distribution
- Product Category Distribution
- Promotional Impact Analysis
- Holiday Impact Analysis
- Correlation Analysis
- Outlier Detection

---

# Feature Engineering

Several predictive features were created to improve forecasting performance.

## Calendar Features

- Day
- Month
- Quarter
- Week
- DayOfWeek
- Weekend Flag

## Demand Lag Features

- Lag 1
- Lag 7
- Lag 14
- Lag 30

## Rolling Features

- Rolling Mean 7
- Rolling Mean 14
- Rolling Mean 30
- Rolling Standard Deviation 30

## External Drivers

- Promotions
- State Holidays
- School Holidays
- Fuel Price

---

# Machine Learning Models

Multiple models were trained and compared.

| Model |
|---------|
| Linear Regression |
| Ridge Regression |
| Random Forest |
| XGBoost |

---

# Forecasting Pipeline

```text
Historical Demand
        │
        ▼
Feature Engineering
        │
        ▼
Train/Test Split
        │
        ▼
Model Training
        │
        ▼
Model Evaluation
        │
        ▼
Demand Prediction
```

---

# Inventory Optimization

Demand forecasts are converted into inventory decisions.

---

## ABC Analysis

ABC Analysis was performed using:

Annual Consumption Value

ACV = Annual Demand × Unit Cost

Products were classified as:

| Category | Meaning |
|----------|----------|
| A | High Value Products |
| B | Medium Value Products |
| C | Low Value Products |

### Business Insight

A small percentage of SKUs contribute to the majority of inventory value.

This confirms the Pareto Principle in inventory management.

---

## Pareto Analysis

Pareto analysis was used to identify critical inventory items.

### Business Insight

- A-class products require strict monitoring
- B-class products require periodic review
- C-class products require simplified inventory control

---

## Safety Stock Calculation

Safety stock protects the supply chain from uncertainty.

### Formula

Safety Stock = Z × σ × √Lead Time

Where:

- Z = Service Level Factor
- σ = Demand Variability

### Service Level Policy

| Category | Service Level |
|----------|--------------|
| A | 99% |
| B | 95% |
| C | 90% |

### Business Insight

High-value products maintain larger safety buffers to prevent stockouts.

---

## Reorder Point

### Formula

ROP = Average Daily Demand × Lead Time + Safety Stock

### Business Insight

Reorder Point provides an automatic replenishment trigger and minimizes stockout risk.

---

## Economic Order Quantity (EOQ)

EOQ minimizes total inventory cost.

### Formula

EOQ = √((2DS)/H)

Where:

- D = Annual Demand
- S = Ordering Cost
- H = Holding Cost

### Business Insight

EOQ balances:

- Ordering Cost
- Holding Cost

to identify the most economical order quantity.

---

# Procurement Analytics

## Kraljic Matrix

Products were categorized according to:

### Profit Impact

Measured through:

- Annual Consumption Value

### Supply Risk

Measured through:

- Lead Time
- Supplier Reliability
- MOQ

---

### Kraljic Categories

| Category | Description |
|-----------|-------------|
| Strategic | High Profit Impact + High Risk |
| Leverage | High Profit Impact + Low Risk |
| Bottleneck | Low Profit Impact + High Risk |
| Non-Critical | Low Profit Impact + Low Risk |

---

# Key Business Insights

### Demand Planning

- Promotional events significantly influence demand.
- Holiday periods create measurable demand fluctuations.

### Inventory Management

- A-class products contribute disproportionately to inventory value.
- Inventory control efforts should focus primarily on A-class products.

### Safety Stock

- Demand variability is a major driver of inventory requirements.
- Different service levels are required across inventory classes.

### Procurement

- Strategic products require supplier relationship management.
- Bottleneck products require risk mitigation plans.

### Cost Optimization

- EOQ reduces unnecessary ordering frequency.
- Optimized reorder points reduce stockouts while limiting excess inventory.

---

# Project Outcomes

The project successfully integrates:

✅ Demand Forecasting

✅ Inventory Classification

✅ Pareto Analysis

✅ Safety Stock Planning

✅ Reorder Point Optimization

✅ EOQ Optimization

✅ Procurement Risk Assessment

✅ Supply Chain Decision Support

---

# Repository Structure

```text
ai-demand-forecasting-inventory-optimization

│
├── data
│   ├── Daily_Demand_History.csv
│   ├── Inventory_Master_FINAL.csv
│   ├── Warehouse_Stock_FINAL.csv
│   └── Supplier_Data_FINAL.csv
│
├── notebooks
│   ├── Demand_Forcasting.ipynb
│   └── Inventory_Optimization.ipynb
│
├── images
│   ├── demand_distribution.png
│   ├── warehouse_distribution.png
│   ├── category_distribution.png
│   ├── correlation_heatmap.png
│   ├── model_comparison.png
│   ├── feature_importance.png
│   ├── abc_bar_chart.png
│   ├── pareto_chart.png
│   ├── safety_stock_distribution.png
│   ├── reorder_point_distribution.png
│   ├── eoq_distribution.png
│   └── kraljic_matrix.png
│
└── README.md
```

---

# Technologies Used

- Python
- Pandas
- NumPy
- Scikit-Learn
- XGBoost
- Matplotlib
- Seaborn

---

# Author

Sagar Panchal

B.Tech Production & Industrial Engineering

National Institute of Technology Kurukshetra
