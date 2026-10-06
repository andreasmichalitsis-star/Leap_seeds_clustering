# Final Project – Option 3: Seeds Clustering

## How much structure can you find without labels?

### Project overview

This project explores whether the natural structure of a wheat seed data set can be recovered without using the known variety labels during clustering.

The scenario is based on an agricultural co-op receiving unlabelled deliveries of mixed wheat grain. The aim is to estimate how many distinct groups are present, investigate how clearly they separate, and then compare the discovered clusters with the real wheat varieties only after the clustering process has been completed.

The central question is:

> **How much of the real class structure can clustering recover without using the labels?**

---

## Data set

The project uses the **Seeds data set**, which contains **210 wheat kernels** described by **seven geometric measurements**:

- Area
- Perimeter
- Compactness
- Kernel length
- Kernel width
- Asymmetry coefficient
- Groove length

The data set also contains the true wheat variety for each kernel. These labels were kept separate during the unsupervised analysis and were used only at the end to evaluate the clustering result.

---

## Workflow and methods

The analysis followed these main steps:

1. **Feature scaling**  
   The seven measurements were standardised using `StandardScaler` because K-Means is distance-based and variables measured on larger scales could otherwise have too much influence.

2. **Principal Component Analysis (PCA)**  
   PCA was used to reduce the seven scaled measurements to two dimensions for visualisation.

3. **Choosing the number of clusters**  
   K-Means models were fitted for values of `k` from 2 to 7. The number of clusters was assessed using:
   - the **elbow method**, based on inertia;
   - the **silhouette score**, which measures separation between clusters.

4. **Final K-Means clustering**  
   A final K-Means model was fitted using the selected value of `k`.

5. **Evaluation against the true varieties**  
   Only after clustering was complete, the true labels were revealed. The clustering result was assessed using:
   - a cross-tabulation of clusters and true varieties;
   - a **purity score**;
   - a PCA plot coloured by true variety.

6. **Cluster profiling**  
   Cluster centres were transformed back to the original measurement units so that the typical kernel characteristics of each cluster could be interpreted.

---

## Key results

### PCA

The first two principal components explained approximately **89.0% of the total variance**:

- PC1: 71.9%
- PC2: 17.1%

The two-dimensional PCA scatter suggested around three broad regions, although some overlap was visible.

### Choosing k

The diagnostic results were:

| k | Inertia | Silhouette score |
|---:|---:|---:|
| 2 | 659.17 | 0.466 |
| 3 | 430.66 | 0.401 |
| 4 | 371.30 | 0.328 |
| 5 | 326.51 | 0.285 |
| 6 | 289.80 | 0.280 |
| 7 | 262.97 | 0.271 |

The **silhouette score was highest for k = 2**, but the elbow curve showed a strong reduction in inertia up to approximately **k = 3**, followed by much smaller improvements. For this reason, **k = 3** was selected as the final clustering solution.

The final cluster sizes were:

- Cluster 0: 72 kernels
- Cluster 1: 67 kernels
- Cluster 2: 71 kernels

### Agreement with the true varieties

The cross-tabulation between discovered clusters and true varieties was:

| Cluster | Variety 1 | Variety 2 | Variety 3 |
|---:|---:|---:|---:|
| 0 | 6 | 0 | 66 |
| 1 | 2 | 65 | 0 |
| 2 | 62 | 5 | 4 |

The resulting **purity score was 0.919**.

This means that **193 of the 210 kernels** belonged to the majority true variety within their assigned cluster. The clustering therefore recovered most of the real class structure without using the labels during model fitting.

Variety 2 formed the cleanest group, while the greatest overlap occurred mainly between Varieties 1 and 3.

---

## Cluster profiles

The cluster centres, converted back to the original units, were:

| Cluster | Area | Perimeter | Compactness | Kernel length | Kernel width | Asymmetry | Groove length |
|---:|---:|---:|---:|---:|---:|---:|---:|
| 0 | 11.857 | 13.248 | 0.848 | 5.232 | 2.850 | 4.742 | 5.102 |
| 1 | 18.495 | 16.203 | 0.884 | 6.176 | 3.698 | 3.632 | 6.042 |
| 2 | 14.438 | 14.338 | 0.882 | 5.515 | 3.259 | 2.707 | 5.121 |

These profiles show that the discovered clusters differ in several physical seed characteristics rather than being arbitrary groups.

---

## Interpretation

The results show that unsupervised learning can recover a large part of the real wheat variety structure from simple geometric measurements. However, the structure is not perfectly separated.

The elbow method and silhouette score did not give exactly the same answer. The silhouette score preferred two clusters, while the elbow pattern and the visible structure supported three. This disagreement is important because it shows that choosing the number of clusters is not always completely objective.

The high purity score supports the three-cluster solution, but purity should not be interpreted alone because it can increase when more clusters are created.

The PCA visualisation should also be interpreted carefully. K-Means used all seven scaled features, while the PCA plot shows only two dimensions. Therefore, points that appear close or overlapping in the plot may still be separated using information from the remaining dimensions.

---

## Ethical considerations and limitations

The clusters should not automatically be treated as confirmed wheat varieties simply because K-Means produced three groups.

A purity score of about 0.92 is strong, but it still means that some kernels are assigned to groups dominated by a different variety. If an agricultural co-op used these clusters directly for sorting, pricing, or quality decisions, some kernels could be incorrectly classified.

The silhouette result also shows that the data do not contain perfectly clear boundaries between all three groups. For practical use, clustering should therefore be supported by labelled samples, laboratory analysis, or another independent validation method.

---

## Reflection

The strongest part of the analysis was that the clustering recovered the known structure surprisingly well even though the model had no access to the true variety labels during training.

The main difficulty was choosing the number of clusters because the elbow method and silhouette score did not fully agree. This was useful because it showed that clustering requires interpretation rather than simply accepting one diagnostic automatically.

With more time, the analysis could be extended by comparing K-Means with other unsupervised methods, such as hierarchical clustering or Gaussian mixture models, and by examining the stability of the clusters under different initialisations or subsets of the data.

---

## Files

- `Final_Project_Option3_Seeds_Clustering_COMPLETED.ipynb` – completed notebook with code, outputs, plots, and written interpretations.
- `README_Seeds_Clustering.md` – summary of the project, workflow, results, interpretation, and reflection.

---

## Software used

- Python
- pandas
- NumPy
- matplotlib
- scikit-learn
- Jupyter Notebook
