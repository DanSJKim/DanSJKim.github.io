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

- Action Creator

- Reducer

  - 상태를 바꿔주는 함수- Action에 의해 State를 변경시키는 함수

- dispatch

- store
  - Application의 전체 state는 store라고 불리는 곳에서 관리된다.
    store는 redux의 상태값(state)를 갖는 객체이다.
  - application의 전체 state를 가지고 있는 객체. app에는 단 하나의 store를 갖고 있는 것이 좋다고 한다.
- subscribe

- view
  - 리액트에서는 component라고 생각하면 된다.

## 3가지 규칙

1. 하나의 애플리케이션엔 하나의 스토어가 있다.
2. 상태는 읽기전용 이다.

- 예를 들어 객체가 있따면 spread 연산자를 사용해서 객체를 복사한다음에 특정 값을 덮어씌우고
- 배열은 push, splice, reverse 쓰지 말고 concat, filter, map, slice같은 불변성을 지키는 내장함수를 사용해야 한다. 좋은 성능을 지켜내기 위함이다. 불변성을 지켜야만 컴포넌트들이 제대로 리랜더링 된다.

3. 변화를 일으키는 함수 리듀서는 순수한 함수여야 한다.

참조 : [yeri-kim](https://yeri-kim.github.io/posts/redux/)

redux 사용하기 위해 필요한 항목
Ducks 패턴
action 생성함수 action타입 reducer를 한 파일에 몰아놓은 것

## 기본 예제

```jsx
// 1. createStore - 스토어를 만들어주는 함수
import { createStore } from 'redux';

// 2. 초기 값을 세팅한다.
const initialState = {
  counter: 0,
  text: '',
  list: []
};

// 3. 액션 타입들을 정의한다. 대문자로 작성한다.
const INCREASE = 'INCREASE'; //counter++
const DECREASE = 'DECREASE'; //counter--
const CHANGE_TEXT = 'CHANGE_TEXT';
const ADD_TO_LIST = 'ADD_TO_LIST';

// 4. 액션 생성 함수를 작성한다. 소문자로 작성한다.
const increase = () => ({
  type: INCREASE
});

const decrease = () => ({
  type: DECREASE
});

const changeText = text => ({
  type: CHANGE_TEXT,
  text
});

const addToList = item => ({
  type: ADD_TO_LIST,
  item
});

// 5. reducer 작성(state, action)을 파라미터로
// state에 기본 값을 설정해야 한다. 왜 필요하냐면 리덕스에서 초기 상태를 만들 떄 리듀서를 한번 호출하는데 그시점에 state가 undefined면 default return undefined가 되면서 초기 상태가 만들어지지 않기 때문
function reducer(state = initialState, action) {
  // action.type이 무엇이냐에 따라 다른 작업을 한다.
  switch (action.type) {
    case INCREASE:
      return {
        // 기존 값은 유지시키고 반환을 하겠다.
        ...state,
        // 기존 상태의 카운터값을 읽어서 1을 더하고
        counter: state.counter + 1
      };

    case DECREASE:
      return {
        ...state,
        counter: state.counter - 1
      };
    case CHANGE_TEXT:
      return {
        ...state,
        text: action.text
      };

    case ADD_TO_LIST:
      return {
        ...state,
        // 불변성을 지켜야 하기 때문에 push 대신 concat을 사용한다. 기존 리스트에 새로운 아이템을 추가한 list를 만들어서 기존 list를 대체한다.
        list: state.list.concat(action.item)
      };

    // 만약에 위에서 처리 못한 액션일 경우에는 state를 유지하겠다.
    default:
      return state;
  }
}

// 6. reduce가 완성되면 reduce를 사용해서 store를 만들 수 있다.

const store = createStore(reducer);
// 현재 store 안에 있는 상태를 확인한다.
console.log(store.getState());

// 이제 구독과 디스패치를 하자.

// 7. store에 구독을 하기 위해 listener라는 함수를 만든다.
const listener = () => {
  // store를 조회한다.
  const state = store.getState();
  console.log(state);
};

// 8. listener라는 함수를 store에 구독한다.
const unsubscribe = store.subscribe(listener);
// 구독을 해제하고싶을 때는 unsubscribe를 호출해 준다.
// unsubscribe();

// 9. 액션들을 dispatch 한다.
// 이렇게 구독하고 나서 액션들을 디스패치하면 액션이 디스패치될 때 마다 콘솔에 현재 상태가 출력된다.
store.dispatch(increase());
store.dispatch(decrease());
store.dispatch(changeText('안녕하세요'));
store.dispatch(addToList({ id: 1, text: '와우' }));

// store인스턴스를 콘솔에서 사용할 수 있게 한다.
window.store = store;

// 특정 액션이 dispatch되면 store의 상태가 update되고  상태가 update되면 구독했떤 함수가 호출되는 것이다.
```

## counter 실습

#### files

- src/modules/index.js
- src/modules/counter.js
- src/components/Counter.js
- src/containers/CounterContainer.js
- src/App.js
- src/index.js

**src/modules/counter.js**

```js
// 액션 타입 선언
const SET_DIFF = 'counter/SET_DIFF'; // ducks패턴 사용할 땐 액션타입 선언할 때 문자열 앞에 counter/와 같이접두사를 붙인다. 다른모듈과 이름이 중복되지 않게
const INCREASE = 'counter/INCREASE';
const DECREASE = 'counter/DECREASE';

// 액션 생성 함수 선언
export const setDiff = diff => ({ type: SET_DIFF, diff });
export const increase = () => ({ type: INCREASE });
export const decrease = () => ({ type: DECREASE });

// reducer에서 관리 할 초기 상태 선언
const initialState = {
  number: 0,
  diff: 1
};

// reducer - 모듈 이름으로 reducer 작성
export default function counter(state = initialState, action) {
  // 액션 타입에 따라 다른 상태 업데이트
  switch (action.type) {
    case SET_DIFF:
      return {
        ...state,
        diff: action.diff
      };
    case INCREASE:
      return {
        ...state,
        number: state.number + state.diff
      };
    case DECREASE:
      return {
        ...state,
        number: state.number - state.diff
      };

    default:
      return state;
  }
}
```

**src/modules/index.js**

```js
import { combineReducers } from 'redux';
import counter from './counter';
import todos from './todos';

const rootReducer = combineReducers({
  counter,
  todos
});

export default rootReducer;
```

**src/index.js**

```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import { Provider } from 'react-redux';
import { createStore } from 'redux';
import rootReducer from './modules';
import { composeWithDevTools } from 'redux-devtools-extension';

// compose: 개발자도구에서 리덕스 상태를 조회하고 디스패치할수있고 디스패치된 액션들의 모든 내역을 확인할 수 있다.
const store = createStore(rootReducer, composeWithDevTools());

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

**src/App.js**

```jsx
import React from 'react';
import CounterContainer from './containers/CounterContainer';
function App() {
  return <CounterContainer />;
}

export default App;
```

**src/components/Counter.js**

```js
import React from 'react';

function Counter({ number, diff, onIncrease, onDecrease, onSetDiff }) {
  const onChange = e => {
    onSetDiff(parseInt(e.target.value, 10));
  };
  return (
    <div>
      <h1>{number}</h1>
      <div>
        <input type="number" value={diff} onChange={onChange} />
        <button onClick={onIncrease}>+</button>
        <button onClick={onDecrease}>-</button>
      </div>
    </div>
  );
}

export default Counter;
```

**src/containers/CounterContainer.js**  
redux에 있는 상태를 조회하거나 Action을 dispatch할 수 있는 컴포넌트

```js
import React from 'react';
import Counter from '../components/Counter';
import { useSelector, useDispatch } from 'react-redux'; // useSelector: 상태를 조회하는 훅
import { increase, decrease, setDiff } from '../modules/counter';

// redux에 있는 상태를 조회하거나 Action을 dispatch할 수 있는 컴포넌트

function CounterContainer() {
  // useSelector의 결과물이 number, diff값을 선택한 객체가 된다.
  // state: redux의 현재 상태
  const { number, diff } = useSelector(state => ({
    // redux의 상태에서 불러올 state
    number: state.counter.number, // store.getState()
    diff: state.counter.diff
  }));

  // useDispatch: action을 dispatch한다.
  const dispatch = useDispatch();

  // 액션생성함수들이 호출되면 액션 객체가 만들어져서 디스패치가 된다.
  const onIncrease = () => dispatch(increase()); //increase()를 호출해서 액션은 만들고 dispatch를 호출한다.
  const onDecrease = () => dispatch(decrease());
  const onSetDiff = diff => dispatch(setDiff(diff));

  return (
    <Counter
      number={number}
      diff={diff}
      onIncrease={onIncrease}
      onDecrease={onDecrease}
      onSetDiff={onSetDiff}
    />
  );
}

export default CounterContainer;
```
