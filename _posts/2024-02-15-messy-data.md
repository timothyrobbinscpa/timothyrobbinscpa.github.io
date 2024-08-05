---
title: "Data Cleaning: A Comprehensive Guide"
permalink: /messy-data-post/
date: 2024-01-01
published: false
layout: single
author_profile: true
read_time: true
comments: true
toc: true
toc_sticky: true
header:
  teaser: /assets/images/messy_data_cleanup/messy_data_splash.webp
excerpt: "A detailed project on cleaning the FIFA 21 dataset using Python."
# last_modified_at: 2023-05-25T12:00:00-05:00
categories:
  - Data Science
tags:
  - data cleaning
  - python
  - pandas
featured: false
---
## Introduction

Data cleaning is a crucial step in data analysis and machine learning, ensuring that the data is accurate, consistent, and usable. This guide walks through a detailed data cleaning process using Python, addressing common data issues and demonstrating best practices to prepare the dataset for analysis. The dataset, containing FIFA 21 player statistics scraped from sofifa.com, is messy and raw, presenting an excellent opportunity to learn about data cleaning. Problems include missing values, inconsistent data types, duplicates, and non-standard formats.

## Step-by-Step Data Cleaning Process

### Importing Necessary Libraries

First, I imported essential libraries for data manipulation, numerical operations, and visualization.

### Loading the Dataset
Next, I loaded the dataset into a pandas DataFrame. This allowed me to perform various operations and manipulations on the data.

```python
df = pd.read_csv('path_to_your_dataset.csv', low_memory=False)
```

### Initial Data Exploration
I explored the dataset initially to understand its structure and the nature of the data. Checking the first few rows and getting a summary of the dataset, including data types and the presence of missing values, helped in identifying potential issues.

```python
df.head()
df.info()
df.describe
```

### Handling Missing Values
Missing values can significantly affect analysis and model performance. I identified columns with missing values and decided on an appropriate strategy to handle them. For instance, the loan_date_end column had many missing values, as not all players are on loan. The hits column, which might indicate popularity or performance metrics, had missing values that could skew analysis.

```python
missing_values = df.isnull().sum()
print(missing_values)
```

To handle missing values, I filled numerical columns with the mean and categorical columns with the mode. For more specific cases:

```python
df.fillna(df.mean(), inplace=True)
df['Position'].fillna(df['Position'].mode()[0], inplace=True)
df['loan_date_end'].fillna(method='bfill', inplace=True)
df['hits'].fillna(df['hits'].median(), inplace=True)
```

### Renaming Columns
To standardize column names, I converted them to snake_case. This makes it easier to work with the data programmatically and ensures consistency.

```python
df.columns = [col.strip().replace(' ', '_').lower() for col in df.columns]
```

### Removing Duplicates
Duplicate entries can skew analysis and lead to inaccurate results. I checked for duplicates and removed them to ensure each entry in the dataset was unique. Full and partial duplicates were removed based on specific columns like Name, Age, and club_name.

```python
duplicates = df.duplicated().sum()
print(f"Number of duplicate rows: {duplicates}")

df.drop_duplicates(inplace=True)
df = df.drop_duplicates(subset=['name', 'age', 'club_name'])
```

### Data Type Conversion
Ensuring each column has the correct data type is essential for accurate analysis. I converted data types where necessary, such as transforming date columns to datetime objects and ensuring numerical columns were in the correct format. Converting units like height and weight to a consistent format was also crucial.

```python
df['dob'] = pd.to_datetime(df['dob'])
df['weight_kg'] = df['weight_kg'].astype(float)

# Converting height to cm and weight to kg
df['height_cm'] = df['height'].apply(lambda x: x if 'cm' in x else float(x) * 2.54)
df['weight_kg'] = df['weight'].apply(lambda x: x if 'kg' in x else float(x) * 0.453592)
```

### Handling Outliers
Outliers can distort statistical analyses and model predictions. I identified outliers using statistical methods and visualizations like box plots and handled them appropriately. Outliers can be removed or transformed depending on the context and analysis requirements.

```python
plt.boxplot(df['age'])
plt.show()

from scipy.stats import zscore
df = df[(np.abs(zscore(df['age'])) < 3)]
```

### Feature Engineering
Feature engineering involved creating new features from existing data to enhance model performance. This step included combining columns, creating interaction terms, or extracting information from date columns.

```python
df['BMI'] = df['weight_kg'] / (df['height_cm']/100)**2
df['birth_year'] = df['dob'].dt.year
```

### Data Normalization and Scaling
Normalization and scaling are important for algorithms that are sensitive to the scale of data, such as distance-based algorithms. Techniques like Min-Max scaling or Z-score normalization were used.

```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()
df[['age', 'overall']] = scaler.fit_transform(df[['age', 'overall']])
```

### Formatting Club Names with Foreign Characters
Club names with foreign characters were standardized to maintain consistency across the dataset.

```python
df['club_name'] = df['club_name'].str.normalize('NFKD').str.encode('ascii', errors='ignore').str.decode('utf-8')
```

### Standardizing Formats
Standardizing formats, such as date formats, ensures consistency in the dataset.

```python
df['contract_valid_until'] = pd.to_datetime(df['contract_valid_until'], format='%Y')
```

## Conclusion
Data cleaning is an iterative and detailed process. By following the steps outlined above, the dataset was prepared for analysis and modeling. Clean data leads to more accurate insights and better-performing models. Investing time in data cleaning is crucial for the success of any data-driven project.

To explore the full analysis with all executed code, outputs, and visualizations, see [the complete notebook on NBViewer](https://nbviewer.org/github/timothyrobbinscpa/messy_data_cleaning/blob/master/src/data_cleaning_FINAL_FINAL.ipynb?flush_cache=true).