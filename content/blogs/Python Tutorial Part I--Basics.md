---
title: "Python Tutorial Part I--Basics"
date: 2017-04-01T12:52:36+06:00
image: images/blog/blog-post-1.jpg
author: Michael Gong
featured: true
# thumbnail: "/img/posts/PyTutorial1/pylogo.svg"
thumbnail: "/img/posts/PyTutorial1/pylogo.png"
tags: ["Python","Tutorial"]
---

```python
import this
```

    The Zen of Python, by Tim Peters
    
    Beautiful is better than ugly.
    Explicit is better than implicit.
    Simple is better than complex.
    Complex is better than complicated.
    Flat is better than nested.
    Sparse is better than dense.
    Readability counts.
    Special cases aren't special enough to break the rules.
    Although practicality beats purity.
    Errors should never pass silently.
    Unless explicitly silenced.
    In the face of ambiguity, refuse the temptation to guess.
    There should be one-- and preferably only one --obvious way to do it.
    Although that way may not be obvious at first unless you're Dutch.
    Now is better than never.
    Although never is often better than *right* now.
    If the implementation is hard to explain, it's a bad idea.
    If the implementation is easy to explain, it may be a good idea.
    Namespaces are one honking great idea -- let's do more of those!


# 1. Variable

## 1.1 Numerical variable


```python
a = 3
b = 5
```


```python
print(a+b)
print(a-b)
print(a*b)
print(a/b)
print(a**2 + b%2) # ** is the power operator, % is the mod operator
```

    8
    -2
    15
    0.6
    10


## 1.2 String variable


```python
c = 'hello world'
print(c)
```

    hello world



```python
print('hello' + ' world')
```

    hello world



```python
print(c + ' ' + str(a)) #string concatenation and type broadcasting
```

    hello world 3



```python
dir(c)
```




    ['__add__',
     '__class__',
     '__contains__',
     '__delattr__',
     '__dir__',
     '__doc__',
     '__eq__',
     '__format__',
     '__ge__',
     '__getattribute__',
     '__getitem__',
     '__getnewargs__',
     '__gt__',
     '__hash__',
     '__init__',
     '__iter__',
     '__le__',
     '__len__',
     '__lt__',
     '__mod__',
     '__mul__',
     '__ne__',
     '__new__',
     '__reduce__',
     '__reduce_ex__',
     '__repr__',
     '__rmod__',
     '__rmul__',
     '__setattr__',
     '__sizeof__',
     '__str__',
     '__subclasshook__',
     'capitalize',
     'casefold',
     'center',
     'count',
     'encode',
     'endswith',
     'expandtabs',
     'find',
     'format',
     'format_map',
     'index',
     'isalnum',
     'isalpha',
     'isdecimal',
     'isdigit',
     'isidentifier',
     'islower',
     'isnumeric',
     'isprintable',
     'isspace',
     'istitle',
     'isupper',
     'join',
     'ljust',
     'lower',
     'lstrip',
     'maketrans',
     'partition',
     'replace',
     'rfind',
     'rindex',
     'rjust',
     'rpartition',
     'rsplit',
     'rstrip',
     'split',
     'splitlines',
     'startswith',
     'strip',
     'swapcase',
     'title',
     'translate',
     'upper',
     'zfill']




```python
c.upper()
```




    'HELLO WORLD'




```python
c.replace('e','i')
```




    'hillo world'




```python
c.split(' ')
```




    ['hello', 'world']




```python
# string formatting
print("the value is %d"%(4))
print("another value is {}".format(5))
print("we can also format it as {1} and {0}".format(2,1))
print("the way I like, {first} and {second}".format(first=1,second=2))
```

    the value is 4
    another value is 5
    we can also format it as 1 and 2
    the way I like, 1 and 2


