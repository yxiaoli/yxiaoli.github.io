+++
title =  "Feature Selection for unsupervised Learning with R and Python"
date =  2018-05-13T09:57:55+01:00
tags = ["Data Mining","Machine Learning","bigdata"]
categories = ["tech"]
description = "The third step of data mining is data reduction, one of the most useful method is to select the most useful features before we apply the models"
menu = ""
banner = "banners/w.e.2.jpg"
+++

# Starting from dimensionality reduction

Feature selection is a part technique of *data dimensional reduction*.
According to the book `Data minging: concepts and techniques`, the most ubiquitous methods are:

- wavelet transforms
- principal components analysis (PCA)
- attribute subset selection(or feature selection)

It is worth mentioning, that **PCA**, **Exploratory Factor Analysis (EFA)**, **SVD**, etc are all methods which reconstruct our original attributes. PCA is essentially creates new variables that are linear combinations of the original variables.

However, if we want to reserve the original attributes, then take a look at **Feature selection**.

# Overview of Feature Selection

There exist several ways to category the techniques of feature selection: [1](https://en.wikipedia.org/wiki/Feature_selection)
[2](http://topepo.github.io/caret/feature-selection-overview.html)

Yet From the problem solving prospective,I divide the part of techniques into those ways:

- **Supervised(regression)**: LASSO, REF, Autoencoder, etc. The regression area has been investigated
extensively [more information](https://machinelearningmastery.com/feature-selection-with-the-caret-r-package/)
- **Unsupervised**: principal feature analysis(PFA)

# Concepts of unsupervised method(PFA)
[Details to refer](http://venom.cs.utsa.edu/dmz/techrep/2007/CS-TR-2007-011.pdf)

Steps:

1. Compute the sample covariance matrix,
2. Compute the Principal components and eigenvalues of the
Covariance /Correlation matrix **A**.
3. Choose the subspace dimension **n**, we get new matrix **A_n**,
   the vectors **Vi** are the rows of **A_n**. 
4. Cluster the vectors **|Vi|**, using K-Means
5. For each cluster, find the corresponding vector **Vi** which is closest to the mean of the cluster. 

# Code

```python
class PFA(object):
    def __init__(self, n_features, q=None):
        self.q = q
        self.n_features = n_features

    def fit(self, X):
        if not self.q:
            self.q = X.shape[1]

        sc = StandardScaler()
        X = sc.fit_transform(X)

        pca = PCA(n_components=self.q).fit(X)
        A_q = pca.components_.T

        kmeans = KMeans(n_clusters=self.n_features).fit(A_q)
        clusters = kmeans.predict(A_q)
        cluster_centers = kmeans.cluster_centers_

        dists = defaultdict(list)
        for i, c in enumerate(clusters):
            dist = euclidean_distances([A_q[i, :]], [cluster_centers[c, :]])[0][0]
            dists[c].append((i, dist))

        self.indices_ = [sorted(f, key=lambda x: x[1])[0][0] for f in dists.values()]
        self.features_ = X[:, self.indices_]
```
the usage:

``` python
pfa = PFA(n_features=10)
pfa.fit(dataset)
# To get the transformed matrix
x = pfa.features_
# To get the column indices of the kept features
column_indices = pfa.indices_
```

# Conclusion
Next time we'll take a closer look at supervised method.

# Reference:
1. [Wiki from Feature selectionk](https://en.wikipedia.org/wiki/Feature_selection)
2. [Exploratory Factor Analysis](https://zhuanlan.zhihu.com/p/23981354)
3. [An Introduction to Feature Selection](https://machinelearningmastery.com/an-introduction-to-feature-selection/)
4. [11/2000, Feature selection for unsupervised learning](http://www.jmlr.org/papers/volume5/dy04a/dy04a.pdf)
5. [Feature Selection Using Principal Feature Analysis](http://venom.cs.utsa.edu/dmz/techrep/2007/CS-TR-2007-011.pdf)
6. [降维的多种算法](https://chenrudan.github.io/blog/2016/04/01/dimensionalityreduction.html#3)
