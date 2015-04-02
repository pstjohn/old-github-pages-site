---
layout: post 
title:  "Wine Club Data Analysis!" 
date:  2015-04-01 
comments: true
---
I finally got around to doing the converting necessary to get this wine dataset in workable order. I'll keep updating this post as I get around to doing more analysis. I'm doing this in IPython, you can download the notebook at the bottom of the page.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
```

The databases are stored as CSV files, which I generated after much bash-scripted from the original excel document. You can download the files here: [wines.csv]({{ site.url }}/notebooks/wines.csv) and [scores.csv]({{ site.url }}/notebooks/scores.csv)

```python
wines = pd.read_csv('wines.csv', index_col=0)
scores = pd.read_csv('scores.csv', index_col=0)
```

The wines database lists each wine, with relevant descriptions

```python
wines.tail()
```
<div style="max-height:1000px;max-width:1500px;overflow:auto;">
<table border="1" class="table-condensed table-striped">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>order</th>
      <th>Wine Name</th>
      <th>Region</th>
      <th>Year</th>
      <th>Price</th>
      <th>Purchaser</th>
      <th>season</th>
      <th>night</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>279</th>
      <td> 4</td>
      <td>                                       Albardiales</td>
      <td>             Spain</td>
      <td> 2013</td>
      <td> 11</td>
      <td> Matt/Allie</td>
      <td> 3</td>
      <td> 16</td>
    </tr>
    <tr>
      <th>280</th>
      <td> 5</td>
      <td> Red Guitar Old Vine Tempranillo-Grenache (52%/...</td>
      <td>            Navara</td>
      <td> 2011</td>
      <td>NaN</td>
      <td> NateY/Jack</td>
      <td> 3</td>
      <td> 16</td>
    </tr>
    <tr>
      <th>281</th>
      <td> 6</td>
      <td>                         Tolosa Seasonal Selection</td>
      <td>       Paso Robles</td>
      <td> 2012</td>
      <td> 20</td>
      <td>  Erin/Evan</td>
      <td> 3</td>
      <td> 16</td>
    </tr>
    <tr>
      <th>282</th>
      <td> 7</td>
      <td>                            Hazana Tradicion Rioja</td>
      <td>               NaN</td>
      <td> 2011</td>
      <td> 13</td>
      <td>      NateE</td>
      <td> 3</td>
      <td> 16</td>
    </tr>
    <tr>
      <th>283</th>
      <td> 8</td>
      <td>                                   Milflores Rioja</td>
      <td> Laguardia, Espana</td>
      <td>  NaN</td>
      <td>  8</td>
      <td>      NateK</td>
      <td> 3</td>
      <td> 16</td>
    </tr>
  </tbody>
</table>
</div>
Similarly, the scores database contains information on each tasting

```python
scores.tail()
```
<div style="max-height:1000px;max-width:1500px;overflow:auto;">
<table border="1" class="table-condensed table-striped">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Score</th>
      <th>wine</th>
      <th>season</th>
      <th>night</th>
      <th>order</th>
      <th>Nose</th>
      <th>Flavor</th>
      <th>Finish</th>
      <th>Overall</th>
      <th>Comments</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3191</th>
      <td>  Allie</td>
      <td> 10</td>
      <td> 283</td>
      <td> 3</td>
      <td> 16</td>
      <td> 8</td>
      <td>  3</td>
      <td>  2</td>
      <td>  2</td>
      <td>  3</td>
      <td>                            floral, tannic, hot</td>
    </tr>
    <tr>
      <th>3192</th>
      <td>  Grace</td>
      <td>  7</td>
      <td> 283</td>
      <td> 3</td>
      <td> 16</td>
      <td> 8</td>
      <td>  2</td>
      <td>  2</td>
      <td>  1</td>
      <td>  2</td>
      <td>                                   burns, harsh</td>
    </tr>
    <tr>
      <th>3193</th>
      <td>  Davey</td>
      <td> 10</td>
      <td> 283</td>
      <td> 3</td>
      <td> 16</td>
      <td> 8</td>
      <td>  1</td>
      <td>  2</td>
      <td>  2</td>
      <td>  5</td>
      <td> tinge to it around the edges; strawberry muted</td>
    </tr>
    <tr>
      <th>3194</th>
      <td>   Luke</td>
      <td> 13</td>
      <td> 283</td>
      <td> 3</td>
      <td> 16</td>
      <td> 8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>                                            NaN</td>
    </tr>
    <tr>
      <th>3195</th>
      <td> Kaylan</td>
      <td> 13</td>
      <td> 283</td>
      <td> 3</td>
      <td> 16</td>
      <td> 8</td>
      <td>  4</td>
      <td>  2</td>
      <td>  2</td>
      <td>  5</td>
      <td>                     fruity nose, bitter finish</td>
    </tr>
  </tbody>
</table>
</div>
Average score by tasting order
==============
In a perfect world, the order in which we taste the wines would be completely random, and we wouldn't expect any variation based on position tasted. This looks like its more or less the case, but position 6 does seem to have an advantage

```python
sns.boxplot(vals=scores.Score, groupby=scores.order)
```
![png]({{ site.url }}/assets/images/2015-04-01-wine-analysis/2015-04-01-wine-analysis_10_1.png)

[Download this notebook]({{ site.url }}/notebooks/2015-04-01-wine-analysis.ipynb)
