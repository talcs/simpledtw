# simpledtw

**simpledtw** is a Python Dynamic Programming implementation using NumPy, of the classic Dynamic Time Warping algorithm

## Dependencies
The only dependency of **simpledtw** is **NumPy**


## Usage example
The following code shows using DTW on the two serieses **series_1** and **series_2**:

```
>>> import simpledtw
>>> series_1 = [1, 2, 3, 2, 1]
>>> series_2 = [1, 1, 2, 2, 2.5, 3, 2, 1]
>>> matches, mapping_1, mapping_2, matrix = simpledtw.dtw(series_1, series_2)
>>> matches
[(0, 0), (0, 1), (1, 2), (1, 3), (1, 4), (2, 5), (3, 6), (4, 7)]
>>> mapping_1
[0, 2, 5, 6, 7]
>>> mapping_2
[0, 0, 1, 1, 1, 2, 3, 4]
>>> matrix
array([[ 0. ,  0. ,  1. ,  2. ,  3.5,  5.5,  6.5,  6.5],
       [ 1. ,  1. ,  0. ,  0. ,  0.5,  1.5,  1.5,  2.5],
       [ 3. ,  3. ,  1. ,  1. ,  0.5,  0.5,  1.5,  3.5],
       [ 4. ,  4. ,  1. ,  1. ,  1. ,  1.5,  0.5,  1.5],
       [ 4. ,  4. ,  2. ,  2. ,  2.5,  3. ,  1.5,  0.5]])
```

* Although not shown in the example, the values of **series_1** and ****

## Inputs and outputs
The function **simpledtw.dtw()** gets 2 mandatory parameters and 1 optional parameter:

* ***series_1*** - **MANDATORY** - an iterable object of numbers **or vectors**
* ***series_2*** - **MANDATORY** - an iterable object of numbers **or vectors**
* ***norm_func*** - **OPTIONAL** - a function that computes vector norm or real number absolute value (default is **numpy.linalg.norm**)

The function **simpledtw.dtw()** returns 4 outputs:
* ***matches*** - A list of tuples, where each tuple's first member is an index from **series_1** and the second member is an index from **series_2**
* ***mapping_1*** - A list that contains at each index ***i***, the index ***j*** in **series_2**, which index ***i*** in **series_1** has been assigned to
* ***mapping_2*** - A list that contains at each index ***i***, the index ***j*** in **series_1**, which index ***i*** in **series_2** has been assigned to
* ***matrix*** - The Dynamic Programming **(nxm)** Numpy matrix, where **n** is the length of **series_1** and **m** is the length of **series_2**, which can be used in order to visualize the computations and the selected path

