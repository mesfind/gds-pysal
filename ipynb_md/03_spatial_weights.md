
# Spatial Weights

> [`IPYNB`](../content/part1/03_spatial_weights.ipynb)


Spatial weights are mathematical structures used to represent spatial relationships. Many spatial analytics, such as spatial autocorrelation statistics and regionalization algorithms rely on spatial weights. Generally speaking, a spatial weight $w_{i,j}$ expresses the notion of a geographical relationship between locations $i$ and $j$. These relationships can be based on a number of criteria including contiguity, geospatial distance and general distances.

PySAL offers functionality for the construction, manipulation, analysis, and conversion of a wide array of spatial weights.

We begin with construction of weights from common spatial data formats.



```python
import pysal as ps
import numpy as np
```

There are functions to construct weights directly from a file path. 


```python
shp_path = "../data/texas.shp"
```

## Weight Types

### Contiguity: 
#### Queen Weights

A commonly-used type of weight is a queen contigutiy weight, which reflects adjacency relationships as a binary indicator variable denoting whether or not a polygon shares an edge or a vertex with another polygon. These weights are symmetric, in that when polygon $A$ neighbors polygon $B$, both $w_{AB} = 1$ and $w_{BA} = 1$.

To construct queen weights from a shapefile, use the `queen_from_shapefile` function:


```python
qW = ps.queen_from_shapefile(shp_path)
dataframe = ps.pdio.read_files(shp_path)
```


```python
qW
```




    <pysal.weights.weights.W at 0x7fb96bce8c18>



All weights objects have a few traits that you can use to work with the weights object, as well as to get information about the weights object. 

To get the neighbors & weights around an observation, use the observation's index on the weights object, like a dictionary:


```python
qW[4] #neighbors & weights of the 5th observation (0-index remember)
```




    {0: 1.0, 3: 1.0, 5: 1.0, 6: 1.0, 7: 1.0}



By default, the weights and the pandas dataframe will use the same index. So, we can view the observation and its neighbors in the dataframe by putting the observation's index and its neighbors' indexes together in one list:


```python
self_and_neighbors = [4]
self_and_neighbors.extend(qW.neighbors[4])
print(self_and_neighbors)
```

    [4, 0, 3, 5, 6, 7]


and grabbing those elements from the dataframe:


```python
dataframe.loc[self_and_neighbors]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NAME</th>
      <th>STATE_NAME</th>
      <th>STATE_FIPS</th>
      <th>CNTY_FIPS</th>
      <th>FIPS</th>
      <th>STFIPS</th>
      <th>COFIPS</th>
      <th>FIPSNO</th>
      <th>SOUTH</th>
      <th>HR60</th>
      <th>...</th>
      <th>BLK90</th>
      <th>GI59</th>
      <th>GI69</th>
      <th>GI79</th>
      <th>GI89</th>
      <th>FH60</th>
      <th>FH70</th>
      <th>FH80</th>
      <th>FH90</th>
      <th>geometry</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>Ochiltree</td>
      <td>Texas</td>
      <td>48</td>
      <td>357</td>
      <td>48357</td>
      <td>48</td>
      <td>357</td>
      <td>48357</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.021911</td>
      <td>0.236998</td>
      <td>0.352940</td>
      <td>0.343949</td>
      <td>0.374461</td>
      <td>5.172414</td>
      <td>4.0</td>
      <td>4.758392</td>
      <td>9.159159</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Lipscomb</td>
      <td>Texas</td>
      <td>48</td>
      <td>295</td>
      <td>48295</td>
      <td>48</td>
      <td>295</td>
      <td>48295</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.031817</td>
      <td>0.286929</td>
      <td>0.378219</td>
      <td>0.407005</td>
      <td>0.373005</td>
      <td>6.724512</td>
      <td>4.5</td>
      <td>3.835360</td>
      <td>6.093580</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hansford</td>
      <td>Texas</td>
      <td>48</td>
      <td>195</td>
      <td>48195</td>
      <td>48</td>
      <td>195</td>
      <td>48195</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.253527</td>
      <td>0.357813</td>
      <td>0.393938</td>
      <td>0.383924</td>
      <td>7.591786</td>
      <td>4.7</td>
      <td>5.542986</td>
      <td>7.125457</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Roberts</td>
      <td>Texas</td>
      <td>48</td>
      <td>393</td>
      <td>48393</td>
      <td>48</td>
      <td>393</td>
      <td>48393</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.320275</td>
      <td>0.318656</td>
      <td>0.398681</td>
      <td>0.339626</td>
      <td>5.762712</td>
      <td>5.3</td>
      <td>6.231454</td>
      <td>4.885993</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Hemphill</td>
      <td>Texas</td>
      <td>48</td>
      <td>211</td>
      <td>48211</td>
      <td>48</td>
      <td>211</td>
      <td>48211</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.188172</td>
      <td>0.286707</td>
      <td>0.385605</td>
      <td>0.352996</td>
      <td>0.346318</td>
      <td>6.300115</td>
      <td>6.9</td>
      <td>6.451613</td>
      <td>6.330366</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Hutchinson</td>
      <td>Texas</td>
      <td>48</td>
      <td>233</td>
      <td>48233</td>
      <td>48</td>
      <td>233</td>
      <td>48233</td>
      <td>1</td>
      <td>1.936915</td>
      <td>...</td>
      <td>2.635369</td>
      <td>0.199287</td>
      <td>0.322579</td>
      <td>0.323864</td>
      <td>0.370207</td>
      <td>5.387316</td>
      <td>5.1</td>
      <td>6.222679</td>
      <td>8.484271</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
  </tbody>
</table>
<p>6 rows × 70 columns</p>
</div>



A full, dense matrix describing all of the pairwise relationships is constructed using the `.full` method, or when `pysal.full` is called on a weights object:


```python
Wmatrix, ids = qW.full()
#Wmatrix, ids = ps.full(qW)
```


```python
Wmatrix
```




    array([[ 0.,  0.,  0., ...,  0.,  0.,  0.],
           [ 0.,  0.,  1., ...,  0.,  0.,  0.],
           [ 0.,  1.,  0., ...,  0.,  0.,  0.],
           ..., 
           [ 0.,  0.,  0., ...,  0.,  1.,  1.],
           [ 0.,  0.,  0., ...,  1.,  0.,  1.],
           [ 0.,  0.,  0., ...,  1.,  1.,  0.]])




```python
n_neighbors = Wmatrix.sum(axis=1) # how many neighbors each region has
```


```python
n_neighbors[4]
```




    5.0




```python
qW.cardinalities[4]
```




    5



Note that this matrix is binary, in that its elements are either zero or one, since an observation is either a neighbor or it is not a neighbor. 

However, many common use cases of spatial weights require that the matrix is row-standardized. This is done simply in PySAL using the `.transform` attribute


```python
qW.transform = 'r'
```

Now, if we build a new full matrix, its rows should sum to one:


```python
Wmatrix, ids = qW.full()
```


```python
Wmatrix.sum(axis=1) #numpy axes are 0:column, 1:row, 2:facet, into higher dimensions
```




    array([ 1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,
            1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,
            1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,
            1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,
            1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,
            1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,
            1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,
            1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,
            1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,
            1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,
            1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,
            1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,
            1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,
            1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,
            1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,
            1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,
            1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,
            1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,
            1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,
            1.,  1.,  1.,  1.,  1.,  1.,  1.])



Since weight matrices are typically very sparse, there is also a sparse weights matrix constructor:


```python
qW.sparse
```




    <254x254 sparse matrix of type '<class 'numpy.float64'>'
    	with 1460 stored elements in Compressed Sparse Row format>




```python
qW.pct_nonzero #Percentage of nonzero neighbor counts
```




    2.263004526009052



By default, PySAL assigns each observation an index according to the order in which the observation was read in. This means that, by default, all of the observations in the weights object are indexed by table order. If you have an alternative ID variable, you can pass that into the weights constructor. 

For example, the `texas.shp` dataset has a possible alternative ID Variable, a `FIPS` code.


```python
dataframe.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NAME</th>
      <th>STATE_NAME</th>
      <th>STATE_FIPS</th>
      <th>CNTY_FIPS</th>
      <th>FIPS</th>
      <th>STFIPS</th>
      <th>COFIPS</th>
      <th>FIPSNO</th>
      <th>SOUTH</th>
      <th>HR60</th>
      <th>...</th>
      <th>BLK90</th>
      <th>GI59</th>
      <th>GI69</th>
      <th>GI79</th>
      <th>GI89</th>
      <th>FH60</th>
      <th>FH70</th>
      <th>FH80</th>
      <th>FH90</th>
      <th>geometry</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Lipscomb</td>
      <td>Texas</td>
      <td>48</td>
      <td>295</td>
      <td>48295</td>
      <td>48</td>
      <td>295</td>
      <td>48295</td>
      <td>1</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.031817</td>
      <td>0.286929</td>
      <td>0.378219</td>
      <td>0.407005</td>
      <td>0.373005</td>
      <td>6.724512</td>
      <td>4.5</td>
      <td>3.835360</td>
      <td>6.093580</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Sherman</td>
      <td>Texas</td>
      <td>48</td>
      <td>421</td>
      <td>48421</td>
      <td>48</td>
      <td>421</td>
      <td>48421</td>
      <td>1</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.139958</td>
      <td>0.288976</td>
      <td>0.359377</td>
      <td>0.415453</td>
      <td>0.378041</td>
      <td>5.665722</td>
      <td>1.7</td>
      <td>3.253796</td>
      <td>3.869407</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Dallam</td>
      <td>Texas</td>
      <td>48</td>
      <td>111</td>
      <td>48111</td>
      <td>48</td>
      <td>111</td>
      <td>48111</td>
      <td>1</td>
      <td>0.0</td>
      <td>...</td>
      <td>2.050906</td>
      <td>0.331667</td>
      <td>0.385996</td>
      <td>0.370037</td>
      <td>0.376015</td>
      <td>7.546049</td>
      <td>7.2</td>
      <td>9.471366</td>
      <td>14.231738</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hansford</td>
      <td>Texas</td>
      <td>48</td>
      <td>195</td>
      <td>48195</td>
      <td>48</td>
      <td>195</td>
      <td>48195</td>
      <td>1</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.253527</td>
      <td>0.357813</td>
      <td>0.393938</td>
      <td>0.383924</td>
      <td>7.591786</td>
      <td>4.7</td>
      <td>5.542986</td>
      <td>7.125457</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Ochiltree</td>
      <td>Texas</td>
      <td>48</td>
      <td>357</td>
      <td>48357</td>
      <td>48</td>
      <td>357</td>
      <td>48357</td>
      <td>1</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.021911</td>
      <td>0.236998</td>
      <td>0.352940</td>
      <td>0.343949</td>
      <td>0.374461</td>
      <td>5.172414</td>
      <td>4.0</td>
      <td>4.758392</td>
      <td>9.159159</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 70 columns</p>
