---
title: "Transforming Messy Soccer Data into Reliable Insights"
date: 2024-10-01
permalink: /data-cleaning-portfolio/
layout: single
classes: wide
author_profile: true
read_time: false
comments: false
header:
  teaser: /assets/images/data_cleaning/teaser_image.png
excerpt: "Discover how advanced data cleaning techniques unraveled inconsistencies in a complex soccer dataset, ensuring integrity for future analysis."
tags:
  - Data Cleaning
  - Data Science
  - Soccer
  - FIFA
  - Python
  - Pandas
  - Data Preparation
featured: false
---

## Introduction

Data cleaning is a critical first step in any data science project, and it becomes even more essential when working with large, complex datasets like the FIFA soccer dataset. This project is focused exclusively on the data cleaning process, ensuring that the dataset is ready for future use in analysis or modeling. The cleaning process involves addressing missing data, removing duplicates, and ensuring that all fields are formatted correctly and consistently.

## Leveraging Experience

With over 25 years of experience in accounting, revenue recognition, and data science, my background has provided me with unique insights and skills that were integral to this project. My career transition from accounting into data science has been heavily supported by my technical expertise in Python, SQL, and data processing, enabling me to manage large datasets efficiently and ensure their quality for accurate analysis. Here are some specific ways in which my experience benefited this project:

- **Data Integrity and Automation**: Having managed revenue and data systems for large software and SaaS companies, I have extensive experience implementing data governance and ensuring compliance with industry standards. This expertise translated seamlessly into ensuring that the data in this project was clean, accurate, and ready for analysis. My ability to automate processes using Python helped streamline the data cleaning tasks in this project, reducing manual intervention and improving overall efficiency.
  
- **Handling Complex Financial Data**: My long-standing experience in financial management, particularly with ERP systems and revenue recognition, has been crucial in handling and interpreting financial metrics such as `market_value_euros` and `weekly_wage_euros`. By applying my understanding of financial analysis to soccer player data, I ensured that key economic variables were processed correctly and that any missing or inconsistent values were properly addressed.

- **Technical Expertise in Data Science**: With a Master’s degree in Data Science and practical experience in machine learning and statistical modeling, I utilized advanced data manipulation techniques to clean and standardize this dataset. My proficiency in Python, including libraries such as `pandas` and `NumPy`, allowed me to efficiently identify and resolve data quality issues, such as missing values and duplicates, setting the foundation for deeper analysis and modeling.

## Overview of the Dataset

The dataset comprises detailed statistics for thousands of soccer players, including:

- **Player Demographics**: Attributes such as nationality, height, weight, and age.
- **Performance Metrics**: Player ratings, potential, and various performance-based metrics like dribbling, defending, and physicality ratings.
- **Financial Metrics**: Market value in euros, weekly wages, and release clauses.
- **Club Information**: Player team information, contract start/end dates, and loan status.

The raw dataset contains inconsistencies, missing values, and duplicates that must be addressed before any meaningful analysis or modeling can be performed. This post details the entire cleaning process to ensure the data is ready for future use.

## 1. Importing Libraries

Before starting the cleaning process, I first imported necessary libraries such as `pandas`, `numpy`, and others for data manipulation, and set pandas display options to ensure that all data rows and columns are visible for thorough inspection.

    # Import libraries
    import numpy as np
    import pandas as pd
    import matplotlib.pyplot as plt
    import seaborn as sns
    import re

    # Set pandas options to display all rows and columns
    pd.set_option('display.max_rows', None)
    pd.set_option('display.max_columns', None)

## 2. Reviewing and Handling Missing Values

### 2.1 Identifying Missing Data

In this dataset, some key columns like `release_clause_amount_euros` and `loan_date_end` had missing values. This is a significant issue because missing values in important fields like `release_clause_amount_euros` could distort financial insights when analyzing players' market values.

    # Check for missing values
    df.isna().sum().sort_values(ascending=False).head(10)

### 2.2 Filling Missing Values

To address this, I imputed missing values in numerical columns like `release_clause_amount_euros` with the median of the column, which avoids bias introduced by extreme values. For categorical fields like `loan_date_end`, missing values were filled with 'N/A' since they did not affect further analysis but needed placeholders.

    # Fill missing values with appropriate values
    df['release_clause_amount_euros'].fillna(df['release_clause_amount_euros'].median(), inplace=True)
    df['loan_date_end'].fillna('N/A', inplace=True)

**Explanation**: Imputing missing values is essential for ensuring that future analysis or model predictions based on player value data remain accurate. In this project, financial information such as release clauses directly impacts valuation modeling, so ensuring no gaps in this data is critical.

## 3. Dropping Irrelevant Columns

Some columns, like URLs or player IDs (`sofifa_id`), were not relevant to the project’s objective, which focused on player performance and market value. These columns added no value and were dropped to streamline the dataset.

    # Drop irrelevant columns
    df.drop(['sofifa_id'], axis=1, inplace=True)

**Explanation**: Removing unnecessary columns improves the efficiency of analysis by focusing only on data relevant to player performance and market valuation. This also reduces memory usage and computational overhead during model building.

## 4. Renaming Columns for Clarity

The dataset included several abbreviated or unclear column names that were renamed to more descriptive terms, making the data easier to understand during analysis.

    # Rename columns for clarity
    column_renaming = {
        'sofifa_id': 'player_id',
        'club': 'team_name',
        'market_value': 'market_value_euros',
        'height': 'height_in_inches',
        'weight': 'weight_in_pounds'
    }

    df.rename(columns=column_renaming, inplace=True)

