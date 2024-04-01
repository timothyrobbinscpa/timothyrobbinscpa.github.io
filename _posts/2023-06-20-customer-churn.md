---
title: "Tackling Customer Churn in Telecom: <br> A Data-Driven Journey"
#toc: true
#toc_label: "Page Contents"
#toc_icon: "cog"  # optional, to show an icon next to the ToC title
#toc_sticky: true     # optional, to make the ToC sticky
sidebar:
  title: "Table of Contents"
  nav: "cust-churn-nav"
layout: single
classes: wide
date: 2023-06-20
categories: "Customer-Churn"
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
header:
  overlay_image: /assets/images/customer_churn/predict_churn_customers.jpg
  overlay_filter: 0.4
  caption: "Photo credit: [**Unsplash**](URL)"
excerpt: "Exploring the intricacies of customer churn in the telecom sector using advanced data analysis techniques."
---
## Introduction
In the rapidly evolving telecom sector, understanding and mitigating customer churn is essential. In this project, I employed advanced data science techniques to unravel churn patterns, aiding in the development of effective strategies for customer retention and business growth. Using predictive modeling, I aimed to identify customers with a high likelihood of discontinuing their services, a crucial step for proactive customer management.

## Project Overview
This project is a synthesis of extensive statistical analysis and astute business acumen. Centered on a detailed dataset from a telecom company, my aim was to develop a predictive model to accurately determine the likelihood of customer churn. Given the inherent volatility of the telecom industry's customer base, such predictive insight is of paramount importance.

The accompanying pie chart depicts the distribution of customer churn within the dataset, revealing that 14.6% of customers have churned, while a substantial majority of 85.4% have not. This imbalance poses a particular challenge in predictive modeling, as it requires careful consideration to ensure that the model accurately identifies the minority class of churned customers without being overwhelmed by the majority class.

<img src="/assets/images/customer_churn/churn_pie_chart.png" alt="Proportion of Customer Churn" style="width: 50%;">

## Dataset Overview
The backbone of this analysis was the Orange Telecom's Churn Dataset, rich with intricate details about customer behaviors and churn trends. The dataset was bifurcated into training and testing segments, ensuring a comprehensive approach to model development and validation.

Below is a preview of the initial dataset used in the analysis.

<iframe src="/assets/images/customer_churn/dataframe_snippet.html" width="100%" height="225px" style="border:none;"></iframe>

## Data Exploration and Preprocessing
The initial stages were centered around refining the data for analysis:
- Data Integrity: Recognizing the critical impact of data completeness and reliability, I rigorously addressed any missing or inconsistent data, ensuring a robust base for analysis.
- Data Transformation: I converted categorical data into a numerical format, a necessary step for sophisticated modeling techniques that require quantifiable inputs.
- Outlier Management: Implementing robust scaling was crucial to minimize the influence of outliers, which are extreme data points that can skew analysis and model predictions.

## Exploratory Data Analysis (EDA)
In my analysis of the Orange Telecom's Churn Dataset, I delved into various aspects of the data to uncover patterns that shed light on why customers churn. I've included specific visualizations from this analysis to bring these insights to life.

### Total Day Minutes and Churn
<img src="/assets/images/customer_churn/day_minutes_vs_churn.png" alt="Descriptive Alt Text" class="figure-size" />

I discovered that customers with higher total day minutes are more prone to churn. This box plot clearly demonstrates this trend, suggesting that heavy usage during the day might lead to customer dissatisfaction and churn.

### International Plan and Churn
<img src="/assets/images/customer_churn/international_plan_churn.png" alt="Descriptive Alt Text" class="figure-size" />

My findings also revealed a significant increase in churn among customers with international plans. The bar graph illustrates that these customers are more likely to churn, indicating potential issues with the international plan offerings.

### Customer Service Calls and Churn
<img src="/assets/images/customer_churn/cust_svc_calls_churn.png" alt="Descriptive Alt Text" class="figure-size" />

The point plot presents a clear correlation between the frequency of customer service calls and churn. An increase in customer service interactions typically indicates unresolved issues, leading to greater churn risk.

## Model Development
### Stratified K-Fold Cross-Validation
To ensure representative folds in our imbalanced dataset, Stratified K-Fold Cross-Validation was essential. This approach guarantees that each fold mirrors the overall distribution of the data, particularly important for the minority churn class.

```python
# Define stratified k-fold for both models
stratified_kfold = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
```

### Weight Adjustment
Weight adjustment in the Random Forest model countered the class imbalance by focusing more on the minority class (churned customers). Varying weight ratios helped in calibrating the model's sensitivity to churned customers.

```
# Define the weight ratios for RF model
weight_ratios = np.linspace(0.1, 15.0, 50)
class_weight_options = [{0: 1, 1: ratio} for ratio in weight_ratios]
```

### Subsampling
Subsampling in the Gradient Boosting model helped to address the imbalance. By training on random subsets, the model could better generalize, reducing the risk of overfitting to the majority class.

```
# Parameter grid for Randomized Search with Gradient Boosting
param_dist_gb = {
    ...
    'subsample': [0.5, 0.6, 0.7, 0.8, 0.9, 1.0],  # Subsampling the training set
    ...
}
```

### Recall Focus
Optimizing for recall was a priority, ensuring the model effectively identifies churn cases in our imbalanced dataset. The Randomized Search was tailored to prioritize recall, crucial for minimizing false negatives.

