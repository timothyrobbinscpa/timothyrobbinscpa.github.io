---
title: "Data Cleaning: A Comprehensive Guide"
permalink: /messy-data-post/
date: 2024-04-30
layout: single
classes: wide
author_profile: true
read_time: true
comments: true
toc: true
toc_sticky: true
header:
  teaser: /assets/images/messy_data_cleanup/messy_data_splash.webp
excerpt: "A detailed guide on cleaning the FIFA 21 dataset using Python."
last_modified_at: 2023-05-25T12:00:00-05:00
categories:
  - Data Science
tags:
  - data cleaning
  - python
  - pandas
---

## Summary of Data Cleaning Steps

1. **Importing Necessary Libraries**: Import essential libraries such as pandas, numpy, and matplotlib.
2. **Loading the Dataset**: Load the dataset into a pandas DataFrame.
3. **Initial Data Exploration**: Explore the dataset to understand its structure and the nature of the data.
4. **Handling Missing Values**: Identify and handle missing values using appropriate strategies.
5. **Renaming Columns**: Standardize column names to snake_case.
6. **Removing Duplicates**: Check for and remove duplicate entries.
7. **Data Type Conversion**: Ensure each column has the correct data type.
8. **Handling Outliers**: Identify and handle outliers using statistical methods and visualizations.
9. **Feature Engineering**: Create new features from existing data to enhance model performance.
10. **Data Normalization and Scaling**: Normalize and scale data for algorithms sensitive to the scale of data.
11. **Advanced Data Cleaning Techniques**: Handle mixed data types, parse and extract information from text, and correct misspelled categories.

## Introduction

Data cleaning is a crucial step in data analysis and machine learning, ensuring that the data is accurate, consistent, and usable. This guide walks through a detailed data cleaning process using Python, addressing common data issues and demonstrating best practices to prepare the dataset for analysis. The dataset, containing FIFA 21 player statistics scraped from sofifa.com, is messy and raw, presenting an excellent opportunity to learn about data cleaning. Problems include missing values, inconsistent data types, duplicates, and non-standard formats.

## Step-by-Step Data Cleaning Process

### 1. Importing Necessary Libraries

First, I imported essential libraries for data manipulation, numerical operations, and visualization.

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import re

# Setting pandas options to display all rows and columns in the dataframe
pd.set_option('display.max_rows', None)  # Display all rows
pd.set_option('display.max_columns', None)  # Display all columns

# Setting the plot style of matplotlib to 'ggplot' for better aesthetics
plt.style.use('ggplot')
```

2. Loading the Dataset
Next, I loaded the dataset into a pandas DataFrame. This allowed me to perform various operations and manipulations on the data.

```python
df = pd.read_csv('path_to_your_dataset.csv', low_memory=False)
```

3. Initial Data Exploration
I explored the dataset initially to understand its structure and the nature of the data. Checking the first few rows and getting a summary of the dataset, including data types and the presence of missing values, helped in identifying potential issues.

```python
df.head()

df.info()

df.describe
```

4. Handling Missing Values
Missing values can significantly affect analysis and model performance. I identified columns with missing values and decided on an appropriate strategy to handle them. For instance, the loan_date_end column had many missing values, as not all players are on loan. The hits column, which might indicate popularity or performance metrics, had missing values that could skew analysis.

```python
missing_values = df.isnull().sum()
print(missing_values)
```

Output:

ID                          0
Name                        0
Age                         0
Overall                     0
...                         ...
Contract Valid Until     1061
dtype: int64

To handle missing values, I filled numerical columns with the mean and categorical columns with the mode. For more specific cases:

```python
df.fillna(df.mean(), inplace=True)
df['Position'].fillna(df['Position'].mode()[0], inplace=True)
df['loan_date_end'].fillna(method='bfill', inplace=True)
df['hits'].fillna(df['hits'].median(), inplace=True)
```

5. Renaming Columns
To standardize column names, I converted them to snake_case. This makes it easier to work with the data programmatically and ensures consistency.

``python
df.columns = [col.strip().replace(' ', '_').lower() for col in df.columns]
```

6. Removing Duplicates
Duplicate entries can skew analysis and lead to inaccurate results. I checked for duplicates and removed them to ensure each entry in the dataset was unique. Full and partial duplicates were removed based on specific columns like Name, Age, and club_name.

```python
duplicates = df.duplicated().sum()
print(f"Number of duplicate rows: {duplicates}")

df.drop_duplicates(inplace=True)
df = df.drop_duplicates(subset=['name', 'age', 'club_name'])
```
Output:
Number of duplicate rows: 0

7. Data Type Conversion
Ensuring each column has the correct data type is essential for accurate analysis. I converted data types where necessary, such as transforming date columns to datetime objects and ensuring numerical columns were in the correct format. Converting units like height and weight to a consistent format was also crucial.

```python
df['dob'] = pd.to_datetime(df['dob'])
df['weight_kg'] = df['weight_kg'].astype(float)

# Converting height to cm and weight to kg
df['height_cm'] = df['height'].apply(lambda x: x if 'cm' in x else float(x) * 2.54)
df['weight_kg'] = df['weight'].apply(lambda x: x if 'kg' in x else float(x) * 0.453592)
```

8. Handling Outliers
Outliers can distort statistical analyses and model predictions. I identified outliers using statistical methods and visualizations like box plots and handled them appropriately. Outliers can be removed or transformed depending on the context and analysis requirements.

```python
plt.boxplot(df['age'])
plt.show()

from scipy.stats import zscore
df = df[(np.abs(zscore(df['age'])) < 3)]
```

9. Feature Engineering
Feature engineering involved creating new features from existing data to enhance model performance. This step included combining columns, creating interaction terms, or extracting information from date columns.

```python
df['BMI'] = df['weight_kg'] / (df['height_cm']/100)**2
df['birth_year'] = df['dob'].dt.year
```

10. Data Normalization and Scaling
Normalization and scaling are important for algorithms that are sensitive to the scale of data, such as distance-based algorithms. Techniques like Min-Max scaling or Z-score normalization were used.

```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()
df[['age', 'overall']] = scaler.fit_transform(df[['age', 'overall']])
```

11. Formatting Club Names with Foreign Characters
Club names with foreign characters were standardized to maintain consistency across the dataset.

```python
df['club_name'] = df['club_name'].str.normalize('NFKD').str.encode('ascii', errors='ignore').str.decode('utf-8')
```

12. Standardizing Formats
Standardizing formats, such as date formats, ensures consistency in the dataset.

```python
df['contract_valid_until'] = pd.to_datetime(df['contract_valid_until'], format='%Y')
```

Conclusion
Data cleaning is an iterative and detailed process. By following the steps outlined above, the dataset was prepared for analysis and modeling. Clean data leads to more accurate insights and better-performing models. Investing time in data cleaning is crucial for the success of any data-driven project.

Feel free to reach out if there are any questions or further clarification is needed on any of the steps. Happy cleaning!