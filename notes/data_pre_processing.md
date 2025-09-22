# Data Preprocessing

## 📝 Preprocessing Pipeline for User-Behavior-Based Clustering

### 1. **Drop Irrelevant Columns**

- Remove columns that don’t help describe behavior:

  - `user id`, `movie id`, `timestamp`, `datetime`, `movie title`, `IMDb URL`, `zip code`.

- Keep:

  - Demographics: `age`, `gender`, `occupation`.
  - Movie metadata: `release date` (convert to year).
  - Genre multi-hot columns (`Action, Comedy, Drama, …`).
  - Rating info (`rate`).

---

### 2. **Handle Missing Values**

- Fill missing categorical values (`occupation`, `gender`) with `"Unknown"`.
- For numeric values (`age`, `release year`) → fill with median.

---

### 3. **Clean & Transform Features**

- **Age** → bucketize into groups (`<18`, `18–25`, `26–35`, `36–50`, `50+`).

  - This reduces noise (since clustering works better with groups than raw ages).

- **Gender** → one-hot encode (`M=1, F=0` or both as separate columns).
- **Occupation** → one-hot encode (executive, student, programmer, etc.).
- **Release date** → extract only the `year`, then normalize (older vs newer movies).

---

### 4. **Aggregate User Behavior**

Since clustering is user-level, aggregate ratings + genres:

- For each user:

  - Average rating given.
  - Number of ratings given.
  - Percentage of ratings per genre (normalize genre counts by total movies rated).
  - Recency features (if using timestamp): last active year, rating trend.

👉 End result: **one row per user**, not per rating.

---

### 5. **Outlier Handling**

- Users with _too few ratings_ (e.g., <5) → drop (they don’t provide enough signal).
- Users with _too many ratings_ (e.g., >95th percentile) → cap their counts to reduce skew.

---

### 6. **Scale / Normalize Features**

- Use **StandardScaler** (mean=0, std=1) or **MinMaxScaler (0–1)**.
- Apply scaling to:

  - `age`, `avg rating`, `num ratings`, `genre percentages`.

- Not needed for one-hot categorical features (already 0/1).

---

### 7. **Feature Engineering (Optional but Powerful)**

- Add **rating variance** (shows if user is critical vs generous).
- Add **favorite genre** (genre with max percentage).
- Use **correlation matrix** to drop redundant features.

---

### 8. **Dimensionality Reduction (PCA/UMAP)**

- After encoding + scaling, you’ll have many columns (demographics + 18 genres + occupation + ratings).
- Use **PCA** to reduce to \~20–50 components → denoises and helps clustering.
- Optional: **UMAP** just for visualization of clusters.

---

✅ End result after preprocessing:

- A clean **user–feature matrix** (rows = users, cols = scaled features).
- You can now apply **KMeans, DBSCAN, or GMM** to cluster users.
