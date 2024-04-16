<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap">
</head>

<body>

<div style="background-color: #65BF6F; color: black; text-align: center; padding: 10px; font-family: 'Roboto', sans-serif; font-size: 24px">
    

<div style="text-align:center;">
    <img src="Jaki_Charrua_gatodelespacio.jpg" alt="Drawing" style="width:150px; border-radius:90%;"/>
    
    
</div> 


<body>


    
</html>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://use.typekit.net/your-typekit-id.css">
</head>
<body>
    
    
<div style="background-color: #000000; color: white; text-align: center; padding: 10px; font-family: 'Proxima Nova', sans-serif; font-size: 24px;">
    Replication Lab  <br>
    GEC0319 | Time Series <br>
    
    
</div>



    
    
</body>
</html>

<h3 align="center" style="margin-top:20px">Tutorial 1: Introduction to Python</h3>
<br>

This tutorial is an introduction to the essentials of the Python programming language for purposes of the course.


<a href="#1.-Getting-Started<">Getting Started</a> <br>
<a href="#2.-Modules">Modules</a> <br>
<a href="#3.-Data-Types">Data Types</a> <br>
<a href="#4.-Data-Structures">Data Structures</a> <br>
<a  style="margin-left:20px" href="#4.-Data-Structures">Lists</a><br>
<a  style="margin-left:20px" href="#4.-Data-Structures">Dictionaries</a><br>
<a  style="margin-left:20px" href="#4.-Data-Structures">Tuples</a><br>
<a href="#5.-For-Loops">For Loops</a> <br>
<a href="#6.-Functions">Functions</a> <br>
<a href="#7.-If-Statements">If Statements</a> <br>
<a href="#8.-Arrarys">Arrays</a> <br>
<a href="#9.-Programming-Tips">Programming Tips</a> <br>
<a href="#10.-Exercises">Exercises</a> <br>

### 1. Getting Started

To get started, you can use your notebook as a calculator. For example:


```python
2 + 2
```




    4




```python
3/2
```




    1.5



Exercise: identify the use of following arithmetic operators: `+`,`−`,`∗`,`/`,`∗∗`,`%`.

The following statement assigns a value to the variable `x`. Because the variable does not yet exist, the assignment statements creates the variable.


```python
x = 5
x
```




    5




```python
x + 2
```




    7



Exercise: identify what the syntax `x += 2` does (we say that `+=` is an assignment operator).

The `print` function allows you to display output.


```python
print('For truth is always strange; stranger than fiction.') # Lord Byron (the # starts a comment)
```

    For truth is always strange; stranger than fiction.
    


```python
x = 10
print(x)
```

    10
    

The `print` function is a built-in function which is part of the core of the Python programming language. Another example of a built-in function is `abs`, which computes the absolute value of a number.


```python
abs(-2)
```




    2



### 2. Modules

The Python language by design has a small core. Mst of the fuctionality that we need is in modules or packages that we need to explicity load into our session. There are two ways to do this: either by loading the entire modulue (or a submodule) or a specific function that we need.


```python
import math
math.sqrt(4)
```




    2.0




```python
from math import sqrt 
sqrt(4)
```




    2.0



We will use a number of different Python libraries thoughout this course, including Pandas (data processing),  Matplotlib (plotting), Seaborn (to make plots elegant), StatsModels (statistics), NumPy (scientific computing), and Scikit-Learn (machine learning).

### 3. Data Types

**3.1 Boolean variables**

The most basic data type is a `Boolean` variable, which can be either `True` or `False`.


```python
x = False
print(x)
```

    False
    


```python
x = 2 > 0
print(x)
```

    True
    

Exercise: identify the use of following comparison operators: `==`, `!=`, `>=`, `<=`.

Exercise: explain the code below in detail. You may want to break this down into four steps. 


```python
x = 3 % 2 == 0
print (x)
```

    False
    

In numerical expressions, a `False` is automatically converted to zero and a `True` is converted to one. For example:


```python
x= True
y = 2*x
print(y)
```

    2
    

**3.2 Numbers**

There are two main built-in numerical data types, signed integers (`int`) and floating point real values (`float`).


```python
x = 1 
type(x)
```




    int




```python
x = 1.0
type(x)
```




    float



Sometimes, we need to do a explicit type conversion (typecasting), as the next example shows.


```python
a = 2
x = a > 0
y = int(x)
print(y)
```

    1
    

