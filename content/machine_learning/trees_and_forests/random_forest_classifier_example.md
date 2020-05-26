---
title: "Random Forest Classifier Example"
author: "Chris Albon"
date: 2017-12-20T11:53:49-07:00
description: "random_forest_classifier_example using Scikit."
type: technical_note
draft: false
aliases:
    - /machine_learning/random_forest_classifier_example_scikit.html
---
This tutorial is based on Yhat's 2013 tutorial on [Random Forests in Python](http://blog.yhat.com/posts/random-forests-in-python.html). If you want a good summary of the theory and uses of random forests, I suggest you check out their guide. In the tutorial below, I annotate, correct, and expand on a short code example of random forests they present at the end of the article. Specifically, I 1) update the code so it runs in the latest version of pandas and Python, 2) write detailed comments explaining what is happening in each step, and 3) expand the code in a number of ways.

Let's get started!

### A Note About The Data

The data for this tutorial is famous. Called, [the iris dataset](https://en.wikipedia.org/wiki/Iris_flower_data_set), it contains four variables measuring various parts of iris flowers of three related species, and then a fourth variable with the species name. The reason it is so famous in machine learning and statistics communities is because the data requires very little preprocessing (i.e. no missing values, all features are floating numbers, etc.).

## Preliminaries


```python
# Load the library with the iris dataset
from sklearn.datasets import load_iris

# Load scikit's random forest classifier library
from sklearn.ensemble import RandomForestClassifier

# Load pandas
import pandas as pd

# Load numpy
import numpy as np

# Set random seed
np.random.seed(0)
```

## Load Data


```python
# Create an object called iris with the iris data
iris = load_iris()

# Create a dataframe with the four feature variables
df = pd.DataFrame(iris.data, columns=iris.feature_names)

# View the top 5 rows
df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal length (cm)</th>
      <th>sepal width (cm)</th>
      <th>petal length (cm)</th>
      <th>petal width (cm)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
      <td>3.0</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4.7</td>
      <td>3.2</td>
      <td>1.3</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.6</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.0</td>
      <td>3.6</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Add a new column with the species names, this is what we are going to try to predict
df['species'] = pd.Categorical.from_codes(iris.target, iris.target_names)

# View the top 5 rows
df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal length (cm)</th>
      <th>sepal width (cm)</th>
      <th>petal length (cm)</th>
      <th>petal width (cm)</th>
      <th>species</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
      <td>3.0</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4.7</td>
      <td>3.2</td>
      <td>1.3</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.6</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.0</td>
      <td>3.6</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
  </tbody>
</table>
</div>



## Create Training And Test Data


```python
# Create a new column that for each row, generates a random number between 0 and 1, and
# if that value is less than or equal to .75, then sets the value of that cell as True
# and false otherwise. This is a quick and dirty way of randomly assigning some rows to
# be used as the training data and some as the test data.
df['is_train'] = np.random.uniform(0, 1, len(df)) <= .75

# View the top 5 rows
df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal length (cm)</th>
      <th>sepal width (cm)</th>
      <th>petal length (cm)</th>
      <th>petal width (cm)</th>
      <th>species</th>
      <th>is_train</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>setosa</td>
      <td>True</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
      <td>3.0</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>setosa</td>
      <td>True</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4.7</td>
      <td>3.2</td>
      <td>1.3</td>
      <td>0.2</td>
      <td>setosa</td>
      <td>True</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.6</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.2</td>
      <td>setosa</td>
      <td>True</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.0</td>
      <td>3.6</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>setosa</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create two new dataframes, one with the training rows, one with the test rows
train, test = df[df['is_train']==True], df[df['is_train']==False]
```


```python
# Show the number of observations for the test and training dataframes
print('Number of observations in the training data:', len(train))
print('Number of observations in the test data:',len(test))
```

    Number of observations in the training data: 118
    Number of observations in the test data: 32


## Preprocess Data


```python
# Create a list of the feature column's names
features = df.columns[:4]

# View features
features
```




    Index(['sepal length (cm)', 'sepal width (cm)', 'petal length (cm)',
           'petal width (cm)'],
          dtype='object')




```python
# train['species'] contains the actual species names. Before we can use it,
# we need to convert each species name into a digit. So, in this case there
# are three species, which have been coded as 0, 1, or 2.
y = pd.factorize(train['species'])[0]

# View target
y
```




    array([0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
           0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1,
           1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
           1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,
           2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,
           2, 2, 2])



## Train The Random Forest Classifier


```python
# Create a random forest Classifier. By convention, clf means 'Classifier'
clf = RandomForestClassifier(n_jobs=2, random_state=0)

# Train the Classifier to take the training features and learn how they relate
# to the training y (the species)
clf.fit(train[features], y)
```




    RandomForestClassifier(bootstrap=True, class_weight=None, criterion='gini',
                max_depth=None, max_features='auto', max_leaf_nodes=None,
                min_impurity_split=1e-07, min_samples_leaf=1,
                min_samples_split=2, min_weight_fraction_leaf=0.0,
                n_estimators=10, n_jobs=2, oob_score=False, random_state=0,
                verbose=0, warm_start=False)



Huzzah! We have done it! We have officially trained our random forest Classifier! Now let's play with it. The Classifier model itself is stored in the `clf` variable.

## Apply Classifier To Test Data

If you have been following along, you will know we only trained our classifier on part of the data, leaving the rest out. This is, in my humble opinion, the most important part of machine learning. Why? Because by leaving out a portion of the data, we have a set of data to test the accuracy of our model!

Let's do that now.


```python
# Apply the Classifier we trained to the test data (which, remember, it has never seen before)
clf.predict(test[features])
```




    array([0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 2, 2, 1, 1, 2, 2, 2,
           2, 2, 2, 2, 2, 2, 2, 2, 2])



What are you looking at above? Remember that we coded each of the three species of plant as 0, 1, or 2. What the list of numbers above is showing you is what species our model predicts each plant is based on the the sepal length, sepal width, petal length, and petal width. How confident is the classifier about each plant? We can see that too.


```python
# View the predicted probabilities of the first 10 observations
clf.predict_proba(test[features])[0:10]
```




    array([[ 1. ,  0. ,  0. ],
           [ 1. ,  0. ,  0. ],
           [ 1. ,  0. ,  0. ],
           [ 1. ,  0. ,  0. ],
           [ 1. ,  0. ,  0. ],
           [ 1. ,  0. ,  0. ],
           [ 1. ,  0. ,  0. ],
           [ 0.9,  0.1,  0. ],
           [ 1. ,  0. ,  0. ],
           [ 1. ,  0. ,  0. ]])



There are three species of plant, thus `[ 1. ,  0. ,  0. ]` tells us that the classifier is certain that the plant is the first class. Taking another example, `[ 0.9,  0.1,  0. ]` tells us that the classifier gives a 90% probability the plant belongs to the first class and a 10% probability the plant belongs to the second class. Because 90 is greater than 10, the classifier predicts the plant is the first class.

## Evaluate Classifier

Now that we have predicted the species of all plants in the test data, we can compare our predicted species with the that plant's actual species.


```python
# Create actual english names for the plants for each predicted plant class
preds = iris.target_names[clf.predict(test[features])]
```


```python
# View the PREDICTED species for the first five observations
preds[0:5]
```




    array(['setosa', 'setosa', 'setosa', 'setosa', 'setosa'],
          dtype='<U10')




```python
# View the ACTUAL species for the first five observations
test['species'].head()
```




    7     setosa
    8     setosa
    10    setosa
    13    setosa
    17    setosa
    Name: species, dtype: category
    Categories (3, object): [setosa, versicolor, virginica]



That looks pretty good! At least for the first five observations. Now let's use look at all the data.

### Create a confusion matrix

A [confusion matrix](https://en.wikipedia.org/wiki/Confusion_matrix) can be, no pun intended, a little confusing to interpret at first, but it is actually very straightforward. The columns are the species we predicted for the test data and the rows are the actual species for the test data. So, if we take the top row, we can wee that we predicted all 13 setosa plants in the test data perfectly. However, in the next row, we predicted 5 of the versicolor plants correctly, but mis-predicted two of the versicolor plants as virginica.

The short explanation of how to interpret a confusion matrix is: anything on the diagonal was classified correctly and anything off the diagonal was classified incorrectly.


```python
# Create confusion matrix
pd.crosstab(test['species'], preds, rownames=['Actual Species'], colnames=['Predicted Species'])
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Predicted Species</th>
      <th>setosa</th>
      <th>versicolor</th>
      <th>virginica</th>
    </tr>
    <tr>
      <th>Actual Species</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>setosa</th>
      <td>13</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>versicolor</th>
      <td>0</td>
      <td>5</td>
      <td>2</td>
    </tr>
    <tr>
      <th>virginica</th>
      <td>0</td>
      <td>0</td>
      <td>12</td>
    </tr>
  </tbody>
</table>
</div>



## View Feature Importance

While we don't get regression coefficients like with OLS, we do get a score telling us how important each feature was in classifying. This is one of the most powerful parts of random forests, because we can clearly see that petal width was more important in classification than sepal width.


```python
# View a list of the features and their importance scores
list(zip(train[features], clf.feature_importances_))
```




    [('sepal length (cm)', 0.11185992930506346),
     ('sepal width (cm)', 0.016341813006098178),
     ('petal length (cm)', 0.36439533040889194),
     ('petal width (cm)', 0.5074029272799464)]


