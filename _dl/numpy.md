---
layout: post
title:  "NumPy for Deep Learning"
date:   2017-12-15 11:44:00
---

### What is Numpy?
NumPy is a Python library that supports multi-dimensional arrays and matrices. Checkout 
[this page](/setup-mac) for installing NumPy on your system.

### Numpy and Deep Learning
At the basic level, deep learning solutions involve manipulating matrices. This includes
operations such as add, subtract, divide, multiply and transpose of matrices, for which 
NumPy provides very simple interfaces. 

### How to use NumPy
NumPy can be imported just like any Python library. 

```python
>>> import numpy as np

>>> a = np.array([10, 20, 30, 40])

>>> a.shape
(4,)

>>> a.reshape(2, 2)
array([[10, 20],
       [30, 40]])
       
>>> a.reshape(2, 2) * 2
array([[20, 40],
       [60, 80]])       
```
Please see [here](https://docs.scipy.org/doc/numpy-dev/user/quickstart.html) for complete reference.

### Array Vs Matrix
Array is a basic data structure supported in many programming languages, while Matrix is a
mathematical term. These two could be used inter-changeably, however they are not the same.
A matrix is implemented using an array. An array can be n-dimensional while matrix
is a rectangular array with two dimensions.

Operations on matrices follow the rules of linear algebra. In contrast, array operations 
behave according to the rules set by a given programming language.

### NumPy Array Vs NumPy Matrix
NumPy uses multi-dimensional array as the basic data type. There is also a method called mat() to 
create matrix from an array. This is more of a syntactical improvement since both NumPy array and
NumPy matrix data types can handle matrix operations.

NumPy matrices are strictly 2 dimensional and NumPy arrays are multi-dimensional. Practially,
the main difference is that the matrix data type allows matrix multiplication using "*" notation.
With NumPy arrays, you need to use the method numpy.dot() or "@" notation which is supported in 
Python versions 3.5 and above. See the examples below:

#### Matrix multiplication using np.mat
```python
>>> a = np.mat('1 2; 3 4')
>>> b = np.mat('5 6; 7 8')
>>> a*b
>>> matrix([[19, 22],
        [43, 50]])
```

#### Matrix mulitplication using np.array
```python
>>> a = np.array([[1,2], [3, 4]])
>>> b = np.array([[5,6], [7, 8]])
>>> a.dot(b)
array([[19, 22],
       [43, 50]])
>>> a@b
array([[19, 22],
       [43, 50]])
```

### Python List Vs NumPy Array
NumPy arrays are different from Python native lists (arrays). Python lists 
don't support elementwise operations such as addition or multiplication. This means you need to iterate
over each element in the list, which is much less efficient than the vectorized operations that
NumPy arrays support. This example illustrates the difference between the two data types:

```python
# Example: multiply each element with 10

# Python lists example

list = [ 1, 2, 3, 4, 5]

list = [ 10 * i for i in list]

print(list)
[10, 20, 30, 40, 50]

# NumPy array example
list = np.array([1, 2, 3, 4, 5])
list = list * 10

list
array([10, 20, 30, 40, 50])

```

### Matrix operations
See below for common matrix operations and the corresponding NumPy code.


#### Add/Subtract

**Adding two matrices**    

$$
\begin{bmatrix}1 & 5\\\ 2 & 3\end{bmatrix}
+
\begin{bmatrix}2 & 6\\\ 3 & 4\end{bmatrix}
=
\begin{bmatrix}3 & 11\\\ 5 & 7\end{bmatrix}
$$

**NumPy implementation**

```bash
>>> a = np.mat('1 5; 2 3')
>>> b = np.mat('2 6; 3 4')
>>> a + b
matrix([[ 3, 11],
        [ 5,  7]])
```

**Subtracting matrices**   

$$
\begin{bmatrix}1 & 5\\\ 2 & 3\end{bmatrix}
-
\begin{bmatrix}2 & 6\\\ 3 & 4\end{bmatrix}
=
\begin{bmatrix}-1 & -1\\\ -1 & -1\end{bmatrix}
$$

**NumPy implementation**

```bash
>>> a = np.mat('1 5; 2 3')
>>> b = np.mat('2 6; 3 4')
>>> a - b
matrix([[-1, -1],
        [-1, -1]])
```

### Multiplication (dot product)

$$
\begin{bmatrix}1 & 5\\\ 2 & 3\end{bmatrix}
x
\begin{bmatrix}2 & 6\\\ 3 & 4\end{bmatrix}
=
\begin{bmatrix}17 & 26\\\ 13 & 24\end{bmatrix}
$$


**NumPy implementation**

```bash
>>> a = np.mat('1 5; 2 3')
>>> b = np.mat('2 6; 3 4')
>>> a * b
matrix([[17, 26],
        [13, 24]])
```


### Divide by a scalar  

$$
\begin{bmatrix}10 & 50\\\ 20 & 30\end{bmatrix}
÷
2
=
\begin{bmatrix}5 & 25\\\ 10 & 15\end{bmatrix}
$$


**Numpy Implementation**

```python
>>> a = np.mat('10 50; 20 30')
>>> a
matrix([[10, 50],
        [20, 30]])
>>> a /2
matrix([[  5.,  25.],
        [ 10.,  15.]])
>>> a//2
matrix([[ 5, 25],
        [10, 15]])
```
### Transpose  

$$
A = \begin{bmatrix}10 & 50 & 60 \\\ 20 & 30 & 70\end{bmatrix}
\\
A ^ T = \begin{bmatrix}10 & 20 \\\ 50 & 30 \\\ 60 & 70\end{bmatrix}
$$

**NumPy implementation**
```python
>>> a = np.mat('10 50 60; 20 30 70')
>>> a
matrix([[10, 50, 60],
        [20, 30, 70]])
>>> a.T
matrix([[10, 20],
        [50, 30],
        [60, 70]])
```

### Inverse
In linear algebra, if you mutliply a matrix by its inverse, you will get an identity matrix.
Let's calculate the inverse of a matrix using NumPy.  

$$
\begin {bmatrix}4 & 8 \\\ 3 & 5\end{bmatrix} ^ {-1}
 = 
\frac{1}{4 \times 5 - 8 \times 3}  \begin{bmatrix}5 & -8 \\\ -3 & 4 \end{bmatrix}
= 
\begin {bmatrix}-1.25 & 2 \\\ .75 & -1\end{bmatrix}
$$


**NumPy implementation**
```python
>>> a = np.mat('4 8; 3 5')
>>> np.linalg.inv(a)
matrix([[-1.25,  2.  ],
        [ 0.75, -1.  ]])
>>> a * np.linalg.inv(a)
matrix([[ 1.,  0.],
        [ 0.,  1.]])
```

### Python Broadcasting
Broadcasting is a powerful feature of Python which allows us to deal with matrices of varying sizes.
Consider, you have a matrix of size 3x3. How can we do elementwise division of  all columns of 
this matrix by the same value?

See the example below for element wise divisions of two matrices.

$$
\begin {bmatrix}5 & 10 & 20 \\\ 10 & 20 & 40 \\\ 15 & 30 & 60\end{bmatrix}
./
\begin {bmatrix}5 & 10 & 20 \\\ 5 & 10 & 20 \\\ 5 & 10 & 20\end{bmatrix}
= 
\begin {bmatrix}1 & 1 & 1 \\\ 2 & 2 & 2 \\\ 3 & 3 &3 \end{bmatrix}
$$

Here the second array has repeating rows, which is redundant.

The Python broadcasting allows using a 1x3 matrix for the divisor so the second matrix can simply be
[5, 10, 20]

```python
>>> a = np.mat('5 10 20; 10 20 40; 15 30 60')

# just divide by 1x3 matrix
>>> a/[5, 10, 20]
matrix([[ 1.,  1.,  1.],
        [ 2.,  2.,  2.],
        [ 3.,  3.,  3.]])
```

Essentially, we divided a 3x3 matrix by a 1x3 matrix. Internally, Python copies the 1x3 matrix
three times to make it a 3x3 matrix and then use it as a divisor. The same principle applies for 
other operations such addition, multiplication and division.

The general idea is that if you multiply/divide/add/subtract a $$m\times n$$ matrix by $$m\times  1$$ matrix, 
the $$m\times 1$$ matrix will be 
horizontally copied n times to make it a $$m\times  n$$ matrix. Conversely, if the second matrix is of size 
$$1\times n$$, it will be vertically copied m times to create a $$m\times n$$ matrix before continuing the
given operation.
 