**3.3 Strings**

String variables represent text data.


```python
sentence = 'For truth is always strange; stranger than fiction.' 
type(sentence)
```




    str



As we are going to see in a text analytics application, Python has sophisticated capability for string manipulation. 

### 4. Data Structures

In computer science, a data structure is a way to store and organise data for efficient retrieval and modification. The four basic Python data structures lists, dictionaries, tuples and arrays. We introduce the first three in this section.

**4.1 Lists**

A list is a sequence of values. The values in a list, known as elements or items, can be of any type. To create a list, we enclose the elements in brackets `[ ]`. 


```python
a = [ ] # empty list
b = [1, 2, 5, 10] # list of four numbers
cities = ['Sydney', 'Melbourne', 'Brisbane']
c = [2, 4, 'Sydney'] # list mixing different variable types.
```

There are several list methods and operations that you should be familiar with.  The `append` method inserts a new element to the end of the list.


```python
cities.append('Perth')
print(cities)
```

    ['Sydney', 'Melbourne', 'Brisbane', 'Perth']
    

The `len` function counts the number of items in a list. It also works for counting the number of items in other types of containers.


```python
len(cities)
```




    4



We retrieve elements by passing the numerical index. What is crucial for you to know is that numerical indexes start from zero in Python. Here are some examples: 


```python
cities[0] # first element
```




    'Sydney'




```python
cities[2] # third element
```




    'Brisbane'




```python
cities[-1] # last element
```




    'Perth'



Often, we need to retrieve a slice of a list. This can be a bit confusing initially, so here are several examples. 


```python
cities[:2] # first two elements/all elements up to index 1 
```




    ['Sydney', 'Melbourne']




```python
cities[1:3] # elements in indexes 1 to 2 (the element in index 3 is not part of the slice)
```




    ['Melbourne', 'Brisbane']




```python
cities[1:] # all elements from index 1 onwards
```




    ['Melbourne', 'Brisbane', 'Perth']




```python
cities[-2:] # last two elements
```




    ['Brisbane', 'Perth']



The `+` operator concatenates lists. 


```python
a = [1, 2]
b = [3, 5, 10]
a + b
```




    [1, 2, 3, 5, 10]



The `in` expression allows to check if a certain item is present in a list. 


```python
'Sydney' in cities
```




    True




```python
'Copenhagen' in cities
```




    False



It is also useful to know how to sort lists. The sorted function will return a sorted copy of an object. 


```python
sorted(cities)
```




    ['Brisbane', 'Melbourne', 'Perth', 'Sydney']



In contrast, the `sort()` method will modify the list itself by sorting it. 


```python
print(cities)
cities.sort()
print(cities)
```

    ['Sydney', 'Melbourne', 'Brisbane', 'Perth']
    ['Brisbane', 'Melbourne', 'Perth', 'Sydney']
    

**4.2 Dictionaries**

A dictionary is a collection of key-value pairs. We create a dictionary by providing the key-value pairs within curly brackets `{ }`. For example, in the dictionary below the keys are the names of the cities and the values are the population of each city. 


```python
population = {'Sydney': 5230330, 'Melbourne': 4936349, 'Brisbane' : 2462637}
```

We retrieve a value by referring to the key.


```python
population['Sydney']
```




    5230330



Another way to create a dictionary is as follows. 


```python
address = {} # empty dictionary
address['country'] = 'Australia'
address['state'] = 'NSW'
address['postcode'] = 2006
print(address)
```

    {'country': 'Australia', 'state': 'NSW', 'postcode': 2006}
    

**4.3 Tuples**

A tuple is an immutable list: we can neither modify the elements of a tuple nor insert or remove items from it. We usually create a tuple by enclosing the elements in parentheses `( )`. 


```python
a = (1, 2, 'cat', 'dog')
print(a)
```

    (1, 2, 'cat', 'dog')
    

It's also possible to create a tuple without the parentheses in the syntax, though this can make the code less clear. 


```python
a = 1, 2, 'cat', 'dog'
print(a)
```

    (1, 2, 'cat', 'dog')
    

A useful operation is tuple unpacking, shown in the next two examples. 


```python
numbers = (1, 2)
a, b = numbers
print(a)
print(b)
```

    1
    2
    


```python
[*numbers, 3]
```




    [1, 2, 3]



### 5. For Loops

Often, we need to traverse a list and run code that takes each item as an input. We use a `for` block to do this.


