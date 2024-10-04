### Data Description

This dataset contains information on clients' credit card behavior, provided by a financial institution in Taiwan. The target variable is `default_payment_next_month`, which indicates whether the client defaulted on their credit card payment the next month.

#### Variables:

- **ID**: ID of each client.
- **LIMIT_BAL**: Amount of given credit in NT dollars (includes individual and family/supplementary credit).
- **SEX**: Gender (1 = male, 2 = female).
- **EDUCATION**: Education level (1 = graduate school, 2 = university, 3 = high school, 4 = others, 5 = unknown, 6 = unknown).
- **MARRIAGE**: Marital status (1 = married, 2 = single, 3 = others).
- **AGE**: Age in years.

#### Payment History (PAY_X):
- **PAY_0**: Repayment status in September 2005 (-1 = pay duly, 1 = payment delay for one month, 2 = payment delay for two months, ... 8 = payment delay for eight months, 9 = payment delay for nine months and above).
- **PAY_2**: Repayment status in August 2005.
- **PAY_3**: Repayment status in July 2005.
- **PAY_4**: Repayment status in June 2005.
- **PAY_5**: Repayment status in May 2005.
- **PAY_6**: Repayment status in April 2005.

#### Bill Statement Amount (BILL_AMT_X):
- **BILL_AMT1**: Amount of bill statement in September 2005 (NT dollars).
- **BILL_AMT2**: Amount of bill statement in August 2005 (NT dollars).
- **BILL_AMT3**: Amount of bill statement in July 2005 (NT dollars).
- **BILL_AMT4**: Amount of bill statement in June 2005 (NT dollars).
- **BILL_AMT5**: Amount of bill statement in May 2005 (NT dollars).
- **BILL_AMT6**: Amount of bill statement in April 2005 (NT dollars).

#### Previous Payment Amount (PAY_AMT_X):
- **PAY_AMT1**: Amount of previous payment in September 2005 (NT dollars).
- **PAY_AMT2**: Amount of previous payment in August 2005 (NT dollars).
- **PAY_AMT3**: Amount of previous payment in July 2005 (NT dollars).
- **PAY_AMT4**: Amount of previous payment in June 2005 (NT dollars).
- **PAY_AMT5**: Amount of previous payment in May 2005 (NT dollars).
- **PAY_AMT6**: Amount of previous payment in April 2005 (NT dollars).

- **default_payment_next_month**: Default payment indicator (1 = yes, 0 = no).

### Data Cleaning

- **Feature Reordering**: The bill statement and payment amounts are listed in reverse chronological order in the dataset. To ensure that the feature names match the actual sequence of events, I reverse the column names for `BILL_AMT` and `PAY_AMT` features so that they correctly represent the time sequence from April 2005 to September 2005.
- **Pay Delay Categorical Column Processing**: Labeled each pay delay column with meaningful descriptions, such as "Paid in full" or "2 months delay," for improved clarity over raw numerical codes.
- **Education Level Categorization**: Remapped the 'education' column to categories like 'Graduate School', 'University', 'High School', and 'Other/Unknown', providing a structured understanding of educational attainment. Converted it into an ordered categorical variable to reflect the hierarchy in education levels.
- **Logging and Error Handling**: Implemented logging throughout the script to track successful operations and provide informative error messages, facilitating troubleshooting. 


### Skewness & Outlier Detection, Removal, and Transformation Techniques

- **Outliers and Variance**: Skewed distributions often result in outliers and high variance, potentially causing model instability. Addressing skewness is crucial for building more robust and accurate models.
- **Identifying Skewness**: Used statistical tests (e.g., skewness, kurtosis) and visual methods (e.g., histograms, boxplots) to identify columns with significant skewness.
- **Transformation Techniques**: Applied advanced techniques like `PowerTransformer` (Box-Cox or Yeo-Johnson), `QuantileTransformer`, and `RobustScaler` to improve the normality of skewed data.
- **Combined Outlier Handling**: Implemented a combined approach using Winsorization to cap extreme outliers, and applied `RobustScaler` to scale data effectively, achieving a more balanced transformation with minimal data loss.

### Handling Imbalanced Data

- **Impact on Model Performance**:
  - **Bias Toward Majority Class**: Many machine learning algorithms become biased toward the majority class because they assume balanced classes. This results in misleading high accuracy for the majority class, but poor performance in predicting the minority class. The minority class is typically what you are focused on predicting, in this case loan defaults. 
  - **Misleading Metrics**: Accuracy can be misleading in imbalanced datasets, as a model that always predicts the majority class may appear to perform well but fails to correctly identify minority instances.
  - **Poor Generalization**: Models trained on imbalanced data may struggle to generalize, particularly in correctly identifying minority class instances in unseen data.
