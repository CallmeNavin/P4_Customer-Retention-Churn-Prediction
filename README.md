**Telco Customer Churn Prediction**

**VERSION 1**

**A. Project Overview**

- This project predicts the likelihood of customer churn based on transaction and contract information, in order to support effective retention strategies.

**B. Dataset Information**

- Source: Kaggle – WA_Fn-UseC_-Telco-Customer-Churn.csv
- Churn rate: 26.6% churn vs. 73.4% retained → imbalanced dataset

**C. Methodology**

- Basic EDA: shape, info, isnull, describe
- Converted TotalCharges from object → numeric & handled 11 missing values by fill = 0
- Applied one-hot encoding for categorical variables (drop_first=True)
- Dropped customerID as it has no predictive value.
- Standardized numeric variables prior to model training.

**D. Key Findings & Actionable Plans**

_**Modeling Strategy**_

- If the goal is to capture as many churn cases as possible (high Recall) → Logistic Regression tuned (Recall ≈ 79.7%).
- If the goal is to balance Precision and Recall → Random Forest tuned (F1 = 0.6323, highest ROC-AUC).

_**Key Findings**_

- High monthly charges and Fiber optic internet users show higher churn risk.
- Two-year contracts and longer tenure are strong retention drivers.
- Customers using Electronic check payments should be investigated further for pain points in their payment experience.

_**Actionable Plans**_

- 

**E. Appendix**

**Correlation Analysis**
Numeric (Point Biserial Correlation)
| Feature        | r\_pb  | Interpretation                                    |
| -------------- | ------ | ------------------------------------------------- |
| tenure         | -0.354 | Longer tenure → less likely to churn              |
| TotalCharges   | -0.199 | Lower accumulated payments → more likely to churn |
| MonthlyCharges | +0.193 | Higher monthly fees → more likely to churn        |

Categorical (Cramér’s V)
| Feature                         | V     | Interpretation                 |
| ------------------------------- | ----- | ------------------------------ |
| InternetService\_Fiber optic    | 0.307 | Strongly associated with churn |
| Contract\_Two year              | 0.301 | Longer contracts reduce churn  |
| PaymentMethod\_Electronic check | 0.301 | Strongly associated with churn |

**VERSION 2**

**A. Project Overview**

- This project predicts the likelihood of customer churn based on transaction and contract information, in order to support effective retention strategies.

**B. Methodology**

- Check outliers: For 03 numeric columns --> 0% outliers
- Predictive Analysis:
  + Define target columns (y): churn
    - One-hot encoding: to int
  + Prepare input columns (x): Other columns, except customerID
    - Comprehensive One-hot encoding:
      + For 06 Binary columns - by maps: to int
      + For 10 Category columns - by get_dummies: to bool
    - Drop customerID columns cause it has no predictive value
  + Train Split Test
  + Modeling
  + Model Evaluation (More chart: Heatmap, Boxplot for more visual)

**C. Key Findings & Actionable Plans**

**_Key Findings_**

**1. Logistics Regression**

- Accuracy Score_LR: 0.72
- Precision Score_LR: 0.48 --> Too low, below the average range 0.5
- Recall Score_LR: 0.83
- F1 Score_LR: 0.61
- ROC AUC Score_LR: 0.83 --> Model distinct churn/non churn good
- Confusion Matrix_LR:

![Confusion Matrix](https://github.com/CallmeNavin/P4_Customer-Retention-Churn-Prediction/blob/main/Version%202/Visualization/Confusion_Matrix_LR.png)

- Classification Report_LR:

![Classification Report_LR](https://github.com/CallmeNavin/P4_Customer-Retention-Churn-Prediction/blob/main/Version%202/Visualization/Classification%20Report_LR.png)

- ROC Curve & Precision Recall Curve_LR:

![ROC Curve & Precision Recall Curve](https://github.com/CallmeNavin/P4_Customer-Retention-Churn-Prediction/blob/main/Version%202/Visualization/ROC%20Curve%20%26%20Precision%20Recall%20Curve_LR.png)

**About Me**

Hi, I'm Navin (Bao Vy) – an aspiring Data Analyst passionate about turning raw data into actionable business insights. I’m eager to contribute to data-driven decision making and help organizations translate analytics into business impact. For more details, please reach out at:

🌐 LinkedIn: https://www.linkedin.com/in/navin826/

📂 Portfolio: https://github.com/CallmeNavin/
