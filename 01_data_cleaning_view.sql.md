-- =============================================================================
-- Script 01: Telco Customer Churn Data Cleaning and Semantic View Setup
-- Database: PostgreSQL (Aiven / Local)
-- Schema: telco_churn
-- Description: Cleans data types, standardizes naming, handles whitespace 
--              in TotalCharges, and creates a production-ready analytical view.
-- =============================================================================

CREATE OR REPLACE VIEW telco_churn.v_clean_customer AS
SELECT 
    "customerID" AS customer_id,
    gender,
    "SeniorCitizen" = 1 AS is_senior_citizen,
    "Partner" = 'Yes' AS has_partner,
    "Dependents" = 'Yes' AS has_dependents,
    tenure AS tenure_months,
    "PhoneService" = 'Yes' AS has_phone_service,
    "MultipleLines" AS multiple_lines,
    "InternetService" AS internet_service,
    "OnlineSecurity" AS online_security,
    "OnlineBackup" AS online_backup,
    "DeviceProtection" AS device_protection,
    "TechSupport" AS tech_support,
    "StreamingTV" AS streaming_tv,
    "StreamingMovies" AS streaming_movies,
    "Contract" AS contract_type,
    "PaperlessBilling" = 'Yes' AS has_paperless_billing,
    "PaymentMethod" AS payment_method,
    ROUND("MonthlyCharges"::numeric, 2) AS monthly_charges,
    COALESCE(
        ROUND(NULLIF(TRIM("TotalCharges"), '')::numeric, 2), 
        0.00
    ) AS total_charges,
    "Churn" = 'Yes' AS is_churned
FROM telco_churn.customer;