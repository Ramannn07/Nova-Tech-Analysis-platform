-- =========================================================
-- PHASE 4: ADVANCED BUSINESS ANALYTICS & AUDITING SUITE
-- PURPOSE: EXTRACT VARIANCE METRICS AND FINANCIAL COHORTS
-- =========================================================

-- ---------------------------------------------------------
-- QUERY 1: CORE VARIANCE AUDITING VIA SYSTEM CTEs
-- Objective: Isolate exactly which business units and vendors
-- are driving overspending anomalies mapped by BR001.
-- ---------------------------------------------------------
WITH CalculatedVariances AS (
    SELECT 
        fact.transaction_id,
        bu.business_unit_name,
        cat.vendor_name,
        cat.expense_type,
        fact.actual_cost,
        fact.budgeted_cost,
        (fact.actual_cost - fact.budgeted_cost) AS raw_variance,
        ROUND(((fact.actual_cost - fact.budgeted_cost) / fact.budgeted_cost) * 100, 2) AS variance_percentage,
        fact.variance_status
    FROM Fact_Operational_Transactions fact
    JOIN Dim_Business_Units bu ON fact.unit_id = bu.unit_id
    JOIN Dim_Expense_Categories cat ON fact.category_id = cat.category_id
)
SELECT 
    business_unit_name,
    vendor_name,
    expense_type,
    COUNT(transaction_id) AS total_breach_incidents,
    SUM(raw_variance) AS total_capital_leakage,
    ROUND(AVG(variance_percentage), 2) AS avg_percentage_over_budget
FROM CalculatedVariances
WHERE variance_status = 'CRITICAL_VARIANCE'
GROUP BY business_unit_name, vendor_name, expense_type
ORDER BY total_capital_leakage DESC;


-- ---------------------------------------------------------
-- QUERY 2: CHRONOLOGICAL COHORT TRACKING VIA WINDOW FUNCTIONS
-- Objective: Analyze historical spending trends over time without
-- flattening data, creating rolling metrics for executive review.
-- ---------------------------------------------------------
SELECT 
    cal.fiscal_year,
    cal.quarter,
    cal.month_name,
    bu.business_unit_name,
    SUM(fact.actual_cost) AS monthly_spend,
    
    -- Window Function: Calculate the average department spend across the entire fiscal year for baseline comparison
    ROUND(AVG(SUM(fact.actual_cost)) OVER(
        PARTITION BY bu.business_unit_name, cal.fiscal_year
    ), 2) AS annual_departmental_monthly_avg,
    
    -- Window Function: Rank departments by their total spending within each individual quarter
    DENSE_RANK() OVER(
        PARTITION BY cal.fiscal_year, cal.quarter 
        ORDER BY SUM(fact.actual_cost) DESC
    ) AS spending_rank_in_quarter
FROM Fact_Operational_Transactions fact
JOIN Dim_Calendar cal ON fact.date_id = cal.date_id
JOIN Dim_Business_Units bu ON fact.unit_id = bu.unit_id
GROUP BY cal.fiscal_year, cal.quarter, cal.month_name, bu.business_unit_name
ORDER BY cal.fiscal_year DESC, cal.quarter ASC, spending_rank_in_quarter ASC;