```
# Parameter grid for Randomized Search with Gradient Boosting
param_dist_gb = {
    ...
    'scoring': 'recall',  # Focus on recall
    ...
}
```

## Model Selection
In this project, I utilized two esteemed models, Random Forest and Gradient Boosting, to predict customer churn, particularly focusing on the challenge posed by an imbalanced dataset.

### Random Forest Model

- **Approach**: Constructs multiple decision trees, combining their outputs for a more accurate and stable prediction.
- **Strengths**: Effectively combats overfitting, adept at managing imbalanced data, and provides insights on feature importance.

### Gradient Boosting Model

- **Approach**: Builds the model in stages, with each successive tree improving on the previous one's errors.
- **Strengths**: High precision, flexible across various loss functions, and excels at identifying complex interactions between features.

## Model Performance
Our analysis highlights the importance of recall in predicting customer churn, where capturing the majority of churn cases is more critical than the precision of the prediction due to the imbalanced nature of our dataset:

<img src="/assets/images/customer_churn/model_comparison.png" alt="Model Performance Comparison Bar Chart" class="figure-size" />

- **Recall**: The Random Forest model demonstrated a superior recall of 82.11%, indicating its strength in identifying true churn cases, which is crucial for minimizing the risk of false negatives.
- **Precision**: Although Gradient Boosting had a slightly lower recall of 73.68%, its precision rate of 89.74% reflects its effectiveness in correctly labeling customers who are likely to churn.
- **F1 Score & Accuracy**: Both models performed comparably in F1 Score, with Random Forest at 81.25% and Gradient Boosting slightly lower at 80.92%. Despite the focus on recall, both models achieved high accuracy, with Gradient Boosting slightly ahead.

### Confusion Matrix Analysis
The confusion matrices further illustrated the models' strengths. Random Forest correctly identified 78 churned customers but had 17 false negatives, suggesting room for improvement in churn prediction. In contrast, the Gradient Boosting model correctly predicted 70 churn instances, with 25 false negatives, showcasing its strength in pinpointing customers at risk of churn.

<img src="/assets/images/customer_churn/confusion.png" alt="Fig.1: Comparative Confusion Matrices for Random Forest and GBM" class="figure-size" />

The evaluation of our models zeroes in on recall—a crucial metric given the imbalanced nature of our churn dataset:

### Precision-Recall Curve Comparison
The Precision-Recall Curve is a model evaluation metric that graphically demonstrates the trade-off between the precision of a predictive model and its recall (true positive rate) at different threshold settings. The area under the curve (AUC) provides a single metric to compare models. In our churn prediction project, this metric is particularly insightful due to the class imbalance inherent in the dataset.

<img src="/assets/images/customer_churn/pr_curve.png" alt="Precision-Recall Curve" class="figure-size" />

- **Random Forest**: Exhibited a strong performance with a PR AUC of 0.8679, suggesting a robust balance between precision and recall across different thresholds.
- **Gradient Boosting**: The Gradient Boosting model showed a slightly better PR AUC of 0.8739, indicating its efficiency in maintaining precision while maximizing recall.

The comparison reveals that while both models are quite effective, Gradient Boosting edges out slightly in terms of the area under the Precision-Recall Curve. This suggests that for this particular dataset, Gradient Boosting may be the more appropriate choice when optimizing for a balance between identifying as many true churn cases as possible (recall) while maintaining precision.

### Feature Importances Comparison
Analyzing the feature importances from our Random Forest and Gradient Boosting models gives us insight into the factors most predictive of customer churn. Both models highlight similar features as significant, but with some differences in the degree of importance attributed to each feature.

<img src="/assets/images/customer_churn/feature_imp.png" alt="Feature Importances Visualization" class="figure-size" />

- **Customer Service Calls**: The number of customer service calls is a strong predictor in both models, with a notably high importance in the Random Forest model. This suggests a direct link between service calls and churn likelihood, underscoring the importance of customer service quality.
  
- **Total Day Charge**: For both models, the total day charge is a prominent feature, which may indicate that higher charges during the day contribute to customer churn.
  
- **Total Day Minutes**: This feature is a top predictor for churn, with both models assigning it high importance. It appears that longer call durations are associated with a greater chance of customers leaving the service.
  
- **International Plan**: The presence of an international plan is a more significant predictor in the Gradient Boosting model than in Random Forest, suggesting that factors related to international calling plans may influence the decision to churn.

The bar chart comparison above provides a visual representation of these key features' importances, indicating where each model's predictive focus lies. Notably, the first four features listed are among the most critical across both models. Targeting improvements in these areas could be beneficial in reducing churn rates.

## Conclusion
Completing this customer churn project independently marks a significant advancement in my data science journey. It has demonstrated my ability to tackle intricate business challenges through rigorous predictive modeling and data analysis. This endeavor has sharpened my skills in critical areas such as model evaluation and data-driven decision-making, underlining my aptitude for deriving actionable insights from complex data scenarios.

As I look to the future, my focus is on deepening my expertise through exploring advanced modeling techniques and integrating broader data sets. My dedication to continuous learning and applying cutting-edge methods like deep learning showcases my potential as a valuable asset in any data-centric role. This project is a testament to my capacity for innovative problem-solving and strategic analysis, ideally positioning me for opportunities where my analytical acumen and drive for impactful solutions can contribute to business success.