</div>



The observation we were discussing above is in the fifth row: Ochiltree county, Texas. Note that its FIPS code is 48357.

Then, instead of indexing the weights and the dataframe just based on read-order, use the `FIPS` code as an index:


```python
qW = ps.queen_from_shapefile(shp_path, idVariable='FIPS')
```


```python
qW[4] #fails, since no FIPS is 4. 
```


    ---------------------------------------------------------------------------

    KeyError                                  Traceback (most recent call last)

    <ipython-input-20-1d8a3009bc1e> in <module>()
    ----> 1 qW[4] #fails, since no FIPS is 4.
    

    /home/serge/anaconda2/envs/gds-scipy16/lib/python3.5/site-packages/pysal/weights/weights.py in __getitem__(self, key)
        504         {1: 1.0, 4: 1.0, 101: 1.0, 85: 1.0, 5: 1.0}
        505         """
    --> 506         return dict(list(zip(self.neighbors[key], self.weights[key])))
        507 
        508     def __iter__(self):


    KeyError: 4


Note that a `KeyError` in Python usually means that some index, here `4`, was not found in the collection being searched, the IDs in the queen weights object. This makes sense, since we explicitly passed an `idVariable` argument, and nothing has a `FIPS` code of 4.

Instead, if we use the observation's `FIPS` code:


```python
qW['48357']
```




    {'48195': 1.0, '48211': 1.0, '48233': 1.0, '48295': 1.0, '48393': 1.0}



We get what we need.

In addition, we have to now query the dataframe using the `FIPS` code to find our neighbors. But, this is relatively easy to do, since pandas will parse the query by looking into python objects, if told to. 

First, let us store the neighbors of our target county:


```python
self_and_neighbors = ['48357']
self_and_neighbors.extend(qW.neighbors['48357'])
```

Then, we can use this list in `.query`: 


```python
dataframe.query('FIPS in @self_and_neighbors')
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NAME</th>
      <th>STATE_NAME</th>
      <th>STATE_FIPS</th>
      <th>CNTY_FIPS</th>
      <th>FIPS</th>
      <th>STFIPS</th>
      <th>COFIPS</th>
      <th>FIPSNO</th>
      <th>SOUTH</th>
      <th>HR60</th>
      <th>...</th>
      <th>BLK90</th>
      <th>GI59</th>
      <th>GI69</th>
      <th>GI79</th>
      <th>GI89</th>
      <th>FH60</th>
      <th>FH70</th>
      <th>FH80</th>
      <th>FH90</th>
      <th>geometry</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Lipscomb</td>
      <td>Texas</td>
      <td>48</td>
      <td>295</td>
      <td>48295</td>
      <td>48</td>
      <td>295</td>
      <td>48295</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.031817</td>
      <td>0.286929</td>
      <td>0.378219</td>
      <td>0.407005</td>
      <td>0.373005</td>
      <td>6.724512</td>
      <td>4.5</td>
      <td>3.835360</td>
      <td>6.093580</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hansford</td>
      <td>Texas</td>
      <td>48</td>
      <td>195</td>
      <td>48195</td>
      <td>48</td>
      <td>195</td>
      <td>48195</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.253527</td>
      <td>0.357813</td>
      <td>0.393938</td>
      <td>0.383924</td>
      <td>7.591786</td>
      <td>4.7</td>
      <td>5.542986</td>
      <td>7.125457</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Ochiltree</td>
      <td>Texas</td>
      <td>48</td>
      <td>357</td>
      <td>48357</td>
      <td>48</td>
      <td>357</td>
      <td>48357</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.021911</td>
      <td>0.236998</td>
      <td>0.352940</td>
      <td>0.343949</td>
      <td>0.374461</td>
      <td>5.172414</td>
      <td>4.0</td>
      <td>4.758392</td>
      <td>9.159159</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Roberts</td>
      <td>Texas</td>
      <td>48</td>
      <td>393</td>
      <td>48393</td>
      <td>48</td>
      <td>393</td>
      <td>48393</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.320275</td>
      <td>0.318656</td>
      <td>0.398681</td>
      <td>0.339626</td>
      <td>5.762712</td>
      <td>5.3</td>
      <td>6.231454</td>
      <td>4.885993</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Hemphill</td>
      <td>Texas</td>
      <td>48</td>
      <td>211</td>
      <td>48211</td>
      <td>48</td>
      <td>211</td>
      <td>48211</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.188172</td>
      <td>0.286707</td>
      <td>0.385605</td>
      <td>0.352996</td>
      <td>0.346318</td>
      <td>6.300115</td>
      <td>6.9</td>
      <td>6.451613</td>
      <td>6.330366</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Hutchinson</td>
      <td>Texas</td>
      <td>48</td>
      <td>233</td>
      <td>48233</td>
      <td>48</td>
      <td>233</td>
      <td>48233</td>
      <td>1</td>
      <td>1.936915</td>
      <td>...</td>
      <td>2.635369</td>
      <td>0.199287</td>
      <td>0.322579</td>
      <td>0.323864</td>
      <td>0.370207</td>
      <td>5.387316</td>
      <td>5.1</td>
      <td>6.222679</td>
      <td>8.484271</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
  </tbody>
</table>
<p>6 rows × 70 columns</p>
</div>



Note that we have to use `@` before the name in order to show that we're referring to a python object and not a column in the dataframe. 


```python
#dataframe.query('FIPS in self_and_neighbors') will fail because there is no column called 'self_and_neighbors'
```

Of course, we could also reindex the dataframe to use the same index as our weights:


```python
fips_frame = dataframe.set_index(dataframe.FIPS)
fips_frame.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NAME</th>
      <th>STATE_NAME</th>
      <th>STATE_FIPS</th>
      <th>CNTY_FIPS</th>
      <th>FIPS</th>
      <th>STFIPS</th>
      <th>COFIPS</th>
      <th>FIPSNO</th>
      <th>SOUTH</th>
      <th>HR60</th>
      <th>...</th>
      <th>BLK90</th>
      <th>GI59</th>
      <th>GI69</th>
      <th>GI79</th>
      <th>GI89</th>
      <th>FH60</th>
      <th>FH70</th>
      <th>FH80</th>
      <th>FH90</th>
      <th>geometry</th>
    </tr>
    <tr>
      <th>FIPS</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>48295</th>
      <td>Lipscomb</td>
      <td>Texas</td>
      <td>48</td>
      <td>295</td>
      <td>48295</td>
      <td>48</td>
      <td>295</td>
      <td>48295</td>
      <td>1</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.031817</td>
      <td>0.286929</td>
      <td>0.378219</td>
      <td>0.407005</td>
      <td>0.373005</td>
      <td>6.724512</td>
      <td>4.5</td>
      <td>3.835360</td>
      <td>6.093580</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>48421</th>
      <td>Sherman</td>
      <td>Texas</td>
      <td>48</td>
      <td>421</td>
      <td>48421</td>
      <td>48</td>
      <td>421</td>
      <td>48421</td>
      <td>1</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.139958</td>
      <td>0.288976</td>
      <td>0.359377</td>
      <td>0.415453</td>
      <td>0.378041</td>
      <td>5.665722</td>
      <td>1.7</td>
      <td>3.253796</td>
      <td>3.869407</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>48111</th>
      <td>Dallam</td>
      <td>Texas</td>
      <td>48</td>
      <td>111</td>
      <td>48111</td>
      <td>48</td>
      <td>111</td>
      <td>48111</td>
      <td>1</td>
      <td>0.0</td>
      <td>...</td>
      <td>2.050906</td>
      <td>0.331667</td>
      <td>0.385996</td>
      <td>0.370037</td>
      <td>0.376015</td>
      <td>7.546049</td>
      <td>7.2</td>
      <td>9.471366</td>
      <td>14.231738</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>48195</th>
      <td>Hansford</td>
      <td>Texas</td>
      <td>48</td>
      <td>195</td>
      <td>48195</td>
      <td>48</td>
      <td>195</td>
      <td>48195</td>
      <td>1</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.253527</td>
      <td>0.357813</td>
      <td>0.393938</td>
      <td>0.383924</td>
      <td>7.591786</td>
      <td>4.7</td>
      <td>5.542986</td>
      <td>7.125457</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>48357</th>
      <td>Ochiltree</td>
      <td>Texas</td>
      <td>48</td>
      <td>357</td>
      <td>48357</td>
      <td>48</td>
      <td>357</td>
      <td>48357</td>
      <td>1</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.021911</td>
      <td>0.236998</td>
      <td>0.352940</td>
      <td>0.343949</td>
      <td>0.374461</td>
      <td>5.172414</td>
      <td>4.0</td>
      <td>4.758392</td>
      <td>9.159159</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 70 columns</p>
</div>



Now that both are using the same weights, we can use the `.loc` indexer again:


```python
fips_frame.loc[self_and_neighbors]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NAME</th>
      <th>STATE_NAME</th>
      <th>STATE_FIPS</th>
      <th>CNTY_FIPS</th>
      <th>FIPS</th>
      <th>STFIPS</th>
      <th>COFIPS</th>
      <th>FIPSNO</th>
      <th>SOUTH</th>
      <th>HR60</th>
      <th>...</th>
      <th>BLK90</th>
      <th>GI59</th>
      <th>GI69</th>
      <th>GI79</th>
      <th>GI89</th>
      <th>FH60</th>
      <th>FH70</th>
      <th>FH80</th>
      <th>FH90</th>
      <th>geometry</th>
    </tr>
    <tr>
      <th>FIPS</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>48357</th>
      <td>Ochiltree</td>
      <td>Texas</td>
      <td>48</td>
      <td>357</td>
      <td>48357</td>
      <td>48</td>
      <td>357</td>
      <td>48357</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.021911</td>
      <td>0.236998</td>
      <td>0.352940</td>
      <td>0.343949</td>
      <td>0.374461</td>
      <td>5.172414</td>
      <td>4.0</td>
      <td>4.758392</td>
      <td>9.159159</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>48295</th>
      <td>Lipscomb</td>
      <td>Texas</td>
      <td>48</td>
      <td>295</td>
      <td>48295</td>
      <td>48</td>
      <td>295</td>
      <td>48295</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.031817</td>
      <td>0.286929</td>
      <td>0.378219</td>
      <td>0.407005</td>
      <td>0.373005</td>
      <td>6.724512</td>
      <td>4.5</td>
      <td>3.835360</td>
      <td>6.093580</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>48195</th>
      <td>Hansford</td>
      <td>Texas</td>
      <td>48</td>
      <td>195</td>
      <td>48195</td>
      <td>48</td>
      <td>195</td>
      <td>48195</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.253527</td>
      <td>0.357813</td>
      <td>0.393938</td>
      <td>0.383924</td>
      <td>7.591786</td>
      <td>4.7</td>
      <td>5.542986</td>
      <td>7.125457</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>48393</th>
      <td>Roberts</td>
      <td>Texas</td>
      <td>48</td>
      <td>393</td>
      <td>48393</td>
      <td>48</td>
      <td>393</td>
      <td>48393</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.320275</td>
      <td>0.318656</td>
      <td>0.398681</td>
      <td>0.339626</td>
      <td>5.762712</td>
      <td>5.3</td>
      <td>6.231454</td>
      <td>4.885993</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>48211</th>
      <td>Hemphill</td>
      <td>Texas</td>
      <td>48</td>
      <td>211</td>
      <td>48211</td>
      <td>48</td>
      <td>211</td>
      <td>48211</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.188172</td>
      <td>0.286707</td>
      <td>0.385605</td>
      <td>0.352996</td>
      <td>0.346318</td>
      <td>6.300115</td>
      <td>6.9</td>
      <td>6.451613</td>
      <td>6.330366</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>48233</th>
      <td>Hutchinson</td>
      <td>Texas</td>
      <td>48</td>
      <td>233</td>
      <td>48233</td>
      <td>48</td>
      <td>233</td>
      <td>48233</td>
      <td>1</td>
      <td>1.936915</td>
      <td>...</td>
      <td>2.635369</td>
      <td>0.199287</td>
      <td>0.322579</td>
      <td>0.323864</td>
      <td>0.370207</td>
      <td>5.387316</td>
      <td>5.1</td>
      <td>6.222679</td>
      <td>8.484271</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
  </tbody>
</table>
<p>6 rows × 70 columns</p>
</div>



#### Rook Weights

Rook weights are another type of contiguity weight, but consider observations as neighboring only when they share an edge. The rook neighbors of an observation may be different than its queen neighbors, depending on how the observation and its nearby polygons are configured. 

We can construct this in the same way as the queen weights, using the special `rook_from_shapefile` function:


```python
rW = ps.rook_from_shapefile(shp_path, idVariable='FIPS')
```


```python
rW['48357']
```




    {'48195': 1.0, '48295': 1.0, '48393': 1.0}



These weights function exactly like the Queen weights, and are only distinguished by what they consider "neighbors."


```python
self_and_neighbors = ['48357']
self_and_neighbors.extend(rW.neighbors['48357'])
fips_frame.loc[self_and_neighbors]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NAME</th>
      <th>STATE_NAME</th>
      <th>STATE_FIPS</th>
      <th>CNTY_FIPS</th>
      <th>FIPS</th>
      <th>STFIPS</th>
      <th>COFIPS</th>
      <th>FIPSNO</th>
      <th>SOUTH</th>
      <th>HR60</th>
      <th>...</th>
      <th>BLK90</th>
      <th>GI59</th>
      <th>GI69</th>
      <th>GI79</th>
      <th>GI89</th>
      <th>FH60</th>
      <th>FH70</th>
      <th>FH80</th>
      <th>FH90</th>
      <th>geometry</th>
    </tr>
    <tr>
      <th>FIPS</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>48357</th>
      <td>Ochiltree</td>
      <td>Texas</td>
      <td>48</td>
      <td>357</td>
      <td>48357</td>
      <td>48</td>
      <td>357</td>
      <td>48357</td>
      <td>1</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.021911</td>
      <td>0.236998</td>
      <td>0.352940</td>
      <td>0.343949</td>
      <td>0.374461</td>
      <td>5.172414</td>
      <td>4.0</td>
      <td>4.758392</td>
      <td>9.159159</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>48295</th>
      <td>Lipscomb</td>
      <td>Texas</td>
      <td>48</td>
      <td>295</td>
      <td>48295</td>
      <td>48</td>
      <td>295</td>
      <td>48295</td>
      <td>1</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.031817</td>
      <td>0.286929</td>
      <td>0.378219</td>
      <td>0.407005</td>
      <td>0.373005</td>
      <td>6.724512</td>
      <td>4.5</td>
      <td>3.835360</td>
      <td>6.093580</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>48195</th>
      <td>Hansford</td>
      <td>Texas</td>
      <td>48</td>
      <td>195</td>
      <td>48195</td>
      <td>48</td>
      <td>195</td>
      <td>48195</td>
      <td>1</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.253527</td>
      <td>0.357813</td>
      <td>0.393938</td>
      <td>0.383924</td>
      <td>7.591786</td>
      <td>4.7</td>
      <td>5.542986</td>
      <td>7.125457</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>48393</th>
      <td>Roberts</td>
      <td>Texas</td>
      <td>48</td>
      <td>393</td>
      <td>48393</td>
      <td>48</td>
      <td>393</td>
      <td>48393</td>
      <td>1</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.320275</td>
      <td>0.318656</td>
      <td>0.398681</td>
      <td>0.339626</td>
      <td>5.762712</td>
      <td>5.3</td>
      <td>6.231454</td>
      <td>4.885993</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
  </tbody>
</table>
<p>4 rows × 70 columns</p>
</div>



#### Bishop Weights

In theory, a "Bishop" weighting scheme is one that arises when only polygons that share vertexes are considered to be neighboring. But, since Queen contiguigy requires either an edge or a vertex and Rook contiguity requires only shared edges, the following relationship is true:

$$ \mathcal{Q} = \mathcal{R} \cup \mathcal{B} $$

where $\mathcal{Q}$ is the set of neighbor pairs *via* queen contiguity, $\mathcal{R}$ is the set of neighbor pairs *via* Rook contiguity, and $\mathcal{B}$ *via* Bishop contiguity. Thus:

$$ \mathcal{Q} \setminus \mathcal{R} = \mathcal{B}$$

Bishop weights entail all Queen neighbor pairs that are not also Rook neighbors.

PySAL does not have a dedicated bishop weights constructor, but you can construct very easily using the `w_difference` function. This function is one of a family of tools to work with weights, all defined in `ps.weights`, that conduct these types of set operations between weight objects.


```python
bW = ps.w_difference(qW, rW, constrained=False, silent_island_warning=True) #silence because there will be a lot of warnings
```


```python
bW.histogram
```




    [(0, 161), (1, 48), (2, 33), (3, 8), (4, 4)]



Thus, the vast majority of counties have no bishop neighbors. But, a few do. A simple way to see these observations in the dataframe is to find all elements of the dataframe that are not "islands," the term for an observation with no neighbors:


```python
islands = bW.islands
```


```python
dataframe.query('FIPS not in @islands')
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NAME</th>
      <th>STATE_NAME</th>
      <th>STATE_FIPS</th>
      <th>CNTY_FIPS</th>
      <th>FIPS</th>
      <th>STFIPS</th>
      <th>COFIPS</th>
      <th>FIPSNO</th>
      <th>SOUTH</th>
      <th>HR60</th>
      <th>...</th>
      <th>BLK90</th>
      <th>GI59</th>
      <th>GI69</th>
      <th>GI79</th>
      <th>GI89</th>
      <th>FH60</th>
      <th>FH70</th>
      <th>FH80</th>
      <th>FH90</th>
      <th>geometry</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Lipscomb</td>
      <td>Texas</td>
      <td>48</td>
      <td>295</td>
      <td>48295</td>
      <td>48</td>
      <td>295</td>
      <td>48295</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.031817</td>
      <td>0.286929</td>
      <td>0.378219</td>
      <td>0.407005</td>
      <td>0.373005</td>
      <td>6.724512</td>
      <td>4.5</td>
      <td>3.835360</td>
      <td>6.093580</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Sherman</td>
      <td>Texas</td>
      <td>48</td>
      <td>421</td>
      <td>48421</td>
      <td>48</td>
      <td>421</td>
      <td>48421</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.139958</td>
      <td>0.288976</td>
      <td>0.359377</td>
      <td>0.415453</td>
      <td>0.378041</td>
      <td>5.665722</td>
      <td>1.7</td>
      <td>3.253796</td>
      <td>3.869407</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Dallam</td>
      <td>Texas</td>
      <td>48</td>
      <td>111</td>
      <td>48111</td>
      <td>48</td>
      <td>111</td>
      <td>48111</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>2.050906</td>
      <td>0.331667</td>
      <td>0.385996</td>
      <td>0.370037</td>
      <td>0.376015</td>
      <td>7.546049</td>
      <td>7.2</td>
      <td>9.471366</td>
      <td>14.231738</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hansford</td>
      <td>Texas</td>
      <td>48</td>
      <td>195</td>
      <td>48195</td>
      <td>48</td>
      <td>195</td>
      <td>48195</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.253527</td>
      <td>0.357813</td>
      <td>0.393938</td>
      <td>0.383924</td>
      <td>7.591786</td>
      <td>4.7</td>
      <td>5.542986</td>
      <td>7.125457</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Ochiltree</td>
      <td>Texas</td>
      <td>48</td>
      <td>357</td>
      <td>48357</td>
      <td>48</td>
      <td>357</td>
      <td>48357</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.021911</td>
      <td>0.236998</td>
      <td>0.352940</td>
      <td>0.343949</td>
      <td>0.374461</td>
      <td>5.172414</td>
      <td>4.0</td>
      <td>4.758392</td>
      <td>9.159159</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Roberts</td>
      <td>Texas</td>
      <td>48</td>
      <td>393</td>
      <td>48393</td>
      <td>48</td>
      <td>393</td>
      <td>48393</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.320275</td>
      <td>0.318656</td>
      <td>0.398681</td>
      <td>0.339626</td>
      <td>5.762712</td>
      <td>5.3</td>
      <td>6.231454</td>
      <td>4.885993</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Hemphill</td>
      <td>Texas</td>
      <td>48</td>
      <td>211</td>
      <td>48211</td>
      <td>48</td>
      <td>211</td>
      <td>48211</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.188172</td>
      <td>0.286707</td>
      <td>0.385605</td>
      <td>0.352996</td>
      <td>0.346318</td>
      <td>6.300115</td>
      <td>6.9</td>
      <td>6.451613</td>
      <td>6.330366</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Hutchinson</td>
      <td>Texas</td>
      <td>48</td>
      <td>233</td>
      <td>48233</td>
      <td>48</td>
      <td>233</td>
      <td>48233</td>
      <td>1</td>
      <td>1.936915</td>
      <td>...</td>
      <td>2.635369</td>
      <td>0.199287</td>
      <td>0.322579</td>
      <td>0.323864</td>
      <td>0.370207</td>
      <td>5.387316</td>
      <td>5.1</td>
      <td>6.222679</td>
      <td>8.484271</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Hartley</td>
      <td>Texas</td>
      <td>48</td>
      <td>205</td>
      <td>48205</td>
      <td>48</td>
      <td>205</td>
      <td>48205</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.247661</td>
      <td>0.272450</td>
      <td>0.397082</td>
      <td>0.424883</td>
      <td>0.356978</td>
      <td>6.164384</td>
      <td>2.3</td>
      <td>4.411765</td>
      <td>3.073967</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Moore</td>
      <td>Texas</td>
      <td>48</td>
      <td>341</td>
      <td>48341</td>
      <td>48</td>
      <td>341</td>
      <td>48341</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.531766</td>
      <td>0.189861</td>
      <td>0.336510</td>
      <td>0.327273</td>
      <td>0.357514</td>
      <td>5.195843</td>
      <td>4.0</td>
      <td>5.148473</td>
      <td>8.786952</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Potter</td>
      <td>Texas</td>
      <td>48</td>
      <td>375</td>
      <td>48375</td>
      <td>48</td>
      <td>375</td>
      <td>48375</td>
      <td>1</td>
      <td>5.479610</td>
      <td>...</td>
      <td>8.861393</td>
      <td>0.243162</td>
      <td>0.346547</td>
      <td>0.364754</td>
      <td>0.405154</td>
      <td>9.375662</td>
      <td>10.7</td>
      <td>12.991039</td>
      <td>19.264843</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Carson</td>
      <td>Texas</td>
      <td>48</td>
      <td>065</td>
      <td>48065</td>
      <td>48</td>
      <td>65</td>
      <td>48065</td>
      <td>1</td>
      <td>4.283940</td>
      <td>...</td>
      <td>0.167275</td>
      <td>0.260729</td>
      <td>0.359089</td>
      <td>0.349048</td>
      <td>0.347856</td>
      <td>6.394763</td>
      <td>6.2</td>
      <td>4.453871</td>
      <td>7.252515</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Gray</td>
      <td>Texas</td>
      <td>48</td>
      <td>179</td>
      <td>48179</td>
      <td>48</td>
      <td>179</td>
      <td>48179</td>
      <td>1</td>
      <td>5.285133</td>
      <td>...</td>
      <td>3.750991</td>
      <td>0.231194</td>
      <td>0.332303</td>
      <td>0.361997</td>
      <td>0.375647</td>
      <td>6.751104</td>
      <td>7.3</td>
      <td>7.759827</td>
      <td>10.283032</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Wheeler</td>
      <td>Texas</td>
      <td>48</td>
      <td>483</td>
      <td>48483</td>
      <td>48</td>
      <td>483</td>
      <td>48483</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>2.619493</td>
      <td>0.312700</td>
      <td>0.411916</td>
      <td>0.422483</td>
      <td>0.369445</td>
      <td>8.687864</td>
      <td>7.2</td>
      <td>7.210179</td>
      <td>10.006064</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Oldham</td>
      <td>Texas</td>
      <td>48</td>
      <td>359</td>
      <td>48359</td>
      <td>48</td>
      <td>359</td>
      <td>48359</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.395083</td>
      <td>0.305945</td>
      <td>0.388915</td>
      <td>0.341502</td>
      <td>0.364881</td>
      <td>6.833713</td>
      <td>5.7</td>
      <td>5.366726</td>
      <td>6.616257</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Randall</td>
      <td>Texas</td>
      <td>48</td>
      <td>381</td>
      <td>48381</td>
      <td>48</td>
      <td>381</td>
      <td>48381</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>1.243407</td>
      <td>0.247431</td>
      <td>0.324107</td>
      <td>0.340954</td>
      <td>0.347242</td>
      <td>5.880373</td>
      <td>6.2</td>
      <td>7.978263</td>
      <td>10.817222</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Armstrong</td>
      <td>Texas</td>
      <td>48</td>
      <td>011</td>
      <td>48011</td>
      <td>48</td>
      <td>11</td>
      <td>48011</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.278896</td>
      <td>0.308473</td>
      <td>0.370382</td>
      <td>0.382613</td>
      <td>8.196721</td>
      <td>3.0</td>
      <td>3.253425</td>
      <td>7.705779</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Collingsworth</td>
      <td>Texas</td>
      <td>48</td>
      <td>087</td>
      <td>48087</td>
      <td>48</td>
      <td>87</td>
      <td>48087</td>
      <td>1</td>
      <td>5.311239</td>
      <td>...</td>
      <td>6.437168</td>
      <td>0.344296</td>
      <td>0.414353</td>
      <td>0.408792</td>
      <td>0.454245</td>
      <td>10.029674</td>
      <td>7.1</td>
      <td>7.824726</td>
      <td>11.681772</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Donley</td>
      <td>Texas</td>
      <td>48</td>
      <td>129</td>
      <td>48129</td>
      <td>48</td>
      <td>129</td>
      <td>48129</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>3.436147</td>
      <td>0.356151</td>
      <td>0.438339</td>
      <td>0.422450</td>
      <td>0.405774</td>
      <td>8.097484</td>
      <td>7.0</td>
      <td>5.764411</td>
      <td>6.792453</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Wilbarger</td>
      <td>Texas</td>
      <td>48</td>
      <td>487</td>
      <td>48487</td>
      <td>48</td>
      <td>487</td>
      <td>48487</td>
      <td>1</td>
      <td>9.390729</td>
      <td>...</td>
      <td>8.921368</td>
      <td>0.302814</td>
      <td>0.410308</td>
      <td>0.426747</td>
      <td>0.373689</td>
      <td>9.480574</td>
      <td>9.3</td>
      <td>9.461924</td>
      <td>11.004667</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba3f...</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Motley</td>
      <td>Texas</td>
      <td>48</td>
      <td>345</td>
      <td>48345</td>
      <td>48</td>
      <td>345</td>
      <td>48345</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>4.438642</td>
      <td>0.339096</td>
      <td>0.470519</td>
      <td>0.448932</td>
      <td>0.393306</td>
      <td>9.335038</td>
      <td>9.5</td>
      <td>4.530201</td>
      <td>9.839817</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba3f...</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Cottle</td>
      <td>Texas</td>
      <td>48</td>
      <td>101</td>
      <td>48101</td>
      <td>48</td>
      <td>101</td>
      <td>48101</td>
      <td>1</td>
      <td>23.769907</td>
      <td>...</td>
      <td>8.856253</td>
      <td>0.378049</td>
      <td>0.471019</td>
      <td>0.471828</td>
      <td>0.439617</td>
      <td>8.548531</td>
      <td>8.5</td>
      <td>5.707763</td>
      <td>11.446741</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba3f...</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Floyd</td>
      <td>Texas</td>
      <td>48</td>
      <td>153</td>
      <td>48153</td>
      <td>48</td>
      <td>153</td>
      <td>48153</td>
      <td>1</td>
      <td>2.694909</td>
      <td>...</td>
      <td>3.766035</td>
      <td>0.351635</td>
      <td>0.413255</td>
      <td>0.403059</td>
      <td>0.412453</td>
      <td>7.333333</td>
      <td>5.3</td>
      <td>5.913371</td>
      <td>5.899957</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba3f...</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Hale</td>
      <td>Texas</td>
      <td>48</td>
      <td>189</td>
      <td>48189</td>
      <td>48</td>
      <td>189</td>
      <td>48189</td>
      <td>1</td>
      <td>7.246771</td>
      <td>...</td>
      <td>5.341640</td>
      <td>0.332412</td>
      <td>0.409569</td>
      <td>0.406499</td>
      <td>0.406477</td>
      <td>7.507144</td>
      <td>6.4</td>
      <td>8.085064</td>
      <td>11.858724</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba3f...</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Lamb</td>
      <td>Texas</td>
      <td>48</td>
      <td>279</td>
      <td>48279</td>
      <td>48</td>
      <td>279</td>
      <td>48279</td>
      <td>1</td>
      <td>4.567044</td>
      <td>...</td>
      <td>5.453822</td>
      <td>0.340662</td>
      <td>0.435044</td>
      <td>0.408710</td>
      <td>0.428881</td>
      <td>7.693704</td>
      <td>6.5</td>
      <td>6.724512</td>
      <td>10.753209</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba3f...</td>
    </tr>
    <tr>
      <th>33</th>
      <td>Bailey</td>
      <td>Texas</td>
      <td>48</td>
      <td>017</td>
      <td>48017</td>
      <td>48</td>
      <td>17</td>
      <td>48017</td>
      <td>1</td>
      <td>7.334067</td>
      <td>...</td>
      <td>1.755379</td>
      <td>0.342457</td>
      <td>0.431008</td>
      <td>0.433146</td>
      <td>0.425405</td>
      <td>5.674378</td>
      <td>5.3</td>
      <td>8.508733</td>
      <td>7.206759</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba3f...</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Wichita</td>
      <td>Texas</td>
      <td>48</td>
      <td>485</td>
      <td>48485</td>
      <td>48</td>
      <td>485</td>
      <td>48485</td>
      <td>1</td>
      <td>4.857198</td>
      <td>...</td>
      <td>9.169132</td>
      <td>0.260984</td>
      <td>0.367212</td>
      <td>0.377672</td>
      <td>0.384929</td>
      <td>8.887872</td>
      <td>10.2</td>
      <td>11.884130</td>
      <td>15.576266</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba3f...</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Red River</td>
      <td>Texas</td>
      <td>48</td>
      <td>387</td>
      <td>48387</td>
      <td>48</td>
      <td>387</td>
      <td>48387</td>
      <td>1</td>
      <td>4.251158</td>
      <td>...</td>
      <td>20.060068</td>
      <td>0.395561</td>
      <td>0.449877</td>
      <td>0.435867</td>
      <td>0.417029</td>
      <td>13.960114</td>
      <td>8.6</td>
      <td>11.627907</td>
      <td>16.951673</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba3f...</td>
    </tr>
    <tr>
      <th>41</th>
      <td>Lamar</td>
      <td>Texas</td>
      <td>48</td>
      <td>277</td>
      <td>48277</td>
      <td>48</td>
      <td>277</td>
      <td>48277</td>
      <td>1</td>
      <td>11.684290</td>
      <td>...</td>
      <td>14.555508</td>
      <td>0.363003</td>
      <td>0.400307</td>
      <td>0.386814</td>
      <td>0.405578</td>
      <td>14.317865</td>
      <td>10.2</td>
      <td>12.753698</td>
      <td>14.797558</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba3f...</td>
    </tr>
    <tr>
      <th>43</th>
      <td>King</td>
      <td>Texas</td>
      <td>48</td>
      <td>269</td>
      <td>48269</td>
      <td>48</td>
      <td>269</td>
      <td>48269</td>
      <td>1</td>
      <td>52.083333</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.355761</td>
      <td>0.328461</td>
      <td>0.396784</td>
      <td>0.315586</td>
      <td>9.142857</td>
      <td>7.6</td>
      <td>3.389831</td>
      <td>0.925926</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba3f...</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>119</th>
      <td>Midland</td>
      <td>Texas</td>
      <td>48</td>
      <td>329</td>
      <td>48329</td>
      <td>48</td>
      <td>329</td>
      <td>48329</td>
      <td>1</td>
      <td>11.321628</td>
      <td>...</td>
      <td>7.767491</td>
      <td>0.270389</td>
      <td>0.357749</td>
      <td>0.378468</td>
      <td>0.391824</td>
      <td>7.898149</td>
      <td>8.8</td>
      <td>8.849243</td>
      <td>12.870669</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba8c...</td>
    </tr>
    <tr>
      <th>126</th>
      <td>Anderson</td>
      <td>Texas</td>
      <td>48</td>
      <td>001</td>
      <td>48001</td>
      <td>48</td>
      <td>1</td>
      <td>48001</td>
      <td>1</td>
      <td>13.019909</td>
      <td>...</td>
      <td>23.202982</td>
      <td>0.340194</td>
      <td>0.401471</td>
      <td>0.393085</td>
      <td>0.389099</td>
      <td>13.174946</td>
      <td>10.6</td>
      <td>12.236064</td>
      <td>14.820564</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba8c...</td>
    </tr>
    <tr>
      <th>128</th>
      <td>Freestone</td>
      <td>Texas</td>
      <td>48</td>
      <td>161</td>
      <td>48161</td>
      <td>48</td>
      <td>161</td>
      <td>48161</td>
      <td>1</td>
      <td>7.984032</td>
      <td>...</td>
      <td>19.047920</td>
      <td>0.405111</td>
      <td>0.437318</td>
      <td>0.419314</td>
      <td>0.385411</td>
      <td>14.628225</td>
      <td>10.4</td>
      <td>8.377724</td>
      <td>14.456473</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba8c...</td>
    </tr>
    <tr>
      <th>132</th>
      <td>Hudspeth</td>
      <td>Texas</td>
      <td>48</td>
      <td>229</td>
      <td>48229</td>
      <td>48</td>
      <td>229</td>
      <td>48229</td>
      <td>1</td>
      <td>9.971084</td>
      <td>...</td>
      <td>0.514580</td>
      <td>0.312484</td>
      <td>0.373474</td>
      <td>0.440944</td>
      <td>0.476631</td>
      <td>14.115899</td>
      <td>7.7</td>
      <td>8.959538</td>
      <td>11.363636</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba9a...</td>
    </tr>
    <tr>
      <th>140</th>
      <td>Tom Green</td>
      <td>Texas</td>
      <td>48</td>
      <td>451</td>
      <td>48451</td>
      <td>48</td>
      <td>451</td>
      <td>48451</td>
      <td>1</td>
      <td>5.673320</td>
      <td>...</td>
      <td>4.200776</td>
      <td>0.303284</td>
      <td>0.383894</td>
      <td>0.387174</td>
      <td>0.388441</td>
      <td>11.773710</td>
      <td>10.4</td>
      <td>11.413655</td>
      <td>14.124800</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba9a...</td>
    </tr>
    <tr>
      <th>144</th>
      <td>Upton</td>
      <td>Texas</td>
      <td>48</td>
      <td>461</td>
      <td>48461</td>
      <td>48</td>
      <td>461</td>
      <td>48461</td>
      <td>1</td>
      <td>10.685473</td>
      <td>...</td>
      <td>2.113785</td>
      <td>0.202995</td>
      <td>0.346924</td>
      <td>0.398850</td>
      <td>0.363767</td>
      <td>6.484425</td>
      <td>6.6</td>
      <td>5.614321</td>
      <td>7.591623</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba9a...</td>
    </tr>
    <tr>
      <th>145</th>
      <td>Reagan</td>
      <td>Texas</td>
      <td>48</td>
      <td>383</td>
      <td>48383</td>
      <td>48</td>
      <td>383</td>
      <td>48383</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>2.813469</td>
      <td>0.268458</td>
      <td>0.281433</td>
      <td>0.370690</td>
      <td>0.343537</td>
      <td>5.098855</td>
      <td>6.2</td>
      <td>6.384743</td>
      <td>4.332756</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba9a...</td>
    </tr>
    <tr>
      <th>146</th>
      <td>Leon</td>
      <td>Texas</td>
      <td>48</td>
      <td>289</td>
      <td>48289</td>
      <td>48</td>
      <td>289</td>
      <td>48289</td>
      <td>1</td>
      <td>13.398988</td>
      <td>...</td>
      <td>12.751678</td>
      <td>0.395601</td>
      <td>0.431984</td>
      <td>0.431664</td>
      <td>0.399155</td>
      <td>14.813375</td>
      <td>10.0</td>
      <td>8.828895</td>
      <td>10.147300</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba9a...</td>
    </tr>
    <tr>
      <th>149</th>
      <td>Concho</td>
      <td>Texas</td>
      <td>48</td>
      <td>095</td>
      <td>48095</td>
      <td>48</td>
      <td>95</td>
      <td>48095</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.525624</td>
      <td>0.360067</td>
      <td>0.429736</td>
      <td>0.418632</td>
      <td>0.418878</td>
      <td>9.989806</td>
      <td>7.0</td>
      <td>6.765068</td>
      <td>9.259259</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba9a...</td>
    </tr>
    <tr>
      <th>158</th>
      <td>Robertson</td>
      <td>Texas</td>
      <td>48</td>
      <td>395</td>
      <td>48395</td>
      <td>48</td>
      <td>395</td>
      <td>48395</td>
      <td>1</td>
      <td>10.315446</td>
      <td>...</td>
      <td>27.457933</td>
      <td>0.384890</td>
      <td>0.441826</td>
      <td>0.431414</td>
      <td>0.432907</td>
      <td>18.477439</td>
      <td>15.3</td>
      <td>16.568199</td>
      <td>18.991878</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba9a...</td>
    </tr>
    <tr>
      <th>165</th>
      <td>Madison</td>
      <td>Texas</td>
      <td>48</td>
      <td>313</td>
      <td>48313</td>
      <td>48</td>
      <td>313</td>
      <td>48313</td>
      <td>1</td>
      <td>19.756013</td>
      <td>...</td>
      <td>23.556857</td>
      <td>0.394568</td>
      <td>0.427850</td>
      <td>0.410753</td>
      <td>0.431100</td>
      <td>12.794811</td>
      <td>9.8</td>
      <td>10.671410</td>
      <td>16.734694</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba9a...</td>
    </tr>
    <tr>
      <th>166</th>
      <td>Schleicher</td>
      <td>Texas</td>
      <td>48</td>
      <td>413</td>
      <td>48413</td>
      <td>48</td>
      <td>413</td>
      <td>48413</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.903010</td>
      <td>0.300170</td>
      <td>0.387936</td>
      <td>0.419192</td>
      <td>0.419375</td>
      <td>10.155148</td>
      <td>9.8</td>
      <td>7.222914</td>
      <td>8.363636</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96b9cb...</td>
    </tr>
    <tr>
      <th>167</th>
      <td>Menard</td>
      <td>Texas</td>
      <td>48</td>
      <td>327</td>
      <td>48327</td>
      <td>48</td>
      <td>327</td>
      <td>48327</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.310835</td>
      <td>0.396395</td>
      <td>0.436034</td>
      <td>0.388606</td>
      <td>0.450877</td>
      <td>12.323492</td>
      <td>8.3</td>
      <td>11.651917</td>
      <td>9.538951</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96b9cb...</td>
    </tr>
    <tr>
      <th>172</th>
      <td>Brazos</td>
      <td>Texas</td>
      <td>48</td>
      <td>041</td>
      <td>48041</td>
      <td>48</td>
      <td>41</td>
      <td>48041</td>
      <td>1</td>
      <td>5.197312</td>
      <td>...</td>
      <td>11.219248</td>
      <td>0.344508</td>
      <td>0.412487</td>
      <td>0.378731</td>
      <td>0.415012</td>
      <td>11.094050</td>
      <td>9.2</td>
      <td>9.862888</td>
      <td>16.013367</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96b9cb...</td>
    </tr>
    <tr>
      <th>183</th>
      <td>Presidio</td>
      <td>Texas</td>
      <td>48</td>
      <td>377</td>
      <td>48377</td>
      <td>48</td>
      <td>377</td>
      <td>48377</td>
      <td>1</td>
      <td>6.105006</td>
      <td>...</td>
      <td>0.090402</td>
      <td>0.349471</td>
      <td>0.470173</td>
      <td>0.449250</td>
      <td>0.466640</td>
      <td>19.399351</td>
      <td>17.1</td>
      <td>14.664587</td>
      <td>13.944223</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96b9cb...</td>
    </tr>
    <tr>
      <th>191</th>
      <td>Bastrop</td>
      <td>Texas</td>
      <td>48</td>
      <td>021</td>
      <td>48021</td>
      <td>48</td>
      <td>21</td>
      <td>48021</td>
      <td>1</td>
      <td>3.938946</td>
      <td>...</td>
      <td>11.792071</td>
      <td>0.370264</td>
      <td>0.419933</td>
      <td>0.390927</td>
      <td>0.380907</td>
      <td>14.747191</td>
      <td>12.5</td>
      <td>10.559006</td>
      <td>12.281387</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96b9cb...</td>
    </tr>
    <tr>
      <th>200</th>
      <td>Fayette</td>
      <td>Texas</td>
      <td>48</td>
      <td>149</td>
      <td>48149</td>
      <td>48</td>
      <td>149</td>
      <td>48149</td>
      <td>1</td>
      <td>4.905808</td>
      <td>...</td>
      <td>8.390147</td>
      <td>0.384188</td>
      <td>0.447189</td>
      <td>0.400252</td>
      <td>0.399672</td>
      <td>13.089665</td>
      <td>9.2</td>
      <td>8.037521</td>
      <td>9.588563</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96b9cb...</td>
    </tr>
    <tr>
      <th>205</th>
      <td>Caldwell</td>
      <td>Texas</td>
      <td>48</td>
      <td>055</td>
      <td>48055</td>
      <td>48</td>
      <td>55</td>
      <td>48055</td>
      <td>1</td>
      <td>9.677544</td>
      <td>...</td>
      <td>10.704001</td>
      <td>0.380080</td>
      <td>0.412614</td>
      <td>0.410802</td>
      <td>0.413940</td>
      <td>15.229616</td>
      <td>10.5</td>
      <td>12.894034</td>
      <td>17.191502</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96b9f2...</td>
    </tr>
    <tr>
      <th>212</th>
      <td>Gonzales</td>
      <td>Texas</td>
      <td>48</td>
      <td>177</td>
      <td>48177</td>
      <td>48</td>
      <td>177</td>
      <td>48177</td>
      <td>1</td>
      <td>9.339684</td>
      <td>...</td>
      <td>9.973845</td>
      <td>0.401194</td>
      <td>0.426078</td>
      <td>0.419167</td>
      <td>0.426628</td>
      <td>14.986438</td>
      <td>9.6</td>
      <td>10.080000</td>
      <td>13.719512</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96b9f2...</td>
    </tr>
    <tr>
      <th>214</th>
      <td>Medina</td>
      <td>Texas</td>
      <td>48</td>
      <td>325</td>
      <td>48325</td>
      <td>48</td>
      <td>325</td>
      <td>48325</td>
      <td>1</td>
      <td>3.526590</td>
      <td>...</td>
      <td>0.336848</td>
      <td>0.323677</td>
      <td>0.406149</td>
      <td>0.388869</td>
      <td>0.403101</td>
      <td>10.971429</td>
      <td>7.8</td>
      <td>9.844390</td>
      <td>10.355764</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96b9f2...</td>
    </tr>
    <tr>
      <th>217</th>
      <td>Uvalde</td>
      <td>Texas</td>
      <td>48</td>
      <td>463</td>
      <td>48463</td>
      <td>48</td>
      <td>463</td>
      <td>48463</td>
      <td>1</td>
      <td>3.964950</td>
      <td>...</td>
      <td>0.201371</td>
      <td>0.362398</td>
      <td>0.432786</td>
      <td>0.414992</td>
      <td>0.441084</td>
      <td>14.506570</td>
      <td>12.4</td>
      <td>12.584791</td>
      <td>13.314062</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96b9f2...</td>
    </tr>
    <tr>
      <th>218</th>
      <td>Kinney</td>
      <td>Texas</td>
      <td>48</td>
      <td>271</td>
      <td>48271</td>
      <td>48</td>
      <td>271</td>
      <td>48271</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>1.827509</td>
      <td>0.409324</td>
      <td>0.439334</td>
      <td>0.416630</td>
      <td>0.422357</td>
      <td>19.963031</td>
      <td>16.6</td>
      <td>7.046980</td>
      <td>7.525656</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96b9f2...</td>
    </tr>
    <tr>
      <th>224</th>
      <td>Atascosa</td>
      <td>Texas</td>
      <td>48</td>
      <td>013</td>
      <td>48013</td>
      <td>48</td>
      <td>13</td>
      <td>48013</td>
      <td>1</td>
      <td>3.540826</td>
      <td>...</td>
      <td>0.468346</td>
      <td>0.373805</td>
      <td>0.418729</td>
      <td>0.393757</td>
      <td>0.424795</td>
      <td>12.849813</td>
      <td>10.9</td>
      <td>8.591923</td>
      <td>13.925480</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96b9f2...</td>
    </tr>
    <tr>
      <th>228</th>
      <td>Zavala</td>
      <td>Texas</td>
      <td>48</td>
      <td>507</td>
      <td>48507</td>
      <td>48</td>
      <td>507</td>
      <td>48507</td>
      <td>1</td>
      <td>5.250998</td>
      <td>...</td>
      <td>2.433810</td>
      <td>0.401632</td>
      <td>0.417851</td>
      <td>0.399455</td>
      <td>0.450532</td>
      <td>16.550021</td>
      <td>10.9</td>
      <td>15.948435</td>
      <td>22.941822</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96b9f2...</td>
    </tr>
    <tr>
      <th>229</th>
      <td>Frio</td>
      <td>Texas</td>
      <td>48</td>
      <td>163</td>
      <td>48163</td>
      <td>48</td>
      <td>163</td>
      <td>48163</td>
      <td>1</td>
      <td>3.296414</td>
      <td>...</td>
      <td>1.358373</td>
      <td>0.390980</td>
      <td>0.463020</td>
      <td>0.435098</td>
      <td>0.473507</td>
      <td>14.665445</td>
      <td>9.4</td>
      <td>11.842919</td>
      <td>18.330362</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96b9f2...</td>
    </tr>
    <tr>
      <th>230</th>
      <td>Maverick</td>
      <td>Texas</td>
      <td>48</td>
      <td>323</td>
      <td>48323</td>
      <td>48</td>
      <td>323</td>
      <td>48323</td>
      <td>1</td>
      <td>6.892749</td>
      <td>...</td>
      <td>0.087965</td>
      <td>0.393409</td>
      <td>0.446688</td>
      <td>0.409530</td>
      <td>0.464567</td>
      <td>19.538623</td>
      <td>15.0</td>
      <td>14.095941</td>
      <td>16.682785</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96b9f2...</td>
    </tr>
    <tr>
      <th>234</th>
      <td>La Salle</td>
      <td>Texas</td>
      <td>48</td>
      <td>283</td>
      <td>48283</td>
      <td>48</td>
      <td>283</td>
      <td>48283</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>1.008755</td>
      <td>0.421556</td>
      <td>0.482174</td>
      <td>0.489173</td>
      <td>0.492687</td>
      <td>18.167702</td>
      <td>14.1</td>
      <td>13.052937</td>
      <td>20.088626</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96b9f2...</td>
    </tr>
    <tr>
      <th>235</th>
      <td>McMullen</td>
      <td>Texas</td>
      <td>48</td>
      <td>311</td>
      <td>48311</td>
      <td>48</td>
      <td>311</td>
      <td>48311</td>
      <td>1</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.379538</td>
      <td>0.432833</td>
      <td>0.349443</td>
      <td>0.400104</td>
      <td>11.724138</td>
      <td>5.5</td>
      <td>9.442060</td>
      <td>3.017241</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96b9f2...</td>
    </tr>
    <tr>
      <th>239</th>
      <td>Webb</td>
      <td>Texas</td>
      <td>48</td>
      <td>479</td>
      <td>48479</td>
      <td>48</td>
      <td>479</td>
      <td>48479</td>
      <td>1</td>
      <td>2.057899</td>
      <td>...</td>
      <td>0.117083</td>
      <td>0.382594</td>
      <td>0.443082</td>
      <td>0.439100</td>
      <td>0.461075</td>
      <td>20.292824</td>
      <td>15.5</td>
      <td>17.419676</td>
      <td>20.521271</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96b9f3...</td>
    </tr>
    <tr>
      <th>242</th>
      <td>Duval</td>
      <td>Texas</td>
      <td>48</td>
      <td>131</td>
      <td>48131</td>
      <td>48</td>
      <td>131</td>
      <td>48131</td>
      <td>1</td>
      <td>2.487934</td>
      <td>...</td>
      <td>0.092894</td>
      <td>0.370217</td>
      <td>0.427660</td>
      <td>0.421041</td>
      <td>0.458937</td>
      <td>15.829478</td>
      <td>13.2</td>
      <td>12.803677</td>
      <td>20.699881</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96b9f3...</td>
    </tr>
  </tbody>
</table>
<p>93 rows × 70 columns</p>
</div>



## Distance

There are many other kinds of weighting functions in PySAL. Another separate type use a continuous measure of distance to define neighborhoods. 


```python
radius = ps.cg.sphere.RADIUS_EARTH_MILES
radius
```




    3958.755865744055




```python
ps.min_threshold_dist_from_shapefile?
```


```python
threshold = ps.min_threshold_dist_from_shapefile('../data/texas.shp',radius) # now in miles, maximum nearest neighbor distance between the n observations
```


```python
threshold
```




    60.47758554135752



### knn defined weights


```python
knn4_bad = ps.knnW_from_shapefile('../data/texas.shp', k=4) # ignore curvature of the earth
```


```python
knn4_bad.histogram
```




    [(4, 254)]




```python
knn4 = ps.knnW_from_shapefile('../data/texas.shp', k=4, radius=radius)
```


```python
knn4.histogram
```




    [(4, 254)]




```python
knn4[0]
```




    {3: 1.0, 4: 1.0, 5: 1.0, 6: 1.0}




```python
knn4_bad[0]
```




    {4: 1.0, 5: 1.0, 6: 1.0, 13: 1.0}



#### Kernel W

Kernel Weights are continuous distance-based weights that use kernel densities to define the neighbor relationship.
Typically, they estimate a `bandwidth`, which is a parameter governing how far out observations should be considered neighboring. Then, using this bandwidth, they evaluate a continuous kernel function to provide a weight between 0 and 1.

Many different choices of kernel functions are supported, and bandwidths can either be fixed (constant over all units) or adaptive in function of unit density.

For example, if we want to use adaptive bandwidths for the map and weight according to a gaussian kernel:


```python
kernelWa = ps.adaptive_kernelW_from_shapefile('../data/texas.shp', radius=radius)
kernelWa
```




    <pysal.weights.Distance.Kernel at 0x7fb96b8979b0>




```python
dataframe.loc[kernelWa.neighbors[4] + [4]]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NAME</th>
      <th>STATE_NAME</th>
      <th>STATE_FIPS</th>
      <th>CNTY_FIPS</th>
      <th>FIPS</th>
      <th>STFIPS</th>
      <th>COFIPS</th>
      <th>FIPSNO</th>
      <th>SOUTH</th>
      <th>HR60</th>
      <th>...</th>
      <th>BLK90</th>
      <th>GI59</th>
      <th>GI69</th>
      <th>GI79</th>
      <th>GI89</th>
      <th>FH60</th>
      <th>FH70</th>
      <th>FH80</th>
      <th>FH90</th>
      <th>geometry</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>Ochiltree</td>
      <td>Texas</td>
      <td>48</td>
      <td>357</td>
      <td>48357</td>
      <td>48</td>
      <td>357</td>
      <td>48357</td>
      <td>1</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.021911</td>
      <td>0.236998</td>
      <td>0.352940</td>
      <td>0.343949</td>
      <td>0.374461</td>
      <td>5.172414</td>
      <td>4.0</td>
      <td>4.758392</td>
      <td>9.159159</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Roberts</td>
      <td>Texas</td>
      <td>48</td>
      <td>393</td>
      <td>48393</td>
      <td>48</td>
      <td>393</td>
      <td>48393</td>
      <td>1</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.320275</td>
      <td>0.318656</td>
      <td>0.398681</td>
      <td>0.339626</td>
      <td>5.762712</td>
      <td>5.3</td>
      <td>6.231454</td>
      <td>4.885993</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hansford</td>
      <td>Texas</td>
      <td>48</td>
      <td>195</td>
      <td>48195</td>
      <td>48</td>
      <td>195</td>
      <td>48195</td>
      <td>1</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.253527</td>
      <td>0.357813</td>
      <td>0.393938</td>
      <td>0.383924</td>
      <td>7.591786</td>
      <td>4.7</td>
      <td>5.542986</td>
      <td>7.125457</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Ochiltree</td>
      <td>Texas</td>
      <td>48</td>
      <td>357</td>
      <td>48357</td>
      <td>48</td>
      <td>357</td>
      <td>48357</td>
      <td>1</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.021911</td>
      <td>0.236998</td>
      <td>0.352940</td>
      <td>0.343949</td>
      <td>0.374461</td>
      <td>5.172414</td>
      <td>4.0</td>
      <td>4.758392</td>
      <td>9.159159</td>
      <td>&lt;pysal.cg.shapes.Polygon object at 0x7fb96ba50...</td>
    </tr>
  </tbody>
</table>
<p>4 rows × 70 columns</p>
</div>




```python
kernelWa.bandwidth[0:7]
```




    array([[ 30.30546757],
           [ 30.05684855],
           [ 39.14876899],
           [ 29.96302462],
           [ 29.96302462],
           [ 30.21084447],
           [ 30.23619029]])




```python
kernelWa[4]
```




    {3: 9.99999900663795e-08, 4: 1.0, 5: 0.002299013803371608}




```python
kernelWa[2]
```




    {1: 9.99999900663795e-08, 2: 1.0, 8: 0.23409571720488287}



## Distance Thresholds


```python
ps.min_threshold_dist_from_shapefile?
```


```python
# find the largest nearest neighbor distance between centroids
threshold = ps.min_threshold_dist_from_shapefile('../data/texas.shp', radius=radius) # decimal degrees
Wmind0 = ps.threshold_binaryW_from_shapefile('../data/texas.shp', radius=radius, threshold=threshold*.9)
```

    WARNING: there are 2 disconnected observations
    Island ids:  [133, 181]



```python
Wmind0.histogram
```




    [(0, 2),
     (1, 3),
     (2, 5),
     (3, 4),
     (4, 10),
     (5, 26),
     (6, 16),
     (7, 31),
     (8, 70),
     (9, 32),
     (10, 29),
     (11, 12),
     (12, 5),
     (13, 2),
     (14, 5),
     (15, 2)]




```python
Wmind = ps.threshold_binaryW_from_shapefile('../data/texas.shp', radius=radius, threshold=threshold)
```


```python
Wmind.histogram
```




    [(1, 2),
     (2, 3),
     (3, 4),
     (4, 8),
     (5, 5),
     (6, 20),
     (7, 26),
     (8, 9),
     (9, 32),
     (10, 31),
     (11, 37),
     (12, 33),
     (13, 23),
     (14, 6),
     (15, 7),
     (16, 2),
     (17, 4),
     (18, 2)]




```python
centroids = np.array([list(poly.centroid) for poly in dataframe.geometry])
```


```python
centroids[0:10]
```




    array([[-100.27156111,   36.27508641],
           [-101.8930971 ,   36.27325425],
           [-102.59590795,   36.27354996],
           [-101.35351324,   36.27230422],
           [-100.81561379,   36.27317803],
           [-100.81482387,   35.8405153 ],
           [-100.2694824 ,   35.83996075],
           [-101.35420366,   35.8408377 ],
           [-102.59375964,   35.83958662],
           [-101.89248229,   35.84058246]])




```python
Wmind[0]
```




    {3: 1, 4: 1, 5: 1, 6: 1, 13: 1}




```python
knn4[0]
```




    {3: 1.0, 4: 1.0, 5: 1.0, 6: 1.0}



## Visualization


```python
%matplotlib inline
import matplotlib.pyplot as plt
from pylab import figure, scatter, show
```


```python
wq = ps.queen_from_shapefile('../data/texas.shp')
```


```python
wq[0]
```




    {4: 1.0, 5: 1.0, 6: 1.0}




```python
fig = figure(figsize=(9,9))
plt.plot(centroids[:,0], centroids[:,1],'.')
plt.ylim([25,37])
show()
```


![png](03_spatial_weights_files/03_spatial_weights_95_0.png)



```python
wq.neighbors[0]
```




    [4, 5, 6]




```python
from pylab import figure, scatter, show
fig = figure(figsize=(9,9))

plt.plot(centroids[:,0], centroids[:,1],'.')
#plt.plot(s04[:,0], s04[:,1], '-')
plt.ylim([25,37])
for k,neighs in wq.neighbors.items():
    #print(k,neighs)
    origin = centroids[k]
    for neigh in neighs:
        segment = centroids[[k,neigh]]
        plt.plot(segment[:,0], segment[:,1], '-')
plt.title('Queen Neighbor Graph')
show()
```


![png](03_spatial_weights_files/03_spatial_weights_97_0.png)



```python
wr = ps.rook_from_shapefile('../data/texas.shp')
```


```python
fig = figure(figsize=(9,9))

plt.plot(centroids[:,0], centroids[:,1],'.')
#plt.plot(s04[:,0], s04[:,1], '-')
plt.ylim([25,37])
for k,neighs in wr.neighbors.items():
    #print(k,neighs)
    origin = centroids[k]
    for neigh in neighs:
        segment = centroids[[k,neigh]]
        plt.plot(segment[:,0], segment[:,1], '-')
plt.title('Rook Neighbor Graph')
show()
```


![png](03_spatial_weights_files/03_spatial_weights_99_0.png)



```python
fig = figure(figsize=(9,9))
plt.plot(centroids[:,0], centroids[:,1],'.')
#plt.plot(s04[:,0], s04[:,1], '-')
plt.ylim([25,37])
for k,neighs in Wmind.neighbors.items():
    origin = centroids[k]
    for neigh in neighs:
        segment = centroids[[k,neigh]]
        plt.plot(segment[:,0], segment[:,1], '-')
plt.title('Minimum Distance Threshold Neighbor Graph')
show()
```


![png](03_spatial_weights_files/03_spatial_weights_100_0.png)



```python
Wmind.pct_nonzero
```




    3.8378076756153514




```python
wr.pct_nonzero
```




    2.0243040486080974




```python
wq.pct_nonzero
```




    2.263004526009052




```python

```


```python

```