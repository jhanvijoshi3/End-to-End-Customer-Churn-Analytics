# Customer Churn Prediction & Risk Segmentation  
End-to-End SQL + Machine Learning + Power BI Solution

---

## 1. Executive Summary

This project delivers a complete churn analytics pipeline — from raw data cleaning in SQL to machine learning prediction and business-ready dashboards.

Using historical customer data (6,007 records), I built and evaluated multiple classification models to estimate churn probability. The final Random Forest model achieved:

- ROC-AUC: 0.90  
- Accuracy: 84%  
- Strong recall for churned customers (0.77)

The model was then applied to newly joined customers (411 records) to generate churn probability scores and segment them into actionable risk tiers.

A high-risk segment (≥ 0.80 probability) represented 68.6% of newly joined customers — driven primarily by contract type and early tenure characteristics.

---

## 2. Business Problem

Customer churn directly impacts recurring revenue and customer lifetime value. 

Acquisition cost is significantly higher than retention cost, making early churn detection critical.

The objective of this project was to:

- Identify churn drivers
- Predict churn probability at the individual customer level
- Segment customers by risk category
- Translate predictions into retention strategy recommendations

---

## 3. Data Engineering & Preparation (SQL)

### NULL Value Audit

A full NULL audit was performed across all columns using conditional aggregation.

Business-friendly replacements were applied:
- Service-related NULLs → “No”
- Value_Deal → “None”
- Churn_Category / Churn_Reason → “Others”

A production-ready table (`prod_churn`) was created to ensure clean downstream modeling.

### Dataset Segmentation via SQL Views

Two views were created:

- `vw_ChurnData` → Historical customers (Stayed + Churned)  
- `vw_JoinData` → Newly joined customers  

This separation ensures:
- Proper supervised model training
- Realistic forward-looking scoring

---

## 4. Exploratory Analysis

Dataset Overview:
- 6,007 customers
- 28.8% churn rate
- Avg tenure: 17 months
- Avg monthly charge: 65

Key Observations:

- Month-to-Month contracts show significantly higher churn.
- Early tenure customers (< 18 months) show elevated churn behavior.
- Revenue contribution varies by customer status.
- Contract flexibility appears strongly correlated with churn likelihood.

---

## 5. Machine Learning Approach

### Preprocessing

Implemented a production-grade pipeline using:

- ColumnTransformer
- StandardScaler (numerical features)
- OneHotEncoder (categorical features)
- Stratified 5-Fold Cross Validation
- Class imbalance handling via `class_weight='balanced'`

### Models Evaluated

Logistic Regression (Baseline)  
- CV ROC-AUC: 0.88  
- Test ROC-AUC: 0.885  
- Accuracy: 79%

Random Forest (Final Model)  
- CV ROC-AUC: 0.90  
- Test ROC-AUC: 0.899  
- Accuracy: 84%  

Random Forest demonstrated stronger discriminatory power and improved recall balance.

---

## 6. Churn Scoring & Risk Segmentation

The final model was applied to newly joined customers (411 records).

Each customer received:
- Churn Probability Score
- Risk Category:
  - High Risk (≥ 0.80)
  - Medium Risk (0.50–0.79)
  - Low Risk (< 0.50)

### Results

- Total new customers analyzed: 411  
- High-risk customers identified: 282  
- High-risk percentage: 68.61%

### Why Is the High-Risk Percentage So High?

The high-risk concentration is primarily explained by structural characteristics in the new-customer dataset:

1. 100% of high-risk customers are on Month-to-Month contracts.
2. Average tenure among high-risk customers ≈ 15 months.
3. Early-tenure + flexible contracts historically correlate with churn in training data.

This indicates that the model is learning genuine churn-driving patterns rather than random noise.

Important clarification:
This does NOT mean 68% will churn.
It means 68% have risk characteristics similar to past churners.

The probability score is a prioritization tool, not a deterministic outcome.

---

## 7. Business Recommendations

1. Offer discounted 12-month contract upgrades to high-risk Month-to-Month customers.
2. Launch targeted retention campaigns for top 20% highest probability customers.
3. Provide loyalty incentives within first 12–18 months.
4. Integrate churn scoring into CRM for monthly monitoring.
5. A/B test retention offers to measure ROI impact.

---

## 8. Power BI Dashboard

Developed an interactive dashboard featuring:

- Churn rate overview (27%)
- Total churn count (1,732)
- Churn by contract, payment method, tenure
- Geographic churn distribution
- High-risk joiner segmentation page

The dashboard translates model outputs into decision-support insights for leadership teams.

---

## 9. Limitations

- No hyperparameter tuning beyond baseline configuration.
- No SHAP or model explainability visualization implemented.
- No financial impact simulation performed.
- Model trained on historical snapshot; real-world performance may vary.

---

## 10. Next Steps

- Hyperparameter tuning (GridSearchCV)
- Gradient Boosting / XGBoost comparison
- SHAP-based explainability
- Retention ROI simulation
- API deployment (FastAPI / Flask)
- Automated monthly batch scoring

---

## Conclusion

This project demonstrates an end-to-end churn analytics workflow integrating:

- SQL-based data engineering
- Machine learning pipelines
- Risk segmentation
- Business intelligence reporting

The solution provides probabilistic churn estimation and actionable segmentation to support data-driven retention strategies.
