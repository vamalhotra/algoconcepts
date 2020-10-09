---
layout: post
---
# Numpy Arrays

Arrays in numpy are called as ndarray, n-dim array.

## Properties

- Rank = number of dimension of ndarray.
- Shape = tuple giving size along each dimension
- ndim = no. of dimensions
- size = total no. of elements across all dimensions
- dtype = type of elements in the array (leaf element type)

## Constructing array

np.array(python_list_or_tuple)

np.zeros(shape_tuple) : an ndarray of given shape filled with all zeros. If you want to fill with some value, use np.fill. For ones, np.ones(). np.empty() for nd array without initializing with 0's or 1's.

np.fill(shape_tuple, const_value, dtype = 'dtype') 

## Array Indexing

### Slicing

n[start : end : step] start default value is 0, end default value is length of array in that dimension, step default value is 1

::2 will return list containing alternate element

: -1 will return list containing all except last element (-1 means last element because if you go beyond 0, array loops back to last element, so -2 will be last second array)

[-1] will return just the last element

### Indexing 

Lists are passed for each dimension. 

`n[[0,1,2],[0,1,2]]` will return elements at (0,0), (1,1), (2,2)

`arr = np.array([[1,2,3],[4, 5, 6],[7, 8,9]])`

`print(arr[[0,1,2],[0,1,2]])`

Indexing using boolean: 

`cond = arr > 1`

`arr[cond]` 

### Other Useful functions

arr.max(), arr.max(axis=1), arr.sum(), arr.cumsum(axis=1)

arr1*arr2 #element-wise multiplication

arr1.dot(arr2) #matrix multiplication

np.sin(arr), np.exp(arr), np.sqrt(arr)

arr.reshape((2,8)) #a 4x4 array can be reshaped into 2,8.

arr.flatten('C') or arr.flatten('F') #row or col wise