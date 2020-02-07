---
layout: post
title: Python Algorithm 자주 쓰이는 함수
tags: [Algorithm]
---

### 입출력

```python
N = int(input())

# 여러 문자를 받을 수 있음
N, M = map(int, input().split())

# input에 비해 훨씬 빠름
# 런타임 오류가 나네요 근데??....
N = int(sys.stdin.readline())

# 소숫점 출력
num = 0.01
print('%.0f' % num) # 0.0
```



### 정렬

```python
arr = [1, 10, 5, 7 ,6]

arr.sort()	# [1, 5, 6, 7, 10]

arr.sort(reverse=True) # [10, 7, 6, 5, 1]

# 왠만하면 sorted 사용!
# 정렬된 결과를 반환, 원본 리스트는 건드리지 않음
arr_return = sorted(arr) # [1, 5, 6, 7, 10]

# sorted 함수는 key와 함께 사용 가능하다.
sorted(arr, key=lambda key: value)
```



### element 추가 방식

```python
# list
arr.append(x)

# dictionary
di['key'] = value


```



### 합

```python
avg = sum(num)
```



### collections

tuple과 dict에 대한 확장 데이터 구조를 제공

[Slideshare](https://www.slideshare.net/dahlmoon/collections-20160313) 자료에 자세히 설명 되있음

그 중에서 Counters()는 한 컨테이너에 동일한 자료가 몇 개인지를 파악하는데 사용하는 객체임

dictionary 자료형으로 반납해줌

```python
import sys
from collections import Counter

arr = [1, 3, 8, -2, 2]

arr.sort()
cnt = Counter(arr)
print(cnt)
# Counter({-2: 1, 1: 1, 2: 1, 3: 1, 8: 1})

cnt = Counter(arr)
sort_cnt = sorted(cnt.items(), key= lambda x: (x[0], x[1]))
if sort_cnt[0][1] == sort_cnt[1][1]:
  freq_value = sort_cnt[1][0]
else:
  freq_value = sort_cnt[0][0]

```



### lambda

함수를 딱 한줄만으로 만들게 해줄 수 있는 녀석임

```python
def hap(x, y):
    return x + y

hap(10, 20)

# labmda 식 표현
(lambda x,y: x+y)(10, 20)
```







<br>