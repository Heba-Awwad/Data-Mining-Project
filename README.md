# Data-Mining-Project

Unsupervised Learning for Student Performance Analysis Using K-Means Clustering

1. Introduction

Educational data mining plays a crucial role in understanding students’ academic performance and identifying hidden patterns within educational datasets. Unlike supervised learning, unsupervised learning techniques do not rely on predefined labels and instead aim to discover intrinsic structures in data.
This project applies K-Means clustering, an unsupervised machine learning algorithm, to analyze student performance data collected from grades one to six. The objective is to group students based on academic achievement and attendance patterns, allowing educators to better understand performance levels and design targeted intervention

2. Dataset Description

The dataset contains student records from grades 1 to 6. Each record includes:
•	Arabic language score
•	English language score
•	Mathematics score
•	Number of absences
•	Grade level
Some grades were recorded in categorical form (e.g., "Excellent", "Very Good") rather than numeric values, which required preprocessing before clustering.

3. Data Preprocessing

3.1 Data Cleaning

To preserve the original dataset, all preprocessing steps were applied to a copy of the data. Missing values were handled as follows:
•	Missing values in absence counts were replaced with zero.
•	Missing academic scores were replaced with the mean of the corresponding subject.
This approach ensures data completeness while avoiding unnecessary data loss.

3.2 Grade-to-Score Transformation

In the dataset, students in grades one through four were evaluated using categorical grade labels instead of exact numeric scores. Since clustering algorithms require numerical input, these categorical values were converted into representative numeric scores using a fixed mapping strategy.
The transformation function was designed to handle different data types safely. Specifically:
•	Missing values (NaN) were preserved and not altered.
•	Existing numeric values were left unchanged.
•	Only valid categorical grade labels were converted.

For students in grades 1 to 4, academic evaluations were provided as categorical grades rather than numeric scores.
Each categorical grade was mapped to a fixed numeric value representing the midpoint of its 
corresponding performance range, as shown below:

Grade	Numeric Value
Excellent (ممتاز)	95
Very Good (جيد جداً)	85
Good (جيد)	75
Average (متوسط)	65
Pass (مقبول)	55

This mapping ensures consistency and interpretability while maintaining meaningful performance differentiation across students.
Importantly, the transformation was applied exclusively to students in grades one to four, as students in higher grades already had numeric scores recorded. This selective application prevented unnecessary modification of valid numerical data.
By using a fixed-value mapping approach, the dataset became fully numerical and suitable for distance-based clustering methods such as K-Means.

3.3 Feature Selection and Scaling

The following features were selected for clustering:
•	Arabic score
•	English score
•	Mathematics score
•	Number of absences
Since K-Means is distance-based, Standardization was applied to ensure that all features contribute equally to the clustering process.


4. Methodology

4.1 Clustering Algorithm: K-Means

K-Means clustering partitions the dataset into k clusters by minimizing the within-cluster sum of squares (WCSS). The algorithm follows these steps:
1.	Initialize k centroids randomly.
2.	Assign each data point to the nearest centroid using Euclidean distance.
3.	Recompute centroids as the mean of assigned points.
4.	Repeat steps 2–3 until convergence.
In this study, the number of clusters was set to k = 3, representing high-performing, average-performing, and low-performing students.

4.2 Clustering Algorithm: Hierarchical Clustering

Hierarchical clustering builds a hierarchy of clusters either in an agglomerative (bottom-up) or divisive (top-down) manner. In this study, agglomerative clustering was used, following these steps:
1.	Start with each data point as a single cluster.
2.	Compute the distance between all clusters using a chosen metric (e.g., Euclidean distance).
3.	Merge the two closest clusters.
4.	Repeat step 3 until all points belong to a single cluster or a stopping criterion is reached.
The result is visualized as a dendrogram, which helps determine the optimal number of clusters for student performance groups.

4.3 Clustering Algorithm: DBSCAN

DBSCAN (Density-Based Spatial Clustering of Applications with Noise) groups points based on density rather than distance, which allows it to identify clusters of arbitrary shape and detect outliers. The algorithm follows these steps:
1.	For each point, retrieve all neighboring points within a specified radius ε.
2.	If the number of neighbors is greater than a minimum threshold MinPts, a cluster is formed.
3.	Expand the cluster by recursively adding all density-reachable points.
4.	Points that do not belong to any cluster are considered noise.
In this study, DBSCAN was applied to detect dense regions of student performance and identify students whose results significantly deviated from the main clusters.

