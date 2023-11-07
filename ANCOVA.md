# Load the dataset


```python
import pandas
from scipy import stats
import numpy
import patsy

from plotly import express
from plotly import graph_objects
from plotly import subplots
from plotly import io
io.templates.default = 'plotly_white'
#io.templates.default = 'plotly_dark'

from statsmodels.formula.api import ols
from statsmodels.stats.anova import anova_lm

import matplotlib.pyplot as plt
from pingouin import ancova

import warnings
warnings.simplefilter(action='ignore', category=FutureWarning)

#df = pandas.read_csv("data60.csv")
#df = pandas.read_csv("data59.csv")
df = pandas.read_csv("data58.csv")
df[:5]

# State levels of treatment
df.VLEs = pandas.Categorical(df.VLEs, ordered=True, categories=['VLE1', 'VLE2', 'VLE3'])

# Verify data type
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 58 entries, 0 to 57
    Data columns (total 3 columns):
     #   Column    Non-Null Count  Dtype   
    ---  ------    --------------  -----   
     0   VLEs      58 non-null     category
     1   pretest   58 non-null     float64 
     2   posttest  58 non-null     float64 
    dtypes: category(1), float64(2)
    memory usage: 1.2 KB


# Exploratory Analysis

## Posttest per VLEs


```python
# Absolute counts
df.VLEs.value_counts()
```




    VLEs
    VLE1    20
    VLE2    19
    VLE3    19
    Name: count, dtype: int64




```python
# Comparitive summary statistics of dependent variable (posttest) given each treatment level (VLEs)
df.groupby('VLEs').posttest.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
    <tr>
      <th>VLEs</th>
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
      <th>VLE1</th>
      <td>20.0</td>
      <td>57.612000</td>
      <td>13.309711</td>
      <td>40.00</td>
      <td>47.6375</td>
      <td>55.28</td>
      <td>65.4175</td>
      <td>90.56</td>
    </tr>
    <tr>
      <th>VLE2</th>
      <td>19.0</td>
      <td>56.490526</td>
      <td>20.769682</td>
      <td>23.33</td>
      <td>48.8900</td>
      <td>54.44</td>
      <td>58.8900</td>
      <td>98.33</td>
    </tr>
    <tr>
      <th>VLE3</th>
      <td>19.0</td>
      <td>77.456842</td>
      <td>13.539188</td>
      <td>57.22</td>
      <td>65.5600</td>
      <td>77.78</td>
      <td>90.0000</td>
      <td>98.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
express.box(df, x = 'VLEs', y = 'posttest', color = 'VLEs', title = 'Posttest per VLEs', labels = {'posttest':'posttest'})
#fig = express.box(df, x = 'VLEs', y = 'posttest', color = 'VLEs', title = 'Posttest per VLEs', labels = {'posttest':'posttest'})
#fig.write_html('boxplotPRE60.html', auto_open=True)
#fig.write_image("boxplotPOST60.png")
```


        <script type="text/javascript">
        window.PlotlyConfig = {MathJaxConfig: 'local'};
        if (window.MathJax && window.MathJax.Hub && window.MathJax.Hub.Config) {window.MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}
        if (typeof require !== 'undefined') {
        require.undef("plotly");
        define('plotly', function(require, exports, module) {
            /**
* plotly.js v2.27.0
* Copyright 2012-2023, Plotly, Inc.
* All rights reserved.
* Licensed under the MIT license
*/
/*! For license information please see plotly.min.js.LICENSE.txt */
        });
        require(['plotly'], function(Plotly) {
            window._Plotly = Plotly;
        });
        }
        </script>




<div>                            <div id="0fa5eb4a-804a-463a-b018-7a80885c984b" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("0fa5eb4a-804a-463a-b018-7a80885c984b")) {                    Plotly.newPlot(                        "0fa5eb4a-804a-463a-b018-7a80885c984b",                        [{"alignmentgroup":"True","hovertemplate":"VLEs=%{x}\u003cbr\u003eposttest=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"VLE1","marker":{"color":"#636efa"},"name":"VLE1","notched":false,"offsetgroup":"VLE1","orientation":"v","showlegend":true,"x":["VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1"],"x0":" ","xaxis":"x","y":[48.33,90.56,55.0,43.33,51.67,50.56,65.0,76.67,41.67,70.0,61.67,55.56,43.33,45.56,53.33,60.56,66.67,74.44,40.0,58.33],"y0":" ","yaxis":"y","type":"box"},{"alignmentgroup":"True","hovertemplate":"VLEs=%{x}\u003cbr\u003eposttest=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"VLE2","marker":{"color":"#EF553B"},"name":"VLE2","notched":false,"offsetgroup":"VLE2","orientation":"v","showlegend":true,"x":["VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2"],"x0":" ","xaxis":"x","y":[23.33,56.67,60.0,52.22,41.11,93.33,57.78,95.0,55.0,56.67,52.22,53.33,48.89,67.78,98.33,25.0,48.89,54.44,33.33],"y0":" ","yaxis":"y","type":"box"},{"alignmentgroup":"True","hovertemplate":"VLEs=%{x}\u003cbr\u003eposttest=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"VLE3","marker":{"color":"#00cc96"},"name":"VLE3","notched":false,"offsetgroup":"VLE3","orientation":"v","showlegend":true,"x":["VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3"],"x0":" ","xaxis":"x","y":[91.67,98.33,86.11,88.33,81.67,67.78,57.22,77.22,91.67,81.67,95.0,65.56,62.78,74.44,77.78,58.33,91.67,65.56,58.89],"y0":" ","yaxis":"y","type":"box"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"#C8D4E3","linecolor":"#C8D4E3","minorgridcolor":"#C8D4E3","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"#C8D4E3","linecolor":"#C8D4E3","minorgridcolor":"#C8D4E3","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"white","polar":{"bgcolor":"white","angularaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":""},"radialaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":""}},"ternary":{"bgcolor":"white","aaxis":{"gridcolor":"#DFE8F3","linecolor":"#A2B1C6","ticks":""},"baxis":{"gridcolor":"#DFE8F3","linecolor":"#A2B1C6","ticks":""},"caxis":{"gridcolor":"#DFE8F3","linecolor":"#A2B1C6","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":"","title":{"standoff":15},"zerolinecolor":"#EBF0F8","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":"","title":{"standoff":15},"zerolinecolor":"#EBF0F8","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"white","gridcolor":"#DFE8F3","linecolor":"#EBF0F8","showbackground":true,"ticks":"","zerolinecolor":"#EBF0F8","gridwidth":2},"yaxis":{"backgroundcolor":"white","gridcolor":"#DFE8F3","linecolor":"#EBF0F8","showbackground":true,"ticks":"","zerolinecolor":"#EBF0F8","gridwidth":2},"zaxis":{"backgroundcolor":"white","gridcolor":"#DFE8F3","linecolor":"#EBF0F8","showbackground":true,"ticks":"","zerolinecolor":"#EBF0F8","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"white","subunitcolor":"#C8D4E3","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"VLEs"},"categoryorder":"array","categoryarray":["VLE1","VLE2","VLE3"]},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"posttest"}},"legend":{"title":{"text":"VLEs"},"tracegroupgap":0},"title":{"text":"Posttest per VLEs"},"boxmode":"overlay"},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('0fa5eb4a-804a-463a-b018-7a80885c984b');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


## Pretest per VLEs


```python
# Comparitive summary statistics of covariate (pretest) per treatment level (VLEs)
df.groupby('VLEs').pretest.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
    <tr>
      <th>VLEs</th>
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
      <th>VLE1</th>
      <td>20.0</td>
      <td>44.472000</td>
      <td>9.815169</td>
      <td>27.78</td>
      <td>37.780</td>
      <td>42.22</td>
      <td>52.085</td>
      <td>63.89</td>
    </tr>
    <tr>
      <th>VLE2</th>
      <td>19.0</td>
      <td>54.210000</td>
      <td>22.366567</td>
      <td>27.78</td>
      <td>37.780</td>
      <td>48.33</td>
      <td>61.670</td>
      <td>95.00</td>
    </tr>
    <tr>
      <th>VLE3</th>
      <td>19.0</td>
      <td>70.467368</td>
      <td>17.751129</td>
      <td>36.11</td>
      <td>58.335</td>
      <td>80.00</td>
      <td>86.110</td>
      <td>91.67</td>
    </tr>
  </tbody>