```python
cities = ['Sydney', 'Melbourne', 'Brisbane', 'Perth']

for city in cities:
    print(city)
```

    Sydney
    Melbourne
    Brisbane
    Perth
    

There are two important details to note in this syntax. The `for` loop would work with any alias instead of `city`, as long as we use it consistently. However, we say that choosing a meaningful alias makes the code more *Pythonic* (clean and readable).  

Each iteration of the loop will repeat the code in the indented part of the block, below the `for` statement. In order for the syntax to be correct, the indentation needs to be four spaces. The editor adds it automatically.

Here's another example. 


```python
numbers = [1, 2, 5, 10]

for number in numbers:
    x = number**2
    print(x) 
    
print('The code then continues from here') # outside the for block
```

    1
    4
    25
    100
    The code then continues from here
    

For loops are applicable to any iterable objects. We commonly write loops over a numerical range, as the next two examples show. 


```python
for i in range(3):
    print(i)
```

    0
    1
    2
    


```python
for i in range(1, 11, 2): # starts at 1, ends before 11, step size 2
    print(i)
```

    1
    3
    5
    7
    9
    

The `enumerate` function is useful for obtaining an indexed list: 


```python
cities = ['Sydney', 'Melbourne', 'Brisbane', 'Perth']

for i, city in enumerate(cities):
    print(f'City {i}: {city}')
```

    City 0: Sydney
    City 1: Melbourne
    City 2: Brisbane
    City 3: Perth
    

### 6. Functions

In programming, a function is a piece of code that (optionally) takes inputs, performs a set of instructions, and (optionally) returns an output. 


```python
def square(x):
    return x**2

y = square(4)
print(y)
```

    16
    

Here's an example of a function that has no input or output. 


```python
import time 

def today():
    date = time.strftime("%d/%m/%Y")
    print(f'Today is {date}')
    
today()
```

    Today is 08/08/2019
    

When calling a function, we can use positional and keyword arguments. In this next example, we use positional arguments only, which means that Python will assign 2 and 3 to parameters`x` and `p` respectively.


```python
def power(x, p):
    return x**p

y = power(2,3)
print(y)
```

    8
    

The next example does exactly the same, but based on keyword arguments. 


```python
y = power(x=2,p=3)
print(y)
```

    8
    

When using keyword arguments, the inputs do not need to be in any particular order. 


```python
y = power(p=3,x=2)
print(y)
```

    8
    

We can also mix positional and keyword arguments, but in this case the positional arguments need to come first. 


```python
y = power(2, p=3)
print(y)
```

    8
    

Many functions that you will be using have default arguments. It's important for you to pay attention to these default values and ask if they make sense for your current application. 


```python
def hello(name='user'):
    print(f'Hello {name}!')

hello('John')
hello()
```

    Hello John!
    Hello user!
    

### 7. If Statements

An if statement evaluates if an expression is `True` or `False`, and executes different code accordingly. For example, suppose that we want to code a function to calculate the absolute value of a number, defined as

\begin{equation}
|x|=\begin{cases}
x & \text{if $x\geq0$}\\
-x & \text{if $x<0$}.
\end{cases}
\end{equation}


```python
def absolute(x):
    if x >= 0:
        return x
    else:
        return -x

y = absolute(-2)
print(y)
```

    2
    

As another example, below we code a function that raises a customised error message if the input is invalid.


```python
def log(x):
    if x <= 0:
        raise ValueError('Wake up mate!! The log of zero or a negative number does not exist.')
    else:
        return math.log(x)

log(2)
```




    0.6931471805599453



Now, try taking the log of zero and see what happens. 

### 8. Arrays

