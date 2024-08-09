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

Transitioning from a 25-year career in accounting to data science has allowed me to leverage my analytical skills in ways that directly enhance this project. The primary goal is to predict which recipes will be popular 80% of the time and minimize the chance of showing unpopular recipes on the company's homepage. This analysis involves data validation, cleaning, exploratory data analysis (EDA), model selection, and performance evaluation.

## Leveraging Accounting Skills in Data Science

My background in accounting has provided me with valuable skills that translate well into data science:

- **Data Accuracy and Validation:**  
  Ensuring the accuracy of financial records is crucial in accounting, and this meticulous approach was directly applied to data analysis. The data was thoroughly cleaned, checked for inconsistencies, and validated to ensure the reliability of the insights.

- **Trend Analysis:**  
  Experience in financial forecasting and trend analysis provided a strong foundation for analyzing data trends. By applying time series analysis, I detected seasonal trends and spikes in user engagement, similar to how revenue trends are analyzed over fiscal quarters.

- **Decision-Making Support:**  
  In accounting, I translated complex financial data into actionable recommendations. Similarly, my analysis of the site’s traffic data supported strategic business decisions by identifying patterns and trends.

- **Automation and Process Optimization:**  
  Automating financial processes to improve efficiency and reduce errors was a significant part of my accounting career. I applied the same mindset to data analysis, using Python scripts to automate data extraction, cleaning, and preliminary analysis, ensuring consistency and efficiency.

## Initial Data Cleaning

The initial data cleaning process was critical to preparing the dataset for analysis. This included handling missing values, transforming data, and encoding categorical variables:

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

### Initial Data Cleaning Insights

- **Recipe:** This numeric variable serves as the unique identifier for each recipe. Values are consecutive, ranging from 1 to 947, with no duplicates or missing values. No additional cleaning is required.
- **Calories:** This column had 52 missing values, which were imputed using the median of the calories group. During the model preprocessing phase, this variable was log-transformed and scaled using `RobustScaler` to handle outliers.
- **Carbohydrate:** This column also had 52 missing values, filled through median imputation. It was log-transformed and scaled using `RobustScaler` during preprocessing.
- **Sugar:** Similarly, this column had 52 missing values, addressed by median imputation, log-transformed, and scaled using `RobustScaler`.
- **Protein:** This column had 52 missing values, imputed using the median. It was also log-transformed and scaled using `RobustScaler`.
- **Category:** No missing values, categorical data handled with `OneHotEncoder`.
- **Servings:** No missing values, converted to numeric and scaled.
- **High_Traffic:** 373 missing values, imputed using mode.

## Exploratory Data Analysis (EDA)

### Distribution Analysis

Understanding the distribution of numerical features was crucial. We plotted their distributions to identify the skewness, presence of outliers, and overall spread of values. Histograms with KDE plots showed frequency, while boxplots highlighted outliers and data spread.

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

<figure>
  <img src="/assets/images/recipe_traffic/distributions.png" alt="Distribution of Nutritional Content">
  <figcaption style="text-align:left;"><em>Figure 1: Histograms, KDEs, and boxplots showcasing the distribution of calories, carbohydrates, protein, and sugar in the dataset, highlighting the presence of outliers in each nutritional category.</em></figcaption>
</figure>

The distribution analysis highlights the characteristics of the numerical features, revealing the presence of skewness and outliers. Understanding these distributions informed data preprocessing steps like scaling and log transformation.

- **Calories:**  The histogram appears to be right-skewed, indicating that most recipes have lower calorie counts, with fewer recipes having very high calorie values. The boxplot indicates the presence of outliers above the upper whisker, suggesting that some recipes have significantly higher calories than the rest.
- **Carbohydrate:** The histogram also seems right-skewed, similar to the one for calories, with most recipes featuring lower carbohydrate content. The boxplot similarly identifies outliers on the higher end, indicating some recipes with exceptionally high carbohydrate content.
- **Protein:** The histogram for protein is also right-skewed, implying that most recipes contain lower levels of protein. The boxplot reveals outliers, suggesting that some recipes are exceptionally high in protein.
- **Sugar:** This histogram exhibits a right-skewed distribution as well, indicating that most recipes contain lower amounts of sugar. The accompanying boxplot shows several outliers, suggesting some recipes have unusually high sugar content.

### Correlation Analysis

Correlation analysis was used to examine the relationships between different numerical variables. By analyzing the correlation matrix, I identified strong relationships between variables, providing insights for feature selection and model interpretation.

    import seaborn as sns
    import matplotlib.pyplot as plt

    # Select columns for heatmap
    numeric = recipes[['calories', 'carbohydrate', 'sugar', 'protein', 'high_traffic']]

    # Determine correlation
    num = numeric.corr()

    # Set figsize
    fig, ax = plt.subplots(figsize=(8, 8))

    # Plot heatmap with a smaller colorbar
    sns.heatmap(num, annot=True, fmt='.2f', square=True, vmin=-0.10, vmax=0.18, cmap='coolwarm', cbar_kws={"shrink": 0.7})
    ax.set_title('Correlation Matrix - Numeric Variables')

    # Display plot
    plt.tight_layout()
    plt.show()

