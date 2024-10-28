---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.14.1
  kernelspec:
    display_name: mooc
    language: python
    name: python3
---

# NumPy (Numerical Python)

NumPy is a Python library that 

Basic imports

```python
import numpy as np
```

## NumPy Basics

### Construction and basic inspection

There are several ways to create arrays. For example, you can create an array **from a regular Python list or tuple** using the `np.array` function. 

#### Creation and inspection of 1-dim vector (notice the type inference).

```python
x = np.array([1.0,2,3]) # np.array() accepts a seq-like object (the type is inferred)
x.dtype    # dtype('float64')
x.itemsize # 4 (i.e., num of bytes)
x.size     # 3 (num of elements)
x.shape    # (3,)  (tuple with the size for each dimension)
x.ndim     # 1 (a.k.a. rank)
(x.shape, x.dtype, x.size, x.ndim, x.itemsize)
```

A common error is to call `np.array(...)` with multiple arguments, rather than giving a single sequence as argument.

```python
try:
    np.array(1,2,3) # wrong
except Exception as e: print(e)
```

#### Explicit specification of the datatype

```python
np.array([1,2,3], dtype=np.float32)
```

```python
np.array([[1, 2], [3, 4]], dtype=complex)
```

<!-- #region -->
Function `array` **transforms seqs-of-seqs into 2D arrays**, seqs-of-seqs-of-seqs into 3D arrays, and so on.


Two-dimensional vector (notice type inference):
<!-- #endregion -->

```python
y = np.array( [ [1], [2] ] )
y.shape     # (2, 1)
y.ndim      # 2
y.dtype     # dtype('int64')
(y.shape, y.ndim, y.dtype)
```

Invalid shape:

```python
try:
  np.array( [ [1], [2,3] ] ) # Error: inhomogeneous shapes
except Exception as e:
  print('Error: ', e)
```

Heterogeneous arrays

```python
np.array([1, 'b', True, ]) # array(['1', 'b', 'True'], dtype='<U21')s
```

```python
# heterogeneous array of objects
np.array([1,  None]) # array([1, None], dtype=object)
```

#### Printing arrays

Multi-dim arrays are printed like nested lists: the **last axis** is printed from **left-to-right**; the **second-to-last** (and **rest**) is printed from **top-to-bottom**; the rest are also printed from top to bottom, with each slice separated from the next by an empty line.

```python
c = np.arange(24).reshape(2, 3, 4)  # 3d array
print(c)
# [[[ 0  1  2  3]
#  [ 4  5  6  7]
#  [ 8  9 10 11]]
#
# [[12 13 14 15]
#  [16 17 18 19]
#  [20 21 22 23]]]
```

If an array is too large to be printed, the central part is **skipped** (**...**)

```python
print(np.arange(10000))
# [   0    1    2 ... 9997 9998 9999]

print(np.arange(10000).reshape(100, 100))
# [[   0    1    2 ...   97   98   99]
# [ 100  101  102 ...  197  198  199]
# [ 200  201  202 ...  297  298  299]
# ...
# [9700 9701 9702 ... 9797 9798 9799]
# [9800 9801 9802 ... 9897 9898 9899]
# [9900 9901 9902 ... 9997 9998 9999]]
```

### Monodimensional vectors vs. row vectors vs. column vectors

Notice the differences among the following vectors:

```python
vector = np.array([0, 1, 2, 3, 4, 5])
cvec = vector.reshape((6, 1))
rvec = vector.reshape((1, 6))
print(
    'Vettore = ', vector, vector.shape,
    '\nVettore colonna = ', cvec, cvec.shape,
    '\nVettore riga = ', rvec, rvec.shape)
```

* the original `vector` has shape `(6,)`, and is formatted `[0 1 ... 6]`
* `row_vector` has shape `(1,6)`, and is formatted ` [[0 1 ... 6]]`
* `column_vector` has shape `(6,1)`, and is formatted `[[0] [1] ... [6]]`
* **NOTE:** `row_vector` is a matrix `(1,6)` whereas `vector` is a **monodimensional array** `(6,)`
* Use `column_vector.reshape((6,))` to get back the original monodimensional vector


### **Factory methods** for simple construction of sequences


**Ranges.**

```python
np.arange(5)   # array([0, 1, 2, 3, 4])
```

```python
np.arange(1,5) # array([1, 2, 3, 4])
```

When arange is used with **float arguments**, it is generally not possible to predict the number of elements obtained, due to the finite floating point precision. For this reason, it is usually better to use the function `linspace` that receives as an argument the number of elements that we want, instead of the step:

