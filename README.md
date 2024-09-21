# Reveal_Next_Book

Overview
This project is a Book Recommendation System that suggests books to users based on their past ratings. The system utilizes collaborative filtering, specifically k-Nearest Neighbors (kNN), to find books that similar users have highly rated. If the number of recommendations is less than the number requested by the user, the system will automatically suggest highly-rated books from the dataset to ensure the user gets a complete list.

# Dataset
The dataset used for this project is from Goodbooks-10k on Kaggle. It contains:

Book metadata: books.csv
User ratings: ratings.csv
These files provide the foundation for building recommendations based on user preferences and book popularity.

# Files:
books.csv: Contains information about the books (e.g., title, authors, publication year).
ratings.csv: Contains user ratings for the books.
# Features
Personalized Recommendations: Based on collaborative filtering, the system recommends books that similar users have rated highly.
Top-rated Fallback: If the system cannot find enough personalized recommendations, it fills in the remaining slots with the top-rated books from the dataset.
Customizable Number of Recommendations: Users can specify how many book recommendations they would like to receive.
Book Details: The system provides book title, author, year of publication, and original title for each recommendation.

# How It Works
1. Loading and Preprocessing the Data
Load the dataset (ratings.csv and books.csv).
Create a user-item matrix where rows represent users and columns represent books. The values represent the ratings given by users to books.
2. Collaborative Filtering
The system uses k-Nearest Neighbors (kNN) to find similar users based on their ratings.
The ratings of similar users are used to identify books that the target user might enjoy.
3. Recommendation Logic
Books that the similar users rated highly (>= 4 stars) are collected.
Books the target user has already rated are excluded.
If the number of personalized recommendations is less than requested, top-rated books from the dataset are added to complete the list.
# 4. Output
The system returns a list of books including:
Title
Authors
Original Publication Year
Original Title


python
Copy code

License
This project is open-source and available under the MIT License.
