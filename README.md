# dimensionality-reduction-pca-umap-tsne
Educational project comparing PCA, UMAP, and t-SNE for visualizing high-dimensional synthetic data in two dimensions.

# Dimensionality Reduction with PCA, UMAP, and t-SNE

This repository presents an educational comparison of three dimensionality reduction techniques: PCA, UMAP, and t-SNE.

The project simulates a high-dimensional dataset with cluster structure, nonlinear relationships, and correlated features. Then, it projects the data into two dimensions using different methods and creates an animated visualization showing how each technique reveals the structure of the data.

The main message is that the choice of dimensionality reduction method changes the visual interpretation of the same dataset.

---

## Introduction

High-dimensional datasets are common in data science, machine learning, and business analytics. Customer profiles, product embeddings, transaction patterns, image representations, text embeddings, and behavioral data often contain many variables.

However, humans cannot easily visualize data in more than two or three dimensions. Dimensionality reduction methods are useful because they transform high-dimensional data into a lower-dimensional representation while trying to preserve relevant structure.

Principal Component Analysis, commonly known as PCA, is one of the classical methods for linear dimensionality reduction. Its foundations are associated with Pearson (1901), and the method was later developed and widely used in multivariate analysis. Jolliffe (2002) describes PCA as a central technique for reducing dimensionality while retaining as much variation as possible.

Nonlinear methods such as t-SNE and UMAP are frequently used when the goal is visualization. t-SNE was introduced by van der Maaten and Hinton (2008) as a technique for visualizing high-dimensional data in two or three dimensions. UMAP, introduced by McInnes, Healy, and Melville (2018), is a manifold learning technique designed for dimension reduction and visualization.

This project compares these three approaches on the same synthetic dataset.

---

## Problem Context

When working with high-dimensional data, analysts often need to answer questions such as:

* Are there natural groups in the data?
* Are some observations similar to each other?
* Do nonlinear relationships influence the visual structure?
* Does the visualization preserve global or local structure?
* Can the same dataset lead to different interpretations depending on the method?

Dimensionality reduction is not only a technical preprocessing step. It also affects how patterns are communicated and interpreted.

For example, PCA may show broad global variation, while t-SNE and UMAP may reveal local clusters more clearly. Because of this, the choice of method can influence how a data scientist explains the behavior of a dataset.

---

## Objective

The objective of this project is to compare PCA, UMAP, and t-SNE as visualization methods for high-dimensional data.

The project aims to show that:

* PCA provides a linear projection based on variance preservation.
* UMAP tends to preserve local neighborhood structure and can also maintain part of the global organization.
* t-SNE is effective for visualizing local cluster structure.
* Different methods can produce different visual narratives from the same data.

---

## Data Generating Process

The dataset is synthetically generated with 1,600 observations, 12 features, and 8 latent clusters.

The initial data matrix can be represented as:

$$ X \in \mathbb{R}^{1600 \times 12} $$

where each row represents one observation and each column represents one feature.

The synthetic dataset is generated using Gaussian blobs. After that, additional relationships are introduced to make the structure less purely linear.

Some variables are modified using correlated and nonlinear transformations:

$$ x_3 = 0.4x_1 + \epsilon_1 $$

$$ x_6 = \sin(x_2) + \epsilon_2 $$

$$ x_9 = \frac{x_4x_5}{20} + \epsilon_3 $$

where the error terms represent random noise added to the synthetic features.

Before applying dimensionality reduction, the data is standardized:

$$ z_{ij} = \frac{x_{ij} - \mu_j}{\sigma_j} $$

where:

* $z_{ij}$ is the standardized value of observation $i$ in feature $j$;
* $x_{ij}$ is the original value;
* $\mu_j$ is the mean of feature $j$;
* $\sigma_j$ is the standard deviation of feature $j$.

Standardization is important because PCA and distance-based methods such as UMAP and t-SNE can be affected by differences in feature scale.

---

## Methodology

The methodological pipeline follows these steps:

* Generate synthetic high-dimensional data with cluster structure.
* Add correlated and nonlinear relationships among selected features.
* Standardize all variables using z-score normalization.
* Apply PCA to obtain a two-dimensional linear projection.
* Apply UMAP to obtain a two-dimensional manifold-based projection.
* Apply t-SNE to obtain a two-dimensional neighborhood-preserving projection.
* Create an animated visualization comparing the three projections side by side.

The same input matrix is used for all three methods:

$$ X_{scaled} \rightarrow \mathbb{R}^2 $$

The goal is not to determine which method is universally better. The goal is to show that each method emphasizes a different aspect of the data.

---

## PCA

Principal Component Analysis projects the original data onto new orthogonal axes called principal components.

Given a centered data matrix $X$, PCA searches for directions that maximize variance. The first principal component can be written as:

$$ w_1 = \arg\max_{\|w\| = 1} Var(Xw) $$

