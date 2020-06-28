---
title: "React Navigation 1 | Getting started"
date: "2020-02-17T20:40:32.169Z"
template: "post"
draft: false
slug: "react-navigation-1"
category: "React Native"
tags:
  - "react native"
  - "react navigation"
  
description: "react-navigation 시작하기"
socialImage: "https://i.kym-cdn.com/entries/icons/facebook/000/019/513/til.jpg"
---

### 화면 전환 라이브러리의 종류
- react-navigation
- native-navigation
- react-native-navigation
- react-native-router-flux

**이 중 react-navigation을 선택한 이유?**   
react-navigation은 공식 홈페이지가 따로 존재한다.   
계속해서 안정적인 라이브러리 개발에 힘쓰고 있다.


### 화면 전환 시작하기
순서
1. 먼저, React Native 프로젝트에 필요한 패키지들을 설치한다.

```
npm install @react-navigation/native @react-navigation/stack
```

2. Expo로 설치했다면, 다음 dependencies들을 `expo`로 설치한다.

```
expo install react-native-reanimated react-native-gesture-handler react-native-screens react-native-safe-area-context @react-native-community/masked-view
```

React Native project로 설치했다면 다음 dependencies들을 `npm`으로 설치한다.
```
npm install react-native-reanimated react-native-gesture-handler react-native-screens react-native-safe-area-context @react-native-community/masked-view
```

3. react-native-gesture-handler의 설치를 완료하려면 index.js 또는 App.js와 같은 파일의 맨 위에 다음을 추가한다. (맨 위에 있고 앞에 아무것도 없는지 확인).

```
import 'react-native-gesture-handler';
```

dependencies들을 다 깔았는데도 계속 오류가 뜨면 package.json을 확인해보고 패키지들이 잘 추가되었는지 확인해보자.
설치 하다가 꼬일 수도 있기 때문에 그런 경우에는 node modules를 삭제해서 초기화하고 다시 설치해보자.

4. 다음은 컴포넌트 전체를 `<NavigationContainer>`로 감싸 준다. 일반적으로 index.js나 App.js 파일에서 다음 코드를 작성한다.

```jsx
import 'react-native-gesture-handler';
import * as React from 'react';
import {NavigationContainer} from '@react-navigation/native';

export default function App() {
  return (
    <NavigationContainer>
      {/* Rest of your app code */}
    </NavigationContainer>
  );
}
```
이제 앱을 빌드하고 실행 할 준비가 되었다.

### 