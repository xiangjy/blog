---
title: Python实现斐波那契 
date: 2017-08-21 18:41:40
tags: [python, 算法]
---
斐波那契数列(Fibonacci sequence)，又称黄金分割数列，也称为"兔子数列"：
F(0)=0,F(1)=1,F(n)=F(n-1)+F(n-2) （n $\ge$ 2, n $\in$ N\*)。例如 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233，377，610，987，1597，2584，4181，6765，10946，17711，28657，46368........这个数列从第3项开始，每一项都等于前两项之和，而且当n趋向于无穷大时，前一项与后一项的比值越来越逼近黄金分割比例0.618。  

不同方法实现斐波那契：   

### 普通循环   

```python
#!/bin/env python
def fib(n):
    a, b = 0, 1
    for i in range(n):
        a, b = b, a + b
    return a
```

### 递归版本   
```python
#!/bin/env python
def fib(n):
    if 0 == n:
        return 0
    elif 1 == n:
        return 1
    else:
        return fib(n - 1) + fib(n - 2)
```

### 迭代器版本   
```python
class Fib():
    def __init__(self, n):
        self.a = 0
        self.b = 1
        self.n = n
        self.count = 0

    def __iter__(self):
        return self

    def next(self):
        result = self.a
        self.a, self.b = self.b, self.a + self.b
        if self.count > self.n:
            raise StopIteration
        self.count += 1
        return result
```

