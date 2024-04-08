---
title:  "Decoding Telecom Churn: Strategies for Retaining Customers through Data Science"
date: 2023-06-20
author_profile: true
layout: single
classes: wide
header:
  teaser: /assets/images/customer_churn/predict_churn_customers.jpg
excerpt: "Unlocking the Secrets of Customer Loyalty: A Data-Driven Journey into Telecom Churn"

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
![Customer Churn](/assets/images/customer_churn/predict_churn_customers.jpg)

#### Project Overview
In this data science project, I focused on analyzing and predicting customer churn in the telecom sector. The goal was to identify customers at risk of discontinuing services, enabling targeted strategies for retention.

#### Data Used
The analysis centered around the Orange Telecom's Churn Dataset, detailed with customer behaviors and trends. This provided a comprehensive base for understanding the factors contributing to churn.

<img src="/assets/images/customer_churn/churn_pie_chart.png" alt="Proportion of Customer Churn" style="width: 40%;">

#### Techniques and Tools
- **Exploratory Data Analysis:** Investigated factors like total day minutes and customer service calls, uncovering key trends and patterns.
- **Predictive Modeling:** Used Stratified K-Fold Cross-Validation, Random Forest, and Gradient Boosting models. These methods were chosen for their robustness in handling imbalanced datasets like ours.
- **Evaluation Metrics:** Emphasized recall, alongside precision and F1 score, to accurately capture churn cases.

#### Key Findings
- Identified high daily usage, international plans, and frequent service calls as major churn indicators.
- The Random Forest model showed a high recall of 82.11%, indicating its effectiveness in identifying true churn cases.
- Gradient Boosting balanced recall with a precision rate of 89.74%, highlighting its accuracy in predicting churn.

<img src="/assets/images/customer_churn/model_comparison.png" alt="Model Performance Comparison Bar Chart" style="width: 85%;" />

#### Impact
This project demonstrates my ability to transform data into actionable insights, showcasing skills essential for data-centric roles. The findings have implications for targeted customer retention strategies, underlining the importance of data-driven decision making in the telecom industry.

For a more detailed analysis of this project, [click here](/customer-churn).

To explore the full analysis with all executed code, outputs, and visualizations, see [the complete notebook on NBViewer](https://nbviewer.org/github/timothyrobbinscpa/new_customer_churn/blob/master/src/customer_churn.ipynb).