The projected data is obtained by:

$$ Z = XW $$

where:

* $X$ is the standardized data matrix;
* $W$ contains the selected principal component directions;
* $Z$ is the lower-dimensional representation.

PCA is a linear method. Because of this, it is often useful for preserving global variance structure, but it may not capture complex nonlinear relationships.

---

## UMAP

UMAP stands for Uniform Manifold Approximation and Projection.

The method assumes that the data lies on a lower-dimensional manifold embedded in the original high-dimensional space. UMAP builds a graph representation of local neighborhoods and then optimizes a low-dimensional embedding that preserves this structure.

In this project, UMAP is configured with:

```text
n_components = 2
n_neighbors = 20
min_dist = 0.15
metric = euclidean
```

The parameter `n_neighbors` controls the balance between local and broader structure. The parameter `min_dist` affects how tightly points can be packed in the low-dimensional representation.

UMAP is often useful when the analyst wants a visualization that highlights local neighborhoods while still preserving some broader organization of the data.

---

## t-SNE

t-SNE stands for t-distributed Stochastic Neighbor Embedding.

The method converts pairwise similarities in the high-dimensional space into probabilities and tries to preserve those similarities in a low-dimensional map.

A simplified way to describe the objective is that t-SNE tries to make the low-dimensional similarity distribution $q_{ij}$ close to the high-dimensional similarity distribution $p_{ij}$.

The optimization minimizes the Kullback-Leibler divergence:

$$ KL(P \| Q) = \sum_i \sum_j p_{ij} \log\left(\frac{p_{ij}}{q_{ij}}\right) $$

In this project, t-SNE is configured with:

```text
n_components = 2
perplexity = 30
learning_rate = auto
init = pca
```

t-SNE is particularly strong for visualizing local clusters, but distances between faraway groups should be interpreted carefully.

---

## Results

The animation compares the three projections side by side.

The PCA panel tends to show a broader linear view of the dataset. It is useful for understanding the main directions of variance, but the clusters may overlap more because PCA is restricted to linear projections.

The UMAP panel tends to reveal clearer local structure while keeping a more organized global layout. The clusters become more visually separated, and the method often provides a balanced view between neighborhood preservation and global arrangement.

The t-SNE panel tends to emphasize local cluster separation strongly. It can produce visually distinct groups, which is useful for exploratory analysis, but the apparent distance between distant clusters should not be interpreted as a precise global distance.

The main result is that the same high-dimensional dataset can generate different two-dimensional visual interpretations depending on the method used.

---

## Interpretation

The comparison highlights an important principle in data visualization: dimensionality reduction is not neutral.

Each method has assumptions and optimization goals.

PCA prioritizes variance preservation through linear components. UMAP prioritizes manifold structure and neighborhood preservation. t-SNE prioritizes local similarity preservation.

Therefore, visualizations should not be interpreted as absolute representations of reality. They should be interpreted as method-dependent summaries of high-dimensional structure.

In practical data science work, it is often useful to compare more than one method before making conclusions about clusters, separation, or similarity.

---

## Limitations

This project uses synthetic data and is intended for educational purposes.

The main limitations are:

* the dataset is simulated;
* the clusters are generated from Gaussian blobs;
* the true labels are known only because the data is synthetic;
* real-world data may contain noise, missing values, outliers, and mixed variable types;
* PCA, UMAP, and t-SNE are sensitive to preprocessing choices;
* UMAP and t-SNE are sensitive to hyperparameters;
* two-dimensional projections always lose information from the original space.

In real applications, dimensionality reduction should be combined with domain knowledge, validation metrics, and careful interpretation.

---

## Conclusion

This project demonstrates how PCA, UMAP, and t-SNE can produce different two-dimensional views of the same high-dimensional dataset.

PCA provides a linear variance-based projection. UMAP provides a manifold-based projection that often balances local and global structure. t-SNE provides a strong local-neighborhood visualization that can make clusters visually clear.

The key takeaway is that dimensionality reduction should be interpreted as a modeling choice. The visualization depends not only on the data, but also on the assumptions and objectives of the selected method.

---

## References

Pearson, K. (1901). On Lines and Planes of Closest Fit to Systems of Points in Space. Philosophical Magazine, 2(11), 559-572.

Jolliffe, I. T. (2002). Principal Component Analysis. Second Edition. Springer Series in Statistics. Springer.

van der Maaten, L., & Hinton, G. (2008). Visualizing Data using t-SNE. Journal of Machine Learning Research, 9, 2579-2605.

McInnes, L., Healy, J., & Melville, J. (2018). UMAP: Uniform Manifold Approximation and Projection for Dimension Reduction. arXiv:1802.03426.

McInnes, L., Healy, J., Saul, N., & Großberger, L. (2018). UMAP: Uniform Manifold Approximation and Projection. Journal of Open Source Software, 3(29), 861.
