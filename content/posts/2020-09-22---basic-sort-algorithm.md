---
title: '[알고리즘]버블정렬, 선택정렬, 삽입정렬'
date: '2020-09-22T16:09:32.169Z'
template: 'post'
draft: false
slug: 'basic-sort'
category: '알고리즘'
tags:
  - 'sort'
description: '기본 정렬 알고리즘'
socialImage: 'https://i.kym-cdn.com/entries/icons/facebook/000/019/513/til.jpg'
---

# 정렬 (sorting)

- 어떤 데이터들이 주어졌을 때 이를 정해진 순서대로 나열하는 것



## 버블 정렬

- 두 인접 데이터를 비교해서, 앞에 있는 데이터가 뒤의 데이터보다 크면 자리를 바꾸는 정렬 알고리즘

**코드**

```python
import random

def bubble_sort(data):
    for i in range(len(data)-1):
        swap = False
        for j in range(len(data)-i-1):
            if data[j] > data[j+1]:
                data[j], data[j+1] = data[j+1], data[j]
                swap = True
        if swap == False:
            break
    return data

data_list = random.sample(range(100), 50)
print(bubble_sort(data_list))

```

### 알고리즘 분석

- 반복문이 두 개 O(n2)

## 선택 정렬

- 첫 번째 자료를 두 번째 자료부터 마지막 자료까지 차례대로 비교하여 가장 작은 값을 찾아 첫번 째에 놓는 과정을 반복하여 정렬을 수행하는 알고리즘

```python
import random

def selection_sort(data):
    for i in range(len(data)-1):
        lowest = i
        for j in range(i+1, len(data)):
            if data[lowest] > data[j]:
                lowest = j
        data[i], data[lowest] = data[lowest], data[i]
    return data

data_list = random.sample(range(100), 50)
print(selection_sort(data_list))
```

### 알고리즘 분석

- 반복문이 두 개 O(n2)



## 삽입 정렬

- 손안의 카드를 정렬하는 방법과 유사하다.
  - 새로운 카드를 기존의 카드 사이의 올바른 자리를 찾아 삽입한다.
- 두번째 인덱스부터 시작하여 앞쪽 인덱스와 비교하여 삽입할 위치를 지정한 후 자료를 뒤로 옮기고 지정한 인덱스에 자료를 삽입하여 정렬하는 알고리즘이다.

**코드**

```python
import random

def insertion_sort(data):
    for index in range(len(data)-1):
        for index2 in range(index+1, 0, -1):
            if data[index2] < data[index2-1]:
                data[index2], data[index2-1] = data[index2-1], data[index2]
            else:
                break
    return data

data_list = random.sample(range(100), 50)
print(insertion_sort(data_list))
```

### 알고리즘 분석

- 반복문이 두 개 O(n2)