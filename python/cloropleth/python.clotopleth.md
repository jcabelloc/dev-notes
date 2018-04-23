
# Choropleth Maps (from Jose Portilla)

## Offline Plotly Usage

Get imports and set everything up to be working offline.


```python
import plotly.plotly as py
import plotly.graph_objs as go 
from plotly.offline import download_plotlyjs, init_notebook_mode, plot, iplot
```

Now set up everything so that the figures show up in the notebook:


```python
init_notebook_mode(connected=True) 
```



More info on other options for Offline Plotly usage can be found [here](https://plot.ly/python/offline/).

## Choropleth US Maps

Plotly's mapping can be a bit hard to get used to at first, remember to reference the cheat sheet in the data visualization folder, or [find it online here](https://images.plot.ly/plotly-documentation/images/python_cheat_sheet.pdf).


```python
import pandas as pd
```

Now we need to begin to build our data dictionary. Easiest way to do this is to use the **dict()** function of the general form:

* type = 'choropleth',
* locations = list of states
* locationmode = 'USA-states'
* colorscale= 

Either a predefined string:

    'pairs' | 'Greys' | 'Greens' | 'Bluered' | 'Hot' | 'Picnic' | 'Portland' | 'Jet' | 'RdBu' | 'Blackbody' | 'Earth' | 'Electric' | 'YIOrRd' | 'YIGnBu'

or create a [custom colorscale](https://plot.ly/python/heatmap-and-contour-colorscales/)

* text= list or array of text to display per point
* z= array of values on z axis (color of state)
* colorbar = {'title':'Colorbar Title'})

Here is a simple example:


```python
data = dict(type = 'choropleth',
            locations = ['AZ','CA','NY'],
            locationmode = 'USA-states',
            colorscale= 'Portland',
            text= ['text1','text2','text3'],
            z=[1.0,2.0,3.0],
            colorbar = {'title':'Colorbar Title'})
```

Then we create the layout nested dictionary:


```python
layout = dict(geo = {'scope':'usa'})
```

Then we use: 

    go.Figure(data = [data],layout = layout)
    
to set up the object that finally gets passed into iplot()


```python
choromap = go.Figure(data = [data],layout = layout)
```


```python
iplot(choromap)
```


### Real Data US Map Choropleth

Now let's show an example with some real data as well as some other options we can add to the dictionaries in data and layout.


```python
df = pd.read_csv('2011_US_AGRI_Exports')
df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>code</th>
      <th>state</th>
      <th>category</th>
      <th>total exports</th>
      <th>beef</th>
      <th>pork</th>
      <th>poultry</th>
      <th>dairy</th>
      <th>fruits fresh</th>
      <th>fruits proc</th>
      <th>total fruits</th>
      <th>veggies fresh</th>
      <th>veggies proc</th>
      <th>total veggies</th>
      <th>corn</th>
      <th>wheat</th>
      <th>cotton</th>
      <th>text</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AL</td>
      <td>Alabama</td>
      <td>state</td>
      <td>1390.63</td>
      <td>34.4</td>
      <td>10.6</td>
      <td>481.0</td>
      <td>4.06</td>
      <td>8.0</td>
      <td>17.1</td>
      <td>25.11</td>
      <td>5.5</td>
      <td>8.9</td>
      <td>14.33</td>
      <td>34.9</td>
      <td>70.0</td>
      <td>317.61</td>
      <td>Alabama&lt;br&gt;Beef 34.4 Dairy 4.06&lt;br&gt;Fruits 25.1...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AK</td>
      <td>Alaska</td>
      <td>state</td>
      <td>13.31</td>
      <td>0.2</td>
      <td>0.1</td>
      <td>0.0</td>
      <td>0.19</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.00</td>
      <td>0.6</td>
      <td>1.0</td>
      <td>1.56</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.00</td>
      <td>Alaska&lt;br&gt;Beef 0.2 Dairy 0.19&lt;br&gt;Fruits 0.0 Ve...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>AZ</td>
      <td>Arizona</td>
      <td>state</td>
      <td>1463.17</td>
      <td>71.3</td>
      <td>17.9</td>
      <td>0.0</td>
      <td>105.48</td>
      <td>19.3</td>
      <td>41.0</td>
      <td>60.27</td>
      <td>147.5</td>
      <td>239.4</td>
      <td>386.91</td>
      <td>7.3</td>
      <td>48.7</td>
      <td>423.95</td>
      <td>Arizona&lt;br&gt;Beef 71.3 Dairy 105.48&lt;br&gt;Fruits 60...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>AR</td>
      <td>Arkansas</td>
      <td>state</td>
      <td>3586.02</td>
      <td>53.2</td>
      <td>29.4</td>
      <td>562.9</td>
      <td>3.53</td>
      <td>2.2</td>
      <td>4.7</td>
      <td>6.88</td>
      <td>4.4</td>
      <td>7.1</td>
      <td>11.45</td>
      <td>69.5</td>
      <td>114.5</td>
      <td>665.44</td>
      <td>Arkansas&lt;br&gt;Beef 53.2 Dairy 3.53&lt;br&gt;Fruits 6.8...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>CA</td>
      <td>California</td>
      <td>state</td>
      <td>16472.88</td>
      <td>228.7</td>
      <td>11.1</td>
      <td>225.4</td>
      <td>929.95</td>
      <td>2791.8</td>
      <td>5944.6</td>
      <td>8736.40</td>
      <td>803.2</td>
      <td>1303.5</td>
      <td>2106.79</td>
      <td>34.6</td>
      <td>249.3</td>
      <td>1064.95</td>
      <td>California&lt;br&gt;Beef 228.7 Dairy 929.95&lt;br&gt;Frui...</td>
    </tr>
  </tbody>
</table>
</div>



Now out data dictionary with some extra marker and colorbar arguments:


```python
data = dict(type='choropleth',
            colorscale = 'YIOrRd',
            locations = df['code'],
            z = df['total exports'],
            locationmode = 'USA-states',
            text = df['text'],
            marker = dict(line = dict(color = 'rgb(255,255,255)',width = 2)),
            colorbar = {'title':"Millions USD"}
            ) 
```

And our layout dictionary with some more arguments:


```python
layout = dict(title = '2011 US Agriculture Exports by State',
              geo = dict(scope='usa',
                         showlakes = True,
                         lakecolor = 'rgb(85,173,240)')
             )
```


```python
choromap = go.Figure(data = [data],layout = layout)
```


```python
iplot(choromap)
```




# World Choropleth Map

Now let's see an example with a World Map:


```python
df = pd.read_csv('2014_World_GDP')
df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>COUNTRY</th>
      <th>GDP (BILLIONS)</th>
      <th>CODE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Afghanistan</td>
      <td>21.71</td>
      <td>AFG</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Albania</td>
      <td>13.40</td>
      <td>ALB</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Algeria</td>
      <td>227.80</td>
      <td>DZA</td>
    </tr>
    <tr>
      <th>3</th>
      <td>American Samoa</td>
      <td>0.75</td>
      <td>ASM</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Andorra</td>
      <td>4.80</td>
      <td>AND</td>
    </tr>
  </tbody>
</table>
</div>




```python
data = dict(
        type = 'choropleth',
        locations = df['CODE'],
        z = df['GDP (BILLIONS)'],
        text = df['COUNTRY'],
        colorbar = {'title' : 'GDP Billions US'},
      ) 
```


```python
layout = dict(
    title = '2014 Global GDP',
    geo = dict(
        showframe = False,
        projection = {'type':'Mercator'}
    )
)
```


```python
choromap = go.Figure(data = [data],layout = layout)
iplot(choromap)
```

# Great Job!
