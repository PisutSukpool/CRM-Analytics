# Campaing Response

## Exploratory Data

Frequency and distribution of total amount

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2008/EDA_amount_freq.png?raw=true)

Trend of total sale , number of visit and ticket size each month

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2008/EDA_Trend_Customer.png?raw=true)

Number of Response

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2008/Number_of_Response.png?raw=true)

## Implementation

Feature Engineering

   create feature that maybe relate to model more than 50 features
   
    1. Customer Movement viz Repeat and Reactivated in each active customer periods (1 month , 2 months , 3 months)
    
    2. RFM -> (recency , frequency , monetary)
    
    3. Transaction weekday or weekend
    
    4. Time to event in each features
 
Feature Selection

    1. AdaBoostClassifier
    
    2. ExtraTreesClassifier
    
    3. GradientBoostingClassifier
    
    4. RandomForestClassifier
    
Re-Sampling

    1. Under-Sampling
    
    2. Over-Sampling
    
    3. SMOTE
    
    4. SMOTETomek
    
    5. SMOTEENN
    
    6. BorderlineSMOTE
    
    7. SVMSMOTE
    
    8. ADASYN
    
Models

   1. Logistic Regression

   2. XGBoost

Result

 - Logistic Regression all features
 
![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2008/logreg_eval_all.png?raw=true)

 - XGBoost all features

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2008/xgboost_eval_all.png?raw=true)

 - Logistic Regression feature selection
 
![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2008/logreg_eval_filter.png?raw=true)

 - XGBoost all features feature selection

![alt text](https://github.com/PisutSukpool/BADS7105-CRM-analytics-and-intelligence/blob/main/Homework%2008/xgboost_eval_filter.png?raw=true)


Conclusion

   From the result , it measure by AUC. The best model performance is XGBoost model with oversampler. It gets 0.817286 from AUC.
   This experiment can be seen that proper feature selection and resampling can improve the model performance.
