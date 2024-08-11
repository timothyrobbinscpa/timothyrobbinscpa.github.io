---
title: "Building a Robust Recommendation System Using Collaborative Filtering"
permalink: /movie_recommendation-post/
date: 2024-08-11
layout: single
classes: wide
author_profile: true
read_time: true
comments: true
toc: false
toc_sticky: true
header:
  teaser: /assets/images/movie_recommentation/movie_recommendation_graphic.webp
excerpt: "A detailed walkthrough on developing a recommendation system using collaborative filtering techniques in Python."
categories:
  - Data Science
tags:
  - Recommendation Systems
  - Collaborative Filtering
  - Python
  - Data Science Projects
published: false
featured: false
---

In this project, I developed a recommendation system using collaborative filtering techniques. The primary objective was to predict user preferences based on historical interaction data, a crucial task for personalizing user experiences in various applications such as e-commerce, streaming services, and more. This post guides you through the entire process, from data preprocessing and model building to evaluation, with a detailed walkthrough of each step and relevant code snippets.

### Data Preprocessing

Data preprocessing is a critical first step in building any machine learning model. The quality of the input data directly influences the model’s performance. For this recommendation system, the data preprocessing involved handling missing values, addressing outliers, and preparing the data for model consumption.

Handling missing values was an essential step. Missing ratings were imputed with 0, under the assumption that a missing rating implies no interaction between the user and the item. This approach ensures that the user-item matrix is fully populated, making it suitable for collaborative filtering.

    import pandas as pd  # Data manipulation
    import numpy as np  # Numerical operations
    import warnings  # Suppress warnings

    # Suppress warnings from deprecated or future features
    warnings.filterwarnings("ignore", category=DeprecationWarning)
    warnings.filterwarnings("ignore", category=FutureWarning)

    # Load data
    movies_df = pd.read_csv('movies.csv')
    ratings_df = pd.read_csv('ratings.csv')

    # Merge datasets
    merged_df = pd.merge(ratings_df, movies_df, on='movieId')

    # Handle missing values by filling with 0
    user_item_matrix = merged_df.pivot_table(index='userId', columns='movieId', values='rating').fillna(0)

The code snippet above demonstrates how the `user_item_matrix` was created by merging the movies and ratings datasets, followed by filling any missing ratings with zeros. This matrix forms the basis for the collaborative filtering process.

### Visualizing Data Distribution

Understanding the data distribution helps in uncovering patterns and insights that can inform the modeling process. I generated several visualizations to explore the distribution of ratings, the popularity of movies, and the diversity of genres.

The first visualization identifies the most popular movies by plotting the top 10 movies based on the number of ratings they received. This helps in understanding which movies are most frequently rated and thus likely to be recommended.

    # Plot the top 10 movies by number of ratings
    top_movies = merged_df['title'].value_counts().nlargest(10)
    plt.figure(figsize=(10, 5))
    sns.barplot(y=top_movies.index, x=top_movies.values, palette='viridis')
    plt.title('Top 10 Movies by Number of Ratings')
    plt.xlabel('Number of Ratings')
    plt.ylabel('Movie Title')
    plt.show()

<figure class="align-center">
  <img src="/assets/images/movie_recommendation/top_movies.png" alt="Top 10 Movies by Number of Ratings" style="width:80%;">
  <figcaption>Top 10 Movies by Number of Ratings</figcaption>
</figure>

The bar plot above reveals the top 10 movies with the highest number of ratings, providing insight into the most popular titles among users. This is valuable for understanding which movies might be commonly recommended.

Next, I visualized the genre distribution to understand the variety and popularity of genres within the dataset. This is important as it gives an idea of the diversity of content available and helps in tailoring recommendations based on genre preferences.

    # Plot the genre distribution
    merged_df['genres'] = merged_df['genres'].str.split('|')
    all_genres = merged_df['genres'].explode().value_counts()
    plt.figure(figsize=(12, 6))
    sns.barplot(x=all_genres.values, y=all_genres.index, palette='viridis')
    plt.title('Genre Distribution')
    plt.xlabel('Frequency')
    plt.ylabel('Genre')
    plt.show()

