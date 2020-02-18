---
title: "React Native | Switch Navigator"
date: "2020-02-18T23:50:32.169Z"
template: "post"
draft: false
slug: "switch-navigator"
category: "React Native"
tags:
  - "React Native"
  - "React Navigation"
  
description: "React Native | Switch navigator"
socialImage: "https://i.kym-cdn.com/entries/icons/facebook/000/019/513/til.jpg"
---
<!-- ![workflow](/media/react-logo.png) -->

## Switch navigator 사용법

### createSwitchNavigator
`switch navigator`는 화면 전환 시 스택이 쌓이지 않고 새로운 화면을 띄워준다.   
로그인 후 메인화면으로 넘어가는 상황에는 `stack navigator`를 사용하면 안된다.   
이미 로그인을 하고도 아래에 로그인 창이 쌓여있으면 뒤로가기로 다시 되돌아갈 수도 있기 때문이다.   
 

```jsx
createSwitchNavigator(RouteConfigs, SwitchNavigatorConfig);
```
위 함수를 사용해야 한다.
실제 인자로는 아래와 같은 값이 들어간다.   
**ex)**    
```jsx
createSwitchNavigator(
  {
    // 화면 전환에 등록 할 화면
    AuthLoading: AuthLoadingScreen,
    App: AppStack,
    Auth: AuthStack,
  },
  {
    // 처음 시작 화면
    initialRouteName: 'AuthLoading',
  }
```

### 전체 예제

```jsx
import React from 'react';
import {
  ActivityIndicator,
  AsyncStorage,
  Button,
  StatusBar,
  StyleSheet,
  View,
} from 'react-native';
import { createStackNavigator, createSwitchNavigator, createAppContainer } from 'react-navigation';

class SignInScreen extends React.Component {
  static navigationOptions = {
    title: 'Please sign in',
  };

  render() {
    return (
      <View style={styles.container}>
        <Button title="Sign in!" onPress={this._signInAsync} />
      </View>
    );
  }

  _signInAsync = async () => {
    await AsyncStorage.setItem('userToken', 'abc');
    this.props.navigation.navigate('App');
  };
}

class HomeScreen extends React.Component {
  static navigationOptions = {
    title: 'Welcome to the app!',
  };

  render() {
    return (
      <View style={styles.container}>
        <Button title="Show me more of the app" onPress={this._showMoreApp} />
        <Button title="Actually, sign me out :)" onPress={this._signOutAsync} />
      </View>
    );
  }

  _showMoreApp = () => {
    this.props.navigation.navigate('Other');
  };

  _signOutAsync = async () => {
    await AsyncStorage.clear();
    this.props.navigation.navigate('Auth');
  };
}

class OtherScreen extends React.Component {
  static navigationOptions = {
    title: 'Lots of features here',
  };

  render() {
    return (
      <View style={styles.container}>
        <Button title="I'm done, sign me out" onPress={this._signOutAsync} />
        <StatusBar barStyle="default" />
      </View>
    );
  }

  _signOutAsync = async () => {
    await AsyncStorage.clear();
    this.props.navigation.navigate('Auth');
  };
}

class AuthLoadingScreen extends React.Component {
  constructor() {
    super();
    this._bootstrapAsync();
  }

  // Fetch the token from storage then navigate to our appropriate place
  _bootstrapAsync = async () => {
    const userToken = await AsyncStorage.getItem('userToken');

    // This will switch to the App screen or Auth screen and this loading
    // screen will be unmounted and thrown away.
    this.props.navigation.navigate(userToken ? 'App' : 'Auth');
  };

  // Render any loading content that you like here
  render() {
    return (
      <View style={styles.container}>
        <ActivityIndicator />
        <StatusBar barStyle="default" />
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
  },
});

const AppStack = createStackNavigator({ Home: HomeScreen, Other: OtherScreen });
const AuthStack = createStackNavigator({ SignIn: SignInScreen });

export default createAppContainer(createSwitchNavigator(
  {
    AuthLoading: AuthLoadingScreen,
    App: AppStack,
    Auth: AuthStack,
  },
  {
    initialRouteName: 'AuthLoading',
  }
));
```
[사이트 링크](https://snack.expo.io/@react-navigation/auth-flow-v3)