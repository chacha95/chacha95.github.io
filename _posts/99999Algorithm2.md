---
layout: post
title: Sorting
tags: [Algorithm]
---

### Selection Sorting(선택 정렬)

정렬되지 않은 인덱스의 앞~끝에서 제일 작은 값 찾아 바꿈

시간 복잡도가 O(n^2)

> Selection sorting 예시

![](https://upload.wikimedia.org/wikipedia/commons/e/ea/Insertion_sort_001.PNG)

``` python
def swap(arr, idx1, idx2):
    tmp = arr[idx1]
    arr[idx1] = arr[idx2]
    arr[idx2] = tmp
    
def selection_sort(arr):
    for i in range(len(arr)):
        MIN_idx = i
        for j in range(i, len(arr)):
        	if arr[j] < arr[MIN_idx]:
        		MIN_idx = j
        
        swap(arr, i, MIN_idx)
    
    return arr
```

<br>

### Insertion Sorting(삽입정렬)

두번째 index부터 시작, 이전 index와 비교 더 작을시 앞에 삽입

시간 복잡도가 O(n) ~ O(n^2)

삽입 횟수가 적을수록 시간 복잡도가 내려감

> insertion sorting 예시

![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/insertion_sort-recursion.png)

```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i-1
        while(j>=0 and key<arr[j])
        	swap(arr, j, j+1)
            j -= 1
        arr[j+1] = key
   	
    return arr
```

<br>

### Bubble Sorting(삽입 정렬)

가장 기초적인 정렬 방식

시간 복잡도가 O(n^2)



> bubble sorting 구조

![](https://miro.medium.com/max/776/1*7QsZkfrRGhAu5yxxeDdzsA.png)



```python
def bubble_sort(arr):
    for i in range(len(arr)):
        for j in range(1, len(arr)):
            swap(arr, j, j-1)
    
    return arr
```



### Quick Sorting(퀵 정렬)

분할 정복 방식, pivot point를 기준으로 진행

왼쪽 배열, 오른쪽 배열 binary로 나눔

분할과 정렬을 동시에 하기에 merge sort보다 20% 정도 빠르다

시간 복잡도는 O(n*logn) 최악의 경우 O(n^2)

참고로 mergesort의 시간 복잡도는 O(n*logn)로 같지만 실제로 보면 quick sort가 일반적으로는 더  빠르다고 한다. [Medium ](https://medium.com/pocs/locality%EC%9D%98-%EA%B4%80%EC%A0%90%EC%97%90%EC%84%9C-quick-sort%EA%B0%80-merge-sort%EB%B3%B4%EB%8B%A4-%EB%B9%A0%EB%A5%B8-%EC%9D%B4%EC%9C%A0-824798181693)참고

```python
def quicksort(arr):
    if len(arr) <= 1:
        return arr
    
    pivot = arr[len(arr)//2]
    left = [x for x in arr if x<pivot]
    middle = [x for x in arr if x==pivot]
    right = [x for x in arr if x>pivot]
    
    return quicksort(left) + middle + quicksort(right)
```