- **High Cost of Missing Fraudulent Transactions**:
  - In fraud detection, the primary goal is to maximize the detection of fraudulent transactions. Missing fraudulent cases (false negatives) can lead to significant financial and reputational damage, making high recall essential to detect most fraudulent activities (loan defaults).

- **Resampling Techniques**:
  - **Oversampling**: Increase the number of minority class instances using techniques like Random Oversampling and Synthetic Minority Over-sampling Technique (SMOTE) to provide the model more oppotunity to learn how to predict loan defaults.
  - **Undersampling**: Reduced the number of majority class instances using methods such as Random Undersampling and NearMiss.
  - **Advanced Resampling Methods**: Implemented techniques like SMOTEENN (Synthetic Minority Over-sampling Technique and Edited Nearest Neighbors) and ADASYN (Adaptive Synthetic Sampling) to balance class distributions effectively.
 

### **Feature Engineering**: 

Engineered new features that could better capture underlying patterns in the data, creating features that highlighted high-risk customers, chronic delays, and payment patterns. These engineered features were then subjected to statistical tests to identify those that showed strong significance in predicting loan defaults.
- **High Risk Identification**: Identify customers with significant payment delays (3 months or more), indicating a higher risk of default.
- **Delayed Payments**: Counts the number of months a customer's payments have been delayed; higher values signal a greater risk of default.
- **Severe Delay**: Counts the number of months with delays of 3 months or more, quantifying the severity of the customer’s financial risk.
- **Deferred & Decreasing Payments**: Highlighting those already experiencing financial difficulty and increasing their likelihood of default.
- **Cumulative Delay Feature**: Tracked the cumulative sum of payment delays across months; higher cumulative values suggest chronic delinquency.
- **Cumulative Delay Binned**: Binned the cumulative delay values into categories ("low," "moderate," "high," and "severe" risk) to simplify risk assessment and enhance model interpretability.

### Statistical Testing for Feature Significance

- **Chi-Square Test**: Evaluated the relationship between engineered features and the target variable to identify statistically significant features worth retaining, combining, or removing.
- **ANOVA/Kruskal-Wallis Test**: Conducted an ANOVA for normally distributed data or a Kruskal-Wallis test for non-normally distributed data to assess whether the means of numerical features differ significantly across categories.
- **Key Findings**: Include only features that demonstrated strong statistical significance and high predictive value for loan defaults.

### Integrating Feature Engineering into Pipelines

- **Limitations of Standard Functions**: Standard functions used for feature engineering cannot be directly applied in an sklearn pipeline because pipelines require all steps to implement `fit` and `transform` methods.
- **Why Use Custom Transformers?**: To incorporate feature engineering into a pipeline, these functions must be converted into custom transformers. This ensures that transformations are executed in a consistent, automated manner during both training and prediction phases.
- **Pipeline Integration**: Enables seamless integration of custom transformations into an sklearn pipeline, ensuring consistent and streamlined preprocessing. Allow for smooth integration within the pipeline, preserving the flexibility and reliability of the preprocessing workflow. 
- **Reusability**: Encapsulates complex preprocessing logic in a reusable and modular way, making it easy to apply across multiple projects.

### Sklearn Pipelines: Streamlining Machine Learning Workflows

**What are Sklearn Pipelines?**  
In machine learning, pipelines are tools that streamline the process of preprocessing data and training models. They allow you to chain multiple steps into a single object, resulting in cleaner, more maintainable, and less error-prone workflows.

**Why Use Sklearn Pipelines?**

- **Consistency**:
  - *End-to-End Workflow*: Pipelines ensure that the same preprocessing steps are consistently applied during both training and testing phases.
  - *No Data Leakage*: By encapsulating preprocessing within a pipeline, data leakage is prevented, ensuring that only training data influences feature engineering steps.

- **Clean and Maintainable Code**:
  - *Modular Design*: Pipelines organize code into modular, reusable components, making it easier to read, debug, and maintain.
  - *Single Object Management*: The entire workflow, including preprocessing and modeling, is encapsulated in a single object, simplifying the overall process.

- **Simplified Workflow**:
  - *Chaining Steps*: Pipelines allow multiple preprocessing steps and the estimator to be chained into a single object, making the workflow more straightforward.
  - *Ease of Use*: Once defined, pipelines can be used for fitting, predicting, and evaluating, eliminating the need to manually apply preprocessing steps each time.
    