</table>
</div>




```python
express.box(df, x = 'VLEs', y = 'pretest', color = 'VLEs', title = 'Pretest per VLEs', labels = {'pretest':'pretest'})

#fig = express.box(df, x = 'VLEs', y = 'pretest', color = 'VLEs', title = 'Pretest per VLEs', labels = {'pretest':'pretest'})
#fig.write_html('boxplotPRE60.html', auto_open=True)
#fig.write_image("boxplotPRE60.png")
```


<div>                            <div id="72787136-15c1-45a7-8e7a-0569c6bf2dcf" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("72787136-15c1-45a7-8e7a-0569c6bf2dcf")) {                    Plotly.newPlot(                        "72787136-15c1-45a7-8e7a-0569c6bf2dcf",                        [{"alignmentgroup":"True","hovertemplate":"VLEs=%{x}\u003cbr\u003epretest=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"VLE1","marker":{"color":"#636efa"},"name":"VLE1","notched":false,"offsetgroup":"VLE1","orientation":"v","showlegend":true,"x":["VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1"],"x0":" ","xaxis":"x","y":[43.33,48.33,51.67,50.0,38.33,40.0,63.89,56.67,35.0,40.0,58.33,30.0,41.11,37.78,35.0,53.33,53.33,47.78,27.78,37.78],"y0":" ","yaxis":"y","type":"box"},{"alignmentgroup":"True","hovertemplate":"VLEs=%{x}\u003cbr\u003epretest=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"VLE2","marker":{"color":"#EF553B"},"name":"VLE2","notched":false,"offsetgroup":"VLE2","orientation":"v","showlegend":true,"x":["VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2"],"x0":" ","xaxis":"x","y":[34.44,90.0,27.78,47.78,30.0,89.44,54.44,95.0,66.67,36.67,56.67,38.89,43.33,41.11,91.67,28.33,54.44,55.0,48.33],"y0":" ","yaxis":"y","type":"box"},{"alignmentgroup":"True","hovertemplate":"VLEs=%{x}\u003cbr\u003epretest=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"VLE3","marker":{"color":"#00cc96"},"name":"VLE3","notched":false,"offsetgroup":"VLE3","orientation":"v","showlegend":true,"x":["VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3"],"x0":" ","xaxis":"x","y":[88.33,91.67,86.11,80.0,86.11,36.11,57.78,83.33,86.67,80.0,85.0,59.44,48.33,66.11,56.67,40.0,87.22,61.11,58.89],"y0":" ","yaxis":"y","type":"box"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"#C8D4E3","linecolor":"#C8D4E3","minorgridcolor":"#C8D4E3","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"#C8D4E3","linecolor":"#C8D4E3","minorgridcolor":"#C8D4E3","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"white","polar":{"bgcolor":"white","angularaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":""},"radialaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":""}},"ternary":{"bgcolor":"white","aaxis":{"gridcolor":"#DFE8F3","linecolor":"#A2B1C6","ticks":""},"baxis":{"gridcolor":"#DFE8F3","linecolor":"#A2B1C6","ticks":""},"caxis":{"gridcolor":"#DFE8F3","linecolor":"#A2B1C6","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":"","title":{"standoff":15},"zerolinecolor":"#EBF0F8","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":"","title":{"standoff":15},"zerolinecolor":"#EBF0F8","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"white","gridcolor":"#DFE8F3","linecolor":"#EBF0F8","showbackground":true,"ticks":"","zerolinecolor":"#EBF0F8","gridwidth":2},"yaxis":{"backgroundcolor":"white","gridcolor":"#DFE8F3","linecolor":"#EBF0F8","showbackground":true,"ticks":"","zerolinecolor":"#EBF0F8","gridwidth":2},"zaxis":{"backgroundcolor":"white","gridcolor":"#DFE8F3","linecolor":"#EBF0F8","showbackground":true,"ticks":"","zerolinecolor":"#EBF0F8","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"white","subunitcolor":"#C8D4E3","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"VLEs"},"categoryorder":"array","categoryarray":["VLE1","VLE2","VLE3"]},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"pretest"}},"legend":{"title":{"text":"VLEs"},"tracegroupgap":0},"title":{"text":"Pretest per VLEs"},"boxmode":"overlay"},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('72787136-15c1-45a7-8e7a-0569c6bf2dcf');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


## Estimated Posttest given Pretest per VLEs


```python
express.scatter(df, x = 'pretest', y = 'posttest', color = 'VLEs', trendline = 'ols',
    title = 'Posttest given Pretest per VLEs', labels = {
        'pretest':'pretest','posttest':'posttest'})

# fig = express.scatter(df, x = 'pretest', y = 'posttest', color = 'VLEs', trendline = 'ols',
#                  title = 'Posttest given Pretest per VLEs', labels = {
#                      'pretest':'pretest','posttest':'posttest'})
#fig.write_html('scatterPOSTPRE60.html', auto_open=True)
#fig.write_image("scatterPOSTPRE60.png")
```


<div>                            <div id="3fb534fd-f617-4091-84fe-191cdd3ed4c5" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("3fb534fd-f617-4091-84fe-191cdd3ed4c5")) {                    Plotly.newPlot(                        "3fb534fd-f617-4091-84fe-191cdd3ed4c5",                        [{"hovertemplate":"VLEs=VLE1\u003cbr\u003epretest=%{x}\u003cbr\u003eposttest=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"VLE1","marker":{"color":"#636efa","symbol":"circle"},"mode":"markers","name":"VLE1","orientation":"v","showlegend":true,"x":[43.33,48.33,51.67,50.0,38.33,40.0,63.89,56.67,35.0,40.0,58.33,30.0,41.11,37.78,35.0,53.33,53.33,47.78,27.78,37.78],"xaxis":"x","y":[48.33,90.56,55.0,43.33,51.67,50.56,65.0,76.67,41.67,70.0,61.67,55.56,43.33,45.56,53.33,60.56,66.67,74.44,40.0,58.33],"yaxis":"y","type":"scatter"},{"hovertemplate":"\u003cb\u003eOLS trendline\u003c\u002fb\u003e\u003cbr\u003eposttest = 0.702506 * pretest + 26.3701\u003cbr\u003eR\u003csup\u003e2\u003c\u002fsup\u003e=0.268385\u003cbr\u003e\u003cbr\u003eVLEs=VLE1\u003cbr\u003epretest=%{x}\u003cbr\u003eposttest=%{y} \u003cb\u003e(trend)\u003c\u002fb\u003e\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"VLE1","marker":{"color":"#636efa","symbol":"circle"},"mode":"lines","name":"VLE1","showlegend":false,"x":[27.78,30.0,35.0,35.0,37.78,37.78,38.33,40.0,40.0,41.11,43.33,47.78,48.33,50.0,51.67,53.33,53.33,56.67,58.33,63.89],"xaxis":"x","y":[45.885762732976616,47.44532699925938,50.95785913052687,50.95785913052687,52.9108269955116,52.9108269955116,53.297205529951015,54.47039126179436,54.47039126179436,55.25017339493574,56.809737661218506,59.93589125804658,60.322269792486,61.495455524329344,62.66864125617269,63.83480192375349,63.83480192375349,66.18117338744017,67.34733405502098,71.25326978499044],"yaxis":"y","type":"scatter"},{"hovertemplate":"VLEs=VLE2\u003cbr\u003epretest=%{x}\u003cbr\u003eposttest=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"VLE2","marker":{"color":"#EF553B","symbol":"circle"},"mode":"markers","name":"VLE2","orientation":"v","showlegend":true,"x":[34.44,90.0,27.78,47.78,30.0,89.44,54.44,95.0,66.67,36.67,56.67,38.89,43.33,41.11,91.67,28.33,54.44,55.0,48.33],"xaxis":"x","y":[23.33,56.67,60.0,52.22,41.11,93.33,57.78,95.0,55.0,56.67,52.22,53.33,48.89,67.78,98.33,25.0,48.89,54.44,33.33],"yaxis":"y","type":"scatter"},{"hovertemplate":"\u003cb\u003eOLS trendline\u003c\u002fb\u003e\u003cbr\u003eposttest = 0.700089 * pretest + 18.5387\u003cbr\u003eR\u003csup\u003e2\u003c\u002fsup\u003e=0.568389\u003cbr\u003e\u003cbr\u003eVLEs=VLE2\u003cbr\u003epretest=%{x}\u003cbr\u003eposttest=%{y} \u003cb\u003e(trend)\u003c\u002fb\u003e\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"VLE2","marker":{"color":"#EF553B","symbol":"circle"},"mode":"lines","name":"VLE2","showlegend":false,"x":[27.78,28.33,30.0,34.44,36.67,38.89,41.11,43.33,47.78,48.33,54.44,54.44,55.0,56.67,66.67,89.44,90.0,91.67,95.0],"xaxis":"x","y":[37.987174579518324,38.37222351841159,39.54137211468752,42.64976718502592,44.21096560999318,45.76516314516238,47.319360680331584,48.87355821550078,51.98895417563725,52.37400311453052,56.651546781144845,56.651546781144845,57.04359660983618,58.21274520611211,65.21363500417158,81.15466107435296,81.5467109030443,82.71585949932023,85.04715580207403],"yaxis":"y","type":"scatter"},{"hovertemplate":"VLEs=VLE3\u003cbr\u003epretest=%{x}\u003cbr\u003eposttest=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"VLE3","marker":{"color":"#00cc96","symbol":"circle"},"mode":"markers","name":"VLE3","orientation":"v","showlegend":true,"x":[88.33,91.67,86.11,80.0,86.11,36.11,57.78,83.33,86.67,80.0,85.0,59.44,48.33,66.11,56.67,40.0,87.22,61.11,58.89],"xaxis":"x","y":[91.67,98.33,86.11,88.33,81.67,67.78,57.22,77.22,91.67,81.67,95.0,65.56,62.78,74.44,77.78,58.33,91.67,65.56,58.89],"yaxis":"y","type":"scatter"},{"hovertemplate":"\u003cb\u003eOLS trendline\u003c\u002fb\u003e\u003cbr\u003eposttest = 0.656727 * pretest + 31.179\u003cbr\u003eR\u003csup\u003e2\u003c\u002fsup\u003e=0.741373\u003cbr\u003e\u003cbr\u003eVLEs=VLE3\u003cbr\u003epretest=%{x}\u003cbr\u003eposttest=%{y} \u003cb\u003e(trend)\u003c\u002fb\u003e\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"VLE3","marker":{"color":"#00cc96","symbol":"circle"},"mode":"lines","name":"VLE3","showlegend":false,"x":[36.11,40.0,48.33,56.67,57.78,58.89,59.44,61.11,66.11,80.0,80.0,83.33,85.0,86.11,86.11,86.67,87.22,88.33,91.67],"xaxis":"x","y":[54.8934213858291,57.44809046068223,62.918628608118354,68.3957340282405,69.12470129638623,69.85366856453199,70.21486856226187,71.31160310082349,74.59523944382236,83.71718120467324,83.71718120467324,85.9040830091105,87.00081754767211,87.72978481581788,87.72978481581788,88.09755208623375,88.45875208396362,89.18771935210938,91.38118842923262],"yaxis":"y","type":"scatter"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"#C8D4E3","linecolor":"#C8D4E3","minorgridcolor":"#C8D4E3","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"#C8D4E3","linecolor":"#C8D4E3","minorgridcolor":"#C8D4E3","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"white","polar":{"bgcolor":"white","angularaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":""},"radialaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":""}},"ternary":{"bgcolor":"white","aaxis":{"gridcolor":"#DFE8F3","linecolor":"#A2B1C6","ticks":""},"baxis":{"gridcolor":"#DFE8F3","linecolor":"#A2B1C6","ticks":""},"caxis":{"gridcolor":"#DFE8F3","linecolor":"#A2B1C6","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":"","title":{"standoff":15},"zerolinecolor":"#EBF0F8","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":"","title":{"standoff":15},"zerolinecolor":"#EBF0F8","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"white","gridcolor":"#DFE8F3","linecolor":"#EBF0F8","showbackground":true,"ticks":"","zerolinecolor":"#EBF0F8","gridwidth":2},"yaxis":{"backgroundcolor":"white","gridcolor":"#DFE8F3","linecolor":"#EBF0F8","showbackground":true,"ticks":"","zerolinecolor":"#EBF0F8","gridwidth":2},"zaxis":{"backgroundcolor":"white","gridcolor":"#DFE8F3","linecolor":"#EBF0F8","showbackground":true,"ticks":"","zerolinecolor":"#EBF0F8","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"white","subunitcolor":"#C8D4E3","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"pretest"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"posttest"}},"legend":{"title":{"text":"VLEs"},"tracegroupgap":0},"title":{"text":"Posttest given Pretest per VLEs"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('3fb534fd-f617-4091-84fe-191cdd3ed4c5');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


# Assumption for the use of ANCOVA

## Linearity


```python
express.scatter(df, x = 'pretest', y = 'posttest', trendline = 'ols',
    title = 'Posttest given Pretest', labels = {
        'pretest':'pretest','posttest':'posttest'})

# fig = express.scatter(df, x = 'pretest', y = 'posttest', trendline = 'ols',
#                  title = 'Posttest given Pretest', labels = {
#                      'pretest':'pretest','posttest':'posttest'})
#fig.write_html('linearityPOSTPRE60.html', auto_open=True)
#fig.write_image("linearityPOSTPRE60.png")
```


<div>                            <div id="5d88cb15-7341-4964-b107-fff3a990979a" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("5d88cb15-7341-4964-b107-fff3a990979a")) {                    Plotly.newPlot(                        "5d88cb15-7341-4964-b107-fff3a990979a",                        [{"hovertemplate":"pretest=%{x}\u003cbr\u003eposttest=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"","marker":{"color":"#636efa","symbol":"circle"},"mode":"markers","name":"","orientation":"v","showlegend":false,"x":[43.33,48.33,51.67,50.0,38.33,40.0,63.89,56.67,35.0,40.0,58.33,30.0,41.11,37.78,35.0,53.33,53.33,47.78,27.78,37.78,34.44,90.0,27.78,47.78,30.0,89.44,54.44,95.0,66.67,36.67,56.67,38.89,43.33,41.11,91.67,28.33,54.44,55.0,48.33,88.33,91.67,86.11,80.0,86.11,36.11,57.78,83.33,86.67,80.0,85.0,59.44,48.33,66.11,56.67,40.0,87.22,61.11,58.89],"xaxis":"x","y":[48.33,90.56,55.0,43.33,51.67,50.56,65.0,76.67,41.67,70.0,61.67,55.56,43.33,45.56,53.33,60.56,66.67,74.44,40.0,58.33,23.33,56.67,60.0,52.22,41.11,93.33,57.78,95.0,55.0,56.67,52.22,53.33,48.89,67.78,98.33,25.0,48.89,54.44,33.33,91.67,98.33,86.11,88.33,81.67,67.78,57.22,77.22,91.67,81.67,95.0,65.56,62.78,74.44,77.78,58.33,91.67,65.56,58.89],"yaxis":"y","type":"scatter"},{"hovertemplate":"\u003cb\u003eOLS trendline\u003c\u002fb\u003e\u003cbr\u003eposttest = 0.721732 * pretest + 23.2002\u003cbr\u003eR\u003csup\u003e2\u003c\u002fsup\u003e=0.611763\u003cbr\u003e\u003cbr\u003epretest=%{x}\u003cbr\u003eposttest=%{y} \u003cb\u003e(trend)\u003c\u002fb\u003e\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"","marker":{"color":"#636efa","symbol":"circle"},"mode":"lines","name":"","showlegend":false,"x":[27.78,27.78,28.33,30.0,30.0,34.44,35.0,35.0,36.11,36.67,37.78,37.78,38.33,38.89,40.0,40.0,40.0,41.11,41.11,43.33,43.33,47.78,47.78,48.33,48.33,48.33,50.0,51.67,53.33,53.33,54.44,54.44,55.0,56.67,56.67,56.67,57.78,58.33,58.89,59.44,61.11,63.89,66.11,66.67,80.0,80.0,83.33,85.0,86.11,86.11,86.67,87.22,88.33,89.44,90.0,91.67,91.67,95.0],"xaxis":"x","y":[43.249948983206956,43.249948983206956,43.64690152768637,44.852193799105656,44.852193799105656,48.056683430903064,48.460853294373,48.460853294373,49.26197570232235,49.6661455657923,50.46726797374165,50.46726797374165,50.86422051822105,51.268390381691,52.06951278964035,52.06951278964035,52.06951278964035,52.8706351975897,52.8706351975897,54.4728800134884,54.4728800134884,57.68458696427634,57.68458696427634,58.081539508755746,58.081539508755746,58.081539508755746,59.28683178017504,60.49212405159434,61.69019900402309,61.69019900402309,62.49132141197244,62.49132141197244,62.89549127544239,64.10078354686168,64.10078354686168,64.10078354686168,64.90190595481103,65.29885849929045,65.70302836276038,66.0999809072398,67.30527317865909,69.31168785802772,70.91393267392642,71.31810253739637,80.93878875177911,80.93878875177911,83.34215597562716,84.54744824704646,85.34857065499581,85.34857065499581,85.75274051846576,86.14969306294516,86.95081547089451,87.75193787884386,88.15610774231381,89.3614000137331,89.3614000137331,91.76476723758115],"yaxis":"y","type":"scatter"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"#C8D4E3","linecolor":"#C8D4E3","minorgridcolor":"#C8D4E3","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"#C8D4E3","linecolor":"#C8D4E3","minorgridcolor":"#C8D4E3","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"white","polar":{"bgcolor":"white","angularaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":""},"radialaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":""}},"ternary":{"bgcolor":"white","aaxis":{"gridcolor":"#DFE8F3","linecolor":"#A2B1C6","ticks":""},"baxis":{"gridcolor":"#DFE8F3","linecolor":"#A2B1C6","ticks":""},"caxis":{"gridcolor":"#DFE8F3","linecolor":"#A2B1C6","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":"","title":{"standoff":15},"zerolinecolor":"#EBF0F8","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":"","title":{"standoff":15},"zerolinecolor":"#EBF0F8","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"white","gridcolor":"#DFE8F3","linecolor":"#EBF0F8","showbackground":true,"ticks":"","zerolinecolor":"#EBF0F8","gridwidth":2},"yaxis":{"backgroundcolor":"white","gridcolor":"#DFE8F3","linecolor":"#EBF0F8","showbackground":true,"ticks":"","zerolinecolor":"#EBF0F8","gridwidth":2},"zaxis":{"backgroundcolor":"white","gridcolor":"#DFE8F3","linecolor":"#EBF0F8","showbackground":true,"ticks":"","zerolinecolor":"#EBF0F8","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"white","subunitcolor":"#C8D4E3","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"pretest"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"posttest"}},"legend":{"tracegroupgap":0},"title":{"text":"Posttest given Pretest"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('5d88cb15-7341-4964-b107-fff3a990979a');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
linear_model_pre_post = ols('posttest ~ pretest', data=df).fit()
linear_model_pre_post.summary()
```




<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>        <td>posttest</td>     <th>  R-squared:         </th> <td>   0.612</td>
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared:    </th> <td>   0.605</td>
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th> <td>   88.24</td>
</tr>
<tr>
  <th>Date:</th>             <td>Mon, 06 Nov 2023</td> <th>  Prob (F-statistic):</th> <td>4.19e-13</td>
</tr>
<tr>
  <th>Time:</th>                 <td>11:02:59</td>     <th>  Log-Likelihood:    </th> <td> -223.95</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td>    58</td>      <th>  AIC:               </th> <td>   451.9</td>
</tr>
<tr>
  <th>Df Residuals:</th>          <td>    56</td>      <th>  BIC:               </th> <td>   456.0</td>
</tr>
<tr>
  <th>Df Model:</th>              <td>     1</td>      <th>                     </th>     <td> </td>   
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>     <td> </td>   
</tr>
</table>
<table class="simpletable">
<tr>
      <td></td>         <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>Intercept</th> <td>   23.2002</td> <td>    4.582</td> <td>    5.064</td> <td> 0.000</td> <td>   14.022</td> <td>   32.378</td>
</tr>
<tr>
  <th>pretest</th>   <td>    0.7217</td> <td>    0.077</td> <td>    9.394</td> <td> 0.000</td> <td>    0.568</td> <td>    0.876</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td> 2.136</td> <th>  Durbin-Watson:     </th> <td>   2.185</td>
</tr>
<tr>
  <th>Prob(Omnibus):</th> <td> 0.344</td> <th>  Jarque-Bera (JB):  </th> <td>   1.364</td>
</tr>
<tr>
  <th>Skew:</th>          <td>-0.165</td> <th>  Prob(JB):          </th> <td>   0.506</td>
</tr>
<tr>
  <th>Kurtosis:</th>      <td> 3.675</td> <th>  Cond. No.          </th> <td>    178.</td>
</tr>
</table><br/><br/>Notes:<br/>[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.



## Homogeneity of regression slopes


```python
# Note the formula syntax
interaction_model = ols('posttest ~ pretest * VLEs', data = df).fit()
anova_lm(interaction_model, type = 3)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>df</th>
      <th>sum_sq</th>
      <th>mean_sq</th>
      <th>F</th>
      <th>PR(&gt;F)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>VLEs</th>
      <td>2.0</td>
      <td>5324.471809</td>
      <td>2662.235905</td>
      <td>20.763683</td>
      <td>2.353318e-07</td>
    </tr>
    <tr>
      <th>pretest</th>
      <td>1.0</td>
      <td>7755.854318</td>
      <td>7755.854318</td>
      <td>60.490545</td>
      <td>2.863964e-10</td>
    </tr>
    <tr>
      <th>pretest:VLEs</th>
      <td>2.0</td>
      <td>7.141466</td>
      <td>3.570733</td>
      <td>0.027849</td>
      <td>9.725494e-01</td>
    </tr>
    <tr>
      <th>Residual</th>
      <td>52.0</td>
      <td>6667.230841</td>
      <td>128.215978</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



## Normality of the residuals


```python
# Ancova model
model = ols('posttest ~ pretest + VLEs', data = df).fit()
```


```python
# Add residuals to dataframe object
df['Residuals'] = model.resid
```


```python
# Histogram of residuals
express.histogram(df, x = 'Residuals', nbins = 14)

# fig = express.histogram(df, x = 'Residuals', nbins = 14)
# #fig.write_html('normality60.html', auto_open=True)
# fig.write_image("normality60.png")
```


<div>                            <div id="a3cd6435-faaf-4aba-93ad-7eb2134477c6" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("a3cd6435-faaf-4aba-93ad-7eb2134477c6")) {                    Plotly.newPlot(                        "a3cd6435-faaf-4aba-93ad-7eb2134477c6",                        [{"alignmentgroup":"True","bingroup":"x","hovertemplate":"Residuals=%{x}\u003cbr\u003ecount=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"","marker":{"color":"#636efa","pattern":{"shape":""}},"name":"","nbinsx":14,"offsetgroup":"","orientation":"v","showlegend":false,"x":[-8.49920713392234,30.303503610046093,-7.545925612983012,-18.071211001468455,-1.7319178778907656,-3.9866324894053093,-5.922220554724163,10.696785130985418,-9.449343233373739,15.453367510594688,-5.44107490201705,7.867946022657833,-11.977490704244325,-7.464916059727301,2.2106567666262578,-3.1237856459854783,2.986214354014521,14.560505428209552,-6.170337547664154,5.305083940272695,-19.60902459744061,-24.35306281046344,21.626124691593446,0.13696766746716094,1.2144082619154233,12.690793586212095,1.1318183784331097,10.549647933505,-10.031331141820118,12.20240439436931,-5.956752629756977,7.340687964691291,-0.1427448946647445,20.26897153501328,16.16222257802201,-13.750877126570025,-7.758181621566891,-2.592038018242434,-19.130034150696318,1.969076835741518,6.339647612712412,-2.069206734580476,4.3389407362901125,-6.509206734580474,13.87368582573525,-11.54018580990558,-9.053633908226914,3.1069368687439862,-2.321059263709884,7.581651480258543,-4.338045842908059,0.49739088399408615,-0.030049710454179035,9.780672404933426,1.7572547845426811,2.729935050580522,-5.482760454422603,-10.631044024744597],"xaxis":"x","yaxis":"y","type":"histogram"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"#C8D4E3","linecolor":"#C8D4E3","minorgridcolor":"#C8D4E3","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"#C8D4E3","linecolor":"#C8D4E3","minorgridcolor":"#C8D4E3","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"white","polar":{"bgcolor":"white","angularaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":""},"radialaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":""}},"ternary":{"bgcolor":"white","aaxis":{"gridcolor":"#DFE8F3","linecolor":"#A2B1C6","ticks":""},"baxis":{"gridcolor":"#DFE8F3","linecolor":"#A2B1C6","ticks":""},"caxis":{"gridcolor":"#DFE8F3","linecolor":"#A2B1C6","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":"","title":{"standoff":15},"zerolinecolor":"#EBF0F8","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":"","title":{"standoff":15},"zerolinecolor":"#EBF0F8","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"white","gridcolor":"#DFE8F3","linecolor":"#EBF0F8","showbackground":true,"ticks":"","zerolinecolor":"#EBF0F8","gridwidth":2},"yaxis":{"backgroundcolor":"white","gridcolor":"#DFE8F3","linecolor":"#EBF0F8","showbackground":true,"ticks":"","zerolinecolor":"#EBF0F8","gridwidth":2},"zaxis":{"backgroundcolor":"white","gridcolor":"#DFE8F3","linecolor":"#EBF0F8","showbackground":true,"ticks":"","zerolinecolor":"#EBF0F8","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"white","subunitcolor":"#C8D4E3","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"Residuals"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"count"}},"legend":{"tracegroupgap":0},"margin":{"t":60},"barmode":"relative"},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('a3cd6435-faaf-4aba-93ad-7eb2134477c6');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
# ﻿Null hypothesis of the Shapiro-Wilk test is that the data are from a population in which we have a normal distribution

stats.shapiro(df.Residuals)
```




    ShapiroResult(statistic=0.9896185398101807, pvalue=0.9013279676437378)



## Homogeneity of varians


```python
# Assign residuals for each treatment level
residVLE1 = df.loc[df.VLEs == 'VLE1', 'Residuals'].to_list()
residVLE2 = df.loc[df.VLEs == 'VLE2', 'Residuals'].to_list()
residVLE3 = df.loc[df.VLEs == 'VLE3', 'Residuals'].to_list()
stats.levene (residVLE1, residVLE2, residVLE3)
```




    LeveneResult(statistic=2.8633697961669444, pvalue=0.0656182941137055)



## Outliers


```python
# Sum over all studentized residuals with absolute value more than 3
numpy.sum(numpy.abs(model.get_influence().resid_studentized_internal) > 3)
```




    0




```python
sample = model.get_influence().resid_studentized_internal
outliers = []
def detect_outliers_zscore(df):
    thres = 3
    mean = numpy.mean(df)
    std = numpy.std(df)
    # print(mean, std)
    for i in df:
        z_score = (i-mean)/std
        if (abs(z_score) > thres):
            outliers.append(i)
    return outliers# Driver code
sample_outliers = detect_outliers_zscore(sample)
print("Outliers from Z-scores method: ", sample_outliers)

plt.boxplot(sample, vert=False)
plt.title("Detecting outliers using Boxplot")
plt.xlabel('Sample')
plt.show()
```

    Outliers from Z-scores method:  []



    
![png](output_27_1.png)
    



```python
express.box(df, y = sample, title = 'Outlier')
```


<div>                            <div id="1f3ed5d4-f07e-43d7-8301-355cf9424011" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("1f3ed5d4-f07e-43d7-8301-355cf9424011")) {                    Plotly.newPlot(                        "1f3ed5d4-f07e-43d7-8301-355cf9424011",                        [{"alignmentgroup":"True","hovertemplate":"y=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"","marker":{"color":"#636efa"},"name":"","notched":false,"offsetgroup":"","orientation":"v","showlegend":false,"x0":" ","xaxis":"x","y":[-0.7843796834547698,2.7978788703725113,-0.6975269815502749,-1.669324190161793,-0.16002217255613901,-0.3681401364909118,-0.5532220107986348,0.9918667146417915,-0.8745354337257278,1.427020132834764,-0.5052313169418551,0.7309891325606898,-1.1057381087170512,-0.6898835616919232,0.20459598370732018,-0.2890016522973206,0.27627404061263955,1.344181440497098,-0.5745550204600391,0.49027881552946057,-1.8352043898047816,-2.3487843723163406,2.044720756749192,0.012674333655663502,0.11439106493861334,1.2222995475293297,0.10459470269344286,1.031329248716473,-0.9316597376285357,1.1389172234609855,-0.5505868385306308,0.6835222936341546,-0.013241669989913035,1.8834756928551695,1.5654842994114155,-1.2988767076635972,-0.7169566386374763,-0.23954231751378544,-1.769817884851063,0.18385322643278768,0.5944716454258594,-0.19273503816566834,0.40214383859289976,-0.6062962136414798,1.3334193868217006,-1.0719925504318955,-0.8411331140319471,0.28956157484331835,-0.2151215558449824,0.7054214042442556,-0.4024588563894689,0.04670289487434948,-0.00277866575115625,0.9094116817614025,0.16743832785208412,0.25457570059764184,-0.5081018042010418,-0.9866822742085509],"y0":" ","yaxis":"y","type":"box"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"#C8D4E3","linecolor":"#C8D4E3","minorgridcolor":"#C8D4E3","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"#C8D4E3","linecolor":"#C8D4E3","minorgridcolor":"#C8D4E3","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"white","polar":{"bgcolor":"white","angularaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":""},"radialaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":""}},"ternary":{"bgcolor":"white","aaxis":{"gridcolor":"#DFE8F3","linecolor":"#A2B1C6","ticks":""},"baxis":{"gridcolor":"#DFE8F3","linecolor":"#A2B1C6","ticks":""},"caxis":{"gridcolor":"#DFE8F3","linecolor":"#A2B1C6","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":"","title":{"standoff":15},"zerolinecolor":"#EBF0F8","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":"","title":{"standoff":15},"zerolinecolor":"#EBF0F8","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"white","gridcolor":"#DFE8F3","linecolor":"#EBF0F8","showbackground":true,"ticks":"","zerolinecolor":"#EBF0F8","gridwidth":2},"yaxis":{"backgroundcolor":"white","gridcolor":"#DFE8F3","linecolor":"#EBF0F8","showbackground":true,"ticks":"","zerolinecolor":"#EBF0F8","gridwidth":2},"zaxis":{"backgroundcolor":"white","gridcolor":"#DFE8F3","linecolor":"#EBF0F8","showbackground":true,"ticks":"","zerolinecolor":"#EBF0F8","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"white","subunitcolor":"#C8D4E3","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0]},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"y"}},"legend":{"tracegroupgap":0},"title":{"text":"Outlier"},"boxmode":"group"},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('1f3ed5d4-f07e-43d7-8301-355cf9424011');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


# ANCOVA model

## ANOVA and ANCOVA


```python
# Model and anova table
anova = ols('posttest ~ VLEs', data = df).fit()
anova_lm(anova)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>df</th>
      <th>sum_sq</th>
      <th>mean_sq</th>
      <th>F</th>
      <th>PR(&gt;F)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>VLEs</th>
      <td>2.0</td>
      <td>5324.471809</td>
      <td>2662.235905</td>
      <td>10.146963</td>
      <td>0.000177</td>
    </tr>
    <tr>
      <th>Residual</th>
      <td>55.0</td>
      <td>14430.226625</td>
      <td>262.367757</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Sum of squares due to the regression
ssrANOVA = anova.ess
ssrANOVA
```




    5324.471809219596




```python
# Sum of squares due to the error
sseANOVA = anova.ssr
sseANOVA
```




    14430.226625263162




```python
# Total sum of squares
sstANOVA = ssrANOVA + sseANOVA
sstANOVA
```




    19754.698434482758




```python
# Calculate the value for the coefficient of determination
ssrANOVA/sstANOVA

```




    0.26952938952110145




```python
# Use the rsquared attribute of the model
anova.rsquared
```




    0.2695293895211015




```python
# Ancova table of Anova model
anova_lm(model)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>df</th>
      <th>sum_sq</th>
      <th>mean_sq</th>
      <th>F</th>
      <th>PR(&gt;F)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>VLEs</th>
      <td>2.0</td>
      <td>5324.471809</td>
      <td>2662.235905</td>
      <td>21.539215</td>
      <td>1.325310e-07</td>
    </tr>
    <tr>
      <th>pretest</th>
      <td>1.0</td>
      <td>7755.854318</td>
      <td>7755.854318</td>
      <td>62.749891</td>
      <td>1.320739e-10</td>
    </tr>
    <tr>
      <th>Residual</th>
      <td>54.0</td>
      <td>6674.372307</td>
      <td>123.599487</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
sseANCOVA = model.ssr
sseANCOVA
```




    6674.372306963574




```python
# Difference between sum of squares due to the error between ANOVA and ANCOVA
print('Sum of squares due to the error of ANOVA model: ', '\t', sseANOVA)
print('Sum of squares due to the error of ANCOVA model: ', '\t', sseANCOVA)
print('Selisihnya = ', sseANOVA-sseANCOVA)
print('Persentase = ',sseANCOVA/sseANOVA*100,'%')
```

    Sum of squares due to the error of ANOVA model:  	 14430.226625263162
    Sum of squares due to the error of ANCOVA model:  	 6674.372306963574
    Selisihnya =  7755.854318299587
    Persentase =  46.252719934963984 %



```python
# Total sum of squares of ANCOVA model
sstANCOVA = model.ess + model.ssr
sstANCOVA
```




    19754.698434482758




```python
# Print SST for both models
print('Total sum of squares of ANOVA model:','\t', sstANOVA)
print('Total sum of squares of ANCOVA model: ', '\t', sstANCOVA)
```

    Total sum of squares of ANOVA model: 	 19754.698434482758
    Total sum of squares of ANCOVA model:  	 19754.698434482758



```python
anova.rsquared
```




    0.2695293895211015




```python
model.rsquared
```




    0.6621374743279734




```python
# Coeficient
model.params
```




    Intercept       27.128318
    VLEs[T.VLE2]    -7.796462
    VLEs[T.VLE3]     2.026113
    pretest          0.685458
    dtype: float64




```python
# Standard errors
model.bse
```




    Intercept       4.581358
    VLEs[T.VLE2]    3.659953
    VLEs[T.VLE3]    4.212493
    pretest         0.086532
    dtype: float64




```python
# t statistics
model.tvalues
```




    Intercept       5.921458
    VLEs[T.VLE2]   -2.130208
    VLEs[T.VLE3]    0.480977
    pretest         7.921483
    dtype: float64




```python
# p values for each coefficieent
model.pvalues
```




    Intercept       2.278401e-07
    VLEs[T.VLE2]    3.772696e-02
    VLEs[T.VLE3]    6.324759e-01
    pretest         1.320739e-10
    dtype: float64




```python
# 95% of confidence interval
model.conf_int()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Intercept</th>
      <td>17.943248</td>
      <td>36.313388</td>
    </tr>
    <tr>
      <th>VLEs[T.VLE2]</th>
      <td>-15.134227</td>
      <td>-0.458697</td>
    </tr>
    <tr>
      <th>VLEs[T.VLE3]</th>
      <td>-6.419426</td>
      <td>10.471652</td>
    </tr>
    <tr>
      <th>pretest</th>
      <td>0.511973</td>
      <td>0.858943</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Add fitted value to dataframe object
df['Estimated'] = model.fittedvalues
```


```python
fig = subplots.make_subplots(rows = 1, cols = 2)

fig.add_trace(
    graph_objects.Box(
        y = df.posttest,
        x = df.VLEs,
        boxpoints = 'all',
        name = 'posttest'
    ),
    row = 1,
    col = 1
)

fig.add_trace(
    graph_objects.Box(
        y = df.Estimated,
        x = df.VLEs,
        boxpoints = 'all',
        name = 'Corrected posttest'
    ),
    row = 1,
    col = 2
)
```


<div>                            <div id="02255b50-71ff-40cb-8a7b-ac2168f064d4" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("02255b50-71ff-40cb-8a7b-ac2168f064d4")) {                    Plotly.newPlot(                        "02255b50-71ff-40cb-8a7b-ac2168f064d4",                        [{"boxpoints":"all","name":"posttest","x":["VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3"],"y":[48.33,90.56,55.0,43.33,51.67,50.56,65.0,76.67,41.67,70.0,61.67,55.56,43.33,45.56,53.33,60.56,66.67,74.44,40.0,58.33,23.33,56.67,60.0,52.22,41.11,93.33,57.78,95.0,55.0,56.67,52.22,53.33,48.89,67.78,98.33,25.0,48.89,54.44,33.33,91.67,98.33,86.11,88.33,81.67,67.78,57.22,77.22,91.67,81.67,95.0,65.56,62.78,74.44,77.78,58.33,91.67,65.56,58.89],"type":"box","xaxis":"x","yaxis":"y"},{"boxpoints":"all","name":"Corrected posttest","x":["VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE1","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE2","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3","VLE3"],"y":[56.82920713392234,60.25649638995391,62.54592561298301,61.401211001468454,53.40191787789077,54.54663248940531,70.92222055472416,65.97321486901458,51.11934323337374,54.54663248940531,67.11107490201705,47.69205397734217,55.30749070424432,53.0249160597273,51.11934323337374,63.68378564598548,63.68378564598548,59.879494571790445,46.170337547664154,53.0249160597273,42.93902459744061,81.02306281046344,38.373875308406554,52.08303233253284,39.895591738084576,80.6392064137879,56.64818162156689,84.450352066495,65.03133114182012,44.46759560563069,58.176752629756976,45.98931203530871,49.032744894664745,47.51102846498672,82.16777742197799,38.750877126570025,56.64818162156689,57.03203801824243,52.460034150696316,89.70092316425848,91.99035238728759,88.17920673458048,83.99105926370989,88.17920673458048,53.90631417426475,68.76018580990558,86.27363390822691,88.56306313125602,83.99105926370989,87.41834851974146,69.89804584290806,62.282609116005915,74.47004971045418,67.99932759506657,56.57274521545732,88.94006494941948,71.0427604544226,69.5210440247446],"type":"box","xaxis":"x2","yaxis":"y2"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"#C8D4E3","linecolor":"#C8D4E3","minorgridcolor":"#C8D4E3","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"#C8D4E3","linecolor":"#C8D4E3","minorgridcolor":"#C8D4E3","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"white","polar":{"bgcolor":"white","angularaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":""},"radialaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":""}},"ternary":{"bgcolor":"white","aaxis":{"gridcolor":"#DFE8F3","linecolor":"#A2B1C6","ticks":""},"baxis":{"gridcolor":"#DFE8F3","linecolor":"#A2B1C6","ticks":""},"caxis":{"gridcolor":"#DFE8F3","linecolor":"#A2B1C6","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":"","title":{"standoff":15},"zerolinecolor":"#EBF0F8","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"#EBF0F8","linecolor":"#EBF0F8","ticks":"","title":{"standoff":15},"zerolinecolor":"#EBF0F8","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"white","gridcolor":"#DFE8F3","linecolor":"#EBF0F8","showbackground":true,"ticks":"","zerolinecolor":"#EBF0F8","gridwidth":2},"yaxis":{"backgroundcolor":"white","gridcolor":"#DFE8F3","linecolor":"#EBF0F8","showbackground":true,"ticks":"","zerolinecolor":"#EBF0F8","gridwidth":2},"zaxis":{"backgroundcolor":"white","gridcolor":"#DFE8F3","linecolor":"#EBF0F8","showbackground":true,"ticks":"","zerolinecolor":"#EBF0F8","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"white","subunitcolor":"#C8D4E3","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,0.45]},"yaxis":{"anchor":"x","domain":[0.0,1.0]},"xaxis2":{"anchor":"y2","domain":[0.55,1.0]},"yaxis2":{"anchor":"x2","domain":[0.0,1.0]}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('02255b50-71ff-40cb-8a7b-ac2168f064d4');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


## Linear algebra


```python
y, X = patsy.dmatrices('posttest ~ pretest + VLEs', data = df)
```


```python
X[:5]
```




    array([[ 1.  ,  0.  ,  0.  , 43.33],
           [ 1.  ,  0.  ,  0.  , 48.33],
           [ 1.  ,  0.  ,  0.  , 51.67],
           [ 1.  ,  0.  ,  0.  , 50.  ],
           [ 1.  ,  0.  ,  0.  , 38.33]])




```python
y = numpy.array(y)
X = numpy.array(X)
```


```python
XT = X.transpose()
XTX = numpy.matmul(XT, X)
XTXI= numpy.linalg.inv(XTX)
XTXIXT = numpy.matmul(XTXI, XT)
beta = numpy.matmul (XTXIXT, y)
```


```python
beta
```




    array([[27.12831844],
           [-7.79646224],
           [ 2.02611273],
           [ 0.68545785]])




```python
model.params
```




    Intercept       27.128318
    VLEs[T.VLE2]    -7.796462
    VLEs[T.VLE3]     2.026113
    pretest          0.685458
    dtype: float64



## ANCOVA


```python
ancova(data = df, dv = 'posttest', covar = 'pretest', between = 'VLEs')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Source</th>
      <th>SS</th>
      <th>DF</th>
      <th>F</th>
      <th>p-unc</th>
      <th>np2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>VLEs</td>
      <td>995.136789</td>
      <td>2</td>
      <td>4.025651</td>
      <td>2.346158e-02</td>
      <td>0.129752</td>
    </tr>
    <tr>
      <th>1</th>
      <td>pretest</td>
      <td>7755.854318</td>
      <td>1</td>
      <td>62.749891</td>
      <td>1.320739e-10</td>
      <td>0.537473</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Residual</td>
      <td>6674.372307</td>
      <td>54</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Hasil ANCOVA menunjukkan bahwa terdapat perbedaan yang signifikan dalam hasil posttest prestasi belajar [F(2, 54) = 4.02; p < .05] antar VLEs sambil menyesuaikan pengaruh pretest.
# ANCOVA results indicate that there are significant differences in mean posttest among VLEs (p = 0.023) whilst adjusting the effect of pretest.
# The covariate pretest is significant [F(1, 54) = 62.75; p = .00; p < .05] suggesting it is an important predictor of posttest.
```

# Other visualization


```python
# from dfply import *
# import pandas as pd
# import seaborn as sns
# import matplotlib.pyplot as plt

# df=pd.read_csv("data58.csv")
# #df.head(2)

# # summary statistics for dependent variable posttest
# #df >> group_by(X.VLEs) >> summarize(n=X['posttest'].count(), MEAN=X['posttest'].mean(), STD_DEV=X['posttest'].std(), SE=X['posttest'].sem())

# # summary statistics for covariate pretest
# #df >> group_by(X.VLEs) >> summarize(n=X['pretest'].count(), MEAN=X['pretest'].mean(), STD_DEV=X['pretest'].std(), SE=X['pretest'].sem())

# # visualize dataset
# fig, axs = plt.subplots(ncols=3)

# sns.scatterplot(data=df, x="pretest", y="posttest", hue="VLEs", ax=axs[0])
# # sns1 = sns.lmplot(data=df, x="pretest", y="posttest", hue="VLEs", legend=False)
# # plt.legend(loc='lower right', title='VLEs')

# # seaborn1 = sns.boxplot(data=df, x="VLEs", y="posttest", hue="VLEs", ax=axs[0])
# # sns.move_legend (seaborn1, "lower right")


# sns.boxplot(data=df, x="VLEs", y="posttest", hue="VLEs", ax=axs[1])
# sns.boxplot(data=df, x="VLEs", y="pretest", hue="VLEs", ax=axs[2])
# plt.tight_layout()

# #sns1.savefig('lin60.png')

# #fig.savefig('summ60.png')
```