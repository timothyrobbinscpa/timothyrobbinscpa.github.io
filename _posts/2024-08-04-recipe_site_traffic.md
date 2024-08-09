---
title: "Predicting Popular Recipes: A Comprehensive Data Science Approach"
date: 2024-08-08
permalink: /recipe-post/
toc: false
toc_sticky: true
layout: single
classes: wide
categories:
  - Data Science
  - Predictive Modeling
tags:
  - Data Science
  - Python
  - Predictive Modeling
  - Jupyter Notebook
published: false
---
## Introduction

Transitioning from a 25-year career in accounting to data science has allowed me to leverage my analytical skills in new and exciting ways. The main goal of this project is to predict which recipes will be popular 80% of the time and minimize the chance of showing unpopular recipes on the company's homepage. This analysis involves several steps, including data validation, initial data cleaning, exploratory data analysis (EDA), model selection, and performance evaluation.

## Leveraging Accounting Skills in Data Science

- **Data Accuracy and Validation:**
In accounting, ensuring the accuracy of financial records is paramount. This meticulous approach was applied to the data analysis process, where I cleaned the dataset, checked for inconsistencies, and validated the accuracy of the data before proceeding with the analysis. This step is critical in ensuring that the insights drawn are reliable and actionable.
- **Trend Analysis:**
My experience with financial forecasting and trend analysis in accounting provided a solid foundation for analyzing data trends. I applied these skills to identify patterns in the recipe site's traffic data. By applying time series analysis, I was able to detect seasonal trends and spikes in user engagement, similar to how I would analyze revenue trends over fiscal quarters.
- **Decision-Making Support:**
Throughout my accounting career, I provided critical financial insights to support strategic decision-making. This often involved translating complex financial data into understandable and actionable recommendations. Similarly, the conclusions drawn from my analysis of the site’s traffic data were geared towards supporting the site’s business strategy.
- **Automation and Process Optimization:**
A significant part of my accounting career involved automating financial processes to improve efficiency and reduce error margins. I brought this same mindset to the analysis process, using Python scripts to automate data extraction, cleaning, and preliminary analysis. This not only saved time but also ensured consistency across the analysis.

### Initial Data Cleaning

The initial data cleaning process was crucial to prepare the dataset for analysis. This involved handling missing values, transforming data, and encoding categorical variables to ensure the dataset was suitable for modeling. Below is the detailed cleaning process I followed:

```python
# Handle missing values with median imputation
recipes['calories'].fillna(recipes['calories'].median(), inplace=True)
recipes['carbohydrate'].fillna(recipes['carbohydrate'].median(), inplace=True)
recipes['sugar'].fillna(recipes['sugar'].median(), inplace=True)
recipes['protein'].fillna(recipes['protein'].median(), inplace=True)

# Log-transform and scale features to handle skewness and outliers
from sklearn.preprocessing import RobustScaler
import numpy as np

for col in ['calories', 'carbohydrate', 'sugar', 'protein']:
    recipes[col] = np.log1p(recipes[col])  # log-transform to reduce skewness
    recipes[col] = RobustScaler().fit_transform(recipes[[col]])  # scale with RobustScaler

# Impute 'High_Traffic' feature using mode
recipes['high_traffic'].fillna(recipes['high_traffic'].mode()[0], inplace=True)
```

#### Initial Data Cleaning Insights

- **Recipe**: This numeric variable serves as the unique identifier for each recipe. Values are consecutive, ranging from 1 to 947, with no duplicates or missing values. No additional cleaning is required.
- **Calories**: This column had 52 missing values, which were imputed using the median of the calories group. During the model preprocessing phase, this variable was log-transformed and scaled using RobustScaler to handle outliers.
- **Carbohydrate**: This column also had 52 missing values, filled through median imputation. It was log-transformed and scaled using RobustScaler during preprocessing.
- **Sugar**: Similarly, this column had 52 missing values, addressed by median imputation, log-transformed, and scaled using RobustScaler.
- **Protein**: This column had 52 missing values, imputed using the median. It was also log-transformed and scaled using RobustScaler.
- **Category**: No missing values, categorical data handled with OneHotEncoder.
- **Servings**: No missing values, converted to numeric and scaled.
- **High_Traffic**: 373 missing values, imputed using mode.

## Exploratory Data Analysis (EDA)

### Distribution Analysis

To understand the distribution of numerical features, we plotted their distributions, which helped identify the skewness of the data, the presence of outliers, and the overall spread of values. The visualizations included histograms with KDE plots to show the frequency of data points and boxplots to highlight any outliers and the spread of the data.