<figure class="align-center">
  <img src="/assets/images/movie_recommendation/genre_distribution.png" alt="Genre Distribution" style="width:80%;">
  <figcaption>Genre Distribution</figcaption>
</figure>

The genre distribution plot shows the frequency of each genre in the dataset, providing insights into the most and least common genres among the movies. This information can guide the recommendation system in making genre-specific suggestions.

### Collaborative Filtering Techniques

Collaborative filtering is a popular technique for building recommendation systems. It leverages the power of similarities between users or items to predict preferences. In this project, I explored two main types of collaborative filtering: user-based and item-based.

User-based collaborative filtering recommends items to a user by finding other users with similar preferences. The underlying assumption is that if two users rate several items similarly, they will likely rate other items similarly as well.

To begin with, I calculated the user similarity matrix using the cosine similarity metric, which measures the cosine of the angle between two vectors—in this case, the vectors represent user ratings.

    from sklearn.metrics.pairwise import cosine_similarity

    # Calculating cosine similarity
    user_similarity = cosine_similarity(user_item_matrix)

The cosine similarity metric was chosen because it effectively handles sparse data, which is common in recommendation systems where not every user has rated every item. The resulting `user_similarity` matrix quantifies how similar each user is to every other user.

Once the user similarity matrix was constructed, I predicted the ratings a user would give to items based on the ratings given by similar users. This is achieved by weighting the ratings of similar users and using them to predict how the target user might rate a particular item.

    # Predicting ratings
    def predict_ratings(user_similarity, user_item_matrix):
        mean_user_rating = user_item_matrix.mean(axis=1)
        ratings_diff = (user_item_matrix - mean_user_rating[:, np.newaxis])
        pred = mean_user_rating[:, np.newaxis] + user_similarity.dot(ratings_diff) / np.array([np.abs(user_similarity).sum(axis=1)]).T
        return pred

    predicted_ratings = predict_ratings(user_similarity, user_item_matrix)

The `predict_ratings` function adjusts the mean user ratings by the weighted sum of the ratings from similar users, generating predictions for how a user might rate items they haven’t yet interacted with.

Item-based collaborative filtering operates on a similar principle but focuses on the similarities between items rather than users. The assumption here is that users tend to like items similar to those they have already rated highly.

The item similarity matrix is calculated using the same cosine similarity metric, but this time the focus is on comparing items rather than users.

    # Calculating item similarity
    item_similarity = cosine_similarity(user_item_matrix.T)

With the item similarity matrix in place, I predicted the ratings by taking a weighted average of the ratings for similar items.

    # Predicting item-based ratings
    def predict_item_based_ratings(item_similarity, user_item_matrix):
        return user_item_matrix.dot(item_similarity) / np.array([np.abs(item_similarity).sum(axis=1)])

    item_predicted_ratings = predict_item_based_ratings(item_similarity, user_item_matrix)

The item-based approach is often preferred in scenarios where the similarity between items is more stable and reliable than user behavior, which can be more erratic.

### Visualizing Similarity Matrices

Visualizing the similarity matrices helps in understanding the relationships between users and between items. Heatmaps are particularly useful for this purpose, as they allow for quick identification of clusters of similar users or items.

    # Plotting the similarity matrices
    plt.figure(figsize=(10, 8))
    sns.heatmap(user_similarity, cmap='coolwarm')
    plt.title('User Similarity Matrix')
    plt.show()

<figure class="align-center">
  <img src="/assets/images/movie_recommendation/user_similarity_matrix.png" alt="User Similarity Matrix" style="width:80%;">
  <figcaption>User Similarity Matrix</figcaption>
</figure>

The user similarity matrix heatmap illustrates the degree of similarity between users. Darker shades indicate higher similarity, highlighting clusters of users with similar preferences.

    plt.figure(figsize=(10, 8))
    sns.heatmap(item_similarity, cmap='coolwarm')
    plt.title('Item Similarity Matrix')
    plt.show()

