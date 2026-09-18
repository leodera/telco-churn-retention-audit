-- =============================================================================
-- Script 02: Cohort Risk Auditing and Multi-Dimensional Risk Profiling
-- =============================================================================

-- 1. Executive Baseline Metrics
SELECT 
    COUNT(*) AS total_customers,
    COUNT(*) FILTER (WHERE is_churned) AS churned_customers,
    ROUND(100.0 * (COUNT(*) FILTER (WHERE is_churned)) / COUNT(*), 2) AS churn_rate_pct,
    ROUND(SUM(monthly_charges) FILTER (WHERE NOT is_churned), 2) AS active_mrr,
    ROUND(SUM(monthly_charges) FILTER (WHERE is_churned), 2) AS lost_mrr
FROM telco_churn.v_clean_customer;

-- 2. Internet Service vs Tech Support Retention Matrix
SELECT 
    internet_service,
    tech_support,
    COUNT(*) AS total_customers,
    COUNT(*) FILTER (WHERE is_churned) AS churned_customers,
    ROUND(100.0 * (COUNT(*) FILTER (WHERE is_churned)) / COUNT(*), 2) AS churn_rate_pct,
    ROUND(AVG(monthly_charges), 2) AS avg_monthly_charges
FROM telco_churn.v_clean_customer
GROUP BY 1, 2
ORDER BY 1, churn_rate_pct DESC;

-- 3. Multi-Factor "High Risk Cohort" Evaluation
SELECT 
    CASE 
        WHEN contract_type = 'Month-to-month' 
         AND tenure_months <= 12 
         AND internet_service = 'Fiber optic' 
         AND tech_support = 'No' 
        THEN 'High Risk Customers'
        ELSE 'All Other Customers'
    END AS customer_segment,
    COUNT(*) AS total_customers,
    COUNT(*) FILTER (WHERE is_churned) AS churned_customers,
    ROUND(100.0 * (COUNT(*) FILTER (WHERE is_churned)) / COUNT(*), 2) AS churn_rate_pct,
    ROUND(AVG(monthly_charges), 2) AS avg_monthly_charges,
    ROUND(SUM(monthly_charges) FILTER (WHERE is_churned), 2) AS segment_lost_mrr
FROM telco_churn.v_clean_customer
GROUP BY 1
ORDER BY churn_rate_pct DESC;