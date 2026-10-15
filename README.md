# Understanding Magazine Subscription Behavior

Predicting which customers will subscribe to a magazine, using logistic regression and a Support Vector Machine (SVM) in Python.

Course project for ALY6020: Predictive Analytics, Master of Professional Studies in Analytics, Northeastern University (Vancouver).

## The problem

A magazine company saw subscriptions drop last year, even though people were spending more time at home and were expected to read more. The goal was to use customer data to understand what is and isn't working, and to predict who is likely to subscribe.

Only 14.9% of customers in the dataset subscribed. A model that simply guesses "no" every time would look accurate but be useless, so the models needed to actually find the customers most likely to say yes.

## Data

- Source file: `marketing_campaign.xlsx` (provided by the course, not included in this repository)
- Customer demographic and purchase behavior features
- Target: whether the customer subscribed (heavily imbalanced, 14.9% yes)

## Data cleaning

- Capped extreme income values at the 99th percentile
- Filled 24 missing income values with the median
- Removed 3 records with ages over 90 (likely data entry errors)
- Dropped columns that had the same value in every row
- Grouped non-standard marital status entries into clear categories

## Feature engineering

Created seven new features from the raw columns:

| Feature | What it captures |
| --- | --- |
| TotalSpend | Total amount spent across product categories |
| TotalPurchases | Total number of purchases across channels |
| TotalCampaignsAccepted | How many past campaigns the customer accepted |
| Children | Number of children and teens in the household |
| EducationOrd | Education level on a 0 to 4 scale, from Basic to PhD |
| IsCouple | Whether the customer lives with a partner |
| CustomerTenure | Days from enrollment to January 1, 2015 |

Age was also calculated from year of birth. In total, 12 features were used for modeling.
## Models

The data was split 80/20 with stratified sampling, and all features were standardized. Both models used `class_weight='balanced'` so the small "yes" group was not ignored. Results are on the 448-customer test set.

| Metric | Logistic Regression | SVM (RBF kernel) |
| --- | --- | --- |
| Accuracy | 77.01% | 79.46% |
| Precision | 36.76% | 39.84% |
| Recall | 74.63% | 73.13% |
| AUC | 86.52% | 86.70% |

## Key findings

- **Past campaign response is the strongest signal.** Customers who accepted earlier campaigns were the most likely to subscribe (logistic regression coefficient 1.04).
- **Recent buyers subscribe more.** The longer since a customer's last purchase, the less likely they were to subscribe (coefficient -0.97).
- **Long-time customers convert better.** Customer tenure had a strong positive effect (coefficient 0.94).
- **Partnered customers were less likely to subscribe** (coefficient -0.51), while higher total spend and education level pointed toward subscribing.

Coefficients come from standardized features, so their sizes can be compared directly.

## Recommendation

The SVM is recommended for targeting because it has higher accuracy and precision, which means fewer offers go to customers who will not subscribe. Logistic regression catches slightly more subscribers and is easier to explain, so it remains useful for showing stakeholders why customers subscribe.

For the business, the clearest next step is to focus on customers who responded to past campaigns, bought recently, and have been with the company the longest.

## Tools

Python, pandas, NumPy, scikit-learn, matplotlib, seaborn, Google Colab

## Repository contents

- `marketing_campaign_prediction.ipynb`: full notebook with cleaning, feature engineering, modeling, and evaluation
- `README.md`: project overview

## How to run

1. Open the notebook in Google Colab or Jupyter.
2. Place your copy of `marketing_campaign.xlsx` in the same folder, or update the `file_path` variable to point to it.
3. Run all cells from top to bottom.

## Author

Rabiat Blessing Ibrahim
[LinkedIn](https://www.linkedin.com/in/ibrahimblessing)