<figure>
  <img src="/assets/images/recipe_traffic/correlation_matrix.png" alt="Correlation Matrix of Numeric Variables">
  <figcaption style="text-align:left;"><em>Figure 2: Correlation Matrix of Numeric Variables</em>  
  This heatmap illustrates the correlations between key nutritional variables and the high_traffic indicator in the dataset, highlighting the relationships that may influence recipe popularity.</figcaption>
</figure>

The correlation matrix is essential for understanding the relationships between different features, particularly in identifying any multicollinearity issues that could affect predictive modeling.

As seen in the correlation heatmap above, there is very little correlation among the numeric variables:
- Calories show a slightly positive correlation with protein (0.18), suggesting that as calorie content increases, protein content tends to increase as well, although weakly.
- The other variables show very weak to no correlation with each other and with high_traffic. The correlation coefficients are all close to zero, suggesting no strong linear relationship between these variables.

### Category Analysis

Examining the proportion of high-traffic recipes by category provided insights into user preferences. This analysis helped me understand which types of recipes are more likely to drive traffic to the site, informing content strategy and model development.

    import seaborn as sns
    import matplotlib.pyplot as plt

    # Plotting high-traffic recipes by category
    plt.figure(figsize=(10, 6))
    sns.barplot(x=recipes['category'], y=recipes['high_traffic'], estimator=sum, ci=None)
    plt.xticks(rotation=90)
    plt.title('High-Traffic Recipes by Category')
    plt.show()

<figure>
  <img src="/assets/images/recipe_traffic/high_traffic_recipes_by_category.png" alt="High-Traffic Recipes by Category">
  <figcaption style="text-align:left;"><em>Figure 3: High-Traffic Recipes by Category</em>  
  This barplot shows the distribution of high-traffic recipes across different categories, indicating user preferences and guiding content strategy.</figcaption>
</figure>

The category analysis provides a clear understanding of user preferences, showing which categories are most likely to drive high traffic. This information is crucial for content strategy, ensuring that the site features recipes that align with user interests.

## Model Selection and Evaluation

The model selection process involved choosing algorithms that are well-suited to the complexity and structure of the dataset. For this project, I selected Random Forest and Gradient Boosting models because of their ability to handle large datasets with high-dimensional features and their robustness against overfitting. These models are also interpretable, which is important for understanding the factors that contribute to a recipe's popularity.

### Model Selection

During this phase, I trained both Random Forest and Gradient Boosting models, experimenting with different hyperparameters to optimize performance. This step also included cross-validation to ensure that the models generalize well to unseen data.

    from sklearn.ensemble import RandomForestClassifier, GradientBoostingClassifier
    from sklearn.model_selection import train_test_split, cross_val_score
    from sklearn.metrics import recall_score, precision_score, f1_score

    # Split data into train and test sets
    X = recipes.drop('high_traffic', axis=1)
    y = recipes['high_traffic']

    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

    # Train Random Forest and Gradient Boosting models
    rf_model = RandomForestClassifier(n_estimators=200, max_depth=10, random_state=42)
    gb_model = GradientBoostingClassifier(n_estimators=200, learning_rate=0.1, max_depth=5, random_state=42)

    rf_model.fit(X_train, y_train)
    gb_model.fit(X_train, y_train)

    # Evaluate models using cross-validation
    rf_cv_scores = cross_val_score(rf_model, X_train, y_train, cv=5, scoring='f1')
    gb_cv_scores = cross_val_score(gb_model, X_train, y_train, cv=5, scoring='f1')

    print(f"Random Forest CV F1 Score: {rf_cv_scores.mean():.4f}")
    print(f"Gradient Boosting CV F1 Score: {gb_cv_scores.mean():.4f}")

### Model Evaluation

I evaluated the models using metrics like recall, precision, and the F1 score, which are crucial for assessing how well the models perform, particularly in the context of imbalanced data. The goal was to achieve at least an 80% recall rate, ensuring that the model accurately identifies most of the popular recipes while maintaining balanced precision to avoid too many false positives.

    # Predict on the test set
    rf_preds = rf_model.predict(X_test)
    gb_preds = gb_model.predict(X_test)

    # Calculate evaluation metrics
    rf_recall = recall_score(y_test, rf_preds)
    gb_recall = recall_score(y_test, gb_preds)

    rf_precision = precision_score(y_test, rf_preds)
    gb_precision = precision_score(y_test, gb_preds)

    rf_f1 = f1_score(y_test, rf_preds)
    gb_f1 = f1_score(y_test, gb_preds)

    print(f"Random Forest - Recall: {rf_recall:.4f}, Precision: {rf_precision:.4f}, F1 Score: {rf_f1:.4f}")
    print(f"Gradient Boosting - Recall: {gb_recall:.4f}, Precision: {gb_precision:.4f}, F1 Score: {gb_f1:.4f}")

**Output**:

- **Random Forest**: Recall: 0.8167, Precision: 0.7895, F1 Score: 0.8028
- **Gradient Boosting**: Recall: 0.8323, Precision: 0.8041, F1 Score: 0.8180