```python
import seaborn as sns
import matplotlib.pyplot as plt

# Select numeric columns for plotting
numeric_columns = ['calories', 'carbohydrate', 'protein', 'sugar']

# Set number of rows based on number of numeric columns
num_rows = len(numeric_columns)

# Create subplots, set figsize
fig, axes = plt.subplots(nrows=num_rows, ncols=2, figsize=(12, num_rows * 4))

# Loop through relevant numeric variables
for i, col in enumerate(numeric_columns):
# Plot histogram and KDE on the left
sns.histplot(recipes[col], ax=axes[i, 0], kde=True, stat="density", linewidth=0)
axes[i, 0].set_title(f'Histogram and KDE of {col}')

# Plot boxplot on the right
sns.boxplot(x=recipes[col], ax=axes[i, 1])
axes[i, 1].set_title(f'Boxplot of {col}')

# Display plot
plt.tight_layout()
plt.show()
```

<figure>
  <img src="/assets/images/recipe_traffic/distributions.png" alt="Distribution of Nutritional Content">
  <figcaption style="text-align:left;"><em>Figure 1: Histograms and boxplots showcasing the distribution of calories, carbohydrates, protein, and sugar in the dataset, highlighting the presence of outliers in each nutritional category.</em></figcaption>
</figure>

- **Calories:**  The histogram appears to be right-skewed, indicating that most recipes have lower calorie counts, with fewer recipes having very high calorie values. The boxplot indicates the presence of outliers above the upper whisker, suggesting that some recipes have significantly higher calories than the rest.
- **Carbohydrate:** The histogram also seems right-skewed, similar to the one for calories, with most recipes featuring lower carbohydrate content. The boxplot similarly identifies outliers on the higher end, indicating some recipes with exceptionally high carbohydrate content.
- **Protein:** The histogram for protein is also right-skewed, implying that most recipes contain lower levels of protein. The boxplot reveals outliers, suggesting that some recipes are exceptionally high in protein.
- **Sugar:** This histogram exhibits a right-skewed distribution as well, indicating that most recipes contain lower amounts of sugar. The accompanying boxplot shows several outliers, suggesting some recipes have unusually high sugar content.

### Correlation Analysis

Correlation analysis is used to examine the relationships between different numerical variables. By analyzing the correlation matrix, I identified strong relationships between variables, which provided valuable insights for feature selection and model interpretation. For example, understanding how variables like calories, carbohydrates, and protein are related can help in predicting high-traffic recipes.

```python
import seaborn as sns
import matplotlib.pyplot as plt

# Calculate the correlation matrix
corr_matrix = recipes.corr()

# Generate a heatmap of the correlation matrix
plt.figure(figsize=(10, 8))
sns.heatmap(corr_matrix, annot=True, fmt=".2f", cmap="coolwarm", linewidths=0.5)
plt.title('Correlation Matrix of Nutritional Variables')
plt.show()
```

The correlation matrix provides a visual representation of how closely related the different nutritional variables are. For instance, a strong positive correlation between calories and carbohydrates might suggest that higher-calorie recipes tend to have higher carbohydrate content as well. Such correlations can inform the feature selection process, guiding which variables to prioritize in model development. This understanding is crucial in building models that accurately predict which recipes are likely to attract more traffic based on their nutritional profiles.

<figure>
  <img src="/assets/images/recipe_traffic/correlation_matrix.png" alt="Correlation Matrix of Nutritional Variables">
  <figcaption style="text-align:left;"><em>Figure 2: The correlation matrix highlights the relationships between key nutritional variables in the dataset, providing insights into how these features might collectively influence recipe popularity.</em></figcaption>
</figure>

### Category Analysis

Examining the proportion of high-traffic recipes by category provided insights into user preferences. This analysis helped me understand which types of recipes are more likely to drive traffic to the site, informing both content strategy and model development.

[Placeholder for code snippet: Category analysis]

**Visualization: High-Traffic Recipes by Category**

