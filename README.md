# Reveal_Next_Book
Book Recommendation System
Overview
This project is a Book Recommendation System that suggests books to users based on their past ratings. The system utilizes collaborative filtering, specifically k-Nearest Neighbors (kNN), to find books that similar users have highly rated. If the number of recommendations is less than the number requested by the user, the system will automatically suggest highly-rated books from the dataset to ensure the user gets a complete list.

Dataset
The dataset used for this project is from Goodbooks-10k on Kaggle. It contains:

Book metadata: books.csv
User ratings: ratings.csv
These files provide the foundation for building recommendations based on user preferences and book popularity.

Files:
books.csv: Contains information about the books (e.g., title, authors, publication year).
ratings.csv: Contains user ratings for the books.
Features
Personalized Recommendations: Based on collaborative filtering, the system recommends books that similar users have rated highly.
Top-rated Fallback: If the system cannot find enough personalized recommendations, it fills in the remaining slots with the top-rated books from the dataset.
Customizable Number of Recommendations: Users can specify how many book recommendations they would like to receive.
Book Details: The system provides book title, author, year of publication, and original title for each recommendation.
Requirements
To run the program, you will need the following libraries:

bash
Copy code
pip install pandas scikit-learn numpy
How It Works
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
4. Output
The system returns a list of books including:
Title
Authors
Original Publication Year
Original Title
Example Code
Here’s the main function for recommending books:

python
Copy code
def recommend_books(user_id, n_recommendations=5):
    # Check if the user_id exists in the ratings dataset
    if user_id not in user_book_matrix.index:
        return f"User ID {user_id} not found in the dataset."
    
    # Locate the user in the matrix
    user_index = user_book_matrix.index.get_loc(user_id)
    
    # Find the nearest neighbors (Increase n_neighbors if necessary)
    distances, indices = model_knn.kneighbors(user_book_matrix.iloc[user_index, :].values.reshape(1, -1), n_neighbors=10)
    
    # Get the ratings of similar users
    similar_users = indices.flatten()[1:]  # Skip the first result as it's the user themselves
    recommended_books = []
    
    for similar_user in similar_users:
        # Get books the similar user rated highly (>=4 stars)
        similar_user_ratings = user_book_matrix.iloc[similar_user]
        high_rated_books = similar_user_ratings[similar_user_ratings >= 4].index.tolist()
        
        recommended_books.extend(high_rated_books)
    
    # Remove books the user has already rated
    user_rated_books = user_book_matrix.columns[user_book_matrix.iloc[user_index] > 0].tolist()
    final_recommendations = [book for book in recommended_books if book not in user_rated_books]
    
    # Handle case when no recommendations are found
    if not final_recommendations:
        print(f"No new personalized recommendations for User ID {user_id}. Recommending popular books instead.")
        final_recommendations = []
    
    # If the number of final recommendations is less than requested, add top-rated books to fill the gap
    if len(final_recommendations) < n_recommendations:
        top_books = books[~books['book_id'].isin(user_rated_books)].sort_values(by='average_rating', ascending=False)
        top_books_to_add = top_books['book_id'].tolist()
        
        # Add top-rated books until the recommendation list is filled
        final_recommendations += [book for book in top_books_to_add if book not in final_recommendations][:n_recommendations - len(final_recommendations)]
    
    # Match final_recommendations with the correct book ID column (either book_id or best_book_id)
    recommended_books_df = books[books['book_id'].isin(final_recommendations)]
    
    # If the DataFrame is still empty, try matching with best_book_id
    if recommended_books_df.empty:
        recommended_books_df = books[books['best_book_id'].isin(final_recommendations)]
    
    # Handle case when no matching books are found in the books dataset
    if recommended_books_df.empty:
        print("No matching books found in the books dataset. Recommending top-rated books.")
        fallback_books = books.sort_values(by='average_rating', ascending=False).head(n_recommendations)
        return fallback_books[['title', 'authors', 'original_publication_year', 'original_title']]
    
    # Return the top N recommendations with specific columns
    return recommended_books_df[['title', 'authors', 'original_publication_year', 'original_title']].head(n_recommendations)
Example Usage
python
Copy code
# Recommend 5 books for user with ID 5
print(recommend_books(user_id=5, n_recommendations=5))
Example Output:
css
Copy code
| title                     | authors       | original_publication_year | original_title            |
|----------------------------|---------------|----------------------------|----------------------------|
| The Catcher in the Rye      | J.D. Salinger | 1951                       | The Catcher in the Rye      |
| To Kill a Mockingbird       | Harper Lee    | 1960                       | To Kill a Mockingbird       |
| 1984                        | George Orwell | 1949                       | Nineteen Eighty-Four        |
How to Run
Download the dataset from Kaggle Goodbooks-10k.
Install the required libraries:
bash
Copy code
pip install pandas scikit-learn numpy
Place the dataset files in the same directory as the script.
Run the script and use the recommend_books function to generate personalized recommendations.
License
This project is open-source and available under the MIT License.
