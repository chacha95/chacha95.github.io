---
layout: post
title: Data Structure
tags: [Algorithm]
---

Python을 이용해 만든 Data Structure입니다.

### Python Basic

```python
# python에서의 입력
# 입력은 무조건 문자열로 받기에 int형 casting이 자주 필요함
num = input()
num = int(num)

arr = []
arr.append(1)
print(arr)			# [1], arr의 맨 끝에 push
```



<br>

### Stack

선입후출구조

```python
class Stack():
    def __init__(self):
        self.arr = []
    
    def push(self, number):
        self.arr.append(number)
    
    def pop(self):
        if len(self.arr) == 0:
            print(-1)
        else:
            print(self.arr.pop())
    
    def size(self):
        print(len(self.arr))
    
    def empty(self):
        if len(self.arr) == 0:
            print(1)
        else:
            print(0)
    
    def top(self):
        if len(self.arr) == 0:
            print(-1)
        else:
            print(self.arr[-1])
```



<br>

### Queue

```python
class Queue():
    def __init__(self):
        self.list = []
    
    def push(self, element):
        self.list.append(element)
    
    def pop(self):
        if len(self.list) == 0:
            print(-1)
        else:
            print(self.list.pop(0))
    
    def size(self):
        print(len(self.list))
    
    def empty(self):
        
```



<br>



### Heap(Priority Queue)

```python

```



<br>

### Linked List

```python

```



<br>

### Hash Table

```python

```



<br>

### Tree

```python

```



<br>



