---
layout: single
title: "Showcasing My Data Science Skills: A Journey from Accounting to Machine Learning"
date: 2024-05-28
published: false  # temporarily suppress project from appearing on website
Categories: 
  - Data Science
tags: 
  - Machine Learning
  - Data Science
  - Recommendation System
---

Welcome to my blog! I'm excited to share my journey from a seasoned accountant and CPA to a data science enthusiast, showcasing my skills and projects that demonstrate my capabilities in this dynamic field. With over 20 years of experience in the SaaS industry as a revenue manager, I've now transitioned into data science, leveraging my analytical skills to build sophisticated machine learning models. In this post, I'll walk you through a comprehensive project where I developed a hybrid recommendation system using collaborative filtering and content-based filtering.

## Project Overview

The project aims to create a hybrid recommendation system that combines the strengths of collaborative filtering (CF) and content-based filtering (CBF) to provide personalized movie recommendations. The project utilizes various Python libraries, including pandas, numpy, surprise, sklearn, matplotlib, and seaborn, for data manipulation, numerical operations, model building, and data visualization.

## Libraries and Data

I used several Python libraries for this project, including:

- **pandas** and **numpy** for data manipulation and numerical operations.
- **surprise** for building the collaborative filtering model.
- **sklearn** for various machine learning utilities.
- **matplotlib** and **seaborn** for data visualization.

The dataset used is the MovieLens dataset, which includes two CSV files: `movies.csv` and `ratings.csv`. These files provide the movie metadata and user ratings, respectively.

## Data Loading and Preprocessing

I started by loading the datasets and merging them to create a comprehensive dataset. This involved reading the CSV files and combining them based on the movie IDs. Additionally, I converted timestamps to a readable date format and extracted the release year from the movie titles.

## Exploratory Data Analysis (EDA)

Performing EDA is crucial to understand the data better. Here are some of the steps I took:

1. **Displaying DataFrames**: I viewed the first few rows of the movies and ratings DataFrames to understand their structure.

2. **Shape and Statistics**: I checked the shape of each DataFrame to see the number of rows and columns, and I also viewed basic statistics to get an overview of the data.

3. **Missing Values**: I identified any missing values in both DataFrames.

4. **Visualization**:
    - **Distribution of Ratings**: I plotted the distribution of movie ratings to understand how users rate movies.
    - **Number of Ratings per Movie**: This helped me see the popularity of different movies.
    - **Number of Ratings per User**: I analyzed how many ratings each user provided, giving insights into user engagement.
    - **Genre Distribution**: I examined the distribution of movie genres.
    - **Distribution of Movie Release Years**: This visualization helped me understand the trends over time.
    - **Top 10 Movies by Number of Ratings**: I identified the most popular movies based on the number of ratings they received.

### Visualization: Distribution of Ratings

![Distribution of Ratings]({{ site.baseurl }}/assets/images/distribution_of_ratings.png)

### Visualization: Number of Ratings per Movie

![Number of Ratings per Movie]({{ site.baseurl }}/assets/images/number_of_ratings_per_movie.png)

### Visualization: Number of Ratings per User

![Number of Ratings per User]({{ site.baseurl }}/assets/images/number_of_ratings_per_user.png)

### Visualization: Genre Distribution

![Genre Distribution]({{ site.baseurl }}/assets/images/genre_distribution.png)

### Visualization: Distribution of Movie Release Years

![Distribution of Movie Release Years]({{ site.baseurl }}/assets/images/distribution_of_movie_release_years.png)

### Visualization: Top 10 Movies by Number of Ratings

![Top 10 Movies by Number of Ratings]({{ site.baseurl }}/assets/images/top_10_movies_by_number_of_ratings.png)

## Time Decay and Standardization

To account for the recency of ratings, I applied a time decay factor. This means that more recent ratings were given more weight compared to older ratings. Additionally, I standardized the features to ensure they were on a similar scale, which is essential for some machine learning algorithms to perform well.

## Building the Hybrid Recommendation System

The core of this project is building a hybrid recommendation system. Here's how I did it:

### Collaborative Filtering (CF)

I used the SVD (Singular Value Decomposition) algorithm from the `surprise` library for collaborative filtering. This algorithm is well-suited for making recommendations based on the patterns in user ratings.

1. **Data Preparation**: I loaded the ratings data into the Surprise library and split it into training and testing sets.
2. **Model Training**: I trained the SVD model using a grid search to find the best parameters for the model.
3. **Model Evaluation**: I evaluated the model using metrics such as RMSE (Root Mean Squared Error) and MAE (Mean Absolute Error).

### Content-Based Filtering (CBF)

For content-based filtering, I derived user preferences based on movie features such as genres. Here are the steps:

1. **Extracting Features**: I extracted the relevant features from the movie dataset, such as genres.
2. **Calculating User Preferences**: I calculated user preferences based on the average features of the movies they rated highly.
3. **Calculating Content Scores**: I computed content-based scores by comparing user preferences with movie features using cosine similarity.

### Hybrid Scoring

I combined CF and CBF scores to generate hybrid recommendations. This involved:

1. **Combining Scores**: I weighted the CF and CBF scores and adjusted for factors such as popularity and recency.
2. **Generating Recommendations**: For each user, I combined the scores to generate a list of top movie recommendations.

## Evaluation and Metrics

Evaluating the performance of the recommendation system is essential. I used various metrics such as RMSE, MAE, Precision, Recall, and F1 Score to assess the model's accuracy. Additionally, I measured catalog coverage, novelty, personalization, and intra-list diversity to understand the quality of the recommendations.

## Conclusion

This project showcases my ability to build and evaluate a sophisticated hybrid recommendation system, demonstrating my expertise in data manipulation, machine learning, and data visualization. My transition from accounting to data science is driven by a passion for uncovering insights from data and applying advanced analytical techniques to solve complex problems. I look forward to leveraging these skills in a data science role, contributing to innovative projects, and driving data-driven decision-making.

Thank you for reading, and I hope you found this project insightful. Feel free to explore the code and visualizations, and reach out if you have any questions or feedback!

---

*Note: The full code for this project is available on my GitHub repository [link to repository]*.

---

### Connect with Me

If you're interested in learning more about my work or discussing potential opportunities, please connect with me on [LinkedIn](#) or [GitHub](#). Let's create something amazing together!
