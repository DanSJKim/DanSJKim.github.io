---
title: '[코딩 테스트] 눈떠보니 코딩테스트 전날 | 문제5 그림자 연결'
date: '2020-07-05T16:09:32.169Z'
template: 'post'
draft: false
slug: 'tomorrow-coding-test-05'
category: 'algorithm'
tags:
  - 'dfs'
  - 'coding-test'
description: '눈떠보니 코딩테스트 전날 5번 문제 풀이'
socialImage: 'https://i.kym-cdn.com/entries/icons/facebook/000/019/513/til.jpg'
---

# 눈떠보니 코딩테스트 전날 문제 풀이

단서를 찾았다는 라이캣의 말에 모두가 모여들었습니다.

> 그래서 그 단서가 무엇이지? 개굴!

> 이 돌들의 그림자를 자세히 보라냥!

라이캣의 말대로 그림자를 자세히 보니 바위 틈 사이로 빛이 통과한 곳에 작은 원들이 그려진 것을 볼 수 있었습니다.

> 하지만 저 모양으로는 아무 단서도 발견할 수 없다독. 활용할 수 있는 데이터가 너무 적다독.

> 저 모양을 보고 떠오르는 것이 없냥?

> 트리!!

자바독과 개리가 동시에 외쳤어요. 그렇지만 자바독이 말한 것처럼 활용할 수 있는데이터가 너무 적었습니다. 모두가 라이캣이 더 설명해주길 기다렸어요.

> 우리가 활용할 수 있는 것은 그림자, 빛, 빛 간의 거리, 그리고 알고리즘이다냥. 트리의 알고리즘이 많지 않으니, 모두 시도해보고 유의미한 데이터가 뽑히는 알고리즘을 사용해보면 된다냥!

라이켓은 각 간선간의 길이 비율이 오후까지 변하지 않는다는 사실을 알아냈습니다. 처음 선의 비율이 1이라고 했을 때, 모든 선의 비율은 아래와 같습니다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1b56744d-b747-4fb6-a270-98290137d35e/KakaoTalk_20200428_105210244.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1b56744d-b747-4fb6-a270-98290137d35e/KakaoTalk_20200428_105210244.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/182be4cb-cd8d-418e-9424-71edf470c56f/KakaoTalk_20200428_105210403.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/182be4cb-cd8d-418e-9424-71edf470c56f/KakaoTalk_20200428_105210403.png)

그리고 라이켓은 그 선의 비율에 100을 곱하여 노드를 완성하였어요.

1. 그림자의 길이 비율이 데이터였습니다. 해당 데이터는 2진트리의 형태를 갖추고 있으며, 각 간선은 아래와 같이 표현됩니다.

   ```python
   graph = {100: set([67, 66]),
            67: set([100, 82, 63]),
            66: set([100, 73, 69]),
            82: set([67, 61, 79]),
            63: set([67]),
            73: set([66]),
            69: set([66, 65, 81]),
            61: set([82]),
            79: set([82, 87, 77]),
            65: set([69, 84, 99]),
            81: set([69]),
            87: set([79, 31, 78]),
            77: set([79]),
            84: set([65]),
            99: set([65]),
            31: set([87]),
            78: set([87])}
   ```

   ```jsx
   graph = {100: new Set([67, 66]),
            67: new Set([100, 82, 63]),
            66: new Set([100, 73, 69]),
            82: new Set([67, 61, 79]),
            63: new Set([67]),
            73: new Set([66]),
            69: new Set([66, 65, 81]),
            61: new Set([82]),
            79: new Set([82, 87, 77]),
            65: new Set([69, 84, 99]),
            81: new Set([69]),
            87: new Set([79, 31, 78]),
            77: new Set([79]),
            84: new Set([65]),
            99: new Set([65]),
            31: new Set([87]),
            78: new Set([87])};
   ```

2. 이 간선들을 2진 깊이우선 탐색하며 작은 값만을 선택해서, 또는 큰 값만을 선택해서 내려와야 합니다.

3. 아래 결과값을 단서로 삼아 다음 미션지로 향하세요! 단, 코드로 풀어야 합니다.



## 풀이

```javascript
let graph = {100: new Set([67, 66]),
         67: new Set([100, 82, 63]),
         66: new Set([100, 73, 69]),
         82: new Set([67, 61, 79]),
         63: new Set([67]),
         73: new Set([66]),
         69: new Set([66, 65, 81]),
         61: new Set([82]),
         79: new Set([82, 87, 77]),
         65: new Set([69, 84, 99]),
         81: new Set([69]),
         87: new Set([79, 31, 78]),
         77: new Set([79]),
         84: new Set([65]),
         99: new Set([65]),
         31: new Set([87]),
         78: new Set([87])};

function dfsMax(graph, start){
  let visit = [];
  let stack = [start];
  
  while(stack){
    let n = 0; // 다음 방문 할 노드
    n = stack.pop();
    if(!visit.includes(n)){ // 꺼낸 노드가 방문되어지지 않았다면
      visit.push(n);
      let difference = new Set([...graph[n]].filter(x => !(new Set(visit).has(x))));
      // 전체 트리를 순회하지 않고 중간에 stop하도록 조건을 건다.
      if([...difference].length == 0){
         break;
      }
      stack.push(Math.max(...difference));
    }
    if(stack.length == 0) {
      break;
    }
  }
  return visit
}

console.log(dfsMax(graph, 100));  // [100, 67, 82, 79, 87, 78]
result_dfsMax = dfsMax(graph, 100);
result = '';
for (let v of result_dfsMax) {
  result += String.fromCharCode(v);
}
console.log(result);

function dfsMin(graph, start){
  let visit = [];
  let stack = [start];
  
  while(stack){
    let n = 0; // 다음 방문 할 노드
    n = stack.pop();
    if(!visit.includes(n)){ // 꺼낸 노드가 방문되어지지 않았다면
      visit.push(n);
      let difference = new Set([...graph[n]].filter(x => !(new Set(visit).has(x))));
      // 전체 트리를 순회하지 않고 중간에 stop하도록 조건을 건다.
      if([...difference].length == 0){
         break;
      }
      stack.push(Math.min(...difference));
    }
    if(stack.length == 0) {
      break;
    }
  }
  return visit
}

console.log(dfsMin(graph, 100)); // [100, 66, 69, 65, 84]
result_dfsMin = dfsMin(graph, 100);
result = '';
for (let v of result_dfsMin) {
  result += String.fromCharCode(v);
}
console.log(result);
```

