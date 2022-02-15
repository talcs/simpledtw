# simpledtw

**simpledtw** is a Python Dynamic Programming implementation of the classic Dynamic Time Warping algorithm. The library enables computing DTW on sequences of scalars or vectors. It uses NumPy in its backend.

## Installation
The package **simpledtw** has not been registered in PyPI so far.
In order to install the package, copy the file **simpledtw.py** to your Python libs directory or to your project directory.

## Dependencies
The only dependency of **simpledtw** is **NumPy**.


## Usage example
The following code demonstrates the usage of **simpledtw** to compute DTW on two series of scalars, **series_1** and **series_2**:

```
>>> import simpledtw
>>> series_1 = [1, 2, 3, 2, 2.13, 1]
>>> series_2 = [1, 1, 2, 2, 2.42, 3, 2, 1]
>>> matches, cost, mapping_1, mapping_2, matrix = simpledtw.dtw(series_1, series_2)
>>> matches
[(0, 0), (0, 1), (1, 2), (1, 3), (1, 4), (2, 5), (3, 6), (4, 6), (5, 7)]
>>> cost
0.55
>>> mapping_1
[[0, 1], [2, 3, 4], [5], [6], [6], [7]]
>>> mapping_2
[[0], [0], [1], [1], [1], [2], [3, 4], [5]]
>>> matrix
array([[ 0.  ,  0.  ,  1.  ,  2.  ,  3.42,  5.42,  6.42,  6.42],
       [ 1.  ,  1.  ,  0.  ,  0.  ,  0.42,  1.42,  1.42,  2.42],
       [ 3.  ,  3.  ,  1.  ,  1.  ,  0.58,  0.42,  1.42,  3.42],
       [ 4.  ,  4.  ,  1.  ,  1.  ,  1.  ,  1.42,  0.42,  1.42],
       [ 5.13,  5.13,  1.13,  1.13,  1.29,  1.87,  0.55,  1.55],
       [ 5.13,  5.13,  2.13,  2.13,  2.55,  3.29,  1.55,  0.55]])
```

Below is the visualization of the above example. In blue and orange are the two value series. In black are the optimal connections computed by the DTW algorithm:


![DTW Visualization](/dtw_vis.png)

If we sum the absolute differences for each pair of matched indices, we will get the cost of the whole match, which will be identical to the value at the **(n,m)** index in the matrix. In our example, we can see that all of those differences are 0, except for the match **(1,4)**, which costs **|2.42 - 2| = 0.42** and match **(4,6)**, which costs **|2 - 2.13| = 0.13**. Therefore, the total warping cost in this example is **0.42 + 0.13 = 0.55**, as can also be indicated from the **(n,m)** index in the matrix.


The below matrix refers to the above example. At each index **(i,j)**, it shows the cost of the optimal warping up to that point, i.e., the cost of warping the prefix of **series_1** up to index **i** and the prefix of **series_2** up to index **j**. Thus, the **(n,m)** index contains the final cost of the whole warping. The gray cells denote the path of the final optimal matches, which has been retrieved by starting from index **(n,m)** and recursively choosing the cheapest neighboring cost, going up, left or both.

![DTW Matrix](/dtw_vis_table.png)

* Although not shown in the example, the values of **series_1** and **series_2** can also be vectors of any dimension

## Inputs and outputs
The function **simpledtw.dtw()** gets 2 mandatory parameters and 1 optional parameter:

| Parameter | Mandatory / Optional | Description |
|:---------:|:--------------------:|-----------|
| series_1 | Mandatory | an iterable object of numbers **or vectors** |
| series_2 | Mandatory | an iterable object of numbers **or vectors** |
| norm_func | Optional | a function that computes vector norm or real number absolute value (default is `numpy.linalg.norm`) |

NOTE: `series_1` and `series_2` must support Python's `len()` function. Thus, iterable objects such as lists, tuples and Numpy arrays will work, while **generators will not**.


The function `simpledtw.dtw()` returns 5 outputs:

| Output | Description |
|:------:|-----------|
| matches | a list of tuples, where each tuple's first member is an index from `series_1` and the second member is an index from `series_2` |
| cost | the cost of the warping, which is the value at the `(n,m)` cell of the Dynamic Programming 2D array |
| mapping_1 | a list that contains at each index `i`, the list of indices `j` in `series_2`, to which index `i` in `series_1` has been matched |
| mapping_2 | a list that contains at each index `i`, the list of indices `j` in `series_1`, to which index `i` in `series_2` has been matched |
| matrix | the Dynamic Programming `(nxm)` Numpy matrix, where `n` is the length of `series_1` and `m` is the length of `series_2`, which can be used in order to visualize the computations and the selected path |

## Using Different Norm Functions
In order to use norms that are different from **L2 distance**, which is the default behavior of  **numpy.linalg.norm**, you could use a lambda expression, for example, for L1 distance you could use:
```
>>> norm_func = lambda x : numpy.linalg.norm(x, ord = 1)
>>> matches, cost, mapping_1, mapping_2, matrix = simpledtw.dtw(series_1, series_2, norm_func)
```

You might want to check NumPy's norm function documentation [here](https://docs.scipy.org/doc/numpy/reference/generated/numpy.linalg.norm.html)