K-Means achieved well-separated clusters with the highest silhouette score, making it suitable for grouping students by performance. Hierarchical clustering provided interpretable cluster relationships through dendrogram visualization. DBSCAN successfully identified outlier students but produced fewer stable clusters due to data density characteristics.


4.4 Comparison of Clustering Algorithms

To evaluate and compare the performance of the clustering algorithms used in this study, the Silhouette Score was calculated for each method. The Silhouette Score measures how similar each point is to its own cluster compared to other clusters, with values ranging from -1 to 1. Higher scores indicate better-defined and more distinct clusters.
The results are summarized in the table below:

Algorithm	Silhouette Score
K-Means	0.421
Hierarchical	0.400
DBSCAN	-1.000




From these results, we can observe the following:
•	K-Means achieved the highest Silhouette Score (0.421), indicating relatively well-separated and compact clusters.
•	Hierarchical Clustering produced a slightly lower score (0.400), showing that its clusters were somewhat less distinct than those from K-Means.
•	DBSCAN returned a negative Silhouette Score (-1.000), suggesting that the clusters identified were poorly defined. This is likely due to the presence of noise points and the sensitivity of DBSCAN to the choice of parameters (ε and MinPts) in this dataset.
Overall, K-Means provided the most reliable clustering for distinguishing high-performing, average-performing, and low-performing students, while Hierarchical Clustering gave comparable but slightly less distinct results. DBSCAN was less effective in this context, primarily due to the dataset characteristics and its density-based nature.

4.5 Principal Component Analysis (PCA) for Visualization

The student dataset used in this study contains multiple numerical features, including academic scores and attendance information. Visualizing such high-dimensional data directly is not feasible. Therefore, Principal Component Analysis (PCA) was applied to project the standardized feature space into a two-dimensional representation for visualization purposes.

To better visualize the clustering results, Principal Component Analysis (PCA) was applied to reduce the dataset to two dimensions. This dimensionality reduction allows us to plot the clusters on a 2D scatter plot while preserving as much of the original variance as possible.
The reduced dataset was then used to visualize the clusters obtained from each algorithm:
K-Means Clustering Visualization
A scatter plot of the first two principal components shows the distribution of students among the three clusters (high-performing, average-performing, and low-performing). Different colors indicate different clusters.

Hierarchical Clustering Visualization
Similarly, the hierarchical clustering results were plotted in the PCA space. This visualization highlights how the agglomerative method grouped students based on performance.

DBSCAN Visualization
DBSCAN clusters, including noise points labeled as -1, were also visualized in the PCA space. The scatter plot clearly identifies dense student groups and outliers.

These visualizations provide an intuitive comparison of the clustering results, complementing the quantitative evaluation (Silhouette Scores) presented earlier. K-Means and Hierarchical Clustering show relatively well-separated clusters, whereas DBSCAN highlights both dense clusters and scattered noise points.




5. Results and Analysis

The clustering process resulted in three distinct student groups:
•	Cluster 0: High academic performance with low absenteeism
•	Cluster 1: Moderate academic performance
•	Cluster 2: Low academic performance with higher absenteeism
The Silhouette score indicated meaningful separation between clusters, confirming the effectiveness of the chosen features and preprocessing strategy.

6. Discussion

The results demonstrate that combining academic scores with attendance data provides valuable insights into student behavior. The use of interval-based grade mapping preserved performance diversity and prevented artificial perfect clustering, which is a common pitfall when categorical grades are converted to fixed numeric values.
This approach allows schools to identify students at risk and apply early interventions, especially in lower-performing clusters.


7. Conclusion

This study successfully applied K-Means clustering to educational data to uncover performance-based student groupings. Through careful preprocessing, feature scaling, and evaluation, the model produced meaningful and interpretable clusters.

Future work could enhance this model by:
•	Incorporating additional behavioral or demographic features.
•	Experimenting with other clustering algorithms like Gaussian Mixture Models.
•	Implementing a system to periodically re-cluster students and track their movement between groups over time, enabling dynamic intervention strategies.
