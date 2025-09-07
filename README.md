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
- Preparation for modelling
  + Define x, y
  + Train Split Test
- Modeling (lần này không tune + More chart: Heatmap, Boxplot for more visual):
  + Logistics Regression
  + Random Forest Classifier

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

→ The model is effective in identifying churn, but it leans toward maximizing Recall at the expense of Precision. This makes it suitable for businesses that prioritize retaining true churn customers, even if it means contacting many non-churn customers unnecessarily.

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

→ Logistic Regression: Good at capturing churn (Recall = 0.81), but suffers from high false positives (Precision = 0.46). Suitable when business goal is not to miss any churn customers, even at the cost of contacting many non-churn ones.
→ Random Forest: Improves Precision to 0.62 while maintaining reasonable Recall (0.48). Provides a more balanced trade-off, suitable when business aims to optimize retention cost and reduce false positives.

**I.3. XGBoost**

- Accuracy Score: 0.80
- Precision Score: 0.65
- Recall Score: 0.53
- F1 Score: 0.59
- ROC AUC Score: 0.84
- Confusion Matrix:

![Confustion Matrix](https://github.com/CallmeNavin/P4_Customer-Retention-Churn-Prediction/blob/main/Version%202/Visualization/Confusion_Matrix_XGB.png)

- Classification Report:

![Classification Report](https://github.com/CallmeNavin/P4_Customer-Retention-Churn-Prediction/blob/main/Version%202/Visualization/Classification%20Report_XGB.png)

- ROC Curve & Precision Recall Curve:

![ROC Curve & Precision Recall Curve](https://github.com/CallmeNavin/P4_Customer-Retention-Churn-Prediction/blob/main/Version%202/Visualization/ROC%20Curve%20%26%20Precision%20Recall%20Curve_XGB.png)

**_II. Actionable Plans_**

- Dual-Model Action Plan: Paralell execute Logistic Regression & Random Forest
  + Step 1 - Early Warning: Use Logistic Regression as a broad monitoring tool because of its high Recall (0.81). All customers flagged by this model are marked as 'At Risk – Monitor'. Send low-cost automated communications
  + Step 2 - Targeted Campaign: Use Random Forest as a precision filter because of its higher Precision (0.62). Deploy personalized retention offers (discounts, loyalty points, service upgrades).
  + Step 3 - Combine Monitor: Logistic Regression flags a wide pool of potential churn. Random Forest narrows it down to a focused segment for costly interventions.

→ High coverage (don’t miss churn) thanks to Logistic Regression. High efficiency (fewer wasted offers) thanks to Random Forest.

**About Me**

Hi, I'm Navin (Bao Vy) – an aspiring Data Analyst passionate about turning raw data into actionable business insights. I’m eager to contribute to data-driven decision making and help organizations translate analytics into business impact. For more details, please reach out at:

🌐 LinkedIn: https://www.linkedin.com/in/navin826/

📂 Portfolio: https://github.com/CallmeNavin/
