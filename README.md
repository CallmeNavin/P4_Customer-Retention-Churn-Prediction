**Telco Customer Churn Prediction**

**VERSION 1**

**A. Project Overview**

- This project predicts the likelihood of customer churn based on transaction and contract information, in order to support effective retention strategies.
- Version 1 target: **Focus on feature importance, correlation analysis, and hyperparameter tuning**

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
- Version 2 target: **Focus on end-to-end predictive pipeline (data prep → modeling → evaluation) with clean visualization.**

**B. Methodology**

- Basic EDA
- Actions before Basic EDA:
  + Change dtype of "TotalCharges" column from object to numeric
  + Fill 11 nulls in "TotalCharges" column
- Outlier Check for 03 numeric columns: tenure, MonthlyCharges, TotalCharges → 0% outliers.
- Predictive Analysis Setup:
  + One-hot encoding:
    - For Binary cols (by map): Partner, Dependents, PhoneService, PaperlessBilling, Churn, gender
    - For others columns (by get_dummies): MultipleLines, InternetService, OnlineSecurity, OnlineBackup, DeviceProtection, TechSupport, StreamingTV, StreamingMovies, Contract, PaymentMethod
  + Drop non-predictive value columns: customerID
  + Last set-up stage:
    - Define target column (y) & input columns (x)
    - Train/Test Split
- Modeling (no tuning applied in this version):
  + Logistic Regression
  + Random Forest Classifier
  + XGBoost Classifier.

**C. Key Findings & Actionable Plans**

**I. Key Findings**

**I.1. Logistics Regression**

- Accuracy Score: 0.69 → Good
- Precision Score: 0.46 → Too low, below the average range (0.5). Many false positives.
- Recall Score: 0.81 → High, few churn cases missed.
- F1 Score: 0.59 → Moderate, showing imbalance between Precision and Recall.
- ROC AUC Score: 0.81 → Good model performance in distinguishing churn vs. non-churn.
- Confusion Matrix: 

![Confusion Matrix](https://github.com/CallmeNavin/P4_Customer-Retention-Churn-Prediction/blob/main/Version%202/Visualization/Confusion_Matrix_LR.png)

- Classification Report_LR:

![Classification Report_LR](https://github.com/CallmeNavin/P4_Customer-Retention-Churn-Prediction/blob/main/Version%202/Visualization/Classification%20Report_LR.png)

- ROC Curve & Precision Recall Curve_LR:

![ROC Curve & Precision Recall Curve](https://github.com/CallmeNavin/P4_Customer-Retention-Churn-Prediction/blob/main/Version%202/Visualization/ROC%20Curve%20%26%20Precision%20Recall%20Curve_LR.png)

- The model performs well in distinguishing churn vs. non-churn (AUC = 0.83).
  + High Recall (0.83) means the model captures most churn cases.
  + Low Precision (0.48) indicates many false positives (non-churn customers misclassified as churn).

→ Trade-off: catching all churn cases requires accepting lower Precision, and vice versa. 

→ Logistic Regression: Good at capturing churn (Recall = 0.81), but suffers from high false positives (Precision = 0.46). Suitable when business goal is not to miss any churn customers, even at the cost of contacting many non-churn ones.

**I.2. Random Forest Classifier**

- Accuracy Score: 0.78 → Good
- Precision Score: 0.62 → Pretty good than Logistics Regression
- Recall Score: 0.48 → Bắt churn không xịn bằng Logistics Regression
- F1 Score: 0.54 → Moderate, but this model show more balance between Precision & Recall than Logistics Regression
- ROC AUC Score: 0.82 → Good model performance in distinguishing churn vs. non-churn, even better than Logistics Regression's score 0.01
- Confusion Matrix:

![Confusion Matrix](https://github.com/CallmeNavin/P4_Customer-Retention-Churn-Prediction/blob/main/Version%202/Visualization/Confusion_Matrix_RF.png)

- Classification Report:

![Classification Report](https://github.com/CallmeNavin/P4_Customer-Retention-Churn-Prediction/blob/main/Version%202/Visualization/Classification%20Report_RF.png)

- ROC Curve & Precision Recall Curve:

![ROC Curve & Precision Recall Curve](https://github.com/CallmeNavin/P4_Customer-Retention-Churn-Prediction/blob/main/Version%202/Visualization/ROC%20Curve%20%26%20Precision%20Recall%20Curve_RF.png)

→ Random Forest: Improves Precision to 0.62 while maintaining reasonable Recall (0.48). Provides a more balanced trade-off, suitable when business aims to optimize retention cost and reduce false positives.

**I.3. XGBoost**

- Accuracy Score: 0.80 → More precise in predicting than Logistics Regression & Random Forest Classifier
- Precision Score: 0.65 → Even better than Logistics Regression & Random Forest Classifier, lower costs
- Recall Score: 0.53 → Even better than Logistics Regression & Random Forest Classifier, higher real churn catch
- F1 Score: 0.59 → Better than Logistics Regression & Random Forest Classifier
- ROC AUC Score: 0.84 → The best model performance in distinguishing churn vs. non-churn
- Confusion Matrix:

![Confustion Matrix](https://github.com/CallmeNavin/P4_Customer-Retention-Churn-Prediction/blob/main/Version%202/Visualization/Confusion_Matrix_XGB.png)

- Classification Report:

![Classification Report](https://github.com/CallmeNavin/P4_Customer-Retention-Churn-Prediction/blob/main/Version%202/Visualization/Classification%20Report_XGB.png)

- ROC Curve & Precision Recall Curve:

![ROC Curve & Precision Recall Curve](https://github.com/CallmeNavin/P4_Customer-Retention-Churn-Prediction/blob/main/Version%202/Visualization/ROC%20Curve%20%26%20Precision%20Recall%20Curve_XGB.png)

→ This best model will be used in actionable plans

**_II. Actionable Plans_**

- Use XGB in prioritizing Targeting Customers
  + Customers with probability > 0.7 → High Risk → give strong retention offers (discounts, premium support, loyalty rewards).
  + Customers with 0.5 - 0.7 → Medium Risk → light-touch actions (follow-up calls, personalized messages).
  + Customers with < 0.5 → Low Risk → standard monitoring.
- Business Strategy Integration:
  + Marketing: design personalized campaigns targeting “high churn probability” segment.
  + Customer Service: proactively contact customers with medium-to-high churn risk.
- Continuous Monitoring:
  + Re-train model monthly/quarterly to capture new churn patterns.
  + Track business KPIs: retention rate, campaign ROI, customer lifetime value.

**VERSION 3**

**A. Project Overview**

- This project predicts the likelihood of customer churn based on transaction and contract information, in order to support effective retention strategies.
- Version 3 target: **Advanced hyperparameter tuning and feature engineering**

**About Me**

Hi, I'm Navin (Bao Vy) – an aspiring Data Analyst passionate about turning raw data into actionable business insights. I’m eager to contribute to data-driven decision making and help organizations translate analytics into business impact. For more details, please reach out at:

🌐 LinkedIn: https://www.linkedin.com/in/navin826/

📂 Portfolio: https://github.com/CallmeNavin/
