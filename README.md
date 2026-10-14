# Book-recommendation-System
# 📚 Book Recommendation System

A collaborative filtering recommendation engine built on the **Book-Crossing dataset** (~1.1M ratings). Given a book, it finds similar books by correlating user rating patterns.

## How It Works

1. **Load & merge** book metadata with user ratings on `ISBN`.
2. **Aggregate** average rating + rating count per title.
3. **Filter** to popular books (>100 ratings) to reduce noise from sparse data.
4. **Pivot** into a user × book matrix.
5. **Compute** Pearson correlation between a target book (`Bag of Bones`) and every other book via `corrwith()`.
6. **Rank** results, joined with popularity counts.

## Project Structure

book-recommendation/
├── datasets/
│   ├── books_titles.csv      # ~271K book records (ISO-8859-1, ';' delimited)
│   └── book_ratings.csv      # ~1.1M user ratings
├── book_recommendation.ipynb
└── README.md

## Getting Started

1. Clone the repo:
git clone https://github.com/mburuericc/book-recommendation.git
cd book-recommendation

2. Install dependencies:
pip install pandas numpy chardet

3. Place the two CSVs in `datasets/` (see Data Source below).

4. Run all cells in `book_recommendation.ipynb`.

## Key Results

Top correlations with *Bag of Bones* (Stephen King):

| Rank | Book | Correlation | Ratings |
|------|------|-------------|---------|
| 1 | High Fidelity | 1.00 | 128 |
| 2 | The Stone Diaries | 1.00 | 133 |
| 3 | Night Sins | 0.99 | 117 |
| 4 | Flesh and Blood | 0.99 | 126 |
| 5 | The Color of Water | 0.98 | 152 |

## Limitations & Next Steps

- **Zero-rating ambiguity:** In Book-Crossing, `rating = 0` means "I own this" (not an actual score). These are currently treated as real low scores — filtering them out would improve accuracy.
- **Correlation ≠ similarity:** Perfect (1.00) correlations come from books rated by only a handful of overlapping users. A minimum-overlap threshold or shrinkage estimator would help.
- **Title-based grouping:** Different editions share titles but have different ISBNs; merging on title conflates them.
- **Next steps:** Try cosine similarity, SVD/Matrix Factorization (`scikit-learn`'s `TruncatedSVD`), or `surprise` library for proper train/test evaluation (RMSE).

## Data Source

[Book-Crossing Dataset](http://www2.informatik.uni-freiburg.de/~cziegler/BX/) — Ziegler et al., WWW 2005.
