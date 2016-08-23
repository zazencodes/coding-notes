## SciPy 2016 Parallel Python Data Analysis
- Parallelizing in ipython notebooks for dealing with large data.   

### Visualizing runtimes with `snakeviz`
```python
# When import libraries
%load_ext snakeviz
# On first line of cell
%%snakeviz
```

### Parallelize a for loop

one core:
```python
results = []   
for i in range(8):   
    do stuff   
    results.append(2*i)   
```

multiple cores:
```python
from concurrent.features import ProcessPoolExecutor   
cores = 8 # number of cores to use   
e = ProcessPoolExecutor(cores)   
def func(i):   
    do stuff   
    return 2*i   
results = list(e.map(func, range(8)))   
```

### Using ujson instead of json
- slightly faster runtime
```python
import ujson
ujson.load('file_name.json')
```

### Dictionary with tuples for keys
```python
a = {}
a['a', 'b'] = 5
print(a) # = {(a, b): 5}
```

### Using `submit` and working with futures
one core:
```python
def add(a, b)
    return a + b
add(1, 2)
```
multiple cores:
```python
from concurrent.futures import ThreadPoolExecutor
e = ThreadPoolExecutor(8) # input number of cores
def add(a, b)
    return a + b
future = e.submit(add, 1 ,2)
future.result()
```
inside a loop:
```python
futures = []
for a, b in zip(a_list, b_list):
    futures.append(e.submit(add, a, b))
    
print([f.result() for f in futures])
```

### Collections provide a fixed set of operations
- collection.map(function): apply function to each element in collection
- collection.cartesian(collection): Create new collection with every pair of inputs
- collection.filter(predicate): Keep only elements of colleciton that match the predicate function
- collection.max(): Compute maximum element
- collection.groupby(key)
- collection.join(other_collection)   

### Parallelizing with `pyspark` 
- works similar to `dask` (which is easier to install - on windows at least)
- can access collection functions above
```python
from pyspark import SparkContext
sc = SparkContext('local[4]')
rdd = sc.parallelize(range(5))  # create collection
print(rdd)
# PythonRDD[2] at RDD at PythonRDD.scala:48

print(rdd.collect())  # Gather results back to local process
# [0, 1, 2, 3, 4]

print(rdd.map(lambda x: x**2).collect())
# [0, 1, 4, 9, 16]

print(rdd.filter(lambda x: x%2 == 0))
# [0, 2, 4]
```
- Chain operations for more complex computations
```python
(rdd.map(lambda x: x**2)
    .filter(lambda x: x%2 == 0)
    .collect())
```

### Parallelizing with `dask`
```python
import dask.bag as db
b = db.from_sequence(filenames)
values = b.map(lambda fn: pd.read_csv(fn)['column_name'].values)
result = (values.map(lambda x: x**2)
                .filter(lambda x: x%2 == 0))
result = result.compute()
```


### Loading multiple .csv files into a dataframe with `dask`
- functions like a pandas dataframe
- can do same thing with `pyspark`
- some operations require `.compute()` calls to evaluate
```python
import dask.dataframe as dd
df = dd.read_csv('path_to_files/*.csv', parse_dates=['timestamp'])
df.describe().compute()
```
- chain operations on dataframe
```python
import datetime as dt
import matplotlib.pyplot as plt
(df.groupby(df.timestamp.dt.dates)
    .column_name # get the desired column name
    .mean()
    .compute()
    .plot())
```

### Search over a group of randomly sampled parameters in `scikit-learn`
- define limits and get random combinations
```python
from sklearn.grid_search import ParameterSampler
param_grid = {
    'C': np.logspace(-10, 10, 1001),
    'gamma': np.logspace(-10, 10, 1001),
    'tol': np.logspace(-4, -1, 4),
}

param_samples = ParameterSampler(param_grid, 10)
```
- can visualize results of fitting models nicely by plotting parameters on x and y axes and color coding the validation score
```python
# e.g. by using a function like this
def plot_param_map(df, target, title):
    fig = plt.figure(figsize=(6, 5))
    plt.xlabel('log10(C)')
    plt.ylabel('log10(gamma)')
    plt.xlim(-10, 10)
    plt.ylim(-10, 10)
    plt.scatter(np.log10(df['C']), np.log10(df['gamma']),
                c=target,
                marker='s', edgecolors='none',
                s=80, alpha=1, cmap='viridis')
    plt.colorbar()
    plt.title(title)
    return fig
```

