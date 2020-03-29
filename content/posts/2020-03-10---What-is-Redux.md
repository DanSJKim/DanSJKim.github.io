---
title: 'What is Redux?'
date: '2020-03-10T16:09:32.169Z'
template: 'post'
draft: false
slug: 'what-is-redux'
category: 'Redux'
tags:
  - 'Redux'
  - 'TIL'

description: 'Redux에 대해 알아보자'
socialImage: 'https://miro.medium.com/max/2800/0*U2DmhXYumRyXH6X1.png'
---

![](https://raw.githubusercontent.com/reduxjs/redux/master/logo/logo-title-dark.png)

# Redux란?

![](https://hackernoon.com/hn-images/1*87dJ5EB3ydD7_AbhKb4UOQ.png)
상태 관리 라이브러리

컴포넌트간에 state를 넘나들기 힘들어서 사용한다.

# Data flow

![](https://yeri-kim.github.io/media/190715.png)

- Action

  - 업데이트해야될 때 어떻게 업데이트할 지 정의하는 객체
  - action은 state 변화를 일으킬 수 있는 하나의 현상이다.
    action을 dispatch(발행)해서 store에 저장하고, state가 변경되면 view에서 감지하게 된다.
  - action은 사용자가 일으키는 이벤트라고 생각해도 되는데, 위의 그림과 같이 view단에서 action이 일어날 수도 있다. view에서 action이 일어나면 -> 다시 dispatcher에 의해 store에 저장되고 -> state가 변경되면 -> 필요한 view에서 감지를 알아차린다.

- Reducer

  - 상태를 바꿔주는 함수- Action에 의해 State를 변경시키는 함수

- store
  - Application의 전체 state는 store라고 불리는 곳에서 관리된다.
    store는 redux의 상태값(state)를 갖는 객체이다.
  - application의 전체 state를 가지고 있는 객체. app에는 단 하나의 store를 갖고 있는 것이 좋다고 한다.
- subscribe

- view
  - 리액트에서는 component라고 생각하면 된다.

## 3가지 규칙

1. 하나의 애플리케이션엔 하나의 스토어가 있다.
2. state는 읽기전용 이다.

- 예를 들어 객체가 있다면 spread 연산자를 사용해서 객체를 복사한다음에 특정 값을 덮어씌우고
- 배열은 push, splice, reverse 쓰지 말고 concat, filter, map, slice같은 불변성을 지키는 내장함수를 사용해야 한다. 좋은 성능을 지켜내기 위함이다. 불변성을 지켜야만 컴포넌트들이 제대로 리랜더링 된다.

3. 변화를 일으키는 함수 리듀서는 순수한 함수여야 한다. 순수한 함수란 input값이 그대로 output으로 나오는 것을 말한다.

참조 : [yeri-kim](https://yeri-kim.github.io/posts/redux/)

redux를 사용하기 위해 필요한 항목
Ducks 패턴
action 생성함수 action타입 reducer를 한 파일에 몰아놓은 것

## 기본 예제

increment 예제

```js
import { createStore } from 'redux';
```
