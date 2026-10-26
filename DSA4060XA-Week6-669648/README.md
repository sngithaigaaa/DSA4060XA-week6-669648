# DSA4060XA – Week 6 Guided Evaluation Lab

**Name:** Sean
**Student ID:** 669648

## Lab objective
Evaluate three recommenders on a small movie-ratings dataset (12 users, 20 movies, 120 ratings) using rating-prediction metrics (MAE, RMSE), ranking metrics (Precision@K, Recall@K, NDCG@K) and beyond-accuracy measures (catalogue coverage, novelty, diversity), then recommend a model for a stated business objective.

## Models compared
1. Popularity baseline (movie mean rating, global mean fallback)
2. Content-based filtering (genre features, rating-weighted user profile, cosine similarity)
3. Item-based collaborative filtering (item-item cosine similarity, similarity-weighted average)

Split: each user's most recent rating is held out for testing (108 train / 12 test). A test item is "relevant" if its rating is 4 or 5 (5 of the 12 test items).

## Results

| Model | MAE | RMSE | P@3 | R@3 | NDCG@3 | Coverage | Novelty | Diversity |
|---|---|---|---|---|---|---|---|---|
| Popularity Baseline | 1.086 | 1.201 | 0.067 | 0.20 | 0.10 | 0.35 | 1.689 | 0.736 |
| Content-Based | 1.094 | 1.381 | 0.067 | 0.20 | 0.20 | 0.70 | 1.344 | 0.542 |
| Item-Based CF | 0.924 | 1.070 | 0.000 | 0.00 | 0.00 | 0.85 | 1.695 | 0.780 |

- **Best model for rating prediction:** Item-Based CF (lowest MAE and RMSE).
- **Best model for ranking:** Content-Based (ties popularity on precision and recall, but ranks its hit higher, NDCG@3 = 0.20). Item-Based CF found no relevant items in the Top-3.
- **Coverage, novelty and diversity:** Item-Based CF is highest on coverage (0.85) and diversity (0.78) and level with popularity on novelty (about 1.69). Popularity has very low coverage (0.35) because everyone gets nearly the same list. Content-Based is lowest on novelty and diversity because it keeps recommending movies with similar genres.
- **Final recommended model:** Item-Based CF for the stated goal (discovery without popular titles dominating), accepting that its ranking relevance was not demonstrated on this test set. In practice I would blend it with the content-based model.

Extension (Option A, K = 1, 3, 5): Precision@K falls as K grows because each user has at most one relevant test item, while Recall@K rises or stays flat. Content-based recall goes 0.20, 0.20, 0.40.

## Limitations
1. Tiny evaluation: only 12 test interactions and only 5 users with a relevant item, so one extra hit changes Precision, Recall and NDCG a lot and the model differences are not statistically reliable.
2. Leave-one-out with a single held-out item per user caps the best possible Precision@3 at 0.33 and gives a noisy picture of ranking quality.
3. Small, synthetic-looking catalogue (20 movies, genre tags only), so content features are crude and coverage/diversity are easy to inflate or deflate.
4. Item-based CF predicts with a similarity-weighted average over few neighbours and falls back to the global mean when there is no similar item, which makes its scores noisy and can produce ties.
5. Relevance threshold (rating >= 4) and the cosine-to-rating conversion for the content model are arbitrary teaching choices.

## Reproducibility
Open `DSA4060XA_Week6_Guided_Evaluation_Lab.ipynb` and run all cells top to bottom (the two CSV files must be in the same folder). Needs pandas, numpy, scikit-learn and matplotlib.