![High-Traffic Recipes by Category](https://seaborn.pydata.org/_images/seaborn-barplot-1.png)

## Model Selection and Evaluation

The model selection process involved choosing algorithms that are well-suited to the complexity and structure of the dataset. For this project, I selected Random Forest and Gradient Boosting models because of their ability to handle large datasets with high-dimensional features and their robustness against overfitting. These models are also interpretable, which is important for understanding the factors that contribute to a recipe's popularity.

### Model Selection

During this phase, I trained both Random Forest and Gradient Boosting models, experimenting with different hyperparameters to optimize performance. This step also included cross-validation to ensure that the models generalize well to unseen data.

[Placeholder for code snippet: Model training using Random Forest and Gradient Boosting]

### Model Evaluation

I evaluated the models using metrics like recall, precision, and the F1 score, which are crucial for assessing how well the models perform, particularly in the context of imbalanced data. The goal was to achieve at least an 80% recall rate, which ensures that the model accurately identifies most of the popular recipes, while maintaining balanced precision to avoid too many false positives.

[Placeholder for code snippet: Model evaluation metrics]

**Output**:

- **Recall**: [Placeholder for recall score]
- **Precision**: [Placeholder for precision score]
- **F1 Score**: [Placeholder for F1 score]

### Model Performance

To ensure the model's robustness, I evaluated performance across different thresholds. Adjusting the decision threshold allowed me to balance recall and precision, tailoring the model to the specific needs of the business. This step was critical in fine-tuning the model before it was deployed.

[Placeholder for code snippet: Evaluate model performance across thresholds]

**Visualization: Model Performance**

![Model Performance](https://seaborn.pydata.org/_images/seaborn-lineplot-1.png)

## Optimization and Final Model

### Threshold Adjustment

Finding the optimal threshold was crucial to balance precision and recall. This step involved analyzing how different threshold values affect model performance, ensuring that the model not only predicts popular recipes accurately but also minimizes the chances of recommending unpopular ones.

[Placeholder for code snippet: Threshold adjustment]

### Final Model

After optimizing the model, the final version was selected to predict daily high-traffic recipes for the homepage. This model was deployed with the optimal threshold, ensuring that it meets the business objective of featuring popular recipes 80% of the time.

[Placeholder for code snippet: Final model prediction]

**Visualization: Final Model Prediction**

![Final Model Prediction](https://seaborn.pydata.org/_images/seaborn-lineplot-2.png)

## Summary

In summary, I was able to address the business problem as follows:

- Recognized the importance of featuring popular recipes to increase website traffic and subscriptions.
- Worked with a comprehensive dataset, including nutritional content, categories, and user interactions.
- Ensured data quality through cleaning and preprocessing.
- Analyzed data trends and patterns, particularly the proportion of high-traffic recipes by category.
- Selected Random Forest and Gradient Boosting models and evaluated them using recall, precision, and F1 score.
- Fine-tuned the models to maintain a balance between precision and recall, achieving at least an 80% recall rate.
- Used the optimized Random Forest model to predict daily high-traffic recipes for the homepage.
- Outlined a plan for ongoing model refinement based on real-world data and user feedback.

Throughout the process, my approach was data-driven, methodical, and aligned with the objective of enhancing user engagement by featuring recipes with the highest likelihood of attracting traffic.

## Reflecting on the Journey

### Overcoming Challenges and Personal Growth

This project was more than just a technical task; it was a journey of personal and professional growth. Transitioning from a long career in accounting to data science presented its own set of challenges, from mastering new analytical techniques to understanding the complexities of predicting user behavior based on recipe data. These challenges pushed me to refine my skills in data processing, feature engineering, and model evaluation, emphasizing the importance of adaptability and continuous learning in the dynamic field of data science. Overcoming these hurdles has deepened my understanding of both the technical and strategic aspects of predictive modeling.

### Project Impact and Future Directions

Completing this recipe prediction project independently marks a significant milestone in my data science journey. It has demonstrated my ability to address complex business challenges with rigorous predictive modeling and thorough data analysis. This project sharpened my skills in model evaluation, data-driven decision-making, and translating technical findings into actionable business strategies. 

Looking ahead, I am excited to explore more advanced modeling techniques and work with larger, more diverse datasets. My commitment to embracing cutting-edge methodologies, such as deep learning and ensemble models, highlights my potential as a valuable contributor in data-focused roles. This project has been a testament to my innovative problem-solving and strategic analytical skills, positioning me for future opportunities where I can drive impactful business solutions in the data science domain.

## Conclusion

Implementing machine learning for predicting popular recipes significantly enhances the ability to optimize content on recipe platforms, ensuring that users are presented with recipes that are most likely to engage them. By applying these models and techniques, businesses can build robust content strategies that adapt to user preferences and drive increased traffic and engagement.

## Discover the Full Story

To explore the full analysis with all executed code, outputs, and visualizations, see [the complete notebook on NBViewer](https://nbviewer.org/github/timothyrobbinscpa/recipe_analysis/blob/master/src/recipe_prediction_FINAL.ipynb?flush_cache=true).
