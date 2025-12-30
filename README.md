# SAP HANA Calculation View – Sales Analytics (Hands-on)
 
## 📌 Overview
This project demonstrates an **end-to-end SAP HANA Calculation View** built from scratch using **SAP Business Application Studio (BAS)** and **HANA Cloud (HDI container)**.
 
The goal of this hands-on was to understand:
- HANA calculation view modeling
- Projections, joins, aggregation, and semantics
- HDI behavior (synonyms, build & deploy)
- Data validation using HANA Database Explorer
 
---
 
## 🎯 Use Case
**Sales Analytics – Total Sales per Customer**
 
The calculation view:
- Joins **Sales Orders** with **Customer master data**
- Aggregates sales amount per customer
- Exposes analytics-ready output
 
---
 
## 🧱 Data Model
 
### CUSTOMER (Master Data)
| Column | Type |
|------|-----|
| CUSTOMER_ID | INTEGER |
| CUSTOMER_NAME | NVARCHAR |
| REGION | NVARCHAR |
 
### SALES_ORDER (Transactional Data)
| Column | Type |
|------|-----|
| SALES_ID | INTEGER |
| CUSTOMER_ID | INTEGER |
| ORDER_DATE | DATE |
| AMOUNT | DECIMAL |
 
Tables are created using **`.hdbtable` artifacts** and deployed into an **HDI container**.
 
---
 
## 🧩 Calculation View Design
 
**View Name:** `CV_SALES_BY_CUSTOMER`  
**Type:** Cube (Graphical)  
**Data Category:** Cube with Star Schema  
 
### Node Flow
PROJ_SALES PROJ_CUSTOMER
\ /
\ /
JOIN_SALES_CUSTOMER
|
AGG_SALES
|
SEMANTICS

---
 
## 🔹 Projection Nodes
- **PROJ_SALES**
  - Columns: `SALES_ID`, `CUSTOMER_ID`, `AMOUNT`
 
- **PROJ_CUSTOMER**
  - Columns: `CUSTOMER_ID`, `CUSTOMER_NAME`, `REGION`
 
Projection nodes are used for:
- Column pruning
- Performance optimization
- Explicit column mapping
 
---
 
## 🔹 Join Node
- **Join Type:** Inner Join
- **Join Condition:**  
  `PROJ_SALES.CUSTOMER_ID = PROJ_CUSTOMER.CUSTOMER_ID`
- **Cardinality:** `N : 1`
- **Output Columns Mapped Explicitly**
 
This ensures:
- Correct aggregation
- No data duplication
- Clean analytics output
 
---
 
## 🔹 Aggregation Node
- **Group By (Aggregation = NONE):**
  - CUSTOMER_ID
  - CUSTOMER_NAME
  - REGION
 
- **Measure:**
  - `SUM(AMOUNT)` → renamed as `TOTAL_SALES`
 
Aggregation logic is defined using the **Columns** configuration.
 
---
 
## 🔹 Semantics Node
- **Attributes:**
  - CUSTOMER_ID
  - CUSTOMER_NAME
  - REGION
 
- **Measures:**
  - TOTAL_SALES
 
Semantics node controls how data is exposed for consumption.
 
---
 
## 🚀 Deployment
- Calculation View is deployed via **HDI build**
- No standalone “Activate” button is used
- Build compiles and deploys the view into the HDI container
 
---
 
## 🔍 Validation
- Data preview performed using **HANA Database Explorer**
- View available under:
 Column Views → CV_SALES_BY_CUSTOMER
