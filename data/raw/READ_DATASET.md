## 🎬 MovieLens 100K Dataset Structure

The `ml-data.tar.gz` archive contains the **MovieLens 100K dataset**, consisting of **100,000 movie ratings** by **943 users** across **1682 movies**. Each user has rated **at least 20 movies**.

### 📦 Getting Started

To extract the dataset:

```bash
gunzip ml-data.tar.gz
tar xvf ml-data.tar
./mku.sh  # Rebuilds the u*.base and u*.test files
```

---

## 📁 Directory Structure

```text
ml-data/
│
├── u.data          # Main dataset: 100,000 ratings
├── u.info          # Summary: number of users, items, and ratings
├── u.item          # Movie metadata (titles, genres)
├── u.genre         # List of genres
├── u.user          # User demographic info
├── u.occupation    # List of possible occupations
│
├── u1.base         # 80% training set (Fold 1)
├── u1.test         # 20% test set (Fold 1)
├── u2.base         # 80% training set (Fold 2)
├── u2.test         # 20% test set (Fold 2)
├── u3.base         # ...
├── u3.test
├── u4.base
├── u4.test
├── u5.base
├── u5.test
│
├── ua.base         # Train/test split with exactly 10 ratings per user in test set
├── ua.test
├── ub.base
├── ub.test
│
├── allbut.pl       # Script to generate "all-but-n" training/test splits
└── mku.sh          # Shell script to rebuild training/test sets from u.data
```

---

## 📄 File Descriptions

### `u.data`

> **Format**: Tab-separated
> **Columns**: `user id | item id | rating | timestamp`

- **943 users**, **1682 movies**, **100,000 ratings**
- Timestamps are in **Unix time** (seconds since 01/01/1970 UTC)
- Users and items are numbered **consecutively from 1**
- Data is **randomly ordered**

---

### `u.info`

> Provides a summary of:

- Number of users
- Number of items
- Number of ratings

---

### `u.item`

> **Format**: Tab-separated
> **Columns**:

```text
movie id | title | release date | video release date | IMDb URL | genres (19 columns)
```

- Last 19 columns are **binary genre flags**:

  - `1` if movie belongs to the genre, `0` otherwise
  - Movies can belong to **multiple genres**

---

### `u.genre`

> List of genres used in `u.item`, e.g.:

```text
unknown
Action
Adventure
...
Western
```

---

### `u.user`

> **Format**: Tab-separated
> **Columns**:

```text
user id | age | gender | occupation | zip code
```

- Used to analyze demographic preferences in ratings

---

### `u.occupation`

> List of all possible user occupations (used in `u.user`)

---

### `u1.base` – `u5.test`

> **5-fold cross-validation** sets:

- Each fold contains an **80%/20%** training/test split
- Test sets are **disjoint** (no overlapping test data)

---

### `ua.*` / `ub.*`

> Alternative train/test splits:

- **Exactly 10 ratings per user** in each test set
- `ua.test` and `ub.test` are **disjoint**

---

### `allbut.pl`

> Perl script to generate training/test splits with "**all but n**" ratings in the training set for each user.

---

### `mku.sh`

> Shell script to rebuild all training and test sets from the original `u.data`.

---
