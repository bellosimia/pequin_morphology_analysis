# K-Means Clustering — Penguin Morphology Analysis

## Project Overview

This project applies unsupervised K-Means clustering to the Palmer Penguins dataset to uncover morphological groupings without using species labels. We preprocess numeric features, scale them for Euclidean distance-based clustering, determine optimal k using Elbow and Silhouette methods, and interpret results in a biological and ecological context.

## Dataset

**Source:** Palmer Penguins (Palmer Station Antarctica LTER)  
**Records:** 344 total, 342 usable (2 rows had missing numeric values)  
**Species:** Adelie (152), Gentoo (124), Chinstrap (68)  
**Islands:** Torgersen, Biscoe, Dream  

## Preprocessing Steps

1. **Dropped categorical columns** — `species`, `island`, `sex` (not suitable for Euclidean distance)
2. **Removed rows with NA values** — 2 rows removed, leaving 342 clean records
3. **Applied StandardScaler** — normalized all four numeric features to mean=0, std=1 to prevent `body_mass_g` from dominating distance calculations

**Features used for clustering:**
- `bill_length_mm`
- `bill_depth_mm`
- `flipper_length_mm`
- `body_mass_g`

## Optimal k Selection

| Method | Optimal k | Rationale |
|---|---|---|
| Elbow (SSE) | **3** | Clear bend; SSE drops 802 units (k1→2) then 186 (k2→3), then only 79 (k3→4) |
| Silhouette | 2 (peak: 0.5315) | Gentoo are very distinct; k=2 captures that but loses Adelie/Chinstrap separation |
| **Final choice** | **k = 3** | Aligns with elbow inflection, biological species count, and silhouette of 0.447 |

## Key Results

| Cluster | n | Dominant Species | Bill Length | Bill Depth | Flipper | Body Mass |
|---|---|---|---|---|---|---|
| 0 | 87 | Chinstrap (93%) | 47.5 mm | 18.8 mm | 196.9 mm | 3,902 g |
| 1 | 123 | Gentoo (100%) | 47.5 mm | 15.0 mm | 217.2 mm | 5,076 g |
| 2 | 132 | Adelie (96%) | 38.2 mm | 18.1 mm | 188.4 mm | 3,585 g |

**Silhouette Score (k=3):** 0.4472  
**SSE (inertia):** 379.39

## Biological Interpretation

- **Cluster 1 = Gentoo** — perfectly isolated by large body mass and long, shallow-billed morphology
- **Cluster 2 = Adelie** — small-bodied, short-billed; female-skewed due to sexual dimorphism  
- **Cluster 0 = Chinstrap + large male Adelie** — morphologically overlapping in 4D space; both species share deep bill profiles, and male Adelie fall closer to Chinstrap than to female Adelie

The groupings primarily reflect **species-level anatomy**, with sexual dimorphism playing a secondary role at cluster boundaries.

## Limitations

- K-Means assumes spherical clusters; the Adelie/Chinstrap boundary is not perfectly spherical
- Island of origin (a geographic confound) is not captured
- Sex as a numeric feature could sharpen within-species separation in future work
- Gaussian Mixture Models or DBSCAN may better handle boundary cases

## Repository Structure

```
kmeans_project/
├── kmeans_penguins.ipynb     # Main notebook (all cells executed)
├── penguins.csv              # Source dataset
├── README.md                 # This file
├── requirements.txt          # Python dependencies
└── figures/
    ├── elbow_plot.png        # SSE vs k (Elbow method)
    ├── silhouette_plot.png   # Silhouette score vs k
    └── scatterplot.png       # Flipper length vs body mass, colored by cluster
```
