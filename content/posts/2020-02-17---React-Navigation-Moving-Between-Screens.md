---
title: "React Navigation 3 | Moving between screens"
date: "2020-02-17T23:40:32.169Z"
template: "post"
draft: false
slug: "react-navigation-3"
category: "React Native"
tags:
  - "React Native"
  - "React Navigation"

description: "화면 이동, 중복 화면 띄우기, 화면 되돌아가기, 첫 화면으로 돌아가기"
socialImage: "https://i.kym-cdn.com/entries/icons/facebook/000/019/513/til.jpg"
---

# Keyword
- navigation.navigate('RouteName')
- navigation.push('RouteName')
- navigation.goBack()
- navigation.popToTop()


### 새로운 화면으로 이동하기

```jsx
import * as React from 'react';
import { Button, View, Text } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';

function HomeScreen({ navigation }) {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Home Screen</Text>
      <Button
        title="Go to Details"
        onPress={() => navigation.navigate('Details')}
      />
    </View>
  );
}

// ... other code from the previous section
```
- `navigation` : navigation prop은 stack navigator의 모든 **screen component**로 전달된다.
- `navigate('Details')` : 사용자가 이동하려는 route의 이름과 함께 `navigate` 함수를 호출한다.


### push

Details 페이지에서 Details로 이동하면 이미 해당 페이지에 있기 때문에 이동이 되지 않는다.
이럴 경우에는 push를 사용하면 같은 페이지를 또 띄울 수 있다.
```jsx
<Button
  title="Go to Details... again"
  onPress={() => navigation.push('Details')}
/>
```
push를 호출할 때마다 navigation stack에 새로운 route가 추가된다.
`navigate`를 호출하면 먼저 해당 name으로 기존 경로를 찾으려고 시도하고, stack에 아직 없는 경우에만 새로운 route를 push한다.


### Going back

`stack navigator`가 제공하는 상단의 헤더는 이전 화면으로 돌아갈 수 있을 때 자동으로 뒤로가기 버튼을 포함한다.
`navigation.goBack()`을 이용해 돌아가기 버튼을 코드로 작성할 수도 있다. 
```jsx
function DetailsScreen({ navigation }) {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Details Screen</Text>
      <Button
        title="Go to Details... again"
        onPress={() => navigation.push('Details')}
      />
      <Button title="Go to Home" onPress={() => navigation.navigate('Home')} />
      <Button title="Go back" onPress={() => navigation.goBack()} />
    </View>
  );
}
```

### 모두 지우고 첫 화면으로 돌아가기
`navigation.popToPop()`

```jsx
function DetailsScreen({ navigation }) {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Details Screen</Text>
      <Button
        title="Go to Details... again"
        onPress={() => navigation.push('Details')}
      />
      <Button title="Go to Home" onPress={() => navigation.navigate('Home')} />
      <Button title="Go back" onPress={() => navigation.goBack()} />
      <Button
        title="Go back to first screen in stack"
        onPress={() => navigation.popToTop()}
      />
    </View>
  );
}
```