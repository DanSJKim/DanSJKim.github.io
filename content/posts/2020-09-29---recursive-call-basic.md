---
title: '[알고리즘]재귀 용법'
date: '2020-09-29T16:09:32.169Z'
template: 'post'
draft: false
slug: 'recursive-call-basic'
category: '알고리즘'
tags:
  - 'recursive'
description: '재귀 호출 이해 하기'
socialImage: 'https://i.kym-cdn.com/entries/icons/facebook/000/019/513/til.jpg'
---

# 재귀 용법 (recursive call, 재귀 호출)

- 함수 안에서 동일한 함수를 호출하는 형태
- 여러 알고리즘에 사용되므로, 익숙해져야 한다.
- 재귀함수는 내부적으로 스택처럼 관리된다.
- 파이썬에서 재귀함수의 깊이는 1000회 이하가 되어야 한다.

![recursion](https://www.globalsoftwaresupport.com/wp-content/uploads/2019/03/ezgif.com-crop-2.gif)



- 숫자가 들어있는 리스트의 합을 재귀함수를 이용해 구현하기

```python
def sum_list(data):
    if len(data) <= 1:
        return data[0]
    return data[0] + sum_list(data[1:])
```



- 회문 판별을 재귀함수를 이용해 구현하기

```python
def palindrome(string):
    if len(strung) <= 1:
        return True
    
    if string[0] == string[-1]:
        return palindrome(string[1:-1])
    else:
        return False
```



- 프로그래밍 연습 문제

```
문제: 정수 4를 1, 2, 3의 조합으로 나타내는 방법은 다음과 같이 총 7가지가 있음
1+1+1+1
1+1+2
1+2+1
2+1+1
2+2
1+3
3+1
정수 n이 입력으로 주어졌을 때, n을 1, 2, 3의 합으로 나타낼 수 있는 방법의 수를 구하시오
```

힌트: 정수 n을 만들 수 있는 경우의 수를 리턴하는 함수를 f(n) 이라고 하면,
f(n)은 f(n-1) + f(n-2) + f(n-3) 과 동일하다는 패턴을 다음과 같이 찾아나갈 수 있다.

![algopractice](/Users/seungjune/blog/DanSJKim.github.io/static/media/algopractice.jpg)

```python
def func(data):
    if data == 1:
        return 1
    elif data == 2:
        return 2
    elif data == 3:
        return 4
    
    return func(data -1) + func(data - 2) + func(data - 3)
```

