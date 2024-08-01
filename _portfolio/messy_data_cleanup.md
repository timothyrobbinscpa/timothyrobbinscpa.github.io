---
title: "Transforming Raw Data: A Comprehensive Data Cleaning Project"
date: 2024-05-31
layout: single
classes: wide
author_profile: true
read_time: false
comments: false
header:
  teaser: /assets/images/messy_data_cleanup/messy_data_splash.webp
excerpt: "Discover how extensive data cleaning techniques can transform messy, raw datasets into clean, ready-to-analyze data."
---

![Messy Data Image](/assets/images/messy_data_cleanup/messy_data_splash.webp)

## Introduction

This project leverages my extensive experience in data cleaning to prepare a dataset for analysis using Python. With over two decades of experience as an accountant and CPA, primarily as a Revenue Manager, I have prepared data for several accounting system implementations, including NetSuite, SAP, and Oracle. This background has equipped me with robust data cleaning techniques that ensure the dataset's integrity and readiness for further analysis.

## Overview

The aim of this project is to clean and preprocess a dataset sourced from Kaggle, containing player statistics from EA Sports' FIFA 21 game. This messy and raw dataset, scraped from sofifa.com, provides an excellent opportunity to demonstrate comprehensive data cleaning techniques.

## Technologies and Tools Used

I utilized the following technologies and tools:
- **Python**: The primary programming language used for data cleaning.
- **Pandas**: For data manipulation and cleaning.
- **NumPy**: For numerical operations.
- **Matplotlib and Seaborn**: For data visualization.

## Data Cleaning Steps

1. **Handling Missing Values**: Identifying and filling or removing missing values to ensure data completeness. My experience with NetSuite, SAP, and Oracle has honed my ability to manage large datasets effectively.
2. **Correcting Data Types**: Converting columns to appropriate data types to maintain data integrity.
3. **Cleaning Text and Categorical Data**: Ensuring proper formatting and consistency in text and categorical data.

```python
def clean_and_split_contract(entry):
'''
    - Two Years Found: Sets both years respectively.
    - One Year Found: Assumes it is the start year and also the end year unless 'On Loan' is present.
    - Contains 'Free': Marks both years as 'Free'.
    - Contains 'On Loan': Always sets end year to 'On Loan', regardless of the number of years extracted.
    - No or More than Two Years (or malformed format): Uses NaN for start year and sets end year to 'On Loan' if specified.
    '''
    
    # First, check for 'Free' to handle these separately
    if 'Free' in entry:
        return pd.Series(['Free', 'Free'])

    # Check if 'On Loan' is part of the entry
    on_loan_present = 'On Loan' in entry

    # Extract all year numbers
    years = pd.Series(entry).str.extractall(r'(\d{4})')[0].unique()

    # Determine what to return based on the number of years found and 'On Loan' status
    if len(years) == 2:
        return pd.Series([years[0], years[1]])
    elif len(years) == 1:
        if on_loan_present:
            return pd.Series([years[0], 'On Loan'])
        else:
            return pd.Series([years[0], years[0]])  # Use the same year for both start and end
    elif on_loan_present:
        return pd.Series([np.nan, 'On Loan'])
    else:
        return pd.Series([np.nan, np.nan])
```

4. **Handling Special Characters**: Removing or replacing special characters to maintain data integrity and prevent errors in analysis.

```python
# Accent mapping for normalizing non-ASCII characters
accent_mapping = {
    'á': 'a', 'é': 'e', 'í': 'i', 'ó': 'o', 'ú': 'u', 'ü': 'u', 'ñ': 'n', 'ß': 'ss', 'ö': 'o', 'ä': 'a',
    'ë': 'e', 'ç': 'c', 'ą': 'a', 'ę': 'e', 'ł': 'l', 'ś': 's', 'ź': 'z', 'ż': 'z', 'ø': 'o', 'å': 'a',
    'œ': 'oe', 'ė': 'e', 'ȯ': 'o', 'ű': 'u', 'ơ': 'o', 'ķ': 'k', 'ņ': 'n', 'ģ': 'g', 'š': 's', 'ž': 'z',
    'č': 'c', 'ř': 'r', 'đ': 'd', 'ț': 't', 'ļ': 'l', 'ğ': 'g', 'ı': 'i', 'Č': 'C', 'Ž': 'Z', 'Ş': 'S'
}

def manual_normalize_club_names(club_names, mapping):
    """Replace non-ASCII characters in club names using above mapping."""
    normalized_names = {}
    for name in club_names:
        new_name = ''.join([mapping.get(char, char) for char in name])
        normalized_names[name] = new_name
    return normalized_names
```

5. **Dealing with Outliers**: Identifying and handling outliers to improve data quality.
6. **Dropping Irrelevant Columns**: Removing columns that are not needed for analysis to simplify the dataset and focus on relevant data.
7. **Ensuring Consistency**: Standardizing data formats and values to ensure uniformity across the dataset.

## Results

The cleaned dataset is now ready for analysis, with no missing values, consistent column names, correct data types, and standardized formatting.

## Conclusion

Handling complex data structures and ensuring the integrity of the dataset required sophisticated data manipulation strategies. My previous experience working closely with IT teams and managing large-scale data implementations proved invaluable in navigating these challenges. This data cleaning project demonstrates essential preprocessing steps to prepare data for analysis and machine learning tasks. The structured approach ensures that the dataset is clean, consistent, and ready for advanced analytical tasks.

## Discover the Full Story

Explore the detailed analysis [here](/messy-data-post/).

## Explore the Technical Journey

For more on the project, including detailed code snippets and visuals, visit the notebook on [NBViewer](https://nbviewer.org/github/timothyrobbinscpa/messy_data_cleaning/blob/master/src/data_cleaning_FINAL_FINAL.ipynb?flush_cache=true).
