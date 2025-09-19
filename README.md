
# 📊 Customer Churn Analysis & Prediction Project

## 📁 Project Overview

This project demonstrates an end-to-end **Customer Churn Analysis and Prediction Pipeline**.

It combines:

* **Data Cleaning and Preparation in **
* **Predictive Modeling using  and **
* **Interactive Visualization using **

The final dashboard helps visualize:

* Current churn distribution and trends
* Key churn drivers
* Predicted churners (customers at risk)

---

## 🗄️ Step 1 — Data Cleaning in SQL

**Objectives**

* Perform exploratory analysis
* Handle missing values
* Load clean data for modeling and BI

**Key SQL steps** (from [`SQL Queries.docx`](./SQL%20Queries.docx)):

* Explore categorical distribution:

```sql
SELECT Gender, COUNT(*) AS TotalCount
FROM stg_Churn
GROUP BY Gender;
```

* Check for nulls in all columns:

```sql
SELECT 
  SUM(CASE WHEN Customer_ID IS NULL THEN 1 ELSE 0 END) AS Customer_ID_Null_Count,
  ...
FROM stg_Churn;
```

* Replace null values and load into production table:

```sql
SELECT 
    Customer_ID,
    ISNULL(Value_Deal,'None') AS Value_Deal,
    ISNULL(Multiple_Lines,'No') AS Multiple_Lines,
    ...
INTO [db_Churn].[dbo].[prod_Churn]
FROM [db_Churn].[dbo].[stg_Churn];
```

* Create SQL views for :

```sql
CREATE VIEW vw_ChurnData AS
  SELECT * FROM prod_Churn WHERE Customer_Status IN ('Churned','Stayed');

CREATE VIEW vw_JoinData AS
  SELECT * FROM prod_Churn WHERE Customer_Status='Joined';
```

---

## 🤖 Step 2 — Churn Prediction Model in Python

**Script:** [`churndata.py`](./churndata.py)

**Workflow:**

1. Load cleaned churn dataset (`vw_ChurnData`) using&#x20;
2. Label-encode all categorical features with  `LabelEncoder`
3. Train a `RandomForestClassifier` model
4. Evaluate using confusion matrix and classification report
5. Visualize feature importances
6. Load new joiners dataset (`vw_JoinData`)
7. Predict churn risk for new customers
8. Export predicted churners to `predications.csv`

---

## 📈 Step 3 — Interactive Dashboard in Power BI

**Built in:**&#x20;

**Summary Page Highlights**

* Total customers, new joiners, total churn
* Churn rate by gender, age, tenure, contract, payment method
* Churn distribution by state and internet type
* Churn reasons and services used

**Prediction Page Highlights**

* Profile of predicted churners (gender, age, state, tenure)
* Churners by contract and payment method
* Detailed list of high-risk customers with revenue and referrals

### Dashboard Screenshots

**Churn Summary**

![Churn Summary](./Screenshot%202025-09-19%20194654.png)

**Churn Prediction**

![Churn Prediction](./Screenshot%202025-09-19%20194715.png)

---

## ⚙️ Tech Stack

* **Database:**&#x20;
* **Data Analysis & Modeling:** , , ,&#x20;
* **Visualization:**&#x20;

---

## 📁 Repository Structure

```
├── SQL Queries.docx         # SQL scripts for data cleaning and view creation
├── churndata.py              # Python script for churn prediction model
├── Screenshot 2025-09-19 194654.png  # Power BI - Churn Summary
├── Screenshot 2025-09-19 194715.png  # Power BI - Churn Prediction
└── README.md                 # Project documentation
```

---

## 🚀 How to Run

1. Execute SQL scripts to create clean dataset and views.
2. Run `churndata.py` to train model and generate churn predictions.
3. Load SQL views + prediction CSV into  to build the dashboard.

---

## 📌 Outcome

* Built a full churn analytics pipeline from raw data to business dashboard.
* Identified churn patterns and predicted at-risk customers with a machine learning model.

---