```python
rf1 = np.arange(0.5, 1.05, 0.1)
rf2 = np.linspace(0.5, 1.0, 5)
(rf1, rf2)
```

**Unknown content, known size.** Often, the elements of an array are originally unknown, but its size is known. Hence, NumPy offers several functions to create arrays with initial placeholder content.

```python
np.ones(4)     # array([ 1.,  1.,  1.,  1.])
```

```python
np.ones( (4) )     # array([ 1.,  1.,  1.,  1.])
```

```python
np.empty( (3,3) )  # Doesn't initialize the array to any particular values (depends on memory state)
                   # array([[4.68624319e-310, 0.00000000e+000, 6.90367595e-310],
                   #        [6.90367595e-310, 6.90367595e-310, 6.90367595e-310],
                   #        [4.68624338e-310, 0.00000000e+000, 6.90369654e-310]])
```

```python
np.zeros( (2,2,3) ) # array([[[ 0.,  0., 0.],
                    #         [ 0.,  0., 0.]],
                    #        [[ 0.,  0., 0.],
                    #         [ 0.,  0., 0.]]])
```

```python
a7 = np.full((5,),7)  # array([7, 7, 7, 7, 7])
a7
```

```python
np.zeros_like(a7)  # array([0, 0, 0, 0, 0]) ; also ones_like, empty_like, full_like
```

```python
np.identity(3) # array([[1., 0., 0.],
               #        [0., 1., 0.],
               #        [0., 0., 1.]])
```

#### Legacy random infrastructure

```python
# Signature: rand(numd1,numd2,..) -- i.e., args give the shape
data1 = np.random.randn(2, 3) # Samples from the standard normal distribution
data2 = np.random.rand(2, 3) # will yield 6 float random numbers (from 0 to 1)
(data1, data2)
```

```python
# random.randint(low, high=None, size=None, dtype=int): Return random integers from low (inclusive) to high (exclusive).
ri1 = np.random.randint(10, size=5) # note: if high=None, then results are from [0,low)
ri2 = np.random.randint(5, 10, size=5)
ri3 = np.random.randint(low=-5, high=6, size=(2,3), dtype=np.int32)
(ri1, ri1.dtype, ri2, ri3)
```

Working with **random seeds**:

```python
ri1 = np.random.randint(10, size=(10))
np.random.seed(0)
ri2 = np.random.randint(10, size=(10))
np.random.seed(0)
ri3 = np.random.randint(10, size=(10))
ri4 = np.random.randint(10, size=(10))
(ri1, ri2, ri3, ri4)
```

### NumPy arrays vs. Python lists

NDArrays are

* transformation: more compact, and faster than list when the operation can be vectorised
* append to the end: slower than list
* usually homogeneous (homogeneity is exploited for performing fast operationss)

```python
lst = [1,2,3]
lst2 = [x * 2 for x in lst]

narr = np.array(lst)
narr2 = narr * 2

(lst2, narr2)
```

### Basic operations

#### **Arithmetic operators** apply **elementwise**

```python
a = np.array([20, 30, 40, 50])
b = np.arange(4) # array([0, 1, 2, 3])
c = a - b # array([20, 29, 38, 47])
d = b**2      # array([0, 1, 4, 9])
e = 10 * np.sin(a) # array([ 9.12945251, -9.88031624,  7.4511316 , -2.62374854])
f = a < 35    # array([ True,  True, False, False])
print('a = ', a,
      '\nb = ', b,
      '\na + 1 = ', a + 1,
      '\na - b = ', a-b,
      '\nb**2 = ', b**2,
      '\n10 * sin(a) = ', 10 * np.sin(a),
      '\na < 35 = ', a < 35
)
```

* Note: when `a` is a Python list, `a+a` **concatenates**; when `a` is a numpy array, `a+a` sums elemet-wise. 


Unlike in many matrix languages, the product operator `*` operates elementwise in NumPy arrays. The matrix product can be performed using the `@` operator (in python >=3.5) or the `dot` function or method:

```python
A = np.array([[1, 1],
              [0, 1]])
B = np.array([[2, 0],
              [3, 4]])
print(
    'A = \n', A,
    '\nB = \n', B, 
    '\nA + B = \n', A + B,     # elementwise sum
    '\nA * B = \n', A * B,     # elementwise product
    '\nA @ B = \n', A @ B,     # matrix product
    '\nA.dot(B) = \n', A.dot(B)  # another matrix product
)
```

#### **In-place modification.** 

