# Predicting hospital readmission within 30 days of discharge for diabetic patients

Ignasi Sols

## **Abstract**

In this project I aim to predict which diabetic patients will be readmitted within 30 days of discharge and to determine the factors that lead to hospital readmissions. Medicare has a policy to penalize hospitals with excessive readmission rates, making it a priority for hospitals to prevent readmissions also from a financial standpoint. Building this model benefits both patients and hospitals. By identifying specific patients more likely to be readmitted, and more broadly, which factors lead to readmission within 30 days of discharge, the hospitals could implement a variety of strategies to educate patients, provide them with resources, and appropriate outpatient follow-up with a primary care provider. Patients benefit from improved health outcomes and avoiding preventable hospital stays. 

In this classification project, the best performing model was a **random forests model**, but I also trained **Logistic Regression** and **XGBoost** models. 

*This is a **7 min video** that I recorded in which I explain the gist of this project: https://www.youtube.com/watch?v=tw0uWofQOTg. Bear in mind that this video was recorded before the current iteration of this project, which means that there is no mention to the XGBoost models implemented.

*All the code is available in this github repository in the Jupyter notebook **{'predicting_hospital_readmissions'}**

### **Data**

I have chosen a comprehensive dataset of hospital readmissions in the US for which diabetes mellitus was listed as one of the diagnoses,  "Diabetes 130-Hospitals for years 1999-2008" https://archive-beta.ics.uci.edu/ml/datasets/diabetes+130+us+hospitals+for+years+1999+2008
This dataset has 101,765 rows and 50 columns. Each column corresponds to one patient. The individual sample/unit of analysis of this project is a given patient. Most of the variables are categorical, with just a few of them numerical. 

### Packages used:

Scikit-learn, Pandas, numpy, xgboost, imblearn, seaborn.

###  Classification metric:

I chose the F1 score as the classification metric, which is a harmonic balance between precision and recall. The reason is that we care both about recall and precision: we want to predict as much readmissions as possible, but the nurse educator team might have limited resources and time. The assumptions and risks of this project are (1) The model will be trained on data from 1999-2008, which means a different historical context (the dataset, from 1999 to 2008, might be outdated as science and medicine advance fast). It also assumes that the model will generalize to UMPC hospitals. Most importantly, most patients were caucasian and older than 50 years old. Therefore, the results might not generalize well to other races, and to patients younger than 50.



### **Feature engineering**:

1. **The target feature was binarized:** The original target variable for my classification models is the categorical column **'readmitted'**. It can take three values: “<30” if the patient was readmitted in less than 30 days, “>30”, and “No” for no record of readmission). I decided to *binarize* this target by grouping the observations into two classes: “>30”, and “No”. As a result, I ended up with a positive class “<30”, and a negative class (everything else). The resulting binary target variable is **'readmitted_binary_target'**. 
2. Within each categorical variable, I grouped the classes that had <5% of observations into a new class named 'Other'. 
3. I made **dummy variables** for each categorical variable, resulting in a total of 74 variables (numerical + dummy variables).

### There is class imbalance

The target classes are highly imbalanced (positive class: 11.6% of observations). For this reason, several methods were applied to handle it.

### Some patients appear on more than one row.

Some patients were admitted to the hospital more than once, as I found that they appear on more than one row. If left untreated, this is a huge concern and I had to engineer a solution for the whole cross-validation and testing schemes. In short, I implemented several functions that ensured that data from the same patient never appeared both in the training, and the validation or test splits. This way, data bleeding was prevented, which would cause overfitting. More on this in the next point. 

### Test/ Cross-validation schemes:

- **80 training / 20 testing split**: 

  This split was performed at patient level. This means that every rows from 80% of patients were assigned to the training-validation set, which is different than assigning 80% of the rows. 

- **10-fold cross validation with predefinedSplit and GridSearchCV**: 
A predefinedSplit was generated for 10-fold cross-validation when training and validating the model, and the same predefinedSplit was used throughout the project. This ensured that data from the same patient never appeared on the training and testing folds at the same time. 


### **Models**:
I trained a baseline Logistic regression model (that did not handle class imbalance), followed by several models whose hyperparameters I tuned with GridSearchCV and 10-fold cross-validation,and a predefined split:
- A logistic regression model with all the features included (numerical features + dummy variables).
- A random forests model with all the features included.
- An XGBoost model. I could not use early stopping and GridSearchCV (for tuning the hyperparamters) - at the same time, so I made a custom function to handle it (I made my own parameter grid). 

Class imbalance was assessed with the Logistic Regression model. I tried class different class weights with GridSearchCV. I also tested oversampling (SMOTE and RandomOversampler) with 10-fold cross-validation. And I also tuned the probability thresholds that determined If an observation was classified as belonging to the positive or the negative class. Among all these, the option that showed improvement was class weighting ('balanced'), which I used across the project. 

In the end, the best performing model was a Random Forests model with max_dept = 10, max_features = 'sqrt', 'n_estimators' = 800.

The final F1 score for the Random Forests model was 0.253 (for the test set), slightly better than the other models. The ROC-AUC was 0.6453.

If we look at the confusion matrix, this model would be useful for the hospital and nurse educator team. For each 100 patients that are going to be readmitted every month, the model would detect ~ 47 of those patients. And, if the nurse educator team was to naively target all the patients with customized education programs, that would take them ~468 hours per month (if they spent 30 min with each patient). In contrast, this model would allow them to target all the patients that are going to be readmitted in just 136 hours/month. 

