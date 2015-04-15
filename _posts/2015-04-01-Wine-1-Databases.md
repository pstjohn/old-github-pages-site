---
layout: post 
title:  "Wine Club Data Analysis - Part 1: Databases" 
date:  2015-04-01 
comments: true
---
I finally got around to doing the converting necessary to get this wine dataset in workable order. I'll keep updating this post as I get around to doing more analysis. I'm doing this in IPython, you can download the notebook at the bottom of the page.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
plt.rcParams.update({'savefig.dpi' : 100})
sns.set_context("paper")
sns.set_style("darkgrid")
```

The databases are stored as CSV files, which I generated after much bash-scripted from the original excel document. You can download the files here: [wines.csv]({{ site.url }}/notebooks/wines.csv), [scores.csv]({{ site.url }}/notebooks/scores.csv), and [nights.csv]({{ site.url }}/notebooks/nights.csv),

```python
wines = pd.read_csv('wines.csv', index_col=0)
scores = pd.read_csv('scores.csv', index_col=0)
nights = pd.read_csv('nights.csv')
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
The nights database holds information on each individual night, including the varietal chosen and number of wines we drunk.

```python
nights.head()
```
<div style="max-height:1000px;max-width:1500px;overflow:auto;">
<table border="1" class="table-condensed table-striped">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>season</th>
      <th>night</th>
      <th>Date</th>
      <th>Varietal</th>
      <th>numwines</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td> 1</td>
      <td> 1</td>
      <td> 27-Mar-2012</td>
      <td>   Zinfandel</td>
      <td> 4</td>
    </tr>
    <tr>
      <th>1</th>
      <td> 1</td>
      <td> 2</td>
      <td>  1-Apr-2012</td>
      <td>  Chardonnay</td>
      <td> 7</td>
    </tr>
    <tr>
      <th>2</th>
      <td> 1</td>
      <td> 3</td>
      <td>  9-Apr-2012</td>
      <td> Petit Sirah</td>
      <td> 5</td>
    </tr>
    <tr>
      <th>3</th>
      <td> 1</td>
      <td> 4</td>
      <td> 16-Apr-2012</td>
      <td>       Syrah</td>
      <td> 9</td>
    </tr>
    <tr>
      <th>4</th>
      <td> 1</td>
      <td> 5</td>
      <td> 23-Apr-2012</td>
      <td>  Pinot Noir</td>
      <td> 8</td>
    </tr>
  </tbody>
</table>
</div>
## Number of tastings per person
Some of us have been to wine club more frequently (or longer) than others, so its useful to visualize the number of datapoints we have per person.

```python
tastings = scores.groupby('Name').count().Score.copy()
tastings.sort()
plt.figure()
ax = sns.barplot(x = np.arange(len(tastings)), y = tastings.values[::-1])
tl = ax.set_xticklabels(tastings.index[::-1], rotation=90)
```
![png]({{ site.url }}/assets/images/2015-04-01-Wine-1-Databases/2015-04-01-Wine-1-Databases_12_0.png)

## Average score per person
We don't want anyone to have a higher overall weight on a wines averaged score, so we'll next look on how well we each do on sticking to a consistent scoring basis. (Note, ratings from season 1 have already been re-normalized to 0-21). We're also not too interested in people who have only shown up once or twice ("Are you guys playing some sort of game?"), so we'll make the cuttoff at Zac and above (sorry Jeremy)

```python
best_tasters = tastings[tastings >= tastings['Zac']].index.values
pruned_scores = scores[scores.Name.isin(best_tasters)]
order = pruned_scores.groupby('Name').median().Score.argsort()
ax = sns.boxplot(vals=pruned_scores.Score, groupby=pruned_scores.Name, order=order.index[order.values])
tl = plt.setp(ax.get_xticklabels(), rotation=90)
```
![png]({{ site.url }}/assets/images/2015-04-01-Wine-1-Databases/2015-04-01-Wine-1-Databases_14_0.png)

Easy there Luke / Kaylan, we all like wine, but not _that_ much :)
[Download this notebook]({{ site.url }}/notebooks/2015-04-01-Wine-1-Databases.ipynb)
