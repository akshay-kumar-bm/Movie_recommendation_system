# Movie Recommendation System

A content-based movie recommender built in a Jupyter/Colab notebook with TF-IDF and cosine similarity, with fuzzy matching so misspelled titles still work.

## What it does
Given a favourite movie name (typed by the user), the notebook finds the closest matching title in the dataset and returns the top-N most similar movies (top 30, then a cleaner top 10 cell).

## How it works
1. Load the YBI Foundation "Movies Recommendation" CSV (4,760 movies, 21 columns; a copy is also in this repo).
2. Select five text features: `Movie_Genre`, `Movie_Keywords`, `Movie_Tagline`, `Movie_Cast`, `Movie_Director`; fill missing values with blanks and concatenate them per movie.
3. Vectorise with scikit-learn `TfidfVectorizer` (resulting matrix: 4760 x 26690).
4. Compute a 4760 x 4760 pairwise `cosine_similarity` matrix.
5. Use `difflib.get_close_matches` to correct the user's input (the notebook demo maps "avataar" to "Avatar").
6. Sort similarity scores for that movie and print the top results.

```mermaid
flowchart LR
A[CSV dataset] --> B[Select 5 text features] --> C[TF-IDF] --> D[Cosine similarity matrix]
E[User input] --> F[difflib closest title] --> G[Rank by similarity] --> H[Top 10 / 30 movies]
D --> G
```

## Tech stack
Python, pandas, NumPy, scikit-learn, difflib, Jupyter / Google Colab.

## Structure
```
Movie_Recommendation_System.ipynb   # full workflow (Colab badge included)
Movies Recommendation_dataset.csv   # dataset copy
requirements.txt                    # pandas, numpy, scikit-learn, difflib
readme.md                           # original short readme
```

## Run
```bash
pip install pandas numpy scikit-learn
jupyter notebook Movie_Recommendation_System.ipynb
```
Or open in Colab via the badge in the notebook. Note the notebook reads the CSV from a raw GitHub URL (YBI-Foundation/Dataset), so internet access is needed unless you point `pd.read_csv` at the local copy. `difflib` is in the standard library (listing it in requirements.txt is unnecessary).

## Results shown
For the query "avataar" the notebook returns Avatar first, followed by Act of Valor, Elizabethtown, New Nightmare, The Godfather, ... The similarity scores are low (about 0.09 for the second result); no quantitative evaluation is included.

## Limitations / future work
- Content-based only; no collaborative filtering or user ratings.
- No evaluation metrics; results are inspected by eye.
- Uses the `Movie_ID` as a row index into the similarity matrix, which only works while IDs match row positions.
- Could add a web UI, popularity weighting, and a persisted similarity model.
