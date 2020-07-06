---
title: '[코딩 테스트] 눈떠보니 코딩테스트 전날 | 문제6 밭의 비밀'
date: '2020-07-05T16:09:32.169Z'
template: 'post'
draft: false
slug: 'tomorrow-coding-test-06'
category: 'algorithm'
tags:
  - 'coding-test'
description: '눈떠보니 코딩테스트 전날 6번 문제 풀이'
socialImage: 'https://i.kym-cdn.com/entries/icons/facebook/000/019/513/til.jpg'
---

# 눈떠보니 코딩테스트 전날 문제 풀이

성산일출봉, 99개의 봉우리가 왕관(**Crown**)처럼 둘러싸고 있는 곳!

일행은 그곳에서 수상한 **북극곰**을 발견했습니다.

> 안녕 하우꽈? 나는 **Drop the Beat**에서 커피 내리는 소울곰마심. 커피 마시러 와수과?

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a8c21f8a-446d-453f-8977-8b92939c01da/.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a8c21f8a-446d-453f-8977-8b92939c01da/.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/af424af8-e573-4bf0-b475-a5d9ea12fea9/(!)_.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/af424af8-e573-4bf0-b475-a5d9ea12fea9/(!)_.png)

> 네가 보물이 있는 위치를 알고 있다는걸 안다냥! 우리에게 알려주라냥!

> 삐빅삐빅.

소울 곰이 정색을 하며 이상한 소리를 내기 시작했어요.

> 냥?

> 혼저 옵서. 고냉이 같은 호랑이. 하지만 여기서부터는 쉽지 않을꺼라. 이 텃밭봥 단서를 찾아 보물의 위치를 알아보십서! 삐빅삐빅! NPC 모드 종료.

소울 곰은 아무일 없다는 듯이 새로온 손님들에게 커피를 팔기 시작했습니다. 순간 당황한 일행에게 정적이 흘렀지만, 문제를 풀어야 하기에 텃밭으로 향했습니다.

------

1. 텃밭 앞에는 꽃이 심어져 있고 꽃마다 번호가 붙어 있습니다.

   ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/65864dda-c435-44f5-94e2-967b325ee450/.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/65864dda-c435-44f5-94e2-967b325ee450/.png)

2. 2개의 텃밭이 있고, 각 텃밭의 주된 방향이 90도를 이루고 있습니다.

   ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1189d87e-5f4d-458a-9f47-1ad0614f4d19/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1189d87e-5f4d-458a-9f47-1ad0614f4d19/Untitled.png)

3. 텃밭의 비밀을 풀어 다음 장소로 이동하세요.



## 풀이

```javascript
let sample = [];

let firstField = [
  [1, 0, 0, 0, 0],
  [0, 0, 1, 0, 1],
  [0, 0, 1, 0, 1],
  [0, 0, 1, 0, 1],
  [0, 0, 1, 0, 1]
];

let secondField = [
  [0, 0, 0, 0, 1],
  [0, 0, 0, 0, 3],
  [0, 0, 0, 0, 4],
  [0, 2, 0, 0, 2],
  [4, 5, 0, 2, 0]
];

for (let i = 0 ; i < firstField.length ; i++) {
  sample[i] = new Array(firstField[0].length);
}

for (let i = 0 ; i < secondField.length ; i++) {
  for (let j = 0 ; j < secondField.length ; j++) {
    sample[i][j] = secondField[j][secondField[0].length-1-i];
    sample[i][j] += firstField[i][j];
  }
}

console.log(sample);

let result = '';
for(let v of sample) {
  result += String.fromCharCode(parseInt(v.join(''), 8));
}

console.log(result);
```

