---
layout: post
title: "Python Map()/Filter()/Reduce()方法"
date: 2020-09-16
---

个人理解这几种方法是为了减少循环，让代码更精简/美观，其中`map`和`filter`方法应该可以和list comprehesion相互替代。

### Map()
`map`方法传入一个方法、以及一个列表，让这个函数作用于列表中每一个元素，得到结果。
比如要对一个数组每个元素都求平方，直接循环：
```python
l = [1,2,3,4,5]
res = []
for i in l:
    res.append(i ** i)
print(res) # [1, 4, 9, 16, 25]
```
使用`map`方法：
```python
def square(i):
    return i ** i
res = map(square, l)
print(res) # map返回的是一个迭代器对象，此时打印的话不是列表，而是内存对象
res = list(map(square, l))
print(res) # [1, 4, 9, 16, 25]
```
这个`square`函数比较简单，可以直接用lambda替代：
```python
res = list(map(lambda i: i**i, l))
print(res) # [1, 4, 9, 16, 25]
```
这个例子和使用以下list comprehension等效，个人感觉差别不大：
```python
res = [i**2 for i in l]
```

### Filter()
`filter`也是传入一个方法（过滤函数）、以及一个列表，返回满足判断条件的元素————返回的结果数量自然是小于等于输入的。
例如要过滤掉列表中小于3的元素，直接循环的方法：
```python
l = [1,2,3,4,5]
res = []
for i in l:
    if i >= 3:
        res.append(i)
print(res) # [3, 4, 5]
```
使用`filter`的话：
```python
def greater_than_3(i):
    return i >= 3
res = list(map(greater_than_3, l))
# Or with lambda
res = list(map(lambda x: x >= 3, l))
# Equivalent to
res = [i for i in l if i >= 3]
print(res) # [3, 4, 5]
```

### Reduce()
`reduce`是先对列表的头两个元素计算，再拿算完的结果和第三个元素计算新的结果，依次类推，直到最后一个元素算完。输入参数除了方法、和元素列表外，还有一个可选参数`initial`，表示计算的初始值。
例如要计算一个序列的和，直接循环的方法：
```python
l = [1,2,3,4,5]
sum = 0
for i in l:
    sum += i
print(sum)
```
使用`reduce`:
```python
from functools import reduce # reduce不是built-in函数
def sum_two(i, j):
    return i + j
res = reduce(sum_two, l)
# Or with lambda
res = reduce(lambda x, y : x + y, l)
print(res) # 1+2+3+4+5 = 15
```
如果要对这个序列和加上一个初始值`2`:
```python
res = reduce(lambda x, y : x + y, l, 2)
print(res) # 1+2+3+4+5+2 = 17
```
