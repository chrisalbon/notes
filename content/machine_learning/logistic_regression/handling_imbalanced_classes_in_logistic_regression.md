---
title: "Handling Imbalanced Classes In Logistic Regression"
author: "Chris Albon"
date: 2017-12-20T11:53:49-07:00
description: "How to handle imbalanced classes in logistic regression in scikit-learn."
type: technical_note
draft: false
---
Like many other learning algorithms in scikit-learn, `LogisticRegression` comes with a built-in method of handling imbalanced classes. If we have highly imbalanced classes and have no addressed it during preprocessing, we have the option of using the `class_weight` parameter to weight the classes to make certain we have a balanced mix of each class. Specifically, the `balanced` argument will automatically weigh classes inversely proportional to their frequency:

$$w\_j = \frac{n}{kn\_{j}}$$

where $w_j$ is the weight to class $j$, $n$ is the number of observations, $n_j$ is the number of observations in class $j$, and $k$ is the total number of classes.

## Preliminaries


```python
# Load libraries
from sklearn.linear_model import LogisticRegression
from sklearn import datasets
from sklearn.preprocessing import StandardScaler
import numpy as np

```

## Load Iris Flower Dataset


```python
# Load data
iris = datasets.load_iris()
X = iris.data
y = iris.target
```

## Make Classes Imbalanced


```python
# Make class highly imbalanced by removing first 40 observations
X = X[40:,:]
y = y[40:]

# Create target vector indicating if class 0, otherwise 1
y = np.where((y == 0), 0, 1)
```

## Standardize Features


```python
# Standarize features
scaler = StandardScaler()
X_std = scaler.fit_transform(X)
```

## Train A Logistic Regression With Weighted Classes


```python
# Create logistic regression classifer object
clf = LogisticRegression(random_state=0, class_weight='balanced')

# Train model
model = clf.fit(X_std, y)
```
