---
title: "Loading scikit-learn's Boston Housing Dataset"
author: "Chris Albon"
date: 2017-12-20T11:53:49-07:00
description: "Loading the built-in Boston housing datasets of scikit-learn."
type: technical_note
draft: false
---
## Preliminaries


```python
# Load libraries
from sklearn import datasets
import matplotlib.pyplot as plt 
```

## Load Boston Housing Dataset

The [Boston housing dataset](http://www.cs.toronto.edu/~delve/data/boston/bostonDetail.html) is a famous dataset from the 1970s. It contains 506 observations on housing prices around Boston. It is often used in regression examples and contains 15 features.


```python
# Load dataset
boston = datasets.load_boston()

# Create feature matrix
X = boston.data

# Create target vector
y = boston.target

# View the first observation's feature values
X[0]
```




    array([  6.32000000e-03,   1.80000000e+01,   2.31000000e+00,
             0.00000000e+00,   5.38000000e-01,   6.57500000e+00,
             6.52000000e+01,   4.09000000e+00,   1.00000000e+00,
             2.96000000e+02,   1.53000000e+01,   3.96900000e+02,
             4.98000000e+00])



As you can see, the features are not standardized. This is more easily seen if we display the values as decimals:


```python
# Display each feature value of the first observation as floats
['{:f}'.format(x) for x in X[0]]
```




    ['0.006320',
     '18.000000',
     '2.310000',
     '0.000000',
     '0.538000',
     '6.575000',
     '65.200000',
     '4.090000',
     '1.000000',
     '296.000000',
     '15.300000',
     '396.900000',
     '4.980000']



Therefore, it is often beneficial and/or required to standardize the value of the features.