### Iterative Feature Selection and Model Optimization

In an effort to identify the best-performing model, I employed an iterative approach that involved two rounds of feature engineering, feature selection, model training, and performance comparison. This rigorous and scientific process was designed to maximize the model's predictive power and ensure robust performance.

#### **Feature Selection Process**

1. **Feature Selection**: I performed feature selection using **logistic regression** and **random forest models** to rank features based on their importance for each model type. By conducting this process iteratively, I refined the feature sets to enhance model performance. Using cross-validation and recursive feature elimination (RFECV), I identified the optimal number of features for each model, striking a balance between complexity and performance. 

2. **Model Training and Evaluation**: After selecting the top features, I trained logistic regression and random forest models on these feature subsets. The models were then evaluated using various metrics, and the classification reports were saved as JSON files. This systematic record-keeping allowed for a detailed comparison of model performance across different feature sets.

### Feature Selection - Filter, Wrapper, and Embedded Methods

#### **Filter Methods**
1. **Variance Threshold**: Removes features with low variance, as they are likely uninformative.
2. **Correlation Matrix (Pearson’s Correlation)**: Identifies and removes highly correlated features to reduce multicollinearity.
3. **ANOVA F-test**: Measures the relationship between continuous features and the target, similar to the Chi-Square test.
4. **Mutual Information**: Assesses how much information the presence of one feature provides about the target variable.

#### **Wrapper Methods**
- **Recursive Feature Elimination (RFE)**: Iteratively removes the least important features based on model coefficients or feature importance scores. The model is retrained on the remaining features at each step, providing a ranking of features.
- **Recursive Feature Elimination with Cross-Validation (RFECV)**: An extension of RFE that incorporates cross-validation to find the optimal subset of features. Unlike RFE, which requires a pre-defined number of features, RFECV uses cross-validation to determine the ideal number, making it more robust in feature selection. 

#### **Embedded Methods**
1. **Lasso Regression (L1 Regularization)**:
   - *Description*: Applies L1 regularization, shrinking less important feature coefficients to zero, effectively performing feature selection.
   - *Strengths*: Useful when there are many features, as it selects only the most relevant ones.

2. **Ridge Regression (L2 Regularization)**:
   - *Description*: Uses L2 regularization to shrink coefficients, reducing the impact of less important features without setting them to zero.
   - *Strengths*: Handles multicollinearity well and reduces overfitting when all features contribute to the model.

3. **Elastic Net (Combination of L1 and L2 Regularization)**:
   - *Description*: Combines the feature selection properties of Lasso and the coefficient shrinking of Ridge.
   - *Strengths*: Balances between L1 and L2 regularization, making it versatile for feature selection and handling multicollinearity.


#### **Benefits of an Iterative Approach**
- **Maximizing Performance**: By iteratively working through feature engineering and selection, I refined the feature set, allowing each model to learn from the most informative variables. This process improved model accuracy and generalization to unseen data.
- **Scientific Methodology**: Each step in the process, from statistical testing to feature selection, was executed in a structured manner. The use of statistical significance testing ensured that only relevant features were considered, while model-specific feature selection identified which features best supported the predictive power of each model type.
- **Data-Driven Decision Making**: Saving selected features and classification reports to JSON files provided a transparent record of the steps taken. This allowed for data-driven decisions when comparing model performances and selecting the best approach for the final model.


### Stacking for Model Performance Improvement

**Description of Stacking in Machine Learning**  
Stacking (Stacked Generalization) is an ensemble learning technique that combines multiple machine learning models to enhance overall performance. The core idea is to leverage the strengths of different models by blending their predictions to make more accurate and robust final predictions.

**What is Stacking?**

- **Base Models (Level-0 Models)**:
  - These are the individual models trained on the same dataset. Each base model captures different aspects of the data, potentially making unique types of errors.
  - Common base models include logistic regression, decision trees, random forests, support vector machines, and gradient boosting machines.
  - By stacking multiple models, I aimed to capitalize on the diversity and strengths of each model, ultimately enhancing the predictive power and stability of the final model.

- **Meta-Model (Level-1 Model)**:
  - The meta-model is trained to combine the predictions from the base models. It uses the outputs (predictions) of the base models as input features.
  - The meta-model learns to predict the final outcome based on the patterns and correlations it identifies in the base models' predictions, improving the accuracy and robustness of the final predictions.


