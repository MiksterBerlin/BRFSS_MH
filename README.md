# BRFSS_MH
Predictions for MH outcomes

The Behavioral Risk Factor Surveillance System (BRFSS) is an annual survey that is led by the Centers for Disease Control and Prevention (CDC) and applied in collaboration with the states and US territories.
BRFSS’s objective is to collect answers to a core of questions related to health risk behaviors, chronic diseases and conditions, access to health care, and use of preventive health services related to the leading causes of death and disability. States can select a number of elective modules to collect state-specific data on top of the annual core module. BRFSS conducts both landline and mobile phone-based surveys with individuals over the age of 18. General factors assessed by the BRFSS in 2021 included health status and healthy days, exercise, insufficient sleep, chronic health conditions, oral health, tobacco use, cancer screenings, and access to healthcare.
Survey data has been published annually by the [CDC](https://www.cdc.gov/) since 1984. You can find the [original dataset](https://www.cdc.gov/brfss/annual_data/annual_2021.html) as a ASCII format and past years data from [here](https://www.cdc.gov/brfss/annual_data/annual_data.htm).
The aim of this project is to build a model to predict mental health outcomes that could serve as a tool to prevent these outcomes from happening.
The data contains information about 6735 unique survey participants.

Jupyter Notebook

https://colab.research.google.com/github/MiksterBerlin/BRFSS_MH/blob/This%26That/Capstone_MH_Prediction.ipynb

Summary

This project aimed to build a model to predict depression using the Behavioral Risk Factor Surveillance System (BRFSS) 2021 dataset.

Key Steps:

Data Loading and Selection: Loaded the SAS dataset and selected relevant variables based on potential association with mental health outcomes.
Data Cleaning and Preprocessing: Handled missing values and unknown responses by replacing them with -1. Recoded several categorical variables for better interpretability. Sub-selected the target variable to only include Yes and No responses.
Feature Engineering and Selection: Separated categorical and numerical features. Applied One-Hot Encoding and StandardScaler. Performed initial feature screening based on correlation with the target variable. Oversampled the minority class (Depression) using SMOTE to address class imbalance. Further reduced features by removing those with high Variance Inflation Factor (VIF) to address multicollinearity.
Model Training and Evaluation: Trained several classification models including Logistic Regression, Bagging Classifier, Decision Tree, SVM, RandomForestClassifier, and XGBoost Classifier. Evaluated models based on training and testing accuracy, precision, and recall. Used Feature Importance from RandomForestClassifier to identify a subset of important features. Performed hyperparameter tuning on the XGBoost classifier using RandomizedSearchCV with cross-validation and evaluated its performance using ROC AUC score and a confusion matrix. Other models were tested including tensorflow, but tested unsatifactory and were excluded fromt this notebook.

Interpretation of Model Results

Based on the SHAP summary plot and the confusion matrix from the Best XGBoost Estimator:


The color of the dots shows the feature value (red for high, blue for low), allowing us to see how feature values relate to their impact on the prediction.
Confusion Matrix Interpretation:


The confusion matrix provides a breakdown of the  XGBClassifier model's predictions on the test set:

True Positives (bottom right, 1644/96.4%): Correctly predicted instances of Depression (0).
True Negatives (top left, 1412/84.0%): Correctly predicted instances of No Depression (1).
False Positives (top right, 271/16.0%): Instances predicted as Depression (0) but are actually No Depression (1) (Type I error).
False Negatives (bottom left 61/3.4%): Instances predicted as No Depression (1) but are actually Depression (0) (Type II error).
Analyzing these values helps assess the model's accuracy, precision, and recall for each class. For depression prediction, a high recall for the positive class (Depression) is often crucial to minimize false negatives (missing actual cases of depression). The balance between precision and recall depends on the specific goals of the model.

The model shows promising performance in identifying instances of diagnosed Depression.
It has a high rate of correctly identifying individuals with Depression (True Positives).
The False Negative rate (missing actual cases of depression) is relatively low, which is important for this type of prediction.
The False Positive rate (predicting depression when it's not present) is higher, which is a trade-off to consider depending on the application.
The Test Set ROC AUC score of 95.11% indicates good discriminatory power.

Important Variables (Based on SHAP):

The SHAP summary plot reveals the features that have the most significant impact on the model's prediction of depression. The features are ranked by their average absolute SHAP value, indicating their overall importance across the dataset.

Features with high SHAP values (dots to the right) push the prediction towards the positive class (Depression).
Features with low SHAP values (dots to the left) push the prediction towards the negative class (No Depression).
For example:

Mental health (e.g., number of days mental health was not good)
Certain chronic conditions (e.g., diabetes, high cholesterol, heart conditions)
Socio-economic factors (e.g., employment)
Experiences of adverse childhood events (ACEs)]

It is important to mention, the included measures are associtated with Depression, but they are not predictive in a sense that they can lead to Depression instead of being the result (high BMI).

Limitations:

Target Variable: The outcome variable is "Where you ever (by a health provider) diagnosed with a Depression?"
By definition that reduces the outcome to people with access to healthcare, and includes individuals that don't currently experience any Depression. Both factors influence model performance and predictability.

Causation vs. Association: The identified important variables are associated with depression in this dataset, but the model does not imply causation. Further research is needed to understand causal relationships.

Survey Weights: While survey weights (_LLCPWT2) were included in model training to account for potential sampling bias, the approach for handling weights with synthetic samples generated by SMOTE (assigning a weight of 1.0) could be further refined. Assigning an average weight of the minority class or using more advanced techniques for handling weights with oversampling might improve results.

Data Specificity: The model is trained on the BRFSS 2021 data from a specific region (California). Its generalizability to other populations or years may need further validation.

Gender Differences: The analysis excluded gender to look for general influencing factors. Further analysis could explore differences in depression prediction by gender by separating the dataset.
