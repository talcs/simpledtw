# simpledtw

**simpledtw** is a Python Dynamic Programming implementation using NumPy, of the classic Dynamic Time Warping algorithm. The library enables computing DTW on sequences of scalars or on sequences of vectors.

## Installation
The package **simpledtw** is currenetly not registered in PyPi.
The current way of installing the package is by copying the file **simpledtw.py** into your Python libs directory or into your project directory

## Dependencies
The only dependency of **simpledtw** is **NumPy**


## Usage example
The following code shows using DTW on the two serieses **series_1** and **series_2**:

```
>>> import simpledtw
>>> series_1 = [1, 2, 3, 2, 1]
>>> series_2 = [1, 1, 2, 2, 2.5, 3, 2, 1]
>>> matches, cost, mapping_1, mapping_2, matrix = simpledtw.dtw(series_1, series_2)
>>> matches
[(0, 0), (0, 1), (1, 2), (1, 3), (1, 4), (2, 5), (3, 6), (4, 7)]
>>> cost
0.5
>>> mapping_1
[[0, 1], [2, 3, 4], [5], [6], [7]]
>>> mapping_2
[[0], [0], [1], [1], [1], [2], [3], [4]]
>>> matrix
array([[ 0. ,  0. ,  1. ,  2. ,  3.5,  5.5,  6.5,  6.5],
       [ 1. ,  1. ,  0. ,  0. ,  0.5,  1.5,  1.5,  2.5],
       [ 3. ,  3. ,  1. ,  1. ,  0.5,  0.5,  1.5,  3.5],
       [ 4. ,  4. ,  1. ,  1. ,  1. ,  1.5,  0.5,  1.5],
       [ 4. ,  4. ,  2. ,  2. ,  2.5,  3. ,  1.5,  0.5]])
```

Here is the visualization of the above example. In blue and orange are the value serieses. In black are the optimal connections computed by the DTW algorithm:


![DTW Visualization](/dtw_vis.png)

If for each pair of matched indices, we sum the absolute differences of their values, we get the cost of the whole match, which is identical to the value in the **(n,m)** index in the matrix. In our example, we can see that all of those differences are 0, except for the match **(1,4)**, which makes a cost of **|2.5 - 2| = 0.5**. Hence, the total cost of this example is **0.5**, as also seen in the **(n,m)** index in the matrix.


* Although not shown in the example, the values of **series_1** and **series_2** can also be vectors of any dimension

## Inputs and outputs
The function **simpledtw.dtw()** gets 2 mandatory parameters and 1 optional parameter:

* ***series_1*** - **MANDATORY** - an iterable object of numbers **or vectors**
* ***series_2*** - **MANDATORY** - an iterable object of numbers **or vectors**
* ***norm_func*** - **OPTIONAL** - a function that computes vector norm or real number absolute value (default is **numpy.linalg.norm**).

NOTE: ***series_1*** and ***series_2*** must support Python's ***len()*** function. Thus, iterable objects such as lists, tuples and Numpy arrays will work, while **generators will not**.


The function **simpledtw.dtw()** returns 5 outputs:
* ***matches*** - A list of tuples, where each tuple's first member is an index from **series_1** and the second member is an index from **series_2**
* ***cost*** - The cost of the warping, which is the value at the ***(n,m)*** cell of the Dynamic Programming 2D array
* ***mapping_1*** - A list that contains at each index ***i***, the list of indices ***j*** in **series_2**, to which index ***i*** in **series_1** has been matched
* ***mapping_2*** - A list that contains at each index ***i***, the list of indices ***j*** in **series_1**, to which index ***i*** in **series_2** has been matched
* ***matrix*** - The Dynamic Programming **(nxm)** Numpy matrix, where **n** is the length of **series_1** and **m** is the length of **series_2**, which can be used in order to visualize the computations and the selected path

## Using Different Norm Functions
In order to use norms that are different from **L2 distance**, which is the default behavior of  **numpy.linalg.norm**, you could use a lambda expression, for example, for L1 distance you could use:
```
>>> norm_func = lambda x : numpy.linalg.norm(x, ord = 1)
>>> matches, cost, mapping_1, mapping_2, matrix = simpledtw.dtw(series_1, series_2, norm_func)
```

You might want to check NumPy's norm function documentation [here](https://docs.scipy.org/doc/numpy/reference/generated/numpy.linalg.norm.html)

