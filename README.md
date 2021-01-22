### Abstract 

This project used dataset obtained from Lending Club. Since loans in Lending Club are either 36 or 60 months, and in order to have a good number of finished loans, loan data issued from 2011 to 2014 were downloaded. More specifically, the target feature loan_status has the following distribution:

    * Fully Paid 193878; Charged Off 41748; Current  1; Late (31-120 days) 1; Default 1
This project will only keep loans with Fully Paid and Charged Off status. And the purpose is to build a classifier based on f1 score to predict whether a loan will be charged off before issuing a loan. 

    * The final selected model is Logistic regression with Lasso penalty C=1 and cut-off threshold 0.3.
    * The testing performance are: F1: 68.43, Precision: 75.75, Recall: 62.39, AUC: 78.46, Accuracy: 87.63. 
According to feature importance based on scale of coefficients, people with high last_fico_range_high and annual_inc are more likely to fully pay their loans, applicants with debt_settlement_flag Y are more likely to charge off loans.


## file name: 0 data preprocessing LendingClub.ipynb
Preparing lending club data for binary class classification:

    1. only keep rows with target classes (‘Charged Off’ and ‘Fully Paid’)
    2. remove    
        * columns with unique value
        * columns contain more than 40% missing values
        * columns which may cause information leakage
        * columns provide redundant information
        * rows with too many missing values
    3. Categorical variable
        * remove variables with too many levels
        * dummy coding  
    4. fill missing values using Multivariate imputer
    5. data normalization
    6. train test splitting 

## file name: 1 Modeling on imbalanced dataset.ipynb
Lending Club loan dataset is an imbalanced dataset with good and bad loan ratio around 8:2. The purpose of this task is to distinguish bad loans from good loans. There are several approaches to deal with this imbalanced problem:

    1. Check is it able to get more data from the minority class.
    2. Change the evaluation metric from accuracy to F1 score, since F1 score is a balanced measure of precision and recall. 
    3. Under, over or SMOTE sampling the training dataset to make classes balance, then train the model.
    4. Put more weight on the minority class.

Since imbalance ratio at 8:2 is not very severe, this notebook is modeled on the original imbalanced dataset and see what best approach it can get. The modeling steps are:

    1. Check correlation between description features and target feature
    2. Select best model from a list of candidates
    3. Model parameter tuning
    4. Threshold selection
    5. Feature importance
    
After all these steps, the final model's performance: 

    * Logistic Regression with L1(Lasso) penalty C=1 and threshold 0.3. 
    * The best training performance are: F1: 69.04, Precision: 76.61, Recall: 62.83, AUC: 78.78, Accuracy: 87.84. 
    * The testing performance are: F1: 68.43, Precision: 75.75, Recall: 62.39, AUC: 78.46, Accuracy: 87.63. 
    * last_fico_range_high and annual_inc have high negative influence on bad loans and debt_settlement_flag Y has high positive effect on bad loans. 
    
There is no big disparency between training and testing performance, which means overfitting is prohibited. 

## file name: 2 Modeling on balanced dataset.ipynb

Previous modeling on imbalanced dataset reached a highest performance on Logistic regression with Lasso penalty (C=1) and cut-off threshold 0.3. 

    * The testing performance are: F1: 68.43, Precision: 75.75, Recall: 62.39, AUC: 78.46, Accuracy: 87.63. 

This notebook focused on training model on under-sampled and SMOTE over-sampled dataset. When do model selection, all the candidate models on under-sampled dataset performed worse than on original dataset with f1 score around 0.55, all candidate models performed better on SMOTE over-sampled dataset with f1 score more than 0.7. Thus, further parameter tuning was performed on over-sampled dataset. To summarize, 

    * The best training model was Logistic regression with Lasso Penalty (C=10) and cut-off threshold 0.5. 
    * The new testing performance are: F1:  68.17, Precision: 85.68, Recall: 56.60, AUC: 76.57, Accuracy: 85.84.
 
According to feature importance, people with high last_fico_range_high and annual_inc are less likely to charge off on loans, applicants with debt_settlement_flag Y are more likely to charge off on loans which is same to feature importance on imbalanced dataset. 

      All in all, models on balanced dataset performed similar to models on original dataset. 
      Therefore, it is more efficient to model on the original imbalanced dataset for the Lending Club risk loan prediction problem. 
      
 🐣🪁
      