Some operations, such as `+=` and `*=`, act in place to modify an existing array rather than create a new one.

```python
a = np.ones((2, 3), dtype=int)
a *= 3
a
```

#### Combining heterogeneous arrays

When **operating with arrays of different types**, the type of the resulting array corresponds to the **more general/precise** one (**upcasting**).

```python
a = np.ones(3, dtype=np.int32) 
b = np.linspace(0, 10, 3) #dtype=float64
c = a + b # c gets the most general type (float64)
(a,b,c, c.dtype)
```

#### **Aggregation operations.** 

Many unary operations, such as computing the sum of all the elements in the array, are implemented as methods of the ndarray class.

```python
a = np.arange(1,10)
(a.min(), a.max(), a.sum(), a.mean(), a.std(), a.var())

```

#### **Aggregation ops on multi-dim arrays** (e.g., min/max in matrices)

By default, these operations apply to the array as though it were **a list of numbers, regardless of its shape**. However, by specifying the axis parameter you can apply an operation along the specified axis of an array:

```python
b = np.array([[9, 8, 3], 
              [4, 0, 6]])
print('b = \n', b, 
      f'\nmin {b.min()} at index {b.argmin()}; ', 
      f'max {b.max()} at index {b.argmax()}',
      f'\nsum = {b.sum()}, mean = {b.mean()}, std = {b.std()}, var = {b.var()}'
)
print('b.min(axis=0) = ', b.min(axis=0), # min in row direction means computing the mins for each column
      '\nb.min(axis=1) = ', b.min(axis=1),
      '\nb.argmin(axis=0) = ', b.argmin(axis=0),
      '\nb.argmin(axis=1) = ', b.argmin(axis=1),
      '\nb.sum(axis=0) = ', b.sum(axis=0),
      '\nb.sum(axis=1) = ', b.sum(axis=1)
)
```

#### Algebraic operations with matrices 

Recall: matrices can be represented as ndarrays of rank 2.
Multiple functions are available for working with matrices.

```python
m = np.array([[1, 2], [5, 6]])
print(
    'm = \n', m,
    '\nm.T = m.transpose() = np.transpose(m) = \n', m.T,
    '\nnp.linalg.inv(m)) = \n', np.linalg.inv(m),
    '\nnp.linalg.det(m) = ', np.linalg.det(m),
    '\nm + m = \n', m + m,
    '\nm * m = (element-wise product) \n', m * m,
    '\nm @ m = np.dot(m,m) = m.dot(m)\n', m @ m
)
```

```python
# scalar product
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])
c = np.array([4, 5, 6]).reshape(3,1)
print(
    'a = ', a,
    '\nb = ', b,
    '\na.dot(b) = np.dot(a,b) = a @ b = ', a.dot(b),
    '\n(a@b).shape, type = ', (a@b).shape, type(a@b),
    '\na.dot(c) = np.dot(a,c) = a @ c = ', a @ c, # N.B.: matrix of shape (1,1)
    '\n(a@c).shape, type = ', (a@c).shape, type(a@c)
)
```

```python
# from 1x1 ndarray to scalar
s = np.array([[12]])
print('s = ', s, type(s), '\ns.item() = s.item((0,0)) = ', s.item(0,0), type(s.item()))
```

#### Product multiplication and vectors: details

Products involving vectors have to be handled differently **based on the ranks** of the operands.

* *both vectors of rank 2*: can use `.dot`, `@`, or `*` indifferently (with the exception in the case `rvec * cvec`which will perform the scalar multiplication)
* *left operand is rank 2 and right operand is rank 1*: must use `*` whereas `.dot` and `@` will result in an error
* *both vectors of rank 1*: using `@` will yield the scalar product; using `*` will perform element-wise product 

```python
rank1vec = np.array([0, 1, 2, 3, 4, 5])
print('rank1vec:', rank1vec, rank1vec.shape)
cvec = rank1vec.reshape((6, 1))
print('cvec (column vector):', cvec, cvec.shape)
rvec = rank1vec.reshape((1, 6))
print('rvec (row vector):', rvec, rvec.shape)
print()

# algebraic multiplication with vectors both of rank 2
print('cvec.dot(rvec):\n', cvec.dot(rvec),
      '\nc(vec.dot(rvec)) == (cvec @ rvec) == (cvec * rvec):', np.allclose(cvec.dot(rvec), cvec @ rvec, cvec * rvec), '\n')
```

