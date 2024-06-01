---
title: "Cinema Revenue Analysis"
excerpt: "Uncovering the factors that drive sales in the cinema industry using machine learning."
sidebar:
  - title: "Table of Contents"
    nav: "toc"
toc: true
layout: single
author_profile: true
---

## Introduction

This project centers on uncovering the factors that drive sales in the cinema industry. Leveraging advanced machine learning methods like Random Forest and Gradient Boosting, the goal is to identify key elements influencing cinema revenue.

## Objective

The primary objective here is to understand which variables significantly impact cinema sales. The analysis delves into various aspects of cinema data, aiming to uncover the influences on consumer behavior and ticket sales.

## Dataset Overview

The dataset was downloaded from Kaggle at [Cinema Ticket Offers](https://www.kaggle.com/datasets/arashnic/cinema-ticket/dataoffers). It offers a comprehensive view of cinema sales across multiple venues during 2018, encompassing a wide range of data points from ticket sales to seating capacities.

### Key Features Include:

- **film_code**: A unique identifier for each film shown.
- **cinema_code**: A unique identifier for each cinema venue.
- **total_sales**: The total revenue generated from each screening.
- **tickets_sold**: Number of tickets sold for each show.
- **tickets_out**: Count of tickets that were canceled.
- **show_time**: The schedule for each film screening.
- **occu_perc**: The percentage of occupied seats in the cinema.
- **ticket_price**: The cost of tickets for each show.
- **ticket_use**: The actual number of tickets used by the audience.
- **capacity**: The seating capacity of the cinema.
- **date**: The specific date of each screening.
- **month**: The month in which the screening took place.
- **quarter**: The business quarter of each screening event.
- **day**: The day of the week the film was shown.

## Initial Exploratory Data Analysis (EDA)

The EDA begins with importing the necessary libraries and loading the dataset. Initial statistics reveal the range of values for identifiers like film_code and cinema_code, along with sales and ticket details.

## Key Insights

1. **Film and Cinema Codes**: These identifiers show a wide range of values, indicating diverse films and cinema venues.
2. **Sales and Tickets**: The total_sales variable shows significant variation, highlighting the importance of ticket sales and cancellations.
3. **Occupancy and Pricing**: The occu_perc and ticket_price variables provide insights into audience behavior and pricing strategies.

## Advanced Machine Learning Methods

The project applies advanced machine learning techniques, including Random Forest and Gradient Boosting, to predict and analyze cinema revenue. These models help identify the key factors that influence sales, providing actionable insights for cinema managers.

## Conclusion

This analysis provides a detailed look at the factors driving cinema sales, utilizing a comprehensive dataset and advanced machine learning methods. The insights gained can help optimize ticket pricing, scheduling, and overall cinema management strategies.
