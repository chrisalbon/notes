---
title: "Logistic Regression On Very Large Data"
author: "Chris Albon"
date: 2017-12-20T11:53:49-07:00
description: "How to train a logistic regression on very large data in scikit-learn."
type: technical_note
draft: false
---
scikit-learn's `LogisticRegression` offers a number of techniques for training a logistic regression, called solvers. Most of the time scikit-learn will select the best solver automatically for us or warn us that you cannot do some thing with that solver. However, there is one particular case we should be aware of.

While an exact explanation is beyond the bounds of this book, stochastic average gradient descent allows us to train a model much faster than other solvers when our data is very large. However, it is also very sensitive to feature scaling, so standardizing our features is particularly important. We can set our learning algorithm to use this solver by setting `solver='sag'`.

## Preliminaries


```python
# Load libraries
from sklearn.linear_model import LogisticRegression
from sklearn import datasets
from sklearn.preprocessing import StandardScaler
```

## Load Iris Flower Data


```python
# Load data
iris = datasets.load_iris()
X = iris.data
y = iris.target
```

## Standardize Features


```python
# Standarize features
scaler = StandardScaler()
X_std = scaler.fit_transform(X)
```

## Train Logistic Regression Using SAG solver


```python
# Create logistic regression object using sag solver
clf = LogisticRegression(random_state=0, solver='sag')

# Train model
model = clf.fit(X_std, y)
```
