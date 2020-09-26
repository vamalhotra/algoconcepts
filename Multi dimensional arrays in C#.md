# Multi dimensional arrays in C# (Jagged vs rectangular arrays)

CSharp (C#, Chash) provides two ways to declare multi-dimensional array

1) Using comma notation - 2d array as [,] , 3d array as [,,], also called as rectangular arrays

2) Using multiple square brackets - 2d array as `[][]` , 3d array as `[][][]` , also called as jagged arrays.

The difference is that comma notation defines a rectangular matrix like 2-d array in which width and height are fixed i.e. each row of matrix will have same no. of columns.

In square brackets notation, we define an array of arrays or array of pointers where each row can have a array of variable length. While initializing, we only need to specify the length of first dimension. 

## Which one is better?

The comma notation array declares a contiguous block of memory of size `width * height`. In array of arrays, each 1-d array is a contiguous block of memory and the 2-d array is array of pointers. It's bit faster to access the array of arrays as it can directly access the pointer. But in practice, there's not much performance difference. You should choose the one you need i.e. if you do not need variable length arrays, you can just go with rectangular notation.

## How to initialize 2-d array in C#

Following initializes a 2-d array of arrays:

`var array2d = new int[][]{
                new int[] { 1, 2 },
                new int[] { 2, 3 },
                new int[] { 5 },
                new int[] { 0 },
                new int[] { 5 },
                new int[] { }
            };`

Following initializes a 2-d matrix:

`var matrix = new int[,]{
                { 1, 3 },
                { 2, 3},
            };`