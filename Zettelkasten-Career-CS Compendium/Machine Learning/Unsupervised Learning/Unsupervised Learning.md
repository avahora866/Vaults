---
tags:
  - "#TRUNK"
---
In Supervsed Learning we are given the input data and the corresponding correct labels, hoever in unsupervised learning we are only given the input data. The algorithm is left to discover the patterns and correlations within the data by itself.

The objective changes from prediction to discovery.

# Algorithms
## Clustering
- [[Clustering K-Means]]
- [[DBSCAN (Density-Based Spatial Clustering)]]: Groups points that are closely packed together. Unlike K-Means, it can find arbitrarily shaped clusters (not just circles) and identifies outliers automatically.
- [[Hierarchical Clustering]]: Builds a tree of clusters (dendrogram). Useful when you want to see how data groups at different levels of granularity (e.g., Animal $\rightarrow$ Mammal $\rightarrow$ Dog).
- [[Gaussian Mixture Models (GMM)]]: A "soft" clustering method. Instead of saying "Point A belongs to Cluster 1," it says "Point A is 70% Cluster 1 and 30% Cluster 2."
[[Isolation Forest]]

## Association Rule Learning (Market Basket Analysis)
- [[Apriori Algorithm]]: Identifies frequent item sets in transactional databases. Used by retailers to find rules like "People who buy Diapers also buy Beer."
- [[Eclat Algorithm]]: A faster, more memory-efficient version of Apriori used for identifying frequent patterns in dense datasets.
- [[FP-Growth (Frequent Pattern)]]: Uses a tree structure to find frequent patterns without generating candidates. Faster than Apriori for large datasets.

## Neural Network-Based Unsupervised Learning
- [[Autoencoders]]: A neural network that learns to compress data (encode) and then reconstruct it (decode). Used for advanced image compression and denoising.
- [[GANs (Generative Adversarial Networks)]]: Two networks competing against each other (Generator vs. Discriminator) to create new data. Used to generate realistic faces, art, or deepfakes.
- [[Self-Organizing Maps (SOM)]]: Maps high-dimensional data onto a 2D grid, preserving topological properties. Used for visual pattern recognition (e.g., grouping colors or voice patterns).

# The Curse of Dimensionality
In machine learnign we often deal with high-dimensional data (data with a lot of columns) such as an image with 100 x 100 pixels has 10,000 features (dimensions) or a genetic sequence might have 20,000 markers.

The problem is that when the number of dimensions increase, the volume of the space increases so fast that the availble data becomes sparse (Increase in number of columns but the number of rows stay the same). Meaning in very high dimensions all data points appear to be roughly the same distance apart (if everything is equally far away from everything else, Clustering stops working becuase it relies on mesuring distance to find close neighbours).

Note: In theory if you exponentially increase the number of rows according to the number of columns, the algorithm will still work fine as the data is enough for clustering to work, however in practice this is impossible

## Techniques
- [[Principal Component Analysis]]: 
- [[t-SNE (t-Distributed Stochastic Neighbor Embedding)]]: Specifically designed for visualization. It takes high-dimensional data and maps it to 2D/3D so humans can "see" clusters.
- [[UMAP (Uniform Manifold Approximation and Projection)]]: Similar to t-SNE but faster and better at preserving the global structure of the data.
- [[LDA (Latent Dirichlet Allocation)]]: Used in NLP for Topic Modeling. It scans thousands of documents to discover hidden "topics" (e.g., identifying that a document is 60% "Politics" and 40% "Economics").

## Dimensionality Reduction (Principal Component Analysis)
To solve this issue we need to simplify the data without losing the important information. We use a technique called Principal Component Analysis (PCA)

This process is merging related columns into one

#todo Need a seperate note for explaining how PCA works
