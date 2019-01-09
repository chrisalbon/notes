---
title: "Calculate The Trace Of A Matrix"
author: "Chris Albon"
date: 2017-12-20T11:53:49-07:00
description: "How to calculate the trace of a matrix in Python."
type: technical_note
draft: false
---
## Preliminaries


```python
# Load library
import numpy as np
```

## Create Matrix


```python
# Create matrix
matrix = np.array([[1, 2, 3],
                   [4, 5, 6],
                   [7, 8, 9]])
```

## Calculate The Trace


```python
# Calculate the trace of the matrix
matrix.diagonal().sum()
```




    15


