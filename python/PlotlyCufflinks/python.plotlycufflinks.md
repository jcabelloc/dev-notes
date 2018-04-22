
# Plotly and Cufflinks (from Jose Portilla)

Plotly is a library that allows you to create interactive plots that you can use in dashboards or websites (you can save them as html files or static images).

## Installation

In order for this all to work, you'll need to install plotly and cufflinks to call plots directly off of a pandas dataframe. These libraries are not currently available through **conda** but are available through **pip**. Install the libraries at your command line/terminal using:

    pip install plotly
    pip install cufflinks

** NOTE: Make sure you only have one installation of Python on your computer when you do this, otherwise the installation may not work. **

## Imports and Set-up


```python
import pandas as pd
import numpy as np
%matplotlib inline
```


```python
from plotly import __version__
from plotly.offline import download_plotlyjs, init_notebook_mode, plot, iplot

print(__version__) # requires version >= 1.9.0
```

    1.12.9
    


```python
import cufflinks as cf
```


```python
# For Notebooks
init_notebook_mode(connected=True)
```


```python
# For offline use
cf.go_offline()
```



### Fake Data


```python
df = pd.DataFrame(np.random.randn(100,4),columns='A B C D'.split())
```


```python
df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.878725</td>
      <td>0.688719</td>
      <td>1.066733</td>
      <td>0.543956</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.028734</td>
      <td>0.104054</td>
      <td>0.048176</td>
      <td>1.842188</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.158793</td>
      <td>0.387926</td>
      <td>-0.635371</td>
      <td>-0.637558</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-1.221972</td>
      <td>1.393423</td>
      <td>-0.299794</td>
      <td>-1.113622</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1.253152</td>
      <td>-0.537598</td>
      <td>0.302917</td>
      <td>-2.546083</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2 = pd.DataFrame({'Category':['A','B','C'],'Values':[32,43,50]})
```


```python
df2.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Category</th>
      <th>Values</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>32</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B</td>
      <td>43</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>50</td>
    </tr>
  </tbody>
</table>
</div>



## Using Cufflinks and iplot()

* scatter
* bar
* box
* spread
* ratio
* heatmap
* surface
* histogram
* bubble

## Scatter


```python
df.iplot(kind='scatter',x='A',y='B',mode='markers',size=10)
```



## Bar Plots


```python
df2.iplot(kind='bar',x='Category',y='Values')
```



```python
df.count().iplot(kind='bar')
```



## Boxplots


```python
df.iplot(kind='box')
```


## 3d Surface


```python
df3 = pd.DataFrame({'x':[1,2,3,4,5],'y':[10,20,30,20,10],'z':[5,4,3,2,1]})
df3.iplot(kind='surface',colorscale='rdylbu')
```



## Spread


```python
df[['A','B']].iplot(kind='spread')
```




## histogram


```python
df['A'].iplot(kind='hist',bins=25)
```



```python
df.iplot(kind='bubble',x='A',y='B',size='C')
```




## scatter_matrix()

Similar to sns.pairplot()


```python
df.scatter_matrix()
```


# Great Job!
