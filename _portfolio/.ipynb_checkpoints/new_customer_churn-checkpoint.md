---
title:  "Predicting Customer Churn: A Telecom Case Study"
date: 2023-06-20
layout: single
classes: wide
header:
  teaser: /assets/images/predict_churn_customers.jpg
excerpt: "The purpose of this project is to identify patterns in customer behavior that predict churn."
categories: Churn Analysis
tags:
  - Data Analysis
  - Predictive Modeling
  - Customer Churn
  - Machine Learning
  - Python
  - Jupyter Notebook
  - Business Intelligence
  - Customer Relationship Management
  - Visualization
  - Data Science
---

![Customer Churn](/assets/images/predict_churn_customers.jpg)


## Project Overview

In the competitive telecom sector, understanding customer behaviors and predicting churn can significantly impact business success. In this project, I applied data science techniques to predict customer churn using the Orange Telecom's Churn Dataset.

### Objective

Build a predictive model to identify high-risk churn customers, providing actionable insights for targeted customer retention strategies.

### Tools & Techniques

- **Data Analysis**: Python, Pandas, Seaborn
- **Machine Learning**: Scikit-learn (Random Forest, Gradient Boosting)

## Exploratory Data Analysis

Conducted a thorough analysis to explore customer demographics, service usage, and churn indicators. Key steps included:

- Distribution analysis of numeric and categorical variables
- Visualization of customer behavior patterns

## Feature Engineering

Implemented creative feature engineering techniques to enhance model performance:

- **Interaction Term**: Merged 'total_day_minutes' and 'international_plan' to examine combined effects on churn
- **Total Minutes Aggregation**: Summed up total minutes (day, evening, night, international) for overall usage insights

## Modeling and Evaluation

Chose Random Forest and Gradient Boosting Models due to their robustness in handling complex datasets:

- Applied hyperparameter tuning and cross-validation for optimization
- Evaluated models based on accuracy, precision, recall, and F1 score

## Insights and Conclusion

The project revealed significant predictors of churn, such as customer service calls and usage patterns. While the Gradient Boosting model showed slightly superior performance, both models provided valuable insights into customer churn behaviors.

### Reflection

This project highlighted the importance of detailed EDA and innovative feature engineering in predictive modeling. It also underscored the nuances of model selection and evaluation in addressing real-world business challenges.

[View Full Project on GitHub](https://github.com/timothyrobbinscpa/new_customer_churn)

