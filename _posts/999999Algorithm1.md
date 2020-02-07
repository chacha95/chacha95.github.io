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

DFS 구현에 사용

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

FIFO(First In First Out)

BFS 구현에 사용

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

### Linked List

- 장점

  dynamic한 data structure

  Insert와 delete가 쉬움

- 단점

  pointer의 사용으로 메모리를 더 사용함

  리스트의 시작지점부터 읽어야하는 문제가 존재

> Linked list

![](https://camo.githubusercontent.com/49cf765ec71d71896a0cf179e9f679923e96c1e4/68747470733a2f2f75706c6f61642e77696b696d656469612e6f72672f77696b6970656469612f636f6d6d6f6e732f7468756d622f362f36642f53696e676c792d6c696e6b65642d6c6973742e7376672f3130323470782d53696e676c792d6c696e6b65642d6c6973742e7376672e706e67)



> insert

![](https://camo.githubusercontent.com/9a17213f9fa47222103f5ac0fa162a45ec877488/68747470733a2f2f75706c6f61642e77696b696d656469612e6f72672f77696b6970656469612f636f6d6d6f6e732f7468756d622f342f34622f4350542d4c696e6b65644c697374732d616464696e676e6f64652e7376672f3130323470782d4350542d4c696e6b65644c697374732d616464696e676e6f64652e7376672e706e67)



> delete

![](https://camo.githubusercontent.com/3b5f084492c5f4c2d71c9e8e55b616d2d21bd67e/68747470733a2f2f75706c6f61642e77696b696d656469612e6f72672f77696b6970656469612f636f6d6d6f6e732f7468756d622f642f64342f4350542d4c696e6b65644c697374732d64656c6574696e676e6f64652e7376672f3130323470782d4350542d4c696e6b65644c697374732d64656c6574696e676e6f64652e7376672e706e67)



<br>

### Binary Heap

- 장점

  Insert, delete시 O(log n)

- 단점

  search 시 O(n)

  

> Binary Heap 구조

![](http://upload.wikimedia.org/wikipedia/commons/thumb/3/38/Max-Heap.svg/480px-Max-Heap.svg.png)

<br>

### Hash Table

- 장점

  search시 O(1) 소요(worst O(n))

  insert와 delete가 쉬움

  Key에 대한 Value가 있는지 혹은 중복의 확인이 쉬움

- 단점

  Collision이 일어날 수 있음

  e.g. 알파벳을 키로 하는 해쉬 테이블 공간을 10개로 놓았을 경우, 충돌이 생길수 있음 이런 문제를 해결하기 위해 별도의 자료구조가 필요함!

Hash Table은 Key와 Value로 저장하는 data structure입니다.

Key를 통해 바로 데이터를 받아올 수 있으므로, 속도가 획기적으로 빠릅니다.

파이썬에선 dictionary 타입으로 구현되어 있습니다.(별도 구현이 필요 없음)

> Hash Table

![](https://www.fun-coding.org/00_Images/hash.png)



```python
# dictionary
# key:value로 구성되 있다
dic = {'name':'pey', 'phone':'01199333'}

# 추가
d[2] = 3
d.update({2:3})

# 삭제
del a[1]

# key를 써 value 얻기
grade['key'] # value값 얻음

# key, value 얻기, 리스트로 반환
a.keys()
a.values()
a.items #둘다 얻을 수 있음
```



<br>

### Binary Search Tree

- 장점

  insert, search시 평균 O(log n), 최악 O(n)

- 단점

  insert시 최악 O(n^2)

> Insert

![](https://camo.githubusercontent.com/1dd146a2c589074854a88cb2e7ff150eadbe904e/68747470733a2f2f626c6f672e70656e6a65652e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031352f31312f62696e6172792d7365617263682d747265652d696e73657274696f6e2d616e696d6174696f6e2e676966)



> Search

![](https://camo.githubusercontent.com/b764eda092d9faa38cba53a6ae874bf0a5f6b7ca/68747470733a2f2f7777772e6d61746877617265686f7573652e636f6d2f70726f6772616d6d696e672f696d616765732f62696e6172792d7365617263682d747265652f62696e6172792d7365617263682d747265652d736f727465642d61727261792d616e696d6174696f6e2e676966)



> delete

![](https://user-images.githubusercontent.com/31475037/64469074-41f62d80-d167-11e9-8551-a0a1f525a8a4.PNG)







<br>



