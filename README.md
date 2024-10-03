# loan_defaults
This dataset contains information on clients' credit card behavior, provided by a financial institution in Taiwan. The target variable is default_payment_next_month, which indicates whether the client defaulted on their credit card payment the next month.

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

#### Explanation for Feature Reordering:
The bill statement and payment amounts are listed in reverse chronological order in the dataset. To ensure that the feature names match the actual sequence of events, we reverse the column names for `BILL_AMT` and `PAY_AMT` features so that they correctly represent the time sequence from April 2005 to September 2005.


**Data Cleaning and Preprocessing Script Overview**

The data preparation process for the loan default project involved multiple steps to ensure data integrity, correct formatting, and optimal organization for further analysis. Below is a breakdown of the key components of the script and how they contribute to efficient data cleaning:

1. **General Column Cleanup**:
   - *Cleaning Column Names*: All column names were converted to lowercase with underscores replacing spaces for consistency and ease of access.
   - *ID Column Removal*: An identifier column ('id') was dropped to avoid including non-informative data in the analysis.

2. **Processing Categorical Variables**:
   - *Sex Column*: The 'sex' variable, represented as numerical values (1 = male, 2 = female), was mapped to categorical labels ('Male', 'Female') for better interpretability and converted to a categorical data type.
   - *Marriage Status*: The 'marriage' column, indicating marital status, was processed to map various codes to labels ('Married', 'Single', 'Unknown/Others') and then transformed into a categorical type with unordered categories.

3. **Target Variable Transformation**:
   - *Default Payment Column*: The target variable 'default_payment_next_month' was mapped to descriptive labels ('No Default' and 'Default') to enhance readability. It was also later converted back to numeric form for modeling purposes.

4. **Sequential Columns for Time-Based Data**:
   - *Renaming Pay Delay Columns*: Pay delay columns were renamed to include the month and a descriptive prefix for clarity (e.g., 'pay_0' became 'pay_delay_9_september'), reflecting a more intuitive structure of payment history.
   - *Bill and Payment Columns*: Similarly, bill and payment columns were renamed to reflect the month they represent for better tracking of payment history and account balances.

5. **Pay Delay Column Processing**:
   - *Labeling Pay Delay Categories*: Each pay delay column was labeled with meaningful descriptions, such as "Paid in full" or "2 months delay," improving clarity over raw numerical codes.
   - *Ordinal Conversion*: These labeled pay delay columns were then converted to ordinal data types with a specified order to represent the logical progression of payment delays.

6. **Education Level Categorization**:
   - *Mapping Education Codes*: The 'education' column was remapped to categories such as 'Graduate School', 'University', 'High School', and 'Other/Unknown'. The grouping and labeling provided a structured understanding of educational attainment.
   - *Ordered Categorical Conversion*: The education column was then converted into an ordered categorical variable, reflecting the hierarchy in education levels.

7. **Logging and Error Handling**:
   - Throughout the script, logging was implemented to track successful operations and provide informative error messages if any issues arise during the cleaning process. This level of error handling ensures that each function performs as expected and aids in debugging any inconsistencies in the data.

---

This structured approach to data cleaning not only improves the readability and interpretability of the dataset but also prepares it effectively for downstream modeling tasks. By designing reusable and modular functions for each aspect of the cleaning process, the script emphasizes scalability and efficient handling of diverse data types, setting the foundation for robust analysis and model building. 


### Skewness & Outlier Detection, Removal, and Transformation Techniques

- **Impact of Skewed Data**: Skewed features can adversely affect models that assume normally distributed data, including linear regression, logistic regression, and many tree-based methods.
- **Outliers and Variance**: Skewed distributions often result in outliers and high variance, potentially causing model instability. Addressing skewness is crucial for building more robust and accurate models.
- **Identifying Skewness**: Used statistical tests (e.g., skewness, kurtosis) and visual methods (e.g., histograms, boxplots) to identify columns with significant skewness.
- **Transformation Techniques**: Applied advanced techniques like `PowerTransformer` (Box-Cox or Yeo-Johnson), `QuantileTransformer`, and `RobustScaler` to improve the normality of skewed data.
- **Combined Outlier Handling**: Implemented a combined approach using Interquartile Range (IQR) to remove extreme outliers, followed by Winsorization to cap residual outliers, and applied `RobustScaler` to scale data effectively, achieving a more balanced transformation.
