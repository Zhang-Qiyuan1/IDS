## Numpy

### Array Creation

#### 1. When know the specific numbers:
```Python
import numpy as np
a = np.array([2, 3, 4])
b = np.array([[2, 3, 4],[3, 4, 5]])
c = np.array([(2, 3, 4),(3, 4, 5)],dtype = np.float32)#using tuple in the list parameter maybe as the same as list.
```
For the basic usage, np.array() can only accept one list parameter and a 'dtype' parameter to set the type of the numbers in the ndarray. So, we can only pass like the above but not like 'b = np.array([2,3,4],[3,4,5])'.

#### 2. When we want to create a regular array
##### <1> Numbers in range
```Python
import numpy as np
a = np.arange(1, 10, 2)
#out [1, 3, 5, 7, 9]
b = np.arange(0, 2, 0.3)
#out [0.  0.3 0.6 0.9 1.2 1.5 1.8]
```
The three parameters represent start, end, step repectively.

However, when calculating the float numbers, there often exists the finite float points presicion. So, we're more like to use the 'np.linspace()' to express the float numbers.
```Python
import numpy as np
a = np.linspace(0, 2, 9) #9 numbers from 0 to 2
#out[0.  , 0.25, 0.5 , 0.75, 1.  , 1.25, 1.5 , 1.75, 2.  ]
```

#### <2> Some special array
```Python
import numpy as np
a = np.zeros((3, 4))
b = np.ones((2, 3))
c = np.empty((2, 3))
```
The function zeros creates an array full of zeros, the function ones creates an array full of ones, and the function empty creates an array whose initial content is random and depends on the state of the memory, which is only used for placing the position but without any meanings. By default, the dtype of the created array is float64, but it can be specified via the key word argument dtype.

#### <3> array with random numbers
```Python
import numpy as np
a = np.random.rand(2, 3)#random numbers from 0 to 1
b = np.random.randn(2, 3)#Standard normal distribution
c = np.random.randint(2, 3, (2, 3))#random intergers
```
It should be noted that in the first two usage, what we pass is the shape of the matrix(the first dim, the second dim and so on), but in the third usage, we need to pass (low, high, shape) to create the matrix correctly.

### Basic Operations
#### 1. Arithmetic operation
Arithmetic operators on arrays apply elementwise. A new array is created and filled with the result.
```Python
a = np.array([20, 30, 40, 50])
b = np.arange(4)
#out [0, 1, 2, 3]
c = a - b
#out [20, 29, 38, 47]
b**2
#out [0, 1, 4, 9]
10 * np.sin(a)
#out [ 9.12945251, -9.88031624,  7.4511316 , -2.62374854]
a < 35
#out [ True,  True, False, False]
```
Noted that the operator '*' operates elementwise in numpy arrays. When we want to do matrix product, we should use @ (Python >= 3.5) or .dot() to do this.
```Python
a = np.array([[1, 2], [3, 4]])
b = np.array([[2, 1], [1, 0]])
c = a * b
#out [[2, 2], 
#     [3, 0]]
d = a.dot(b) #equals to a @ b
#out [[4, 1],
#     [10, 3]]
```
When operating with arrays of different types, the type of the resulting array corresponds to the more general or precise one (a behavior known as upcasting).
That means, we can't do like this:
```Python
#WRONG!
import numpy as np
a = np.ones((2, 3), dtype = np.int32)
b = np.random.rand(2, 3)
a += b
```
Because 'a += b' will not create a new matrix but modify the 'a' itself, we cannot convert the 'int32' array to 'float64' array directly.

#### 2. Some functions
```Python
import numpy as np
b = np.arange(12).reshape(3, 4)
    #   [[ 0,  1,  2,  3],
    #    [ 4,  5,  6,  7],
    #    [ 8,  9, 10, 11]]
b.sum(axis = 0)
#out [12, 15, 18, 21]
b.min(axis = 1)
#out [0, 4, 8]
```
The axis parameter only has two values: 0 and 1, 0 means the operation was done by column, while 1 means by row.
```Python
b = np.arange(3)
#[0, 1, 2]
a = np.exp(b)
#[1.        , 2.71828183, 7.3890561 ]
c = np.sqrt(b)
#[0.        , 1.        , 1.41421356]
d = np.add(a, b)
```
All the operations are elementwise.

### Sliding the Array
#### 1. sliding with numbers
```Python
import numpy as np
b = np.arange(12)
a = b[2: 6: 2]
#[2, 4]
```
In the '[]', we use ':' to split start, end(exclusive) and step.Pay attention to the difference with the function 'arange()'.When there is nothing in front of the ':', it means the start equals to 0; meanwhile, if nothing is behind the ':', it means the end equals to the length of the array.

The multidimensional arrays has one index per axis, spliting with the ',' in '[]'.

Meanwhile, the multidimensional arrays allows us to write this using dots as 'b[i, ...]'. If b has 5 dims, 'b[1, ...]' means 'b[1, :, :, :, :]'

! ! ! It should noted that all the slides was a VIEW of the origin array. To completely copy a new array, we should explicitly use the .copy() function,like this:
```Python
b = np.arange(12)
a = b[2: 6: 2].copy()
```

#### 2. sliding with bool variable
```Python
import numpy as np
a = np.array([[1, 2, 3], [1, 3, 4], [1, 1, 5]])
b = a[(a[:, 1]) <= 2]
#out [[1 2 3]
#     [1 1 5]]
```
We can pass a bool variable in the '[]' to decide whether the pointing row will be added in the new array.

### Copies and Views
#### 1. Not copy at all
The '=' operation won't create any new array.
```Python
a = np.arange(12)
b = a
print(b is a)
#True   That means the address of a and b is the same.
```
That is because Python passes mutable objects as references.

#### 2. View or shallow copy
```Python
import numpy as np
a = np.arange(12).reshape(3, 4)
c = a.view()
print(c is a)
#False   This means that the addresses of c and a are different
```
