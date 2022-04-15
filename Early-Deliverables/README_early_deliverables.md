# Predicting hospital readmission within 30 days of discharge for diabetic patients

Ignasi Sols

## **Abstract**

The framing question of my analysis is to be able to predict which diabetic patients will be readmitted within 30 days of discharge and to determine the factors that lead to hospital readmissions.  Medicare does not reimburse hospitals for patient readmissions that happen within 30 days of a previous discharge, making it a priority for hospitals to prevent readmissions also from a financial standpoint. Building this model benefits both patients and hospitals. By identifying specific patients more likely to be readmitted, and more broadly, which factors lead to readmission within 30 days of discharge, the hospitals could implement a variety of strategies to educate patients, provide them with resources, and appropriate outpatient follow-up with a primary care provider. Patients benefit from improved health outcomes and avoiding preventable hospital stays. The target of this project is UPMC hospitals.


## **Design**

I have chosen a comprehensive dataset of hospital readmissions in the US for which diabetes mellitus was listed as one of the diagnoses,  "Diabetes 130-Hospitals for years 1999-2008" https://archive-beta.ics.uci.edu/ml/datasets/diabetes+130+us+hospitals+for+years+1999+2008
  This dataset has 101,765 rows and 50 columns. Each column corresponds to one patient. The individual sample/unit of analysis of this project is a given patient. Most of the variables are categorical, with just a few of them numerical. The classification metric of interest is the F1 score, which is a harmonic balance between precision and recall. The reason is that we care both about recall and precision: we want to predict as much readmissions as possible, but the nurse educator team might have limited resources and time. The assumptions and risks of this project are (1) The model will be trained on data from 1999-2008, which means a different historical context (the dataset, from 1999 to 2008, might be outdated as science and medicine advance fast). It also assumes that the model will generalize to UMPC hospitals. Most importantly, most patients were caucasian and older than 50 years old. Therefore, the results might not generalize well to other races, and to patients younger than 50.


## **Data**

 The target variable for my classification models is  the categorical column 'readmitted' (it can take three values: “<30” if the patient was readmitted in less than 30 days, “>30”, and “No” for no record of readmission). I decided to binarize this target by grouping the classes “>30”, and “No”. As a result, I ended up with a positive class “<30”, and a negative class (everything else).
I dealt with categorical variables in two ways: first, within each categorical variable, I grouped the classes that had <5% of observations into a new class named 'Other'. Then, I made dummy variables for each categorical variable, resulting in a total of 74 variables.
The target classes are highly imbalanced (positive class: 11.6% of observations).


## **Algorithm**

I first run a linear regression algorithm with just the numerical variables (8 numerical variables:'time_in_hospital','num_lab_procedures','num_procedures','num_medications','number_outpatient','number_emergency','number_inpatient','number_diagnoses', with the aim of establishing a baseline model. The F1 was 0.0, and EDA showed non obvious target separation for the numerical values. The ROC-AUC curve was ~0.5. For this baseline I performed a simple validation split (training/validation/test) (60/20/20) and did not use the holdout data for validation.
Due to poor results, I turned to Random forests. With an estimator of n=100, I could get an improvement (F1 =0.02, with extreme overfitting. I have to mention here that, towards the end of the project, I realized that I had not scaled my numerical features when initially performing linear regression. When scaling the features, with the final holdout test data, the F1 score was 0.0225, very similar to the initial Random forests one. If I had detected earlier, I might have focus more in tuning the Linear regression model.
As explained above, I decided to optimize the random forests model, as it showed better results than the Linear regression model for the numerical variables.
I first established a validation scheme: I split the data into 80/20 training+validation/test. I used the PredefinedSplit method to avoid data bleeding at any possible stage of the test and validation scheme, including the 10-fold cross-validation scheme.
The test data was not touched until the very end, for better generalization of the model.
I dealt with target class imbalance by choosing a 'balanced' class weight multiplier. That improve the model significantly. Additional class imbalance handling by oversampling, as well as threshold adjustment to Optimize F1 (in both cases the parameters were tuned with the same 10-fold cross-validation scheme that prevents data bleeding) did not show further improvements and for this reason, a balanced class weight multiplier was the only implementation used to handle the imbalanced classes.
I identified the numerical variables ('number_emergency', and 'number_inpatient') that gave a particularly better F1 score. Then I converted all the categorical variables into dummy variables. I first run the cross-validation scheme with all the dummy variables (in addition to the two numerical variables already chosen). This lead to a huge overfitting and very low F1 score. For this reason, I added all the dummy variables one by one, to see which ones improved the F1 score. I also add them in groups (those that originally belong to the same categorical feature were tested together). I identified only one feature that improved the F1 score and ROC-AUC: diagnose 3, particularly, 'diagnose 3 other', so I added it to the previously selected features. Using n_estimators = 300 instead of 100 reduced further the variability of the scores of interest.
When I tested the Linear Regression model and Random Forests model (with the final features 'number_inpatient','number_emergency','diag_3_Other'), with class='balanced' for imbalance handling, and n_estimators = 300 for the random forest model, both models performed very similar:
RF: F1 test: 0.2502, ROC-AUC test: 0.613, and no overfitting: F1 training: 0.2547, with LR performing slightly better: F1 test: 0.2538, ROC-AUC: 0.6226 and F1 training : 0.2531


## **Tools**

Numpy and Pandas for data wrangling and computing.
Scikit-learn for machine learning classification models.
