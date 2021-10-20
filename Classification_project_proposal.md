
### Project Proposal Template

Ignasi Sols

#### Question/need:
* What is the framing question of your analysis, or the purpose of the model/system you plan to build? 
The framing question of my analysis is to be able to predict which diabetic patients will be readmitted within 30 days of discharge and to determine the factors that lead to hospital readmissions. 
* Who benefits from exploring this question or building this model/system?
Building this model will benefit both patients and hospitals. By identifying specific patients more likely to be readmitted, and more broadly, which factors lead to readmission within 30 days of discharge, the hospitals could implement a variety of strategies to educate patients, provide them with resources, and appropriate outpatient follow-up with a primary care provider. Patients benefit from improved health outcomes and avoiding preventable hospital stays.
Medicare does not reimburse hospitals for patient readmissions that happen within 30 days of a previous discharge, making it a priority for hospitals to prevent readmissions also from a financial standpoint.

#### Data Description:
* What dataset(s) do you plan to use, and how will you obtain the data?
* I plan to use the dataset "Diabetes 130-Hospitals for years 1999-2008" https://archive-beta.ics.uci.edu/ml/datasets/diabetes+130+us+hospitals+for+years+1999+2008
This dataset has 101,765 rows and 50 columns. Each column corresponds to one patient. The individual sample/unit of analysis of this project is a given patient.
I expect to work with the variables gender, age, weight, race, time, admission_type_id, time_in_hospital, payer_code, number of lab procedures, number of procedures, number of medications, number of outpatient visits of the patient in the year proceeding to the encounter, number of emergencies in the last year, number of diagnoses, max_glu_serum, A1Cresult, metformin, glipizide, insulin, change in medications, if there was a diabetes medication prescribed, and readmission information. There are variables about other medications that I might include in the model if features like 'change in medications' and 'number of medications' turned out to be relevant. 
The target variable for my classification models will be the categorical column 'readmitted' (it can take three values: “<30” if the patient was readmitted in less than 30 days, “>30”, and “No” for no record of readmission). 

#### Tools:
* How do you intend to meet the tools requirement of the project? 
I plan to apply several machine learning classification algorithms: knn neighbors and logistic regression, random forest, naive Bayes, XGB	oost, and support vector machines (SVM). 

#### MVP Goal:
* The MVP for this project would be to provide a logistic regression model that predicts if a patient will be readmitted within 30 days of discharge, as well as identifying the most relevant predictors. 