**Explanation**: Renaming columns like `sofifa_id` to `player_id` and `market_value` to `market_value_euros` helps provide clarity, especially when working with financial metrics. This clarity is important when making data-driven decisions about player contracts or transfers, as it reduces ambiguity during analysis.

## 5. Handling Duplicates

### 5.1 Removing Full Duplicates

The presence of duplicate rows could skew any aggregation or predictive modeling results, particularly when calculating player statistics like average market value or weekly wages. Therefore, I removed full duplicates to ensure each player appeared only once in the dataset.

    # Remove duplicate rows
    df.drop_duplicates(inplace=True)

    # Confirm the removal of duplicates
    print(f"Number of duplicate rows: {df.duplicated().sum()}")

**Explanation**: Full duplicates can distort analyses like average market value by inflating the number of records for certain players. For this project, ensuring that each player has only one entry is crucial for correctly calculating overall statistics.

### 5.2 Investigating Partial Duplicates

I also checked for partial duplicates, especially in fields like `longname` (player name). Some players may have the same name but different IDs (e.g., common names like "John Smith"), so I ensured that only legitimate duplicates were removed.

    # Find duplicates based on 'longname' and 'player_id'
    longname_id_counts = df.groupby('longname')['player_id'].nunique()
    duplicate_longnames = longname_id_counts[longname_id_counts > 1]

    # Display longnames with multiple IDs
    print("Duplicate longnames and their ID counts:", duplicate_longnames)

**Explanation**: For this project, identifying and resolving cases where multiple players share the same name but have different IDs is critical for accuracy. These scenarios could lead to faulty player valuation or transfer analysis, especially when dealing with high-profile players.

## 6. Standardizing Formats

### 6.1 Standardizing Numeric Data (Height and Weight)

Player height and weight were expressed in different units (e.g., feet, inches, kilograms). To ensure consistency across the dataset, I converted all height values to inches and all weight values to pounds.

    # Convert height to inches and weight to pounds
    def convert_units():
        ...

    df['height_in_inches'] = df.apply(lambda row: convert_height_to_inches(row['height_value'], row['height_unit']), axis=1)
    df['weight_in_pounds'] = df.apply(lambda row: convert_weight_to_pounds(row['weight_value'], row['weight_unit']), axis=1)

**Explanation**: This conversion is crucial for ensuring consistency across the dataset. When analyzing player performance metrics in relation to physical attributes, having all height and weight values in the same unit is essential for accurate comparisons and predictions.

### 6.2 Rounding Height and Weight

To further standardize the data, I rounded height and weight to two decimal places, ensuring uniform precision across all player records.

    # Round height and weight to 2 decimal places
    df['height_in_inches'] = df['height_in_inches'].round(2)
    df['weight_in_pounds'] = df['weight_in_pounds'].round(2)

    # Visualize distribution of heights and weights
    plt.figure(figsize=(10, 6))
    sns.histplot(df['height_in_inches'], kde=True, bins=50)
    plt.title('Distribution of Heights (in inches)')
    plt.show()

    plt.figure(figsize=(10, 6))
    sns.histplot(df['weight_in_pounds'], kde=True, bins=50)
    plt.title('Distribution of Weights (in pounds)')
    plt.show()

**Explanation**: Rounding ensures uniformity in the data presentation. This is important when comparing players' physical attributes, as small differences in rounding can introduce inconsistency when analyzing height or weight in relation to player performance.

### 6.3 Standardizing Text Data

Text columns like `nationality` were standardized to ensure consistent spelling and capitalization. This step was particularly important for filtering and grouping players based on nationality during analysis.

    # Standardize the 'nationality' column
    df['nationality'] = df['nationality'].str.title()

**Explanation**: Ensuring consistency in text columns like `nationality` is crucial for accurate analysis, especially when comparing players across different countries. For example, misspelled or inconsistently capitalized country names can result in incorrect grouping, which would affect insights drawn from nationality-specific analyses.

## Reflecting on the Journey

### Overcoming Challenges and Personal Growth

This project presented several challenges, especially when dealing with inconsistencies in player attributes like height, weight, and market value across multiple leagues. Addressing these issues has not only enhanced my technical skills in data cleaning and processing but also provided a deeper understanding of how to prepare complex, real-world datasets for analysis. Through this project, I have gained valuable experience in handling large datasets that involve both numerical and categorical data.

### Project Impact and Future Directions

Completing this comprehensive data cleaning project has significantly impacted how I approach dataset preparation for analysis. The ability to transform raw, inconsistent data into a well-organized, reliable dataset is critical in any data science workflow, and this project has helped refine my skills in this area. Looking ahead, I am excited to apply this cleaned dataset to predictive modeling techniques and further explore insights related to player performance and market value.

## Conclusion

The data cleaning process undertaken in this project has successfully prepared a raw dataset for more advanced analyses and predictive modeling. By handling missing values, removing duplicates, standardizing formats, and ensuring consistency across all features, the dataset is now well-suited for future use.

## Discover the Full Story

To explore the full analysis with all executed code, outputs, and visualizations, see [the complete notebook on NBViewer](https://nbviewer.org/github/timothyrobbinscpa/data_cleaning/blob/master/src/data_cleaning.ipynb).