**More string formating, visit [here](https://pyformat.info/)**

## 1.3 Relational operator

| Symbol | Task Performed |
|----|---|
| == | True, if it is equal |
| !=  | True, if not equal to |
| < | less than |
| > | greater than |
| <=  | less than or equal to |
| >=  | greater than or equal to |


```python
a,b = 1,3 
print(a==b)
```

    False



```python
print(a == 1)
```

    True



```python
print(c == 'hello world')
```

    True



```python
print(a is 1)
```

    True



```python
# caveat to use "is", it compares the memory location rather than the value
a = 257
b = 256
print(a is 257)
print(b is 256)
```

    False
    True


# 2. Containers: list, tuple, and dictionary

## 2.1 List
**list is contructed by bracket [] .**


```python
mylist = ['apple', 'orange', 1, 23]
print(type(mylist))
```

    <class 'list'>



```python
# indexing and slicing
print(mylist[0],mylist[-1])
print(mylist[1:])
print(mylist[:-1])
```

    apple 23
    ['orange', 1, 23]
    ['apple', 'orange', 1]



```python
# append list
mylist.append('new element')
print(mylist)
# delete the last element
last_element = mylist.pop()
print(last_element)
print(mylist)
```

    ['apple', 'orange', 1, 23, 'new element']
    new element
    ['apple', 'orange', 1, 23]



```python
# list comprehension
mylist = [i for i in range(0, 10)]
print(mylist)
mylist = ['var'+str(i) for i in range(0,3)]
print(mylist)
mylist = [(i,j) for (i,j) in zip(range(0,3),range(2,5))]
print(mylist)
```

    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    ['var0', 'var1', 'var2']
    [(0, 2), (1, 3), (2, 4)]


## 2.2 Tuple
**tuple is constructed by parenthesis () **


```python
mytuple = ('apple','orange',1,23)
print(type(mytuple))
```

    <class 'tuple'>



```python
# indexing and slicing
print(mylist[0],mylist[-1])
print(mylist[1:])
print(mylist[:-1])
```

    apple 23
    ['orange', 1, 23]
    ['apple', 'orange', 1]



```python
# tuple is immutable (can't be changed)
mytuple[0] = 3
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-86-9ba0fd94a241> in <module>()
          1 # tuple is immutable (can't be changed)
    ----> 2 mytuple[0] = 3
    

    TypeError: 'tuple' object does not support item assignment


## 2.3 Dictionary
**dictionary is constructed by curly bracket {} **


```python
mydict = {'first':12,'second':32,'third':'string'}
print(type(mydict))
```

    <class 'dict'>



```python
print(mydict['first'])
print(mydict['third'])
```

    12
    string



```python
# adding new entry
mydict['new_entry'] = 43
print(mydict)
```

    {'second': 32, 'third': 'string', 'first': 12, 'new_entry': 43}



```python
# print keys
print(mydict.keys())
# print values
print(mydict.values())
# print items
print(mydict.items())
```

    dict_keys(['second', 'third', 'first', 'new_entry'])
    dict_values([32, 'string', 12, 43])
    dict_items([('second', 32), ('third', 'string'), ('first', 12), ('new_entry', 43)])


model1, model2

fct1 = model1.forecasts()
fct2 = model2.forecasts()

models = [model1, model2]

for model in models:
    fct = model.forecasts()

## 2.4 Iteration throught containers


```python
for i in range(4):
    print(mylist[i])

```


```python
mylist = ['apple', 'orange', 1, 23]
for element in mylist:
    print(element)
```

    apple
    orange
    1
    23



```python
mytuple = ('apple','orange',1,23)
for element in mytuple:
    print(element)
```

    apple
    orange
    1
    23



```python
mydict = {'first':12,'second':32,'third':'string'}
for key,item in mydict.items():
    print(key,item)
```

    second 32
    third string
    first 12



```python
# "in" operator
print('apple' in mylist)
print('fruit' in mytuple)
```

    True
    False



```python
# sometimes the index of iteration is useful

# ugly way
i = 0
for element in mylist:
    print('{} element is {}'.format(i,element))
    i += 1
```

    0 element is apple
    1 element is orange
    2 element is 1
    3 element is 23



```python
# Pythonic way
for i, element in enumerate(mylist):
    print('{} element is {}'.format(i,element))
```

    0 element is apple
    1 element is orange
    2 element is 1
    3 element is 23


# 3. Function

**basic syntax**

def funcname(arg1, arg2,... argN, *args, ***kwargs):
    
    ''' Document String'''

    statements


    return <value>


```python
# basic syntax
def add(a,b):
    '''This function perform addition'''
    result = a + b
    return result

help(add)
```

    Help on function add in module __main__:
    
    add(a, b)
        This function perform addition
    



```python
c = add(1,2)
print(c)
```

    3


**default arguments**


```python
def my_add(a,b=1):
    return a+b
c = my_add(2)
print(c)
d = my_add(2,5) # default parameters b get overwritten
print(d)
```

    3
    7


**arbitary arguments**


```python
def my_add2(a,b=1,*args):
    print("args type:",type(args))
    print("lenght of arguments:",len(args))
    print('a:',a)
    print('b:',b)
    print("arguments are:",args)
c = my_add2(1,4,3,4,5)
```

    args type: <class 'tuple'>
    lenght of arguments: 3
    a: 1
    b: 4
    arguments are: (3, 4, 5)


**key words**


```python
def my_add3(a,b=1,*args,**kwargs):
    print("args type:",type(args))
    print("kwargs type",type(kwargs))
    print("lenght of arguments",len(args))
    print('a:',a)
    print('b:',b)
    print("arguments are:",args)
    print('key words:',kwargs)
my_add3(1,2,3,4,5,option=3)
```

    args type: <class 'tuple'>
    kwargs type <class 'dict'>
    lenght of arguments 3
    a: 1
    b: 2
    arguments are: (3, 4, 5)
    key words: {'option': 3}


**local variable and global variable**


```python
def my_add4(a,b=3):
    c = a+b # c is local variable
    print("a+b:",c)
    # print('d:',var)

a,b = 1,4 # this is the global variables
my_add4(a,b)
```

    a+b: 5


# 4. Build-in function

## 4.1 range


```python
print(range(0,10)) # range function return a generator
```

    range(0, 10)



```python
print(list(range(0,10)))
```

    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]


## 4.2 map
Understand the usage of map is useful for parallel processing
basic syntax

ret = map(function, sequence)


```python
def myfunc(a):
    return a + 1

iterbles = [i for i in range(0,10)]
result = map(myfunc,iterbles)
print(result)
print(list(result))
```

    <map object at 0x7f6d405ce390>
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]


# 5. Flow control

**if**


```python
x = 12
if x >10:
    print ("Hello")
```

    Hello


** if else **


```python
x = 9
if x >10:
    print("Hello")
else:
    print("world")
```

    world


**if elif else**


```python
x = 9
x = 9
if x >10:
    print("Hello")
elif x>0 and x<10:
    print("x is between (0,10)")
else:
    print("world")
```

    x is between (0,10)


** while **


```python
x = 4
while x >0:
    print(x)
    x -= 1
```

    4
    3
    2
    1


** while break**


```python
x = 5 
while x > 0:
    print(x)
    if x == 3:
        break
    x -= 1
```

    5
    4
    3