[NumPy](https://numpy.org/) is the fundamental package for scientific computing in Python. NumPy arrays are data structures that we use to represent and store vectors and matrices. This is very important to us, because all learning algorithms in this course are based on operations on vectors and matrices which typically happen behind the scenes. 

For example, consider the following vector.

\begin{equation}
a = \begin{pmatrix} 5 \\ -2 \\ -3\end{pmatrix}
\end{equation}

We represent this vector in Python as follows. 


```python
import numpy as np

a = np.array([5, -2, -3])
a
```




    array([ 5, -2, -3])



In the same way, we use a two-dimensional NumPy array to represent a matrix. Consider for example the following square matrix.

\begin{equation}
B=\begin{bmatrix} 
1  & 2 \\
5  & -4  \\
\end{bmatrix}
\end{equation}

We create it as follows:


```python
# the input is a list, where each item is itself a list representing a row 
B = np.array([[1,2],[5,-4]])
B
```




    array([[ 1,  2],
           [ 5, -4]])



The `ndim` and `shape` properties allow us to retrieve the number of dimensions and the dimensions themselves of Numpy arrays.


```python
a.ndim # a is a one-dimensional array 
```




    1




```python
B.ndim # B is a two-dimensional array 
```




    2




```python
a.shape # a has three elements along the first and only dimension
```




    (3,)




```python
B.shape # B has 2 rows (first dimension) and two columns (second dimension)
```




    (2, 2)



Retrieving elements and slices works similarly to what we do for lists, except that we need to keep track of multiple dimensions. Here are some examples.


```python
B[1,0] # element in the second row, first column
```




    5




```python
B[:,1] # second column
```




    array([ 2, -4])




```python
B[-1,:] # last row
```




    array([ 5, -4])



### 9. Programming Tips

1. Programming is a completely logical process. Your code will only generate the correct result if it is entirely correct, both in terms of the syntax and the logical consistency of what you are trying to do. Otherwise, you get an error message or the wrong result. This will happen all the time, whatever your level of programming ability. Just go back and find out where the problem is. Troubleshooting is a very important skill.

2. Read the error messages. Students very often ask why their code is not working when the error message already says what the problem is.

3. Not even a full unit on programming would be able to cover all scenarios. You should use the package documentation and internet searches frequently to find out how to do things and fix problems. It's important to conceptualise and articulate clearly what you're trying to do, then you discover how to implement that in Python.

### 10. Exercises

Do the exercises below if you're looking for a challenge. Some are hard. Use the Internet to figure out how to do things that you don't know how to implement yet. For example, try searching how to copy a Python list – you'll need it. 

The solutions are collaborative on Ed. Help your colleagues by posting yours. 

**Exercise 1**

**(a)** Print the third element in the list below.

**(b)** Print elements 3 to 6. 

**(c)** Print the last element.


```python
a=[1,2,3,4,5,6,7,8,9,10]
print(type(a))
print(a)
print(len(a))
```

    <class 'list'>
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    10
    

**Exercise 2**

Write a function called `middle` that takes a list and returns a new list that contains all but the first and last elements (Think Python 10-3). You can test it on the list below.


```python
a=[1,2,3,4]
```

**Exercise 3**

Write a function called `chop` that takes a list and modifies it by removing the first and last elements (Think Python 10-4). Think about the difference between this exercise and the previous one. 

**Exercise 4**

Write a function called `is_sorted` that takes a list as a parameter and returns `True` if the list is sorted in ascending order and false otherwise (Think Python 10-5).

**Exercise 5**

Write a function called `k_largest` that takes a list of numbers and a parameter `k` and returns the k largest numbers in the list. 

**Exercise 6**

Two words are anagrams if you can rearrange the letters from one to spell the other. Write a function called `is_anagram` that takes two strings and returns True if they are anagrams (Think Python 10-6). 

**Exercise 7**

Write a function that takes three numbers as separate inputs and returns the largest of the three. Do this by building a list. 

**Exercise 8**

Create a function `first_k_digits` that takes a number and returns its first k digits as a list. Test it by obtaining the first 5 digits of $\pi$.

**Exercise 9**

Write a function `median` that takes a list of numbers and returns the median. You can assume that the size of the list is an odd number. 

**Exercise 10**

**(a)** Write a function that gets a text and a word as inputs and returns `True` if the word appears in the text, and false otherwise. The following methods are useful for this task: 

1. The string [<TT>lower</TT>](http://www.tutorialspoint.com/python/string_replace.htm) method.
2. The string [<TT>replace</TT>](http://www.tutorialspoint.com/python/string_replace.htm) method to remove special characters. 
3. The string [<TT>split</TT>](http://www.tutorialspoint.com/python/string_split.htm) method.

You can text your function with Shakespeare:

<blockquote>
We are such stuff as dreams are made on; and our little life is rounded with a sleep.
</blockquote>

Beyond the exercise, the Python [Natural Language Toolkit](http://www.nltk.org/) has functions for text processing tasks of this type.

### 11. Credits

Credits to [Dr Marcel Scharth](https://www.linkedin.com/in/marcel-scharth-aa5bab112/?originalSubdomain=au)