```python
print(
    'rvec = ', rvec, rvec.shape, 
    '\ncvec = ', cvec, cvec.shape,
    '\n rvec * cvec = \n', rvec * cvec,
    '\n cvec * rvec = \n', cvec * rvec,
    '\n rvec @ cvec = !!!!!!!!\n', rvec @ cvec,
    '\n cvec @ rvec = \n', cvec @ rvec,
)
```

```python
# algebraic multiplication when right operand is a rank 1 vector
mul4 = cvec * rank1vec
print('cvec * rank1vec:\n', cvec * rank1vec,
      # cvec @ rank1vec, # ValueError - matmul: Input operand 1 has a mismatch in its core dimension 0
      # cvec.dot(rank1vec) # ValueError: shapes (6,1) and (6,) not aligned: 1 (dim 1) != 6 (dim 0)
)
```

```python
# algebraic multiplication when both operands are rank 1 vectors
print(
    'rank1vec = ', rank1vec, rank1vec.shape,
    '\nrank1vec * rank1vec = \n', rank1vec * rank1vec,
    '\nrank1vec @ rank1vec = ', rank1vec @ rank1vec,
    '\nrank1vec.dot(rank1vec) = ', rank1vec.dot(rank1vec),
)
```

### Universal functions

In NumPy, an **universal function**, or [ufunc](https://numpy.org/devdocs/reference/ufuncs.html), is a function that works on `ndarray`s in an **element-by-element** fashion, supporting array broadcasting, type casting, etc.
In other words, a ufunc is a **"vectorized" wrapper** for a classical function like `sin`, `sqrt`, etc.
Examples of ufuncs (available from `np`, e.g., `np.sin`):

* predicates: `all`, `any`, `nonzero`, `where`
* statistics: `average`, `mean`, `median`, `cov`, `var`, `std`
* bitwise ops: `invert`
* aggregation: `min`, `max`, `sum`
* math ops: `round`, `ceil`, `floor`
* matrices ops: `dot`


```python
a = np.arange(3) # array([0, 1, 2])
a_exp = np.exp(a) # array([1.        , 2.71828183, 7.3890561 ])
a_sqrt = np.sqrt(a) # array([0.        , 1.        , 1.41421356])
print('a = ', a, '\nnp.exp(a) = ', a_exp, '\nnp.sqrt(a) = ', a_sqrt)

b = np.array([2., -1., 4.])
c = np.add(a, b) # array([2., 0., 6.])
print(a, ' + ', b, ' = ', c)
```

### Indexing, slicing, iterating

* 1D-arrays can be indexed, sliced and iterated over, much like lists and other Python sequences.
* ND-arrays can have **one index per axis** (these indices are given in a tuple) and **slicing can be separately applied to each dimension**

```python
# indexing 

a = np.arange(6)**2 # [0,1,4,9,16,25]
a[1:4:2] = 77 # [0,77,4,77,16,25]
print('a = ', a, '\na[2] = ',a[2])
```

```python
# slicing
(
    a[2:], # [4, 77, 16, 25]
    a[::-1] # [25, 16, 77, 4, 77, 0]
)
```

```python
# iterating
for i in a:
    print(i**(1/2), sep=' ', end=' ')
```

Indexing with multidimensional arrays: `ndarr[indexAxis1, indexAxis2, ...]`, `ndarr[sliceAxis1, sliceAxis2, ...]`

* Note: When fewer indices are provided than the number of axes, the **missing indices are considered complete slices** `:`

```python
def f(x, y): return 10 * x + y
b = np.fromfunction(f, (5, 4), dtype=int)
print('b = ', b) # [ [ 0  1  2  3], [10 11 12 13], [20 21 22 23], [30 31 32 33], [40 41 42 43] ]
print('b[2,3] = ', b[2,3]) # 23
print('b[0:5, 1] = ', b[0:5, 1]) # [ 1 11 21 31 41]
print('b[:, 1:2] = ', b[:, 1:2]) # [ [ 1], [11], [21], [31], [41] ]
print('b[1:2] = ', b[1:3]) # [ [10 11 12 13], [20 21 22 23] ] (same as b[1:3, :]) (i.e., second and third rows)
print('b[-1] = ', b[-1]) # [40 41 42 43] (last row)
print('b[-1,-1::-1] = ', b[-1,-1::-1]) # [40 41 42 43] (last row, inverted)
```

NumPy also allows you to omit complete slices with `...`. E.g., `a[i]` is the same as `a[i,...]`. I.e. the dits `...` represent as many colons as needed to produce a complete indexing tuple.
E.g., if `a` is an array with 5 axes, then:

* `x[1,2,...]` is equivalent to `x[1,2,:,:,:]`
* `x[..., 3]` is eq. to `x[:,:,:,:,3]`
* `x[4, ..., 5, :]` is eq. to `x[4, :, :, 5, :]`


**Iterating over multidimensional arrays is done w.r.t the first axis**:

```python
a = np.array([[1, 2], [3, 4]])
for x in a: print(x) # [1 2] [3 4]
```

Iterating on all the items of a multi-dim array can be done by flattening it (`a.flat`)

```python
a = np.array([[1, 2], [3, 4]])
for x in a.flat: print(x) # 1 2 3 4
```

### Shape manipulation

Recall: An array has a `shape` given by the number of elements along each axis.

The shape of an array can be changed with various commands. The following functions return a **new array with modified shape**:

* `a.ravel()` returns a flattened array
* `a.reshape(dim1,...,dimN)` returns an array with modified shape
* `a.T` returns the transposed array (same as. `np.transpose(a)`)

whereas **`resize()` modifies the array itself**.

```python
a = np.array([[1, 2, 3], [4, 5, 6]])
print('a', a)
print('a.T', a.T)
print('a.ravel()', a.ravel())
print('a.reshape(6, 1)', a.reshape(6, 1))
print('a.reshape(1,6)', a.reshape(1,6))
print('a.reshape(6)', a.reshape(6))
print('a.reshape(6,0)', 'ValueError: cannot reshape array of size 6 into shape (6,0)')
a.resize(3,2) # in-place modification
print('after a.resize(3,2), a equals', a)

```

If (only one) dimension is given as `-1` in a reshaping operation, the other dimensions are automatically calculated:

```python
a = np.array([[1, 2, 3], [4, 5, 6]])
b = a.reshape(3, -1)
print(a, b)
```

### Stacking together different arrays

Several arrays can be stacked together along different axes.

* `hstack(seq_of_ndarrays)`: stack arrays in sequence horizontally (column-wise)
    * This is equivalent to concatenation along the second axis, except for 1-D arrays where it concatenates along the first axis. Rebuilds arrays divided by hsplit.
    * This function makes most sense for arrays with up to 3 dimensions.
    * Note: all the input array dimensions except for the concatenation axis must match exactly
* `vstack(seq_of_ndarrays)`: stack arrays in sequence vertically (row-wise)
    * This is equivalent to concatenation along the first axis after 1-D arrays of shape (N,) have been reshaped to (1,N). Rebuilds arrays divided by vsplit.
* `column_stack()` stacks 1d arrays as columns into a 2d array (same as `hstack` only for 2d arrays)
* In general, for arrays with more than two dimensions, `hstack` stacks along their second axes, `vstack` stacks along their first axes, and `concatenate` allows for an optional arguments giving the number of the axis along which the concatenation should happen.

```python
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])
c = np.vstack((a,b))
print('a = ', a, '\nb = ', b, 
      '\nc = np.vstack((a,b))', c, # with 1d arrays, these are just stacked vertically (i.e., become rows)
      '\nnp.hstack((a,b))', np.hstack((a,b)), # with 1d arrays, these are just concatenated horizontally into a single row
      '\nnp.column_stack((a,b))', np.column_stack((a,b)), # with 1d arrays, these become different columns
      '\nc.shape', c.shape,
      '\nnp.vstack((c,np.ones(3)))', np.vstack((c,np.ones(3))), # with 2d arrays, these are just stacked vertically, along the 1st axis
      '\nnp.vstack((c,c))', np.vstack((c,c)), # ditto
      '\nnp.hstack((c,c))', np.hstack((c,c)), # with 2d arrays, these are just concatenated horizontally, along the 2nd axis
      '\nnp.column_stack((c,c))', np.column_stack((c,c)) # with 2d arrays, same as hstack
      )
```

The functions `concatenate`, `stack` and `block` provide more general stacking and concatenation operations.

* `stack(seq_of_arrays, axis=0, out=None)`: Join a sequence of arrays along a **new** axis.
    * The `axis` parameter specifies the index of the new axis in the dimensions of the result. E.g., if axis=0 it will be the 1st dimension and if axis=-1 it will be the last dimension.
* `concatenate(seq_of_arrays, axis=0, out=None)`: Join a sequence of arrays along an **existing** axis.
* `block(arrays)`: Assemble an nd-array from nested lists of blocks.
    * Blocks in the innermost lists are concatenated (see concatenate) along the last dimension (-1), then these are concatenated along the second-last dimension (-2), and so on until the outermost list is reached.
    * Blocks can be of any dimension, but will not be broadcasted using the normal rules. Instead, leading axes of size 1 are inserted, to make block.ndim the same for all blocks. This is primarily useful for working with scalars, and means that code like np.block([v, 1]) is valid, where v.ndim == 1.
    * When the nested list is two levels deep, this allows block matrices to be constructed from their components.

```python
a = np.array([[1, 2, 3], [4, 5, 6]])
b = np.array([[7, 8, 9], [10, 11, 12]])
print('a = \n', a, '\nb = \n', b, 
      '\nnp.concatenate((a,b), axis=0) (same as vstack)\n', np.concatenate((a,b), axis=0), # same as vstack
      '\nnp.concatenate((a,b), axis=1) (same as hstack)\n', np.concatenate((a,b), axis=1), # same as hstack
      '\nnp.concatenate((a,b), axis=None)\n', np.concatenate((a,b), axis=None), # flattened
      '\nnp.stack((a,b), axis=0)\n', np.stack((a,b), axis=0), # the given matrices a and b become items in a new array
      '\nnp.stack((a,b), axis=1)\n', np.stack((a,b), axis=1), # the items of the outer array are the matrices of given by same rows for a and b
      '\nnp.block([a,b])\n', np.block([[a,b]]) # the given matrices a and b are concatenated along the 2nd axis
      )
```

In complex cases, `r_[]` and `c_[]` are useful for creating arrays by stacking numbers along one axis. They allow the use of range literals `:`.

* `r_`:  There are two use cases.
    1. If the index expression contains comma separated arrays, then stack them along their first axis.
    2. If the index expression contains slice notation or scalars then create a 1-D array with a range indicated by the slice notation.
* `c_`: Translates slice objects to concatenation along the second axis.
    * This is short-hand for `np.r_['-1,2,0', index expression]`, which is useful because of its common occurrence.

```python
np.r_[1:4, 0, 4] # array([1, 2, 3, 0, 4])
```

```python
a = np.arange(9).reshape(3,3)
print(
    'a = \n', a,
    '\nnp.c_[a, a[0], a[2]] = \n', np.c_[a, a[0], a[2]], # concatenates along the 2nd axis
    '\nnp.r_[a, a[0].reshape(1,3), a[2].reshape(1,3)] = \n', np.r_[a, a[0].reshape(1,3), a[2].reshape(1,3)], # concatenates along the 1st axis
    '\na[0] = ', a[0], '\ta[0].shape = ', a[0].shape,
    '\na[0].reshape(1,3)', a[0].reshape(1,3), '\t  a[0].reshape(1,3).shape = ', a[0].reshape(1,3).shape
)
```

### Splitting one array into several smaller ones

* `split(ary, indices_or_sections, axis=0)`: Split an array into multiple sub-arrays of **equal size**.
* Using `hsplit(array, indices_or_sections)`, you can split an array along its **horizontal axis (column-wise)**, either by specifying the number of equally shaped arrays to return, or by specifying the columns after which the division should occur
* `vsplit` splits along the vertical axis
* `dsplit(ary, indices_or_sections)`: Split array into multiple sub-arrays along the 3rd axis (depth).
    * same as `split(..., axis=2)`
* `array_split` allows one to specify along which axis to split
    * The only difference wrt `split()` is that `array_split` allows `indices_or_sections` to be an integer that does not equally divide the axis.


Splitting a 2d array:

```python
a = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9], [10, 11, 12]])
print(
    'a = \n', a,
    '\nnp.split(a, 2) = \n', np.split(a, 2), # implicitly on axis 0
    '\nnp.array_split(a, 3) = \t NOTE: np.split(a,3) is not possible due to unequally sized outputs\n', np.array_split(a, 3), # this allows for unequal division
    '\nnp.split(a, 3, axis=1) = \n', np.split(a, 3, axis=1), # note: on axis 0, we would get an error as we could get an equal division
)
```

```python
print(
    'a = \n', a,
    '\nnp.vsplit(a, 2) = \n', np.vsplit(a, 2), # splits along the 1st axis (i.e., vertically)... same as np.split(a, 2)
    '\nnp.hsplit(a, 3) = \n', np.hsplit(a, 3), # splits along the 2nd axis (i.e., horizontally)... same as np.split(a, 3, axis=1)
)
```

Splitting 3d arrays:

```python
a = np.arange(16).reshape(2,2,4)
print(
    'a = \n', a,
    '\nnp.dsplit(a, 2) = \n', np.dsplit(a, 2), # splits along the 3rd axis
    '\nnp.dsplit(a, [3,]) = \n', np.dsplit(a, [3,]) # splits along the 3rd axis at the given indices
)
```

### Copies and views

When operating and manipulating arrays, their data is sometimes copied into a new array and sometimes not. This is often a source of confusion for beginners. There are three cases:

1. **No copy at all**: 
    * Simple assignments make no copy of objects or their data.
    * Python passes mutable objects as references, so function calls make no copy.
2. **Views (shallow copies)**: Different array objects can **share** the same data. 
    * The `view` method creates a new array object that looks at the same data.
3. **Deep copy**
    * The `copy` method makes a complete copy of the array and its data.

TIP: you may check with property **`.base`** what is the base object; check if two ndarrays share memory with `np.shares_memory(a,b)`; and check if an array owns its data via property `flags.owndata`.


No copy at all (assignments, arguments of functions)

```python
a = np.arange(12).reshape(3,4)
b = a
print('a = \n', a, '\nb = \n', b, '\na is b = ', a is b, '\nnp.shares_memory(a,b) = ', np.shares_memory(a,b))

def f(a): return id(a)
print('\ndef f(a): return id(a)\nf(a) == id(a)? ', f(a) == id(a), '\nf(a) == f(a)? ', f(a) == f(a))
```

Views or shallow copies

```python
a = np.array([[1, 2, 3], [4, 5, 6]])
c = a.view()
d = a[:, 1:]
print(
    'a = \n', a,
    '\na.base is None = ', a.base is None, # Base object if memory is from some other object.
    '\nc = a.view()\n', c, '\na is c = ', a is c, '\nnp.shares_memory(a,c) = ', np.shares_memory(a,c),
    '\nc.base is a = i.e. is c a view of data owned by a? ', c.base is a, 
    '\nc.flags.owndata = ', c.flags.owndata,
    '\nd = a[:, 1:]\n', d, '\na is d = ', a is d, '\nnp.shares_memory(a,d) = ', np.shares_memory(a,d), '\nd.base is a = ', d.base is a
)
```

```python
# NOTE: creation by reshaping creates a view
e = np.arange(12).reshape(3,4)
print('e = np.arange(12).reshape(3,4)\n', e, '\ne.base is None = ', e.base is None, '\ne.flags.owndata = ', e.flags.owndata, '\ne.base = ', e.base)
```

Deep copies

```python
a = np.array([[1, 2], [3, 4]])
d = a.copy()  # a new array object with new data is created
print('a = \n', a, '\nd = \n', d, '\na is d = ', a is d, '\nnp.shares_memory(a,d) = ', np.shares_memory(a,d), '\nd.base is None = ', d.base is None)
```

**Tip**: Sometimes `copy` **should be called after slicing if the original array is not required anymore**. E.g., suppose a is a huge intermediate result and the final result b only contains a small fraction of a, a deep copy should be made when constructing b with slicing:


```python
a = np.arange(int(1e8))
b = a[:100].copy()
del a  # the memory of ``a`` can be released.
```

If `b = a[:100]` is used instead, a is referenced by b and will persist in memory even if `del a` is executed..


### Functions and methods overview

* array creation: `arange`, `array`, `copy`, `empty`, `empty_like`, `eye`, `fromfile`, `fromfunction`, `identity`, `linspace`, `logspace`, `mgrid`, `ogrid`, `ones`, `ones_like`, `r_`, `zeros`, `zeros_like`
* conversions: `ndarray.astype`, `atleast_1d`, `atleast_2d`, `atleast_3d`, mat
* manipulations: `array_split`, `concatenate`, `hstack`/`vstack`, `hsplit`/`vsplit`, `ravel` (flatten), `repeat`, `reshape`, `resize`, `squeeze`, `swapaxes`, `ŧake`, `transpose`
* predicates (questions): `all`, `any`, `nonzero`, `where` 
* statistics: `average`, `mean`, `median`, `cov`, `var`, `std`
* bitwise ops: `invert`
* operations: `choose`, `compress`, `cumprod`, `cumsum`, `inner`, `ndarray.fill`, `prod` `put`, `putmask`, `real`/`imag`, `sum`
* ordering: `argmax`, `argmin`, `argsort`, `max`, `min`, `ptp` (range of values, min to max, i.e., peak to peak), `searchsorted`, `sort`
* math ops: `round`, `ceil`, `floor`
* matrices ops / basic linear algebra: `dot`, `cross`, `outer`, `vdot`, `linalg.svd`


## Less basic

### Broadcasting rules

Broadcasting allows universal functions to deal in a meaningful way with inputs that do not have exactly the same shape.

* 1st RULE: **if all input arrays do not have the same number of dimensions, a “1” will be repeatedly prepended to the shapes of the smaller arrays until all the arrays have the same number of dimensions**.
* 2nd RULE: **arrays with a size of 1 along a particular dimension act as if they had the size of the array with the largest shape along that dimension**. The value of the array element is assumed to be the same along that dimension for the “broadcast” array.
* After application of the broadcasting rules, the sizes of all arrays must match.


### Advanced indexing

NumPy offers more indexing facilities than regular Python sequences. In addition to indexing by integers and slices, as we saw before, **arrays can be indexed by**:

* **arrays of integers**
* **arrays of booleans**




#### Indexing with arrays of indexes

* When the indexed array `a` is multidimensional, a single array of indices refers to the first dimension of `a`.
* We can also give indexes for more than one dimension. 
    * The **arrays of indices for each dimension** must have the **same shape**.
* Note: In Python, `arr[i, j]` is exactly the same as `arr[(i, j)]` so we can put i and j in a tuple and then do the indexing with that.
    * HOWEVER, we can not do this by putting i and j into an array, because this array will be interpreted as indexing the first dimension of a.
* You can also use indexing with arrays as a target to assign to
    * However, when the list of indices contains repetitions, the assignment is done several times, leaving behind the last value
    * This is reasonable enough, but watch out if you want to use Python’s += construct, as it may not do what you expect (the indexed element is incremented only once)



```python
a = np.array([[1, 2, 3], [4, 5, 6]])
print(
    'a = \n', a,
    '\na[[0,0,1]] = ', a[[0,0,1]], # selects the rows at the given indices -- N.B. same as a[([0,0,1],)] and a[[0,0,1],:]
    '\na[(0,0)]', a[(0,0)], # N.B. the two elements of the tuple are the indices for the two dimensions
    '\na[[0,0,1,1],[0,2,0,2]] = ', a[[0,0,1,1],[0,2,0,2]], # selects the elements at the given indices
)
```

```python
palette = np.array([[0, 0, 0],         # black
                    [255, 0, 0],       # red
                    [0, 255, 0],       # green
                    [0, 0, 255],       # blue
                    [255, 255, 255]])  # white
image = np.array([[0, 1, 2, 0],  # each value corresponds to a color in the palette
                  [0, 3, 4, 0]])
palette[image]  # the (2, 4, 3) color image
```

```python
a = np.arange(12).reshape(3, 4)
i = np.array([[0, 1],  # indices for the first dim of `a`
              [1, 2]])
j = np.array([[2, 1],  # indices for the second dim
              [3, 3]])
print(
    'a = \n', a,
    '\ni = \n', i,
    '\nj = \n', j,
    '\na[i, j] = \n', a[i, j], # selects the elements at the given indices
    '\na[i, 2] = \n', a[i, 2], # selects the elements at the given indices for the 2nd dim
    '\na[:, j] = \n', a[:, j] # selects the elements at the given indices for the 1st dim
)
```

```python
# let's use indexing to search the max value of time-dependent series
time = np.linspace(20, 145, 5)  # time scale (5 time points)
data = np.sin(np.arange(20)).reshape(5, 4)  # 4 time-dependent series
print(time, '\n', data) # note: each time series is a different column
max_index = data.argmax(axis=0)  # index of the max value for each series (e.g., the first timeseries has max value at 3rd timestep)
max_timeseries_per_timestep = data.argmax(axis=1) # index of the timeseries with the max value at each timestep
print('max_index = ', max_index)
print('max_timeseries_per_timestep = ', max_timeseries_per_timestep)
# times corresponding to the maxima
time_max = time[max_index]
print('time_max = ', time_max)
data_max = data[max_index, range(data.shape[1])] # => data[max_index[0], 0], data[max_index[1], 1], ...
print('data_max = ', data_max)
np.all(data_max == data.max(axis=0)) # True
```

```python
# indexing and assignment 
a = np.arange(5) # array([0, 1, 2, 3, 4])
print(a)
a[[1,3,4]] = 7 # array([0, 7, 2, 7, 7])
print(a)
a[[0,0,3,0]] = [5,9,6,8] # array([8, 7, 2, 6, 7])
print(a)
a[[0,0,1,1]] += 1
print(a) # array([9, 8, 2, 6, 7])
```

#### Indexing with arrays of booleans
