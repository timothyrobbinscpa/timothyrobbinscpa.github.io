---
title: "Turning the Tide on Customer Churn: A Data-Centric Approach"
date: 2023-06-20
author_profile: true
layout: single
classes: wide
categories:
  - Data Analysis
tags:
  - Customer Churn
  - Data Science
  - Predictive Modeling
excerpt: "Explore how decades of accounting experience and advanced data science techniques converge to tackle customer churn, enhancing customer loyalty and business sustainability."
header:
  teaser: /assets/images/customer_churn/download (1).jfif
---

## Unveiling Churn Dynamics: A Data-Driven Exploration
With extensive experience in assessing the impact of customer churn on revenue and accounts receivable, I leveraged Python to dissect and analyze complex patterns of customer churn. My Master's in Data Science further enabled me to transform these detailed analyses into actionable insights for reducing churn and enhancing customer retention strategies.

## Harnessing Technology: Advanced Tools for Strategic Analysis
My expertise with ERP systems like NetSuite, SAP, and Oracle, combined with SQL skills, enabled efficient handling of large datasets. This project capitalized on Python's capabilities and libraries such as Pandas and Scikit-Learn for robust predictive modeling, supported by dynamic visualizations with Matplotlib and Seaborn.

## Analysis and Methodology: Crafting the Approach for Churn Reduction
The project commenced with an exploratory data analysis to uncover key churn patterns, notably the impact of service calls on churn rates. Although I explored feature engineering techniques such as creating interaction terms and aggregating total minutes, these modifications did not significantly improve model performance and were not included in the final analysis. This phase was vital for determining the most effective data handling techniques, informed by my extensive experience in revenue management and data interpretation across roles at companies like Saba Software and MarketLive.

## Optimal Data Synthesis: Balancing and Tuning for Precision
Advanced statistical techniques, such as Stratified K-Fold Cross-Validation and targeted balancing methods like weight adjustments and subsampling, were employed to address the unbalanced nature of the dataset, with an imbalance of approximately 14.6%. These methods allowed for precise model tuning, focusing particularly on recall to minimize the risk of missing true positive churn cases. This emphasis on precision reflects the rigorous standards required in financial reporting, mirroring the accuracy I maintained in revenue management.

## Model Performance: Assessing Metrics for Churn Prediction
Model performance was quantified, highlighting recall. Random Forest achieved a recall of 0.8211, slightly higher than Gradient Boosting's 0.7368. Both models demonstrated strong precision, F1-scores, and over 94% accuracy, indicative of a well-calibrated balance between recall and precision.
<figure class="align-center">
  <img src="/assets/images/customer_churn/model_comparison.png" alt="Model Comparison Chart" style="width:80%;">
  <figcaption>Comparative Performance of Prediction Models</figcaption>
</figure>

## Strategies in Action: Translating Insights into Business Outcomes
Our analysis surfaced key churn drivers: high usage patterns, frequent service interactions, and certain plan features. Recommendations aimed at improving customer service include implementing robust follow-up procedures post-service calls, personalized pricing strategies responsive to customer usage patterns, and revising international plan offerings to be more competitive—all informed by the data-driven insights from the top feature importances graphed below. These strategies have the potential to enhance customer service and create pricing plans that could improve satisfaction and reduce churn.
<figure class="align-center">
  <img src="/assets/images/customer_churn/top4_feature_importances.png" alt="Top 4 Feature Importances" style="width:80%;">
  <figcaption>Top 4 Features Influencing Customer Churn</figcaption>
</figure>

## Join the Conversation
I invite feedback and discussion on this project and my broader journey into data science. Share ideas and explore synergies with me.

## Explore the Full Analysis
Dive deeper into the comprehensive study in my detailed post [here](/customer-churn/).

## Technical Deep Dive
For a detailed breakdown, including code and visuals, view the project notebook on [NBViewer](https://nbviewer.org/github/timothyrobbinscpa/new_customer_churn/blob/master/src/customer_churn.ipynb).
