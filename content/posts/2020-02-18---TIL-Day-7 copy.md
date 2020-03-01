---
title: "[Wecode] TIL_Day7 Code Kata | React Native Mobile Instagram Clone"
date: "2020-02-18T23:46:32.169Z"
template: "post"
draft: false
slug: "til-200218"
category: "TIL"
tags:
  - "Wecode"
  - "TIL"
  - "React Native"
  - "Westagram"
  - "Code Kata"
  
description: "React Native | Tab navigation, Customizing Tab navigation"
socialImage: "https://i.kym-cdn.com/entries/icons/facebook/000/019/513/til.jpg"
---
<!-- ![workflow](/media/react-logo.png) -->
# TIL 7일차
- 10:15 | Code Kata
- 13:30 | React Native Westagram Clone - 상단 스토리 아이템 추가
- 14:30 | React Native Westagram Clone - 피드 만들기
- 19:00 | React Native Westagram Clone - 하단 메뉴(Tab navigation), 로그인 창 전환 (Switch navigator)

## Code Kata
```
* 문제
숫자로 이루어진 배열인 nums를 인자로 전달합니다.
숫자중에서 과반수(majority, more than a half)가 넘은 숫자를 반환해주세요.

예를 들어,

nums = [3,2,3]
return 3

nums = [2,2,1,1,1,2,2]
return 2


* 가정
nums 배열의 길이는 무조건 2개 이상
```
[코드](https://github.com/DanSJKim/code-kata/blob/master/week2-day2.js)

## React Native Westagram Clone
인스타를 클론할 때 스크롤기능은 필수로 구현해야 한다.   
스크롤을 가능하게 하는 기능은 두가지가 있다.   
`<ScrollView>`와 `<FlatList>`의 차이점은 뭘까?   
`<ScrollView>`는 모든 아이템 요소를 한번에 랜더링하지만 성능이 저하되고 메모리 사용량이 증가한다.   
`<FlatList>`는 요소가 나타나려고 할 때 느리게 랜더링되고, 화면 밖으로 스크롤돼서 보이지 않는 요소는 메모리와 처리시간을 절약하기 위해 제거한다.   

## Tab navigation
스마트폰의 기본적인 기능 중 하나인 탭 네비게이션을 만든다.
시작 전, 설치할 라이브러리 **@react-navigation/bottom-tabs**:

```
npm install @react-navigation/bottom-tabs
```

### Minimal example

```jsx
import * as React from 'react';
import { Text, View } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';

function HomeScreen() {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>Home!</Text>
    </View>
  );
}

function SettingsScreen() {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>Settings!</Text>
    </View>
  );
}

const Tab = createBottomTabNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Tab.Navigator>
        <Tab.Screen name="Home" component={HomeScreen} />
        <Tab.Screen name="Settings" component={SettingsScreen} />
      </Tab.Navigator>
    </NavigationContainer>
  );
}
```


### Nesting navigators | 중첩 navigator 

```jsx
function Home() {
  return (
    <Tab.Navigator>
      <Tab.Screen name="Feed" component={Feed} />
      <Tab.Screen name="Messages" component={Messages} />
    </Tab.Navigator>
  );
}

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={Home} />
        <Stack.Screen name="Profile" component={Profile} />
        <Stack.Screen name="Settings" component={Settings} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

위의 예제에서, Home 컴포넌트에는 Tab navigator가 포함되어 있다.
Home 컴포넌트는 App 컴포넌트 내부의 stack navigator에서도 사용된다.
따라서 `Tab navigator는 Stack navigator 안에 중첩된다.`

- Stack.Navigator
  - Home (Tab.Navigator)
      - Feed (Screen)
      - Messages (Screen)
  - Profile (Screen)
  - Settings (Screen)

- 다음 코드로 원하는 컴포넌트로 이동할 수 있다.

```jsx
navigation.navigate('Root');
```

- `Root` 컴포넌트로 이동하면서 `Settings` 화면을 띄우고 싶다면

```jsx
navigation.navigate('Root', { screen: 'Settings' });
```

- 이 방법으로 매개변수도 전달할 수 있다.

```jsx
navigation.navigate('Root', {
  screen: 'Settings',
  params: { user: 'jane' },
});
```

- 깊게 중첩된 화면은 다음과 같이 접근할 수 있다. 두번쨰 argument인 `navigate`

```jsx
navigation.navigate('Root', {
  screen: 'Settings',
  params: {
    screen: 'Sound',
    params: {
      screen: 'Media',
    },
  },
});
```
위의 경우, Setting 안의 Sound 스크린 안의 중첩 된 미디어 화면으로 이동한다.
코드를 단순하게 유지하기 위해서 중첩 네비게이터는 최대한 적게 하는 것이 좋다.


### Customizing Tab navigation's icon

```jsx
// You can import Ionicons from @expo/vector-icons if you use Expo or
// react-native-vector-icons/Ionicons otherwise.
import { Ionicons } from '@expo/vector-icons';

// (...)

export default function App() {
  return (
    <NavigationContainer>
      <Tab.Navigator
        screenOptions={({ route }) => ({
          tabBarIcon: ({ focused, color, size }) => {
            let iconName;

            if (route.name === 'Home') {
              iconName = focused
                ? 'ios-information-circle'
                : 'ios-information-circle-outline';
            } else if (route.name === 'Settings') {
              iconName = focused ? 'ios-list-box' : 'ios-list';
            }

            // You can return any component that you like here!
            return <Ionicons name={iconName} size={size} color={color} />;
          },
        })}
        tabBarOptions={{
          activeTintColor: 'tomato', // 탭을 누른 상태
          inactiveTintColor: 'gray', // 탭을 누르지 않은 상태
        }}
      >
        <Tab.Screen name="Home" component={HomeScreen} />
        <Tab.Screen name="Settings" component={SettingsScreen} />
      </Tab.Navigator>
    </NavigationContainer>
  );
}
```

### Switch navigator
별도 글 작성