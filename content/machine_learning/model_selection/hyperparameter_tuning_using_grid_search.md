---
title: "Hyperparameter Tuning Using Grid Search"
author: "Chris Albon"
date: 2017-12-20T11:53:49-07:00
description: "How to conduct grid search for hyperparameter tuning in scikit-learn for machine learning in Python."
type: technical_note
draft: false
---
<a alt="Hyperparameter Tuning Using Grid Search" href="https://machinelearningflashcards.com">
    <img src="/images/machine_learning_flashcards/Hyperparameter_Tuning_print.png" class="flashcard center-block">
</a>

## Preliminaries


```python
# Load libraries
import numpy as np
from sklearn import linear_model, datasets
from sklearn.model_selection import GridSearchCV
```

## Load Iris Dataset


```python
# Load data
iris = datasets.load_iris()
X = iris.data
y = iris.target
```

## Create Logistic Regression


```python
# Create logistic regression
logistic = linear_model.LogisticRegression()
```

## Create Hyperparameter Search Space


```python
# Create regularization penalty space
penalty = ['l1', 'l2']

# Create regularization hyperparameter space
C = np.logspace(0, 4, 10).rvs(10)

# Create hyperparameter options
hyperparameters = dict(C=C, penalty=penalty)
```

## Create Grid Search


```python
# Create grid search using 5-fold cross validation
clf = GridSearchCV(logistic, hyperparameters, cv=5, verbose=0)
```

## Conduct Grid Search


```python
# Fit grid search
best_model = clf.fit(X, y)
```

## View Hyperparameter Values Of Best Model


```python
# View best hyperparameters
print('Best Penalty:', best_model.best_estimator_.get_params()['penalty'])
print('Best C:', best_model.best_estimator_.get_params()['C'])
```

    Best Penalty: l1
    Best C: 7.74263682681


## Predict Using Best Model


```python
# Predict target vector
best_model.predict(X)
```




    array([0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
           0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
           0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
           1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1,
           1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,
           2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 1, 2, 2, 2, 2,
           2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2])