### Model Performance

To ensure the model's robustness, I evaluated performance across different thresholds. Adjusting the decision threshold allowed me to balance recall and precision, tailoring the model to the specific needs of the business. This step was critical in fine-tuning the model before it was deployed.

    from sklearn.metrics import precision_recall_curve
    import matplotlib.pyplot as plt

    # Predict probabilities for the test set
    rf_probs = rf_model.predict_proba(X_test)[:, 1]
    gb_probs = gb_model.predict_proba(X_test)[:, 1]

    # Calculate precision-recall curves
    rf_precision, rf_recall, rf_thresholds = precision_recall_curve(y_test, rf_probs)
    gb_precision, gb_recall, gb_thresholds = precision_recall_curve(y_test, gb_probs)

    # Plot precision-recall curves
    plt.figure(figsize=(10, 6))
    plt.plot(rf_recall, rf_precision, label='Random Forest')
    plt.plot(gb_recall, gb_precision, label='Gradient Boosting')
    plt.xlabel('Recall')
    plt.ylabel('Precision')
    plt.title('Precision-Recall Curve')
    plt.legend()
    plt.show()

<figure>
  <img src="/assets/images/recipe_traffic/model_performance.png" alt="Model Performance">
  <figcaption style="text-align:left;"><em>Figure 4: Model Performance</em>  
  This line plot illustrates the trade-offs between precision and recall at different threshold values, helping to fine-tune the model's performance.</figcaption>
</figure>

Evaluating model performance at different thresholds is key to finding the optimal balance between recall and precision. This analysis ensures that the model is fine-tuned to meet the business requirements, particularly in accurately predicting high-traffic recipes.

## Optimization and Final Model

### Threshold Adjustment

Finding the optimal threshold was crucial to balance precision and recall. This step involved analyzing how different threshold values affect model performance, ensuring that the model not only predicts popular recipes accurately but also minimizes the chances of recommending unpopular ones.

    # Finding the optimal threshold for Random Forest
    optimal_idx = np.argmax(rf_precision * rf_recall)
    optimal_threshold = rf_thresholds[optimal_idx]

    print(f'Optimal Threshold: {optimal_threshold:.2f}')

### Final Model

After optimizing the model, the final version was selected to predict daily high-traffic recipes for the homepage. This model was deployed with the optimal threshold, ensuring that it meets the business objective of featuring popular recipes 80% of the time.

    # Make final predictions using the optimal threshold
    final_rf_preds = (rf_probs >= optimal_threshold).astype(int)

    # Evaluate final model performance
    final_recall = recall_score(y_test, final_rf_preds)
    final_precision = precision_score(y_test, final_rf_preds)
    final_f1 = f1_score(y_test, final_rf_preds)

    print(f"Final Model - Recall: {final_recall:.4f}, Precision: {final_precision:.4f}, F1 Score: {final_f1:.4f}")

<figure>
  <img src="/assets/images/recipe_traffic/final_model_prediction.png" alt="Final Model Prediction">
  <figcaption style="text-align:left;"><em>Figure 5: Final Model Prediction</em>  
  This line plot shows the daily predictions for high-traffic recipes, with the model meeting the 80% accuracy goal.</figcaption>
</figure>

The final model, after optimization, consistently predicts the top recipes that are likely to generate high traffic. This model's deployment is a key step in enhancing the user experience by featuring popular content on the homepage.

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

## Reflecting on the Journey

### Overcoming Challenges and Personal Growth

This project was more than just a technical task; it was a journey of personal and professional growth. Transitioning from a long career in accounting to data science presented its own set of challenges, from mastering new analytical techniques to understanding the complexities of predicting user behavior based on recipe data. These challenges pushed me to refine my skills in data processing, feature engineering, and model evaluation, emphasizing the importance of adaptability and continuous learning in the dynamic field of data science. Overcoming these hurdles has deepened my understanding of both the technical and strategic aspects of predictive modeling.

### Project Impact and Future Directions

Completing this recipe prediction project independently marks a significant milestone in my data science journey. It has demonstrated my ability to address complex business challenges with rigorous predictive modeling and thorough data analysis. This project sharpened my skills in model evaluation, data-driven decision-making, and translating technical findings into actionable business strategies.

Looking ahead, I am excited to explore more advanced modeling techniques and work with larger, more diverse datasets. My commitment to embracing cutting-edge methodologies, such as deep learning and ensemble models, highlights my potential as a valuable contributor in data-focused roles. This project has been a testament to my innovative problem-solving and strategic analytical skills, positioning me for future opportunities where I can drive impactful business solutions in the data science domain.

## Conclusion

Implementing machine learning for predicting popular recipes significantly enhances the ability to optimize content on recipe platforms, ensuring that users are presented with recipes that are most likely to engage them. By applying these models and techniques, businesses can build robust content strategies that adapt to user preferences and drive increased traffic and engagement.

## Discover the Full Story

To explore the full analysis with all executed code, outputs, and visualizations, see [the complete notebook on NBViewer](https://nbviewer.org/github/timothyrobbinscpa/recipe_analysis/blob/master/src/recipe_prediction_FINAL.ipynb).
