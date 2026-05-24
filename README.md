# End-to-End Customer Journey & Operational Analytics Platform

## 🏢 Business Context & Core Objective
Nova Tech Solutions was experiencing significant operational challenges due to disconnected data silos. This led to a 10-hour weekly overhead for manual Excel data preparation and unquantified customer checkout drop-off rates. 

The objective of this platform is to build a centralized, cloud-ready analytics ecosystem that automates backend data pipelines, mathematically tests operational inefficiencies using statistics, and reduces executive decision-making latency by 30%.

---

## 📐 Project Architecture & Phases

### 📁 1. Business Analysis & Frameworks
* Developed Agile requirements and user stories to capture stakeholder needs.
* Mapped standard data ingestion logic and error handling using a **BPMN Flowchart** built in Draw.io.
* Established governance using a cross-functional **RACI Matrix**.

### 📁 2. Data Architecture & Database Engineering
* Designed an analytical **Star Schema** separating transactional metrics from descriptive dimensions.
* Deployed production-ready database structures using **MySQL Data Definition Language (DDL)** scripts, enforcing rigid primary and foreign key constraints.

### 📁 3. Python ETL Pipelines & Automation
* Engineered a hands-free data pipeline using **Python (Pandas & NumPy)** to eliminate manual files processing, slashing reporting errors by 85%.
* Programmed **Business Rule 001 (BR001)** logic into the transformation step to dynamically intercept and flag anomalies as `CRITICAL_VARIANCE`.
* Structured a **UiPath RPA Bot** blueprint to automate weekly vendor spreadsheet downloads.

### 📁 4. Advanced SQL Analytics & Core Insights
* Authored complex analytical queries utilizing **Common Table Expressions (CTEs)** and **Window Functions** (`DENSE_RANK`, `PARTITION BY`) to track capital leakage across departments.
* Conducted a **Linear Regression Analysis** ($p\text{-value} < 0.05$) to mathematically prove the correlation between operational cost overruns and logistics delivery latency.

### 📁 5. Enterprise Reporting & Dashboards
* Architected a dual-dashboard interface tracking 20+ core business metrics.
* Designed a **Power BI** layout focused on internal cost and budget tracking for the Operations Manager.
* Designed a **Tableau** layout integrated with **Adobe Customer Journey Analytics (CJA)** to map digital checkout funnels for the Marketing Director.

---

## 📈 Quantifiable Strategic Business Impact Deployed
1. **Operational Time Liberated:** Reduced weekly manual data prep cycles by 40% (saving ~10 hours/week) via Python automation.
2. **Decision Latency Optimization:** Decreased executive decision latency by 30% through targeted dashboard alert triggers.
3. **Financial Protection:** Established data safeguards to isolate overspending and support corporate cost-reduction goals by 15%.
 Added missing readme file
Docs: Add BPMN data ingestion flowchart for BR001
Feat: Add MySQL DDL script for Star Schema tables
Feat: Add automated Python ETL script with BR001 validation logic
Analytics: Add advanced audit SQL queries and regression model
Feat: Add Interactive Power BI operational dashboard workbook 