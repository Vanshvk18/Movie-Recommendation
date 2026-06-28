# Movie Recommendation System

This notebook implements a movie recommendation system based on content-based filtering. It leverages movie metadata, including genres, keywords, cast, crew, and overview, to find similar movies.

## Project Overview

The goal of this project is to build a system that can suggest movies to users based on a movie they already like. The core idea is to find movies that are semantically similar to a given movie using advanced text embedding techniques.

## Data Source

The data used for this project is the TMDB 5000 Movie Dataset, accessed via `kagglehub` and `Hugging Face Datasets`. It consists of two main files:
- `tmdb_5000_movies.csv`: Contains detailed information about movies such as budget, genres, homepage, keywords, overview, and cast/crew references.
- `tmdb_5000_credits.csv`: Contains `movie_id`, `title`, `cast`, and `crew` details, linking to the movies dataset.

## Methodology

1.  **Data Loading**: The movie and credits data are loaded using `kagglehub.dataset_load` and converted into Pandas DataFrames.
2.  **Data Merging**: The two datasets are merged on the `title` column.
3.  **Feature Selection**: Relevant features such as `movie_id`, `title`, `overview`, `genres`, `keywords`, `cast`, and `crew` are selected for the recommendation system.
4.  **Text Preprocessing**: 
    -   Columns containing JSON-like strings (`genres`, `keywords`, `cast`, `crew`) are parsed using `ast.literal_eval` to extract meaningful information (e.g., actor names, director names).
    -   Missing values are handled (dropped).
    -   `overview` is converted into a list of words.
    -   Helper functions (`convert`, `fetch_director`, `collapse`) are used to standardize the format of these features and remove spaces for better tokenization.
    -   All processed text features (`overview`, `genres`, `keywords`, `cast`, `crew`) are concatenated into a single `tags` column for each movie.
5.  **Embedding Generation (Neural Network)**:
    -   The `sentence-transformers` library is used to generate semantic embeddings for the `tags` column. A pre-trained model (`'all-MiniLM-L6-v2'`) is employed to convert the text tags into dense numerical vectors.
6.  **Similarity Calculation**: Cosine similarity is calculated between all pairs of movie embeddings. This results in a similarity matrix where each entry represents the similarity between two movies.
7.  **Recommendation Function**: A `recommend` function is implemented that takes a movie title as input, finds its index, retrieves its similarity scores with all other movies, sorts them, and returns the titles of the top 5 most similar movies (excluding itself).

## How to Use

To get movie recommendations, simply call the `recommend` function with the title of a movie. For example:

```python
print(recommend('Avatar'))
```

This will output a list of 5 movie titles that are most similar to 'Avatar' based on their content.
