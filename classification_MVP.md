### Predicting hospital readmission for diabetic patients
Ignasi Sols Balcells

The goal of this project is to predict which diabetic patients will be readmitted within 30 days of discharge and to determine the factors that lead to hospital readmissions.

To start exploring this goal, I have developed a baseline logistic regression model with 8 numerical features. As the target variable has three possible values ('<30' if the patient was readmitted in less than 30 days, '>30days' if the patient was readmitted in more than 30 days), and 'No' for no record of readmission),  I have grouped '>30', and 'No', converting the problem into a binary classification. 
The data is imbalanced (~11% positive class = readmission within 30 days of discharge).

I have handled the imbalance during model training, by adjusting the class weights. 
The F1 score, before dealing with imbalance, was 0.0 and the model did not predict a single positive observation. After dealing with imbalance, the F1 score is 0.2 but all predictions turn out to be for the positive class. 

To improve the model, I (1) plan to check the split. I generated a split in which the same patient is not in the train split, validation split, and test split. There might have been an error. (2) Try more imbalanced methods, (3) check the distribution of class values (1s and 0s) in the training, validation, and test sets. (4) I might as well consider a different, more sensitive, threshold value.

I also plan to explore the categorical features to identify specific features that classify well the binary target, and I will add them to the model as dummy variables.

In the next few days, I also plan to run a random forest model, and if there was time, an XGBoost model as well. I have been working on a 10-fold cross-validation scheme that does not allow data from the same patient to be in both the training and cross-validation sets, and I would like to apply this validation scheme. 



