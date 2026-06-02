# 🎬 Movie Recommendation System

> A content-based movie recommendation engine that analyzes cast, crew, genres, keywords, and plot to surface the 5 most similar films — built with Python and served via Streamlit.

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?style=flat-square&logo=python)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-ML-orange?style=flat-square&logo=scikit-learn)
![Streamlit](https://img.shields.io/badge/Streamlit-Frontend-red?style=flat-square&logo=streamlit)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)

---

## 📌 Overview

This project is a **content-based filtering** recommendation system. It doesn't need user ratings or watch history — it works purely by understanding what a movie *is*: its genre, cast, director, keywords, and plot. All of these are combined into a unified text representation per movie, vectorized, and compared using cosine similarity to find the closest matches.

---

## ✨ Features

-  **Search or Select** — Type a movie name or pick from a dropdown; both work seamlessly.
-  **Top-5 Recommendations** — Returns the five most content-similar movies on click.
-  **Instant Load** — Model artifacts are pre-computed and Pickle-serialized for fast startup.
-  **AST-based Parsing** — Safely converts stringified Python objects from the raw dataset into usable data structures.
-  **Director Extraction** — Filters only the Director role from the full crew list.
-  **Cosine Similarity** — Mathematically sound metric for comparing high-dimensional text vectors.

---

## 🛠️ Tech Stack

| Layer | Tool |
|---|---|
| Language | Python 3.8+ |
| Data Manipulation | Pandas, NumPy |
| Data Parsing | Python `ast` module |
| Vectorization | Scikit-learn `CountVectorizer` |
| Similarity | Scikit-learn `cosine_similarity` |
| Model Persistence | Pickle |
| Frontend | Streamlit |
| Dataset | TMDB 5000 Movies (Kaggle) |

---

## 📂 Project Structure

```
movie-recommendation-system/
│
├── app.py                      # Streamlit app — entry point
├── MovieRecommendationSystem.ipynb  # Full ML pipeline notebook
│
├── movie_list.pkl              # Serialized movie DataFrame
├── similarity.pkl              # Serialized cosine similarity matrix
│
├── data/
│   ├── tmdb_5000_movies.csv
│   └── tmdb_5000_credits.csv
│
├── requirements.txt
└── README.md
```

---

## 📊 Dataset

Kaggle Dataset Link: [https://www.kaggle.com/datasets/tmdb/tmdb-movie-metadata](https://www.kaggle.com/datasets/tmdb/tmdb-movie-metadata)

Two CSV files — `tmdb_5000_movies.csv` and `tmdb_5000_credits.csv` — merged on movie title. Contains metadata for ~5,000 TMDB movies including genres, cast, crew, keywords, and plot overviews.

---

## 🔬 Workflow & Methodology

### 1. Data Loading & Merging
Both CSV files are loaded separately and then merged on the movie title into a single unified DataFrame.

### 2. Feature Selection
Only the columns useful for content-based filtering are kept — title, overview, cast, crew, genres, keywords, and production companies. Everything else is discarded.

### 3. Null Removal
Any rows with missing values are dropped to prevent errors during processing.

### 4. Parsing Stringified Data with `ast`
Several columns in the raw dataset store list-like data as plain strings rather than actual Python objects. Python's built-in `ast.literal_eval()` is used to safely parse these strings back into real lists and dictionaries, making the data usable. This approach is preferred over `eval()` because it only processes Python literals and cannot execute arbitrary code.

### 5. Cast — Top 3 Only
The full cast list per movie can be very long. Only the top 3 billed actors are kept to reduce noise in the final tags.

### 6. Crew — Director Only
The crew list contains many roles. Only the entry with the job title "Director" is extracted and retained.

### 7. Collapsing Multi-Word Names
Names like "Sam Worthington" are joined into "SamWorthington" so the vectorizer treats the full name as one token rather than two unrelated words.

### 8. Building the Tags Column
All processed fields — cast, director, keywords, genres, production companies, and overview — are concatenated into a single `tags` string per movie. This is the unified representation used for comparison.

### 9. Vectorization
The `tags` column is converted into numerical vectors using Scikit-learn's `CountVectorizer` with a vocabulary of 5,000 most frequent words and English stop words removed. Each movie becomes a point in 5,000-dimensional space.

### 10. Cosine Similarity
Cosine similarity measures the angle between two vectors rather than their magnitude. Two movies with similar content will have vectors pointing in nearly the same direction, producing a score close to 1. This makes it ideal for text data where the length of the tags shouldn't influence results. A full similarity matrix is computed across all ~4,800 movies.

### 11. Serialization with Pickle
The processed DataFrame and the similarity matrix are saved as `.pkl` files. This means the Streamlit app can load everything instantly on startup without re-running the entire pipeline.

---

## 🖥️ How It Works

When a user selects a movie and clicks Recommend, the app locates that movie's position in the DataFrame, retrieves its row from the similarity matrix, sorts all other movies by their similarity score in descending order, and returns the top 5 results — skipping the selected movie itself.

---

## ⚙️ Installation & Usage

### Prerequisites
- Python 3.8+
- pip

### Step 1 — Clone the Repository
```bash
git clone https://github.com/mhttarunno/Movie-Recommendation-System.git
cd Movie-Recommendation-System
```

### Step 2 — Install Dependencies
```bash
pip install -r requirements.txt
```

### Step 3 — Add Dataset Files
Download the dataset from the Kaggle link above and place both CSV files in the project root.

### Step 4 — Generate Pickle Artifacts
Run `MovieRecommendationSystem.ipynb` from top to bottom. This creates `movies_list.pkl` and `similarity.pkl` in the project root.

### Step 5 — Launch the App
```bash
streamlit run app.py
```

---

## 🚀 Future Improvements

- [ ] **Movie Posters** — Integrate TMDB API to display posters alongside recommendations.
- [ ] **Hybrid Filtering** — Combine content-based with collaborative filtering for better accuracy.
- [ ] **Stemming / Lemmatization** — Reduce words to root forms before vectorization to improve tag matching.
- [ ] **Genre & Year Filters** — Let users constrain recommendations by genre or release decade.
- [ ] **Cloud Deployment** — Deploy on Streamlit Cloud or Hugging Face Spaces for public access.

---

## 👤 Author

**[Mahfujul Haque]**

- GitHub: [@mhttarunno](https://github.com/mhttarunno)
- LinkedIn: [Mahfujul Haque](https://www.linkedin.com/in/mhttarunno/)

---

## 📄 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

> ⭐ Enjoyed the project? Consider starring the repo!
