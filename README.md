# Customer Churn Prediction using Machine Learning

This project utilizes the **Bank Customer Churn Dataset** by [Radheshyam Kollipara](https://www.kaggle.com/datasets/radheshyamkollipara/bank-customer-churn) to build predictive models that identify customers at risk of leaving a bank. Two machine learning algorithms — **Random Forest** and **XGBoost** — are used to train, evaluate, and compare performance in churn classification.

---

## Project Overview

The goal of this project is to:
- Perform exploratory data analysis (EDA) on bank data to identify trends, patterns, and key factors influencing customer exit outcomes.
- Build and evaluate machine learning models to predict whether a customer will exit a bank or not.
- Gain insights from the data that can help in decision-making for financial institutions for customer retention stategies.

### Objective

1.Data analysis:Explore historical customer data to identify key factors influencing customer exit(e.g...inactivity,low satisfaction scores ,complaints)

2.Model Development:Build and compare supervised learning algorithms (Random Forest,XGBoost) to predict exit probabillity.

3.Evaluation:Optimize the model using metrics like precision,recall,F1-score, and AUC_ROC to minimise false negatives(missed exit risks).

4.Actionable Insights:Provide the bank with a tool to flag high_risk customers,enabling timely retention efforts.

---

## Project Structure

```
customer-churn-prediction/
│
├── data/
│   └── Bank Customer Churn Prediction.csv
│
├── notebooks/
│   └── churn_prediction.ipynb
│
├── models/
│   ├── random_forest_model.pkl
│   └── xgb_model.pkl
│
├── utils/
│   └── preprocessing.py
│
├── README.md
└── requirements.txt
```

---

## Dataset

The dataset contains 10,000 customer records with 14 features such as age, tenure, credit score, balance, and activity status.

📌 **Target Variable**: `Exited` (1 = Customer left, 0 = Customer stayed)  
📌 **Source**: [Kaggle Dataset - Bank Customer Churn](https://www.kaggle.com/datasets/radheshyamkollipara/bank-customer-churn)

---

## Features Used

After preprocessing, the following features were used for training:

- CreditScore
- Age-group
- Tenure
- Balance
- NumOfProducts
- HasCrCard
- IsActiveMember
- EstimatedSalary
- Geography (One-Hot Encoded)
- Gender (One-Hot Encoded)
- Salary Balance Ratio
- Inactive Premium
- Financial Stability
- Complain impact
- Credit Card Type(Ordinal Encoded)
---


2. **Data Preprocessing:**

   Load the dataset and preprocess it:
   - Handle missing values ( Dataset had no missing values)
   - Drop irrelevant columns i.e surname,row number and customer Id
   - Convert categorical features into numerical form (e.g., using one-hot encoding and ordinal encoding accordingly).
   - Split the data into training and testing sets.

   Example:

   ```python
   import pandas as pd
   from sklearn.model_selection import train_test_split
   from sklearn.preprocessing import StandardScaler

   # Load dataset
   bank_data=pd.read_csv("Customer-Churn-Records.csv")

   # Handle missing values
   data = data.dropna()

   # Convert categorical data and scale featureas that require scaling
   one_hot_cols=["gender","geography"]
   ordinal_cols=["card_type"]
   hierarchy=["SILVER","GOLD","PLATINUM","DIAMOND"]
   scaling_cols=["balance","estimated_salary","credit_score","point_earned","salary_balance_ratio","financial_stability"]
   other_cols=["tenure","total_products","credit_card","age_group","active_member","exited","complain","satisfaction_score","inactive_premium","complaint_impact"]
   preprocessor=ColumnTransformer(transformers=[("onehot",OneHotEncoder(),one_hot_cols),
                                             ("ordinal",OrdinalEncoder(categories=[hierarchy]),ordinal_cols),
                                             ("scaler",StandardScaler(),scaling_cols)],remainder="passthrough")

   bank_data_processed=preprocessor.fit_transform(bank_data)

   feature_names=list(preprocessor.get_feature_names_out())
   bank_data_transformed=pd.DataFrame(bank_data_processed,columns=feature_names)

   # Split data into features and target variable
   X = data.drop('exited', axis=1)
   y = data['exited']

   # Train-test split
   X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

   ```

3. **Exploratory Data Analysis (EDA):**

   Use various visualization techniques to analyze the data and look for patterns.
   - Distribution of exited and age.
   - Correlation heatmaps to check relationships between features.
   - Visualizing exited and satisfaction scores.

   Example:

   ```python
   import seaborn as sns
   import matplotlib.pyplot as plt

   # Plot the distribution of loan amounts
   plt.Figure(figsize=(16,14))
   sns.histplot(x=bank_data["age"],hue=bank_data["exited"],kde=True,multiple="stack")
   plt.title("EXITING CUSTOMER DISTRIBUTION BY AGE")
   plt.xlabel("Age")
   plt.ylabel("Count")
   plt.show()

   # Correlation heatmap
   correlation_matrix = data.corr()
   sns.heatmap(correlation_matrix, annot=True, cmap='coolwarm')
   plt.title('Correlation Heatmap')
   plt.show()
   ```

4. **Model Building:**

   Choose an appropriate machine learning algorithm to predict customer exit status. Common algorithms include:
   - Logistic Regression
   - Random Forest
   - Support Vector Machines (SVM)
   - XGBoost

   Example:

   ```python
   from sklearn.ensemble import RandomForestClassifier
   from sklearn.metrics import classification_report, confusion_matrix

   # Train a random forest classifier
   model = RandomForestClassifier(n_estimators=100, random_state=42)
   model.fit(X_train, y_train)

   # Predictions
   y_pred = model.predict(X_test)

   # Evaluate the model
   print(classification_report(y_test, y_pred))
   print(confusion_matrix(y_test, y_pred))
   ```

5. **Model Evaluation:**

   Evaluate the performance of your model using various metrics:
   - Accuracy
   - Precision, Recall, F1-score
   - Confusion Matrix
   - ROC Curve

   Example:

   ```python
   from sklearn.metrics import roc_auc_score

   # Calculate ROC-AUC score
   roc_auc = roc_auc_score(y_test, model.predict_proba(X_test)[:, 1])
   print(f'ROC-AUC Score: {roc_auc}')
   
##  Model Training

I trained and evaluated two models:

### 1️⃣ Random Forest Classifier
- Handles feature importance and interactions well.
- Ensemble method reducing overfitting compared to single decision trees.

### 2️⃣ XGBoost Classifier
- Gradient boosting implementation optimized for speed and performance.
- Includes regularization to prevent overfitting.

Both models were trained on a train-test split (80/20), and evaluated using:

- Accuracy
- Precision
- Recall
- F1-Score
- ROC-AUC

---

##  Results

| Model               | Accuracy | Precision | Recall | F1-Score | ROC-AUC |
|--------------------|----------|-----------|--------|----------|---------|
| Random Forest       | 85.26%    | 95.0%     | 87.0%  | 91.0%    | 0.928  |
| XGBoost             | 82.55%    | 88.0%     | 92.0%  | 90.0%    | 0.943   |

✅ XGBoost slightly outperforms Random Forest in all key metrics.

---

##  How to Run

1. Clone the repository:
```bash
git clone https://github.com/ZuenaKiezy/Bank-Churn-.git
cd Bank-Churn-
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Run the notebook:
```bash
jupyter notebook notebooks/Customer Churn Prediction.ipynb
```

---

## Insights & Recommendations

- Customers with low balance and low product usage are more likely to churn.
- Inactivity is a strong predictor of churn — banks should increase engagement.
- XGBoost is recommended for production use due to its higher recall and AUC.

---

## Requirements


   Dependencies may include:
   - `pandas`: For data manipulation and analysis.
   - `numpy`: For numerical operations.
   - `matplotlib` and `seaborn`: For data visualization.
   - `scikit-learn`: For building machine learning models.
   - `xgboost`: For advanced boosting models.
---

##  License

This project is open-source and available under the [MIT License](LICENSE).

---

##  Acknowledgements

- Dataset by Radheshyam Kollipara on [Kaggle](https://www.kaggle.com/datasets/radheshyamkollipara/bank-customer-churn)
- Inspired by customer retention strategies in the banking industry

---
