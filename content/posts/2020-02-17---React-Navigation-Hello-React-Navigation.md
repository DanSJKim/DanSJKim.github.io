---
title: "React Navigation 2 | Hello React Navigation"
date: "2020-02-17T21:40:32.169Z"
template: "post"
draft: false
slug: "react-navigation-2"
category: "React Native"
tags:
  - "react native"
  - "react navigation"
  
description: ""
socialImage: "https://i.kym-cdn.com/entries/icons/facebook/000/019/513/til.jpg"
---

### Navigator로 화면 출력하기

1. #### stack navigator 설치하기   
앞서 설치한 라이브러리들은 building blocks와 shared foundations였고, React Navigation에 있는 각 네비게이터들은 자체 라이브러리에 있다.   
stack navigator를 사용하기 위해, `@react-navigation/stack`을 설치해야 한다.   
```
npm install @react-navigation/stack
```


2. #### stack navigator 만들기   
`createStackNavigator`는 `Screen`과 `Navigator`라는 두 가지 속성이 포함된 객체를 반환하는 함수이다.   
둘 다 navigator 구성에 사용되는 React의 컴포넌트이다. 네비게이터는 routes 구성을 정의하기 위해 Screen 요소를 하위 요소로 포함해야 한다.   
`NavigationContainer`는 navigation tree를 관리하고 navigation state를 포함하는 컴포넌트다.   
이 컴포넌트는 모든 네비게이터 구조를 감싸야 한다.   
보통, 우리는 이 컴포넌트를 우리 앱의 루트에서 랜더한다. (보통 App.js에서)   

```jsx
// In App.js in a new project

import * as React from 'react';
import { View, Text } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';

function HomeScreen() {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Home Screen</Text>
    </View>
  );
}

const Stack = createStackNavigator();

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={HomeScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

export default App;
```

#### 결과
![homescreen](https://reactnavigation.org/docs/assets/navigators/stack/basic_stack_nav.png)

#### 네비게이터 구성하기
`stack navigator`에 두번째 화면을 추가하고 먼저 랜더링하도록 홈 화면을 구성해 보자.

```jsx
function DetailsScreen() {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Details Screen</Text>
    </View>
  );
}

const Stack = createStackNavigator();

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator initialRouteName="Home">
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Details" component={DetailsScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```
`name` prop은 경로를 찾을 때 사용하도록 허용 할 이름이다.   
`component` 속성은 랜더링 할 컴포넌트에 해당하는 컴포넌트 prop이다.   
여기서 초기 홈 경로는 `initialRouteName`으로 지정해 주었다.


#### 옵션 지정하기
네비게이터의 각 화면은 제목과 같은 네비게이션의 옵션들을 지정할 수 있다.   
이러한 옵션은 각 화면 컴포넌트의 `options` prop에 전달할 수 있다.   
```jsx
<Stack.Screen
  name="Home"
  component={HomeScreen}
  options={{ title: 'Overview' }}
/>
```

navigator의 모든 화면에 동일한 옵션을 지정하려고 한다. 이를 위해서 네비게이터에 `screenOptions` prop을 전달할 수 있다.

#### 추가 prop 전달하기
두가지 방식으로 화면에 추가 props를 전달할 수 있다.
1. [React context](https://reactjs.org/docs/context.html)를 사용하고 navigator를 context provider로 감싸서 데이터를 화면에 전달한다. (권장)
2. component prop을 지정하는 대신 화면에 render callback을 사용한다.

```jsx
<Stack.Screen name="Home">
  {props => <HomeScreen {...props} extraData={someData} />}
</Stack.Screen>
```