# B2B Sales Analytics — Applied Business Analytics (IPCA, 2025-26)

End-to-end analytics project on a real B2B dataset covering **$120M in revenue**, **262 clients**, and **43,127 invoices** (2017–2020). Built with Python/Jupyter for modelling and Power BI for executive reporting.

> Academic project — Instituto Politécnico do Cávado e do Ave (IPCA) | Applied Business Analytics 2025-26  
> Group: Edgar Couto, Steven Boaventura, Marcelo Dias

---

## 📊 Dashboard Preview

| Visão Geral | Análise RFM |
|---|---|
| ![Visão Geral](screenshots/visao_geral.png) | ![RFM](screenshots/rfm.png) |

| Deteção de Anomalias | Análise MBA |
|---|---|
| ![Anomalias](screenshots/anomalias.png) | ![MBA](screenshots/mba.png) |

| Previsão de Abandono | Análises de Clientes |
|---|---|
| ![Churn](screenshots/churn.png) | ![Clientes](screenshots/clientes.png) |

---

## 🎯 Business Questions Answered

| # | Question | Method |
|---|---|---|
| 1 | How to segment clients by purchase behaviour? | K-Means + RFM |
| 2 | Which products are bought together? | Market Basket Analysis (Apriori, FP-Growth) |
| 3 | Which clients are at risk of churning? | Random Forest Classifier |
| 4 | Are there anomalous transactions? | Isolation Forest + LOF + SMOTE |
| 5 | How to classify the product portfolio by revenue contribution? | ABC / Pareto Analysis |

---

## 📁 Dataset

6 interconnected files — 100% complete on key fields.

| File | Rows | Columns | Description |
|---|---|---|---|
| sales.csv | 43,127 | 12 | Sales / invoice records |
| orders.csv | ~43,000 | 11 | Order records |
| customers.csv | 262 | 8 | B2B client data |
| products.csv | ~250 | 10 | Product catalogue |
| employees.csv | ~20 | 9 | Sales team |
| packagetype.csv | ~10 | 3 | Packaging types |

---

## 🧠 Analytical Methods

### 1. Customer Segmentation — K-Means + RFM
RFM attributes (Recency, Frequency, Monetary) enriched with 6 additional features. K=4 selected via Elbow method and Silhouette Score (>0.40).

| Segment | Profile | Recommended Strategy |
|---|---|---|
| Premium | Low recency, high frequency and value | Loyalty and preferential treatment |
| Ocasionais | High recency, low frequency | Reactivation campaigns, upselling |
| Em Risco | Growing recency, disengagement signals | Proactive contact, personalised offers |
| Inativos | No recent purchases | Win-back or deprioritise |

### 2. Market Basket Analysis — Apriori + FP-Growth
Transaction unit: client (not invoice). Parameters: min_support=0.05, min_confidence=0.30, min_lift=1.38. Top pairs reach confidence of 87.5% and lift of 1.42 — concrete cross-selling opportunities.

### 3. Churn Prediction — Supervised Classification
Churn defined as no purchase in the last 6 months. Strong class imbalance (~8% churn) handled with stratified split. Main metric: AUC-ROC.

| Model | AUC Test | AUC Cross-Val |
|---|---|---|
| Logistic Regression | 1.000 | 0.994 |
| Random Forest | 0.937 | 0.941 |
| Decision Tree | 0.900 | ~0.88 |

Recency is the strongest predictor of churn (Random Forest feature importance).

### 4. Anomaly Detection — SMOTE + Isolation Forest + LOF
Anomaly definition: delivery ≥ 7 days OR negative lineprofit (~3% of transactions). SMOTE applied to training set. Random Forest with SMOTE achieved near-perfect Average Precision (~1.0). Unsupervised models (Isolation Forest, LOF) underperformed — anomalies are rule-based, not geometrically isolated.

### 5. ABC / Pareto Analysis
Classic Pareto confirmed: a small number of Class A products drive ~80% of revenue. Class C products evaluated for potential discontinuation.

---

## 📐 Power BI Data Model

Constellation schema with two fact tables (`sales`, `orders`) and four dimensions (`customers`, `products`, `employees`, `packagetype`). Includes a custom `DimDate` calendar table and K-Means segment labels imported from Python notebooks.

**Key DAX Measures:**
- `Total Revenue = SUM(sales[total_amount])`
- `Profit Margin % = DIVIDE([Total Profit], [Total Revenue], 0)`
- `Revenue YoY %` — year-over-year variance via SAMEPERIODLASTYEAR
- `ABC Class` — calculated column for product classification

---

## 📋 Dashboard Pages

- **Visão Geral** — KPIs: $120.83M revenue (+63.5% YoY), $52.35M profit, 262 clients, 1,282 anomalies. Quarterly revenue and client growth trends. Summary donuts for RFM segments, churn risk, and anomaly rate.
- **Análise RFM** — Segment metrics table, Recency vs Frequency scatter map, revenue by segment, top categories per segment.
- **Deteção de Anomalias** — Anomaly probability distribution, value comparison (anomalous vs normal), top 10 anomalous products, anomalies by packaging type.
- **Análise MBA** — Association rules table with support, confidence and lift. Top pairs by lift. Support vs Confidence scatter with lift as bubble size.
- **Previsão Abandono** — Churn probability distribution, churn by RFM segment, actionable table of 18 high-risk clients with revenue and recency.
- **Análises de Clientes** — Top 15 clients by revenue, revenue by territory, geographic maps of client and salesperson distribution (Azure Maps).

---

## 🛠️ Tools & Technologies

`Python` `Jupyter` `Pandas` `Scikit-learn` `MLxtend` `Power BI` `DAX` `Power Query` `Azure Maps`

---

## 📂 Repository Structure

```
├── Projeto_ABA_BI_G4.pbix       # Power BI report file
├── notebooks/
│   ├── ABA26_F01-G4.ipynb       # Customer segmentation (K-Means + RFM)
│   ├── ABA26_F02-G4.ipynb       # Market Basket Analysis
│   ├── ABA26_F03-G4.ipynb       # Churn prediction
│   ├── ABA26_F04-G4.ipynb       # Anomaly detection
│   ├── ABA26_F05-G4.ipynb       # ABC analysis + KPIs
│   └── ABA26_F06-G4.ipynb       # Exploratory Data Analysis
├── screenshots/
│   ├── visao_geral.png
│   ├── rfm.png
│   ├── anomalias.png
│   ├── mba.png
│   ├── churn.png
│   └── clientes.png
└── Relatorio_Projeto_Integrador_ABA_G4.pdf
```

---

## 💡 Key Findings

- **Rocky Mountain** is the top revenue territory (~$40M) but has **no dedicated salesperson** — a clear resource allocation gap.
- **2 Premium clients** generate **$71M** (~59% of total revenue) — extreme client concentration risk.
- **18 clients** identified as high churn risk, most with 100% predicted probability — actionable retention targets.
- Anomalous transactions cluster heavily in **"Each" packaging type** and specific product variants (Halloween zombie masks).
- Top product pairs reach **87.5% co-purchase confidence** — direct input for cross-selling campaigns.
