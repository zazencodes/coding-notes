## SciPy 2016 Bokeh for Data Applications and Visualizations
- Using bokeh in ipython notbeooks.  
```python
from bokeh.io import output_notebook, show
output_notebook()
```

### Charts   
High level plotting tool that takes in pandas dataframes. Might be a welcoming area for contributions e.g. contour plot, violin plot
- Scatter
```python
from bokeh.charts import Scatter
p = Scatter(df, x='col1', y='col2', color='col3',
            marker='col4', legend=None)
show(p)
```
- Box plot
```python
from bokeh.charts import BoxPlot
p = BoxPlot(df, label='col1', values='col2', color='col1',
            xlabel='', ylabel='Column 2', title='My Title')
show(p)
```
- Other charts include: Bar, Histogram, ...

### Plotting
Lower level plotting tool that seems overall easier to use because the customization appears more straightforward.
```python
from bokeh.plotting import figure
```
- Scatter
```python
p = figure(plot_width=400, plot_height=400)
p.circle(list_1, list_2, radius=list_3, # size=list_3 for fixed point sizes)
         fill_color='orange', fill_alpha=0.6)
show(p)
# Can also do e.g. p.square(...), p.diamond(), p.triangle() ...
```
- Line
```python
p = figure(plot_width=400, plot_height=400, title='Random Stuff'
p.line(np.linspace(0,10,500), np.random.random(500), line_width=3)
show(p)
```
- Can (in principle) load images that are stored in memory as numpy arrays. I wasn't able to get this working.
- A variety of glyphs exist including rectangles, ovals, segments, rays, wedges and arcs.
- Glyphs can be combined, e.g.
```python
x = [1, 2, 3, 4, 5]
y = [6, 7, 8, 7, 3]
p = figure(plot_width=400, plot_height=400, title='Glyph_city')
p.line(x, y, line_width=2)
p.circle(x, y, fill_color="white", size=8)
show(p)
```
### Styling  

