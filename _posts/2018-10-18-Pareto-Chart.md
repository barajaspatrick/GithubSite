---
layout: post
title: Pareto Chart
---

-------------------
In this quick tutorial we will build a simple pareto distribution.

A pareto chart is a graph that contains both a bar graph and line plot. The bars represents the percentage a element appeares in a data set, and the line plot represents the cumulative percentage of all the elements.

For more information on the the Pareto Chart click [https://en.wikipedia.org/wiki/Pareto_chart](here)

### Reading The Data
For this exercise we will use the 'source of Energy' Data set. This is a simple data set containing a catagory column with the type of energy source, and a Percentage column containing the amount of energy produced from that source.


```python
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
```


```python
energySource = pd.read_excel("Source of Energy.xlsx")
energySource
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
      <th>Source of Electricity</th>
      <th>Percentage</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Coal</td>
      <td>0.42</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Hydro and Renewables</td>
      <td>0.13</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Natural Gas</td>
      <td>0.25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Nuclear Power</td>
      <td>0.19</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Other</td>
      <td>0.01</td>
    </tr>
  </tbody>
</table>
</div>



### Preparing the Data
Before we can move on any further we need to add a cumulative Percentage column to our data set.


```python
energySource["Cumulative Percentage"] = energySource["Percentage"].cumsum()
energySource
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
      <th>Source of Electricity</th>
      <th>Percentage</th>
      <th>Cumulative Percentage</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Coal</td>
      <td>0.42</td>
      <td>0.42</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Hydro and Renewables</td>
      <td>0.13</td>
      <td>0.55</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Natural Gas</td>
      <td>0.25</td>
      <td>0.80</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Nuclear Power</td>
      <td>0.19</td>
      <td>0.99</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Other</td>
      <td>0.01</td>
      <td>1.00</td>
    </tr>
  </tbody>
</table>
</div>

### Constructing the Pareto Chart

```python
sns.barplot(x=energySource["Source of Electricity"],
               y=energySource["Percentage"],
               color='b')
```
![](https://github.com/barajaspatrick/barajaspatrick.github.io/blob/master/images/postimages/pareto_1.png?raw=true)


So far so good! Now the next step is to add a line in our bar plot representing the cumulative percentage to finish up our graph.


```python
sns.barplot(x=energySource["Source of Electricity"],
               y=energySource["Percentage"],
               color='b')
sns.pointplot(x=energySource["Source of Electricity"],
               y=energySource["Cumulative Percentage"],
               color='r')
```

![](https://github.com/barajaspatrick/barajaspatrick.github.io/blob/master/images/postimages/pareto_2.png?raw=true)