<figure class="align-center">
  <img src="/assets/images/movie_recommendation/item_similarity_matrix.png" alt="Item Similarity Matrix" style="width:80%;">
  <figcaption>Item Similarity Matrix</figcaption>
</figure>

Similarly, the item similarity matrix heatmap displays the similarities between items. This visualization is valuable for identifying groups of items that are frequently rated similarly, which can inform recommendation strategies.

### Model Evaluation

Evaluating the performance of a recommendation system is crucial to understanding how well it meets the desired objectives. Several evaluation metrics were used to assess the accuracy and effectiveness of the models.

Root Mean Square Error (RMSE) is a commonly used metric that measures the difference between the predicted and actual ratings. It is particularly useful for gauging how close the model's predictions are to the true ratings.

    from sklearn.metrics import mean_squared_error

    # Calculating RMSE
    def rmse(predicted_ratings, true_ratings):
        predicted_flat = predicted_ratings[true_ratings.nonzero()].flatten()
        true_flat = true_ratings[true_ratings.nonzero()].flatten()
        return np.sqrt(mean_squared_error(predicted_flat, true_flat))

    user_based_rmse = rmse(predicted_ratings, user_item_matrix.values)
    item_based_rmse = rmse(item_predicted_ratings, user_item_matrix.values)

    print(f'User-Based CF RMSE: {user_based_rmse}')
    print(f'Item-Based CF RMSE: {item_based_rmse}')

The RMSE values for both user-based and item-based collaborative filtering provide a quantitative measure of the model’s accuracy. Lower RMSE values indicate better predictive performance.

Precision-at-K measures the proportion of recommended items in the top-K that are relevant. This metric is useful for understanding the model's ability to recommend relevant items among the top-K predictions.

    def precision_at_k(predictions, k, threshold):
        top_k = predictions.argsort()[:, -k:]
        hits = (top_k >= threshold).sum(axis=1)
        return hits.mean()

    precision_user_based = precision_at_k(predicted_ratings, k=5, threshold=3.5)
    precision_item_based = precision_at_k(item_predicted_ratings, k=5, threshold=3.5)

    print(f'User-Based CF Precision@K: {precision_user_based}')
    print(f'Item-Based CF Precision@K: {precision_item_based}')

Precision-at-K gives a practical measure of how many of the top-K recommendations are relevant, offering insight into the effectiveness of the recommendation system in a real-world setting.

The precision-recall curve provides a graphical representation of the trade-off between precision and recall for different threshold values. It helps in visualizing the effectiveness of the recommendation system in identifying relevant items.

    from sklearn.metrics import precision_recall_curve

    y_true = user_item_matrix.values.flatten()
    y_scores = predicted_ratings.flatten()

    precision, recall, thresholds = precision_recall_curve(y_true, y_scores)

    plt.figure(figsize=(8, 6))
    plt.plot(recall, precision, marker='.')
    plt.title('Precision-Recall Curve')
    plt.xlabel('Recall')
    plt.ylabel('Precision')
    plt.show()

<figure class="align-center">
  <img src="/assets/images/movie_recommendation/precision_recall_curve.png" alt="Precision-Recall Curve" style="width:80%;">
  <figcaption>Precision-Recall Curve</figcaption>
</figure>

The precision-recall curve illustrates the balance between precision and recall across different thresholds. A higher area under the curve (AUC) indicates a better-performing model.

### Conclusion

This project demonstrates the development of a recommendation system using advanced collaborative filtering techniques. From data preprocessing to model evaluation, each step is meticulously crafted to ensure the recommendation system performs optimally in predicting user preferences.

By exploring both user-based and item-based collaborative filtering, I gained insights into the strengths and weaknesses of each approach. The detailed code snippets and visualizations provided in this post illustrate the systematic approach taken to build, evaluate, and optimize the recommendation system.

I look forward to applying these skills in a professional setting, where I can contribute to developing data-driven solutions that enhance user experiences and drive business value.
