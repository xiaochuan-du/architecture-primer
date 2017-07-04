# Data Manipulation



## Numpy

- (C like pointer in Numpy)array slices is that they return *views* rather than *copies* of the array data. This is one area in which NumPy array slicing differs from Python list slicing: in lists, slices will be copies `x2_sub = x2[:2, :2]`

- `x2_sub_copy = x2[:2, :2].copy()` copy

- ```python
  x = np.array([1, 2, 3])

  x[None,:].shape === x[np.newaxis,:].shape === x.reshape((1, 3)) # add new axis for a matrix
  ```

- ```python
  # stack
  np.concatenate([grid, grid], axis=1)
  np.vstack([x, grid])
  np.hstack([x, grid])
  # split
  upper, lower = np.vsplit(grid, [2])
  ```

- ```
  y = np.zeros(10)
  np.power(2, x, out=y[::2])
  # the following might take more memory
  y[::2] = 2 ** x
  ```

- ```python
  # aggregation
  np.multiply.reduce(x) # reduce
  np.add.accumulate(x)
  x = np.arange(1, 6)
  np.multiply.outer(x, x)
  array([[ 1,  2,  3,  4,  5],
         [ 2,  4,  6,  8, 10],
         [ 3,  6,  9, 12, 15],
         [ 4,  8, 12, 16, 20],
         [ 5, 10, 15, 20, 25]])
  ```

- ```
  Function Name	NaN-safe Version	Description
  np.sum	np.nansum	Compute sum of elements
  np.prod	np.nanprod	Compute product of elements
  np.mean	np.nanmean	Compute mean of elements
  np.std	np.nanstd	Compute standard deviation
  np.var	np.nanvar	Compute variance
  np.min	np.nanmin	Find minimum value
  np.max	np.nanmax	Find maximum value
  np.argmin	np.nanargmin	Find index of minimum value
  np.argmax	np.nanargmax	Find index of maximum value
  np.median	np.nanmedian	Compute median of elements
  np.percentile	np.nanpercentile	Compute rank-based statistics of elements
  np.any	N/A	Evaluate whether any elements are true
  np.all	N/A	Evaluate whether all elements are true
  ```

- Broadcasting:![](http://nbviewer.jupyter.org/github/jakevdp/PythonDataScienceHandbook/blob/master/notebooks/figures/02.05-broadcasting.png)

  - Rule 1: If the two arrays differ in their n**umber of dimensions**, the shape of the one with fewer dimensions is *padded* with **ones** on its leading (left) side.
  - Rule 2: If the shape of the two arrays does not match **in any dimension**, the array(s) with shape equal to 1 in that dimension is stretched to match the other shape. **shape为1数字增加,两个array有可能均需要增加**
  - Rule 3: If in any dimension the sizes disagree and neither is equal to 1, an error is raised.如果strech过后，仍然有纬度不一致，则丢异常
  - Best practices: broadcase applies to any ufunc operations.

-  Boolean

```Python
# aggregation, filter candicates
x[x < 5]
array([0, 3, 3, 3, 2, 4])
# masking
(x > 4) & (x < 8) # not or/and
# and and or perform a single Boolean evaluation on an entire object, while & and | perform multiple Boolean evaluations on the content (the individual bits or bytes) of an objec
```

- Fancy indexing:   定义为坐标皆为list（如果有整数则退化为combined indexing）

  An array with the same shape as the index array, but with the type and values of the array being indexed.

  Rule 1:

  - if the index arrays have a matching shape, 
  - there is an index array for each dimension of the array being indexed,
  - Hence, the resultant array has the same shape as the index arrays, and the values correspond to the index set for each position in the index arrays.

  Rule 2:

  - not matching shape, use broadcasting, 
  - there is an index array for each dimension of the array being indexed,
  - As previous one

  Rule 3:

  - partially index an array with index arrays
  - the construction of a new array where each value of the index array selects one row from the array being indexed 

  ```python
  row = np.array([0, 1, 2])
  col = np.array([2, 1, 3])
  X[row, col] # notice that broadcasting applied here
  X[row[:, np.newaxis], col] # index <=> row[:, np.newaxis] * col
  X[2, [2, 0, 1]] # combined
  X[1:, [2, 0, 1]] # combined, sliced converted to np.array([1,2])
  ```

- ​

- ​