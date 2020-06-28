---
title: "React 개요"
date: "2020-02-10T22:40:32.169Z"
template: "post"
draft: false
slug: "react-foundation"
category: "React"
tags:
  - "react"
  - "wecode"
  - "session"
description: "React에 대해 알아보자"
socialImage: "/media/react-logo.png"
---
![workflow](/media/react-logo.png)
#React란?
**React**는 페이스북에서 제공하는 UI를 만들기 위한 자바스크립트 라이브러리다.

## React를 왜 사용하는가?
`HTML/CSS/Javascript`만으로도 웹페이지를 제작할 수 있지만 Dom 이벤트처리, API호출, CSS등을 일일이 다 개발하기가 힘들다.
그래서 위 세가지를 한 라이브러리에서 해결할 수 있도록 하는 다양한 라이브러리들이 나왔다.
`React`는 `state`를 이용해서 화면을 혁명적으로 수정하기 쉽게 만들었다.
`state`만 수정하면 화면이 알아서 바뀌기 때문에 코드가 엄청나게 줄어들었고
그에 따라 여러명이 함께 협업 하기도 좋아졌으며, 리액트 네이티브가 나오면서 더 많은 인기를 누리게 되었다.

## React의 특징
1. ### Component   
`React`는 `Component`기반 라이브러리다.   
UI가 하나의 큰 덩어리라면 컴포넌트는 그 **덩어리를 이루는 작은 요소들**이다.   
그런 요소들을 설계해서 쌓아 올리면 하나의 UI가 만들어 진다.   
작은 컴포넌트들로 쪼개져 있기 때문에 전체 코드를 파악하기가 상대적으로 쉽다.   
작은 컴포넌트들은 다른 화면에서도 사용될 수 있는 `재사용성`을 가지고 있기 때문에 똑같은 코드를 반복적으로 입력할 필요가 없어서 `효율적`이다.    

2. ### JSX
JSX는 React를 위해 태어난 새로운 자바스크립트 문법이다.
![workflow](https://miro.medium.com/max/1954/1*_gXwacfA-wFIW-F65J7eAw.png)
**React는 작성한 코드를 컴파일하는 과정을 꼭 거쳐야 한다.**   
    #### JSX의 장점
    - 한눈에 이해하기 쉽다.   
    - 개 발자는 JSX를 통해 결과물에 직관적으로 도달할 수 있다.   
    - 예측가능한 개발을 만들어줄 뿐 아니라 유지보수, 협업 등에서도 엄청난 강점을 발휘한다.   

3. ### Virtual DOM
결국 위의 모든 것을 가능하게 만들어주는 것, 그리고 React의 가장 큰 특징이 Virtual DOM이다.
DOM은 웹의 핵심으로써, `브라우저가 화면을 그리기 위한 정보가 담겨있는 문서`이다.   
그러나 문제는 이러한 DOM을 효과적으로 다루는 것이 힘들다.   
`Virtual DOM`은 현재 브라우저에 보여지고 있는 진짜 DOM과 비교하여 달라진 부분을 찾아내고 `바뀐 부분만 적용`한다.
    
![workflow](https://www.devacron.com/wp-content/uploads/2017/07/howto-virtual-dom-600x400.png)

- 이러한 과정을 거치는 이유는?   
  브라우저에게 DOM을 해석하고 렌더링하는 것은 굉장히 비싼 작업이다.   
  `Virtual DOM`은 그 작업을 미리 최적화시켜줄 뿐만 아니라, 컴포넌트 단위로 묶어서 관리할 수 있게 해준다.

**Virtual DOM은 단순한 DOM 조작 도구가 아니라 컴포넌트 단위로 움직이는 Declarative한 개발을 구현하기 위해 도입된 것이다.**

4. ### One Way Data flow
리액트는 데이터의 흐름이 `한 방향`이다.   
데이터가 내려가기만 하고 위로 전달할 수 없다.   
그래서 부모의 데이터를 바꾸기 위해서는 `state`를 이용해야 한다.   

![workflow](https://www.techdiagonal.com/wp-content/uploads/2019/09/react-props-blog-image-design-2.jpg)
5. ### Props and State
    ![workflow](https://i.stack.imgur.com/wqvF2.png)

    #### props
    `props`는 부모 컴포넌트에서 자식 컴포넌트로 전달해주는 데이터를 말한다.
    `props`는 **읽기 전용 데이터**다.
    자식 컴포넌트에서는 전달 받은 `props`를 변경 불가능하고 `props`를 전달해준 최상위 부모 컴포넌트만 `props`를 변경할 수 있다.
    #### state
    `state`는 `동적인 데이터`를 다룰 때 사용한다.
    상황에 따라 변경되어야 하는 값들에 state를 이용한다..

