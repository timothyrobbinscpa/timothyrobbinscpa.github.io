---
title: "Unveiling the Secrets of Cinema Revenue Success"
layout: single
classes: wide
author_profile: true
read_time: true
comments: false
header:
  teaser: /assets/images/ticket_sales/generated image.webp
excerpt: "Discover how I harness machine learning to transform cinema sales data into strategic insights, driving business success."
---

## Introduction
With a foundation in finance and a new foray into data science, I have embarked on a journey to distill cinema sales data into actionable insights.

## Unveiling the Secrets of Cinema Revenue Success
Leveraging two decades of financial expertise, I've tapped into data science to dissect and understand cinema sales, pinpointing the levers of revenue success.

## Dataset Overview
The dataset comprises over 142,000 records, encapsulating ticket sales, seating capacities, and showtimes, providing a fertile ground for analysis.

## Technologies and Tools
Certified by DataCamp in Python, I harnessed the power of pandas, matplotlib, and scikit-learn to convert data into discernible patterns and predictions.

## Exploratory Data Analysis
Careful examination of the dataset established the quality and accuracy needed for robust analysis, revealing critical insights that would inform the subsequent modeling phase.

![Daily Total Sales Over Time](/assets/images/ticket_sales/sales_over_time.png)
*Figure 1: This graph illustrates daily sales patterns, a crucial component in understanding the dynamics affecting revenue.*

### Data Preprocessing
Key steps taken in data preparation included:

- Replacing missing capacity values with the overall average.

```python
# Calculate the mean capacity for each cinema_code, excluding negative values
mean_capacity_per_cinema = df[df['capacity'] > 0].groupby('cinema_code')['capacity'].mean()

# Fill negative capacity values with the mean capacity of the corresponding cinema_code
df.loc[df['capacity'] < 0, 'capacity'] = df.loc[df['capacity'] < 0, 'cinema_code'].apply(
    lambda x: mean_capacity_per_cinema.get(x, np.nan)
)

# Handle cases where mean capacity is not available
df['capacity'].fillna(df['capacity'].mean(), inplace=True)
```

- Converting showtimes to a consistent 24-hour format.
- Normalizing data to address outlier and overcapacity issues.
- Translating categorical variables into a quantitative format.

### Model Selection
Random Forest and Gradient Boosting stood out for their predictive robustness, underpinning my strategy to boost revenue with data-driven precision.

![Aggregated Feature Importances from RF and GB Models](/assets/images/ticket_sales/feature_importances.png)
*Figure 2: Aggregated feature importances from machine learning models highlight the strongest predictors of cinema sales.*

## Actionable Strategies and Key Insights
From dynamic pricing to optimized seating, the model insights have inspired a suite of strategies aimed at augmenting sales and enhancing the cinema experience.

## Challenges and Reflections
The project stretched my skills in data manipulation, reminiscent of financial auditing's precision, and reinforced the value of integrating data science in strategic financial analysis.

For more detailed insights, please view the full analysis [here](/revenue-forecast/).
