---
title: Python中遍历List时删除元素
date: 2019-07-13 00:00:00
tags: Python
---
## 场景复现

* 举一个简单的栗子

``` python
>>> numList = list(range(10))
>>> for i in range(len(numList)):
...     del(numList[i])
...
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
IndexError: list assignment index out of range
>>> print(numList)
[1, 3, 5, 7, 9]
```

* 这里还有另一个栗子

``` python
>>> numList = list(range(10))
>>> for i in numList:
...     numList.remove(i)
... 
>>> print(numList)
[1, 3, 5, 7, 9]
```

## 错误原因

* 不难发现在遍历python的list的时候删除该list的元素这个操作往往不会按照我们的预期运行
* 当使用for发起遍历操作时，for的遍历顺序已经确定了，删除操作会导致列表索引的变化，这可能引起
  * 元素没有按照预期删除
  * 数组访问越界

## 解决方案

* 删除操作在遍历操作之后(记录待删除元素)

  ``` python
  >>> delList = list() 
  >>> numList = list(range(10))
  >>> for i in numList:  
  ...     if i % 2 == 0:
  ...             delList.append(i)
  ... 
  >>> for i in delList: 
  ...     numList.remove(i)
  ... 
  >>> numList
  [1, 3, 5, 7, 9]
  >>> 
  ```

* 使用替代数组

  * Filter

    ``` python
    >>> numList = list(range(10))
    >>> numList = filter(lambda x : x % 2 == 0, numList)
    >>> print(numList)
    <filter object at 0x000002B358100470>
    >>> for i in numList:
    ...     print(i)
    ... 
    0
    2
    4
    6
    8
    ```

  * List

    ``` python
    >>> numList = list(range(10))                   
    >>> numList = [x for x in numList if x % 2 == 0]
    >>> numList
    [0, 2, 4, 6, 8]
    ```

* 遍历list的备份

  ``` python
  >>> numList = list(range(10))                   
  >>> for i in numList[:]:
  ...     if i % 2 == 0:
  ...             numList.remove(i)
  ... 
  >>> numList
  [1, 3, 5, 7, 9]
  ```

  

* 逆序遍历

  ``` python
  >>> numList = list(range(10))   
  >>> for i in range(len(numList) - 1, -1 , -1):
  ...     if numList[i] % 2 == 0:
  ...             del(numList[i])
  ... 
  >>> numList
  [1, 3, 5, 7, 9]
  ```

  