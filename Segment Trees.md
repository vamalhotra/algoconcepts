# Segment Trees

Segment Tree is a very useful data structure to work efficiently with range queries over a n-dimension array. It can be used to answer sum of a given range and update element in the range in O(log N) time. 

In list of various  sample applications below, it is assumed we are given an N element array A[0..N].

## Applications

Find sum of all elements in the given range i.e. sum over subarray A[i..j]

Update any element in the array i.e. A[i] = X

Find maximum or minimum element in the given range [i, j]

Find count of occurrence of maximum or minimum element in the range [i, j]

Find count of occurrences of a number in the range[i, j] or find index of k-th occurence of the number

Compute the GCD / LCM of all numbers of given ranges of the array

Find smallest index i s.t. sum of first i elements is greater than equal to X in O(log N) time

Find maximum sum sub-array in given range

Given three numbers (l,r,x), we have to find the minimal number in the segment 'a[l…r]' which is greater than or equal to x in poly-logarithmic (O log 2 n)



Construction

Problems