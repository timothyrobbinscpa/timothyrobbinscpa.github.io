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
  teaser: /assets/images/messy_data_cleanup/messy_data_splash.webp
excerpt: Transforming extremely messy data into a clean, reliable dataset, ready for analysis and future insights.
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

## Overview

This project focuses exclusively on **data cleaning**, transforming a raw dataset of soccer players’ statistics from FIFA into a clean, consistent, and reliable form. The dataset includes information on player demographics, performance metrics, and financial details. My goal was to ensure that the dataset is prepared for any future analysis or modeling, removing inconsistencies, duplicates, and handling missing values efficiently.

With over 25 years of experience in both accounting and data science, I leveraged my strong technical expertise to ensure the highest standards of data integrity and accuracy for this project.

## Technologies and Tools Used

The project was developed in Python, using:
- **Pandas**: For data manipulation and cleaning tasks such as handling missing values and duplicates.
- **NumPy**: For handling numerical data and calculations.
- **Matplotlib/Seaborn**: To visualize the distributions of key variables after cleaning for consistency checks.

## Data Preprocessing

The primary focus of this project was on cleaning the dataset and ensuring consistency across the data:
- **Handling Missing Data**: Missing values in financial fields like `market_value_euros` and categorical fields like `loan_date_end` were handled carefully using imputation techniques.
- **Removing Duplicates**: Both full and partial duplicates were identified and removed to ensure that each player only appeared once in the dataset, critical for accurate future analysis.
- **Standardizing Formats**: Height and weight, which were expressed in different units, were standardized to inches and pounds, while player names and nationalities were capitalized for consistency.

<figure>
  <img src="/assets/images/data_cleaning/height_distribution.png" alt="Height Distribution After Cleaning">
  <figcaption style="text-align:left;"><em>Figure 1: Distribution of Player Heights After Data Cleaning - This histogram shows the consistent range of player heights after units were standardized to inches.</em></figcaption>
</figure>

## Key Steps

This project followed a structured approach to data cleaning:
1. **Data Inspection**: Initial exploration of the dataset to identify missing values, inconsistencies, and duplicates.
2. **Handling Missing Values**: Filling missing values based on the nature of the data, using median values for numerical fields and 'N/A' for categorical fields.
3. **Duplicate Removal**: Identifying both full and partial duplicates, especially for players with identical names but different IDs, and ensuring only legitimate records remained.
4. **Data Standardization**: Ensuring consistency in numeric and categorical data, with fields like height and weight standardized to uniform units, and text columns like nationality and player names properly capitalized.

## Insights

By cleaning the dataset, I ensured that it is well-structured and ready for future analysis or predictive modeling. The clean dataset will allow for more accurate and reliable conclusions, whether it's for player valuation, performance analysis, or other applications. Ensuring data integrity at this stage prevents errors and biases from carrying over into future steps.

## Challenges

One of the main challenges was managing the varied formats of numeric and categorical data. For example, players' heights and weights were given in inconsistent units, which required careful conversion and validation. Similarly, handling partial duplicates for players with common names presented a unique challenge, ensuring no important data was lost in the cleaning process.

## Conclusion

This project highlights my expertise in preparing complex datasets for analysis, leveraging both my experience in accounting and technical data science skills. The FIFA soccer dataset is now clean, consistent, and reliable, making it a solid foundation for future exploration and analysis.

## Discover the Full Story

For a detailed look at the data cleaning process, including code snippets, methodology, and insights, view the full project breakdown [here](/data-cleaning-post/).

If you’re interested in exploring the complete notebook with all the technical details, view it on [NBViewer](https://nbviewer.org/github/timothyrobbinscpa/data_cleaning/blob/master/src/data_cleaning.ipynb).
