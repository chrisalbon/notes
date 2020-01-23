---
title: "Group Data By Time"
author: "Chris Albon"
date: 2017-12-20T11:53:49-07:00
description: "Group data by time."
type: technical_note
draft: false
---
On March 13, 2016, version 0.18.0 of Pandas was released, with significant changes in how the resampling function operates. This tutorial follows v0.18.0 and will not work for previous versions of pandas.

First let's load the modules we care about

## Preliminaries


```python
# Import required packages
import pandas as pd
import datetime
import numpy as np
```

Next, let's create some sample data that we can group by time as an sample. In this example I am creating a dataframe with two columns with 365 rows. One column is a date, the second column is a numeric value.

## Create Data


```python
# Create a datetime variable for today
base = datetime.datetime.today()
# Create a list variable that creates 365 days of rows of datetime values
date_list = [base - datetime.timedelta(days=x) for x in range(0, 365)]
```


```python
# Create a list variable of 365 numeric values
score_list = list(np.random.randint(low=1, high=1000, size=365))
```


```python
# Create an empty dataframe
df = pd.DataFrame()

# Create a column from the datetime variable
df['datetime'] = date_list
# Convert that column into a datetime datatype
df['datetime'] = pd.to_datetime(df['datetime'])
# Set the datetime column as the index
df.index = df['datetime'] 
# Create a column from the numeric score variable
df['score'] = score_list
```


```python
# Let's take a took at the data
df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>datetime</th>
      <th>score</th>
    </tr>
    <tr>
      <th>datetime</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016-06-02 09:57:54.793972</th>
      <td>2016-06-02 09:57:54.793972</td>
      <td>900</td>
    </tr>
    <tr>
      <th>2016-06-01 09:57:54.793972</th>
      <td>2016-06-01 09:57:54.793972</td>
      <td>121</td>
    </tr>
    <tr>
      <th>2016-05-31 09:57:54.793972</th>
      <td>2016-05-31 09:57:54.793972</td>
      <td>547</td>
    </tr>
    <tr>
      <th>2016-05-30 09:57:54.793972</th>
      <td>2016-05-30 09:57:54.793972</td>
      <td>504</td>
    </tr>
    <tr>
      <th>2016-05-29 09:57:54.793972</th>
      <td>2016-05-29 09:57:54.793972</td>
      <td>304</td>
    </tr>
  </tbody>
</table>
</div>



## Group Data By Date

In pandas, the most common way to group by time is to use the .resample() function. In v0.18.0 this function is two-stage. This means that 'df.resample('M')' creates an object to which we can apply other functions ('mean', 'count', 'sum', etc.)


```python
# Group the data by month, and take the mean for each group (i.e. each month)
df.resample('M').mean()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>score</th>
    </tr>
    <tr>
      <th>datetime</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2015-06-30</th>
      <td>513.629630</td>
    </tr>
    <tr>
      <th>2015-07-31</th>
      <td>561.516129</td>
    </tr>
    <tr>
      <th>2015-08-31</th>
      <td>448.032258</td>
    </tr>
    <tr>
      <th>2015-09-30</th>
      <td>548.000000</td>
    </tr>
    <tr>
      <th>2015-10-31</th>
      <td>480.419355</td>
    </tr>
    <tr>
      <th>2015-11-30</th>
      <td>487.033333</td>
    </tr>
    <tr>
      <th>2015-12-31</th>
      <td>499.935484</td>
    </tr>
    <tr>
      <th>2016-01-31</th>
      <td>429.193548</td>
    </tr>
    <tr>
      <th>2016-02-29</th>
      <td>520.413793</td>
    </tr>
    <tr>
      <th>2016-03-31</th>
      <td>349.806452</td>
    </tr>
    <tr>
      <th>2016-04-30</th>
      <td>395.500000</td>
    </tr>
    <tr>
      <th>2016-05-31</th>
      <td>503.451613</td>
    </tr>
    <tr>
      <th>2016-06-30</th>
      <td>510.500000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Group the data by month, and take the sum for each group (i.e. each month)
df.resample('M').sum()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>score</th>
    </tr>
    <tr>
      <th>datetime</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2015-06-30</th>
      <td>13868</td>
    </tr>
    <tr>
      <th>2015-07-31</th>
      <td>17407</td>
    </tr>
    <tr>
      <th>2015-08-31</th>
      <td>13889</td>
    </tr>
    <tr>
      <th>2015-09-30</th>
      <td>16440</td>
    </tr>
    <tr>
      <th>2015-10-31</th>
      <td>14893</td>
    </tr>
    <tr>
      <th>2015-11-30</th>
      <td>14611</td>
    </tr>
    <tr>
      <th>2015-12-31</th>
      <td>15498</td>
    </tr>
    <tr>
      <th>2016-01-31</th>
      <td>13305</td>
    </tr>
    <tr>
      <th>2016-02-29</th>
      <td>15092</td>
    </tr>
    <tr>
      <th>2016-03-31</th>
      <td>10844</td>
    </tr>
    <tr>
      <th>2016-04-30</th>
      <td>11865</td>
    </tr>
    <tr>
      <th>2016-05-31</th>
      <td>15607</td>
    </tr>
    <tr>
      <th>2016-06-30</th>
      <td>1021</td>
    </tr>
  </tbody>
</table>
</div>



## Grouping Options

There are many options for grouping. You can learn more about them in [Pandas's timeseries docs](http://pandas.pydata.org/pandas-docs/stable/timeseries.html), however, I have also listed them below for your convience.

| Value | Description                                  |
|-------|----------------------------------------------|
| B     | business day frequency                       |
| C     | custom business day frequency (experimental) |
| D     | calendar day frequency                       |
| W     | weekly frequency                             |
| M     | month end frequency                          |
| BM    | business month end frequency                 |
| CBM   | custom business month end frequency          |
| MS    | month start frequency                        |
| BMS   | business month start frequency               |
| CBMS  | custom business month start frequency        |
| Q     | quarter end frequency                        |
| BQ    | business quarter endfrequency                |
| QS    | quarter start frequency                      |
| BQS   | business quarter start frequency             |
| A     | year end frequency                           |
| BA    | business year end frequency                  |
| AS    | year start frequency                         |
| BAS   | business year start frequency                |
| BH    | business hour frequency                      |
| H     | hourly frequency                             |
| T     | minutely frequency                           |
| S     | secondly frequency                           |
| L     | milliseonds                                  |
| U     | microseconds                                 |
| N     | nanosecondsa                                 |
