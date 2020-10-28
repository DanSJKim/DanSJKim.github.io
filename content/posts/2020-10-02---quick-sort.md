---
title: '[알고리즘]퀵 정렬'
date: '2020-10-02T16:09:32.169Z'
template: 'post'
draft: false
slug: 'quick-sort'
category: '알고리즘'
tags:
  - 'recursive'
description: '퀵 정렬 이해 하기'
socialImage: 'https://i.kym-cdn.com/entries/icons/facebook/000/019/513/til.jpg'
---

# 퀵 정렬

- 분할 정복 방법을 통해 리스트를 정렬한다.

## 구현 방법

- 리스트 개수가 한개면 해당 리스트를 그대로 리턴 한다.
- 그렇지 않으면, 리스트의 맨 앞 원소를 pivot으로 지정한다.
- left, right 리스트 변수를 만든다.
- 맨 앞의 데이터를 제외한 나머지 데이터를 pivot과 비교한다.
- pivot보다 작으면 left 리스트에 원소를 저장한다.
- pivot보다 크거나 같으면 right 리스트에 원소를 저장한다.
- 분할된 두 개의 리스트에 대해 재귀적으로 이 과정을 반복한다.
  - return quickSort(left) + [pivot] + quickSort(right)

```python
def qsort(data):
    if len(data) <= 1:
        return data
    
    left, right = list(), list()
    pivot = data[0]
    
    for index in range(1, len(data)):
        if pivot > data[index]:
            left.append(data[index])
        else:
            right.append(data[index])
    
    return qsort(left) + [pivot] + qsort(right)
```

```python
import random

data_list = random.sample(range(100), 10)

qsort(data_list)
```

- 파이써닉하게 구현하기

```python
def qsort(data):
    if len(data) <= 1:
        return data
    
    pivot = data[0]

    left = [ item for item in data[1:] if pivot > item ]
    right = [ item for item in data[1:] if pivot <= item ]
    
    return qsort(left) + [pivot] + qsort(right)
```

```python
import random

data_list = random.sample(range(100), 10)

qsort(data_list)
```

## 알고리즘 분석

- 시간복잡도는 O(n log n)
  - pivot 왼쪽 오른쪽으로 1/2씩 나눠진다고 한다면 log n 만큼의 단계가 생성 된다.
  - 각 단계 별로 n 만큼 피벗과 비교한다.
- 최악의 경우 O(n2)
  - 맨 처음 pivot이 가장 크거나 가장 작은 경우 모든 데이터를 비교하는 상황이 나온다.
  - 독특한 경우이기 때문에 일반적으로 n log n 으로 표기한다.