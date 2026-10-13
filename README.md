# AI-Driven Demand Forecasting & Inventory Optimization for Multi-Warehouse Supply Chains

![Python](https://img.shields.io/badge/Python-3.11-blue)
![Machine Learning](https://img.shields.io/badge/Machine%20Learning-Random_Forest-green)
![Supply Chain](https://img.shields.io/badge/Supply%20Chain-Analytics-orange)
![Inventory Management](https://img.shields.io/badge/Inventory-Optimization-red)

An end-to-end supply chain analytics and decision-support project that connects machine-learning-based demand forecasting with SKU-level inventory planning, warehouse-wise inventory health monitoring, replenishment alerts, and procurement strategy recommendations.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Business Problem](#business-problem)
- [Project Scope](#project-scope)
- [Dataset Summary](#dataset-summary)
- [Project Architecture](#project-architecture)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Feature Engineering](#feature-engineering)
- [Demand Forecasting](#demand-forecasting)
- [Model Evaluation](#model-evaluation)
- [Inventory Optimization](#inventory-optimization)
  - [ABC Analysis and Pareto Analysis](#abc-analysis)
  - [Safety Stock](#safety-stock-calculation)
  - [Reorder Point](#reorder-point-rop)
  - [Economic Order Quantity](#economic-order-quantity-eoq)
  - [Inventory Control Insights](#inventory-control-insights)
- [Inventory Alerts and Monitoring](#inventory-alerts-and-monitoring)
- [Warehouse-wise Performance](#warehouse-wise-inventory-performance--replenishment)
- [Procurement Analytics: Kraljic Matrix](#procurement-analytics)
- [Inventory Policy Recommendation Engine](#inventory-policy-recommendation-engine)
- [Key Business Insights](#key-business-insights)
- [Analytical Outputs](#final-analytical-outputs)
- [Repository Structure](#repository-structure)
- [Technologies Used](#technologies-used)
- [How to Run](#how-to-run)
- [Assumptions and Interpretation Notes](#assumptions-and-interpretation-notes)
- [Project Outcomes](#project-outcomes)
- [Author](#author)

---

## Project Overview

Modern supply chains face two closely connected challenges:

- **Demand uncertainty** — customer demand changes over time due to seasonality, promotions, holidays, and other factors.
- **Inventory inefficiency** — excess stock ties up working capital, while insufficient stock increases the risk of stockouts and service-level failures.

Poor demand planning can lead to stockouts, excess inventory, high holding costs, and reduced service levels.

This project develops an end-to-end Supply Chain Analytics System that combines:

- Machine-learning-based demand forecasting
- Exploratory data analysis and feature engineering
- ABC inventory classification and Pareto analysis
- Safety stock calculation
- Reorder Point (ROP) determination
- Economic Order Quantity (EOQ) calculation
- Inventory control visualizations and SKU-level alerts
- Warehouse-wise inventory health and replenishment analysis
- Kraljic procurement risk classification
- Inventory policy recommendations

The objective is to transform historical demand and inventory inputs into actionable inventory, replenishment, and procurement decisions.

> **Data note:** The project uses structured, generalized/synthetic supply-chain inputs for analytical demonstration. The analysis should not be interpreted as reporting the confidential operational performance of a real company.

## Business Problem

Organizations managing multiple warehouses and thousands of SKUs need to answer four practical questions.

### Demand Planning
- How much demand is expected over the planning horizon?
- Which product, calendar, or promotional features are associated with demand variation?

### Inventory Control
- Which products account for the largest share of inventory consumption value?
- What inventory buffer is required to protect against demand variability?
- Which SKUs are at risk of stockout or excess stock?

### Replenishment
- When should a replenishment order be triggered?
- What quantity should be considered when placing an order?
- Which warehouses have the greatest replenishment burden?

### Procurement
- Which products have high profit impact and/or supply risk?
- Which inventory and supplier-management policies should be prioritized?

The project provides an analytical workflow to support these decisions.

## Project Scope

| Layer | Analysis / Deliverable |
|---|---|
| Demand analytics | Data quality checks, EDA, calendar and lag features |
| Forecasting | Linear Regression, Ridge Regression, Random Forest, and XGBoost comparison |
| Inventory classification | ABC classification and Pareto analysis |
| Inventory planning | Safety Stock, ROP, and EOQ |
| Inventory control | EOQ cost trade-off curves and inventory depletion curves |
| SKU monitoring | Critical, Warning, Healthy, and warehouse-level Overstock classification |
| Warehouse monitoring | Warehouse performance summary, status comparison, risk heatmap, replenishment gap |
| SKU prioritization | Top 20 SKUs ranked by ROP |
| Procurement | Kraljic Matrix and strategic-product analysis |
| Decision support | SKU-level inventory policies and recommendations |

## Dataset Summary

The project workflow uses four principal input datasets.

| Dataset | Description |
|---|---|
| `Daily_Demand_History.csv` | Historical daily demand records, product identifiers, calendar information, and demand drivers |
| `Inventory_Master_FINAL.csv` | SKU master information, product category, unit cost, lead time, ordering cost, and annual holding cost per unit |
| `Warehouse_Stock_FINAL.csv` | Current inventory positions by SKU and warehouse; warehouse identity is required for warehouse-level aggregation |
| `Supplier_Data_FINAL.csv` | Supplier information such as reliability and procurement parameters used for supply-risk analysis |

The demand workflow also creates a merged/final demand dataset for the inventory-planning stages.

## Project Architecture

```text
Historical Demand Data
        |
        v
Data Cleaning and EDA
        |
        v
Feature Engineering
        |
        v
Model Training and Evaluation
        |
        v
Random Forest Demand Forecast
        |
        v
Final Demand / Annual Demand
        |
        +-------------------------------+
        |                               |
        v                               v
ABC and Pareto Analysis           Inventory Master
        |                               |
        +---------------+---------------+
                        |
                        v
           Safety Stock, ROP and EOQ
                        |
                        v
             SKU-Level Inventory Alerts
                        |
                        v
           Warehouse Performance Summary
          /             |               \
         v              v                v
   Health Chart     Risk Heatmap    Replenishment Gap
                        |
                        v
             Kraljic Procurement Matrix
                        |
                        v
          Inventory Policy Recommendations
```

## Exploratory Data Analysis

The demand forecasting workflow begins with exploratory analysis to understand data quality, demand distribution, and potential demand drivers.

### Analysis performed

- Data quality checks
- Missing-value analysis
- Duplicate detection
- Demand distribution analysis
- Warehouse distribution
- Product-category distribution
- Promotional impact analysis
- State-holiday and school-holiday impact analysis
- Correlation analysis
- Outlier detection

EDA helps identify skewed demand patterns and provides context for feature engineering and model evaluation.

## Feature Engineering

Predictive features are created to represent calendar effects, historical demand patterns, and external demand drivers.

### Calendar features
- `Day`
- `Month`
- `Quarter`
- `Week`
- `DayOfWeek`
- Weekend flag

### Demand lag features
- Lag 1
- Lag 7
- Lag 14
- Lag 30

### Rolling features
- Rolling mean over 7 days
- Rolling mean over 14 days
- Rolling mean over 30 days
- Rolling standard deviation over 30 days

### External and operational drivers
- Promotions
- State holidays
- School holidays
- Fuel price
- SKU-level and category-level demand statistics, where available

Feature engineering helps the model represent recurring patterns and differences between products.

## Demand Forecasting

### Models evaluated

| Model | Purpose |
|---|---|
| Linear Regression | Baseline linear model |
| Ridge Regression | Regularized linear baseline |
| Random Forest Regressor | Non-linear ensemble model selected as the final model |
| XGBoost | Gradient-boosted tree model used for comparison |

The target is evaluated in log-transformed demand space to reduce the influence of the highly skewed demand distribution during model comparison. Predictions are inverse-transformed when converting the forecast back to demand units.

### Forecasting pipeline

```text
Historical Demand
        |
        v
Data Preparation
        |
        v
Feature Engineering
        |
        v
Train/Test Split
        |
        v
Model Training
        |
        v
Model Comparison
        |
        v
Selected Random Forest Model
        |
        v
Inverse Transformation to Demand Units
        |
        v
Final Demand Dataset
```

### Final model

**Random Forest Regressor** was selected because it achieved the strongest overall results among the evaluated models in the reported log-space comparison.

The trained model artifact is saved as `best_rf_demand_model.pkl`. The final demand output is saved as `Final_Demand.csv`.

## Model Evaluation

The following results are from the model-comparison run, with metrics evaluated in **log-transformed target space**.

| Model | R² (log target) | RMSE (log target) | MAE (log target) | WAPE (log target) |
|---|---:|---:|---:|---:|
| Random Forest | 0.8585 | 1.2543 | 0.8254 | 17.93% |
| Tuned XGBoost | 0.8529 | 1.2790 | 0.8486 | 18.43% |
| Ridge Regression | 0.6384 | 2.0052 | 1.6027 | 34.81% |
| Linear Regression | 0.6384 | 2.0052 | 1.6027 | 34.81% |

**Selection rationale:** Random Forest had the highest R² and the lowest RMSE, MAE, and reported WAPE among these models.

**Metric interpretation:** These WAPE values are calculated in log space and must not be interpreted as ordinary unit-level/business-scale WAPE. Demand includes zero and very small actual values as well as extreme high-demand observations, so percentage-error metrics on the original scale can behave very differently. The figures above describe the specific model-comparison run and may change if the data, split, features, or model settings change.

## Inventory Optimization

The demand output is used to calculate inventory-planning parameters. The inventory optimization notebook contains Sections 9–12, covering ABC analysis, Safety Stock, ROP, EOQ, control insights, inventory monitoring, warehouse-level analysis, the Kraljic Matrix, and policy recommendations.

### ABC Analysis

ABC Analysis prioritizes SKUs based on **Annual Consumption Value (ACV)**:

\[
ACV = Annual\ Demand \times Unit\ Cost
\]

| Category | Interpretation |
|---|---|
| A | High-value products requiring closer control |
| B | Medium-value products requiring periodic review |
| C | Lower-value products suitable for simpler controls |

The analysis produces a summary table and a bar-chart comparison of SKU share versus inventory-value contribution.

### Pareto Analysis

A Pareto chart ranks products by Annual Consumption Value and displays cumulative contribution to total inventory value.

Typical policy interpretation:

- **A-class products:** strict monitoring and tighter inventory control.
- **B-class products:** periodic review.
- **C-class products:** simplified controls where appropriate.

The actual category distribution depends on the input data and the classification logic used in the notebook.

### Safety Stock Calculation

Safety Stock provides a buffer against demand variability during supplier lead time.

The notebook uses the following model:

\[
Safety\ Stock = Z \times \sigma_d \times \sqrt{LT}
\]

Where:

- \(Z\) = service-level factor
- \(\sigma_d\) = standard deviation of daily demand
- \(LT\) = lead time in days

#### Service-level policy

| ABC Category | Service Level | Z-value |
|---|---:|---:|
| A | 99% | 2.33 |
| B | 95% | 1.65 |
| C | 90% | 1.28 |

The notebook also compares Safety Stock by ABC category and displays the Top 20 SKUs by Safety Stock.

### Reorder Point (ROP)

ROP defines the inventory position at which replenishment should be initiated.

\[
ROP = (Average\ Daily\ Demand \times Lead\ Time) + Safety\ Stock
\]

Where:

- Average Daily Demand = average forecasted daily demand for a SKU
- Lead Time = supplier replenishment lead time in days
- Safety Stock = inventory buffer for uncertainty

The notebook analyzes average ROP by ABC category and ranks the **Top 20 SKUs by Reorder Point**. A high ROP means a larger replenishment trigger level; it does **not** automatically mean that a SKU is currently understocked. Current stock must be compared with the ROP separately.

### Economic Order Quantity (EOQ)

EOQ estimates an order quantity that balances ordering and annual holding costs.

\[
EOQ = \sqrt{\frac{2DS}{H}}
\]

Where:

- \(D\) = projected annual demand in units
- \(S\) = ordering cost per order
- \(H\) = holding cost per unit per year

The notebook calculates EOQ by SKU, compares average EOQ across ABC categories, and presents the Top 20 EOQ SKUs.

### Inventory Control Insights

This section translates inventory parameters into visual explanations of inventory cost and stock behavior.

#### EOQ cost trade-off curves
For the five selected high-value products, the analysis visualizes:

- Ordering cost, which generally decreases as order quantity increases
- Holding cost, which generally increases as order quantity increases
- Total relevant cost
- The EOQ point, where the combined relevant cost is minimized under the model assumptions

#### Inventory depletion curves
For the selected high-value products, the curves illustrate:

- Current inventory position
- Daily demand/consumption rate
- Reorder Point
- Safety Stock
- Lead-time context
- The point at which replenishment should be considered

The notebook also presents a **Top 6 Products Inventory Control Summary** with Annual Demand, Safety Stock, ROP, and EOQ.

## Inventory Alerts and Monitoring

The notebook compares current stock with the calculated Safety Stock, ROP, and EOQ-related inventory parameters. The initial SKU-level alert table classifies inventory into three statuses:

| Status | Notebook interpretation |
|---|---|
| Critical | Current Stock is at or below ROP; immediate replenishment review is required |
| Warning | Current Stock is above ROP but within the warning band up to 1.2 × ROP |
| Healthy | Stock is outside the Critical and Warning conditions |

The SKU-level monitoring output is used to identify inventory requiring review. The warehouse-level extension adds an explicit Overstock category and a replenishment action label.

## Warehouse-wise Inventory Performance & Replenishment Dashboard

The enhanced inventory optimization workflow aggregates stock health by warehouse instead of limiting the analysis to individual SKU alerts.

### Warehouse-level summary

`Warehouse_Summary` provides the following KPIs for each warehouse:

| KPI | Interpretation |
|---|---|
| `SKU_Count` | Number of unique SKUs represented in the warehouse |
| `Total_Current_Stock` | Total on-hand stock across the warehouse records |
| `Total_ROP` | Sum of SKU-level reorder points |
| `Critical_Understock_SKUs` | Number of records classified as Critical / Understock |
| `Warning_SKUs` | Number of records classified as Warning |
| `Healthy_SKUs` | Number of records classified as Healthy |
| `Overstock_SKUs` | Number of records classified as Overstock |
| `Units_to_Replenish` | Total positive stock gap required to bring understocked records up to ROP |
| `Excess_Units_Above_ROP_EOQ` | Units above the ROP + EOQ review threshold |
| `Replenishment_Alert_SKUs` | Critical / Understock plus Warning records |

### Warehouse-level status logic

| Status | Condition | Operational interpretation |
|---|---|---|
| Critical / Understock | `Current_Stock <= Reorder_Point` | Replenishment review now |
| Warning | `Reorder_Point < Current_Stock <= 1.2 × Reorder_Point` | Monitor and plan replenishment |
| Overstock | `Current_Stock > Reorder_Point + EOQ` | Review excess stock and potential redistribution or purchase-plan changes |
| Healthy | Does not meet the preceding conditions | No immediate alert under this rule |

The `ROP + EOQ` threshold is a practical **overstock-review heuristic** used in this project, not a universal industry standard.

### Warehouse dashboard charts

The notebook creates the following inline visualizations:

1. **Warehouse-wise Inventory Health — stacked bar chart:** compares the number of Critical / Understock, Warning, Healthy, and Overstock SKU–warehouse records.
2. **Inventory Risk Heatmap by Warehouse:** highlights how inventory statuses are distributed across warehouses.
3. **Warehouse Replenishment Burden — horizontal bar chart:** compares the number of units needed to bring understocked records up to ROP.

### Warehouse-level CSV outputs

- `Warehouse_Inventory_Alerts.csv` — SKU/warehouse-level inventory status, stock gap, excess units, and replenishment alert.
- `Warehouse_Performance_Summary.csv` — aggregated warehouse KPIs.

**Input requirement:** `Warehouse_Stock_FINAL.csv` must include a warehouse identifier such as `Warehouse`, `Warehouse_Name`, `Warehouse_ID`, `Warehouse_Code`, or `Warehouse_Location`. The notebook can also reconstruct warehouse names from `Warehouse_Whse_*` one-hot columns. If neither a warehouse identifier nor those flags exist, warehouse-level aggregation cannot be performed correctly.


## Procurement Analytics

### Kraljic Matrix

The Kraljic Matrix classifies SKUs on two dimensions:

1. **Profit Impact**, represented by Annual Consumption Value.
2. **Supply Risk**, estimated using supplier reliability.

In the notebook, supply risk is calculated as:

\[
Supply\ Risk = 100 - Supplier\ Reliability
\]

The scores are normalized and used to assign each SKU to a quadrant.

| Kraljic Category | Profit Impact | Supply Risk | Typical strategic direction |
|---|---|---|---|
| Strategic | High | High | Long-term supplier partnerships and risk management |
| Leverage | High | Low | Bulk procurement and cost negotiation |
| Bottleneck | Low | High | Higher safety buffers and risk mitigation |
| Non-Critical | Low | Low | Simplified or automated replenishment |

The notebook visualizes the Kraljic Matrix, identifies top Strategic products, maps initial category-level policies, and exports the classification.

## Inventory Policy Recommendation Engine

The policy engine combines ABC classification with Kraljic category to map each SKU to an inventory-control recommendation.

### Inputs
- ABC Category
- EOQ
- Safety Stock
- Reorder Point
- Supplier reliability
- Kraljic Category

### Recommendation types represented in the workflow
- Continuous Review + Long-Term Contract
- EOQ Ordering + Weekly Review
- High Safety Stock + Backup Supplier
- Weekly Monitoring
- Periodic Review
- EOQ-Based Replenishment
- Additional Buffer Stock
- Monthly Review
- Supplier Development
- Bulk Ordering
- Automated replenishment and other mapped inventory policies

The notebook provides a policy summary and a policy-distribution chart. Recommendations are rule-based decision support, not automatically executed purchase orders.

## Key Business Insights

### Demand Planning
- Promotional events and holiday features are included as potential demand drivers.
- Demand is highly skewed, so the evaluation scale and inverse transformation must be considered when interpreting forecast metrics.

### Inventory Management
- ABC and Pareto analysis help focus monitoring on products with a larger share of annual consumption value.
- SKU-level status checks identify stock positions at or approaching the ROP.
- Warehouse aggregation helps compare inventory-health distribution and replenishment burden across warehouses.

### Safety Stock and Replenishment
- Demand variability and supplier lead time influence safety-stock requirements.
- ROP provides a replenishment trigger, while EOQ estimates an order quantity under the cost assumptions.
- The Top 20 ROP ranking prioritizes high replenishment thresholds but must be interpreted alongside current stock.

### Procurement
- Strategic items call for stronger supplier relationship management.
- Bottleneck items may require additional buffers and risk-mitigation plans.
- Kraljic classification links supplier risk and inventory value to differentiated policies.

### Cost and Operational Control
- EOQ trade-off curves illustrate the relationship between ordering and holding costs.
- Inventory depletion curves show when stock is expected to reach its reorder threshold under the modeled consumption rate.
- Warehouse replenishment-gap analysis estimates the units needed to bring understocked records up to ROP; this is not necessarily the final purchase-order quantity.

## Final Analytical Outputs

The notebooks generate or display the following analytical outputs when the relevant cells are executed.

| Output | Purpose |
|---|---|
| `Final_Demand.csv` | Merged demand output used for inventory planning |
| `ABC_Analysis.csv` | SKU annual demand, unit cost, annual consumption value, and ABC category |
| `Safety_Stock.csv` | SKU-level safety-stock calculation |
| `ROP_Analysis.csv` | SKU-level reorder point analysis |
| `EOQ_Analysis.csv` | SKU-level EOQ and inventory-cost parameters |
| `Inventory_Status_Analysis.csv` | Inventory monitoring / top stock-gap output from the existing alert section |
| `Warehouse_Inventory_Alerts.csv` | SKU/warehouse-level status, stock gap, excess units, and replenishment alert |
| `Warehouse_Performance_Summary.csv` | Warehouse-level inventory-health and replenishment KPIs |
| `Kraljic_Matrix_Analysis.csv` | SKU-level Kraljic category and procurement-risk fields |
| `Inventory_Policies.csv` | SKU-level policy recommendations |
| `best_rf_demand_model.pkl` | Trained Random Forest model artifact |

The notebook also displays tables and plots for ABC/Pareto analysis, Safety Stock, ROP, EOQ, EOQ cost trade-offs, inventory depletion, inventory status, warehouse health, warehouse risk, replenishment burden, Kraljic classification, and policy distribution.

## Repository Structure

The structure below reflects the intended project organization. Include generated CSVs and image files in the repository only if they have been exported and committed.

```text
AI-Driven-Demand-Forecasting-and-Inventory-Optimization-for-Multi-Warehouse-Supply-Chains/
│
├── data/
│   ├── Daily_Demand_History.csv
│   ├── Inventory_Master_FINAL.csv
│   ├── Warehouse_Stock_FINAL.csv
│   └── Supplier_Data_FINAL.csv
│
├── notebooks/
│   ├── Demand_Forcasting.ipynb
│   └── Inventory_Optimization.ipynb
│
├── outputs/
│   ├── Final_Demand.csv
│   ├── ABC_Analysis.csv
│   ├── Safety_Stock.csv
│   ├── ROP_Analysis.csv
│   ├── EOQ_Analysis.csv
│   ├── Inventory_Status_Analysis.csv
│   ├── Warehouse_Inventory_Alerts.csv
│   ├── Warehouse_Performance_Summary.csv
│   ├── Kraljic_Matrix_Analysis.csv
│   ├── Inventory_Policies.csv
│   └── best_rf_demand_model.pkl
│
├── images/
│   ├── correlation_heatmap.png
│   ├── model_comparison.png
│   ├── actual_vs_predicted_rf.png
│   ├── feature_importance.png
│   ├── abc_bar_chart.png
│   ├── pareto_chart.png
│   ├── safety_stock_distribution.png
│   ├── reorder_point_distribution.png
│   ├── eoq_distribution.png
│   ├── eoq_tradeoff_curve.png
│   ├── inventory_depletion_curve.png
│   ├── inventory_status_distribution.png
│   ├── warehouse_inventory_health.png
│   ├── warehouse_inventory_risk_heatmap.png
│   ├── warehouse_replenishment_gap.png
│   ├── kraljic_matrix.png
│   └── inventory_policy_distribution.png
│
└── README.md
```


## Technologies Used

- Python 3.11
- Pandas
- NumPy
- Scikit-learn
- Random Forest Regressor
- XGBoost
- Matplotlib
- Seaborn
- Jupyter Notebook

## How to Run

1. Clone or download the repository.
2. Create and activate a Python environment.
3. Install the required libraries.
4. Keep the input CSV files in the paths expected by the notebooks, or update the notebook paths to match your folder structure.
5. Run `Demand_Forcasting.ipynb` first to prepare the final demand output and forecasting artifact.
6. Run `Inventory_Optimization.ipynb` after the required demand, inventory-master, warehouse-stock, and supplier datasets are available.
7. Execute the notebook cells in order. The later inventory sections depend on variables and outputs created by earlier sections.
8. Export or save plots if you want to commit them under `images/`.

Example installation command:

```bash
pip install pandas numpy scikit-learn xgboost matplotlib seaborn jupyter
```

## Assumptions and Interpretation Notes

- **Demand scale:** The target is log-transformed during model comparison. Reported log-space metrics should not be presented as original-unit forecasting errors.
- **Safety Stock:** The formula shown uses the standard-deviation-of-demand and square-root-of-lead-time formulation. Its assumptions should be reviewed if lead-time variability or demand/lead-time dependence is material.
- **ROP:** The calculation uses average daily demand, lead time, and Safety Stock.
- **EOQ:** The classical EOQ formula assumes stable demand and cost inputs; real procurement constraints such as MOQ, pack sizes, capacity, and supplier constraints may require adjustment.
- **Overstock flag:** `Current_Stock > ROP + EOQ` is used as a review heuristic. It does not independently prove that stock is obsolete or financially excessive.
- **Replenishment gap:** `Units_to_Replenish` measures the positive gap to ROP. It is not automatically equal to the order quantity; EOQ, MOQ, open purchase orders, and operational constraints may also affect an order.
- **Warehouse identity:** Correct warehouse-level outputs require an actual warehouse identifier or the supported `Warehouse_Whse_*` flags. One-hot flags should represent one warehouse per record for the reconstruction logic to be unambiguous.
- **Generated outputs:** Output files and figures appear only after the relevant notebook cells execute successfully with compatible inputs.
- **Decision support:** This is an analytical project. Its rules and recommendations do not automatically execute procurement or warehouse transactions.

## Project Outcomes

The project integrates:

- Demand forecasting using Random Forest
- Feature engineering and demand-pattern analysis
- Comparison of Linear Regression, Ridge Regression, Random Forest, and XGBoost
- ABC inventory classification
- Pareto analysis
- Safety Stock planning
- Reorder Point calculation
- EOQ calculation and cost trade-off visualization
- Inventory depletion analysis
- SKU-level inventory health monitoring
- Warehouse inventory alerts
- Warehouse performance summary
- Replenishment alerts and stock-gap estimation
- Procurement risk assessment
- Kraljic Matrix analysis
- Inventory policy recommendation engine
- End-to-end supply chain decision support

## Author

**Sagar Panchal**  
B.Tech — Production & Industrial Engineering  
National Institute of Technology Kurukshetra
