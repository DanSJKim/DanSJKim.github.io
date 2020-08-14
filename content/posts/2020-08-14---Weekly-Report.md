---
title: '200810-200814 | 8월 둘째주 Report'
date: '2020-08-14T16:09:32.169Z'
template: 'post'
draft: false
slug: '20081014-weekly-report'
category: 'weekly'
tags:
  - 'weekly'
  - 'wecode'
  - 'tips'
description: '8월 둘째주 정리'
socialImage: 'https://i.kym-cdn.com/entries/icons/facebook/000/019/513/til.jpg'

---

# Weekly

- 관리자 기획전 페이지 매핑상품 검색기능 추가건 개발 완료 및 검토
- 옴니어스 응답대기 이슈 개선 curl -> socket
- Python Tuple 스터디
- MySQL Index 스터디



# Tips

## General

- UI, API 동시에 구현할 때 UI쪽부터 개발하는게 낫다. DB 값을 바꾸면서 화면의 변경사항을 확인 하고 값이 잘 나타나면 그 구조에 맞춰서 API를 개발하자.

## MySQL

- eq-ref 생각해서 조인하기!!
- string equal이 내부적으로 동작하는 방법
  - string은 문자 하나하나 비교해야한다. 그러기 위해서 문자를 아스키코드로 변환해서 일일이 비교하는 과정이 필요하기 때문에 성능 비용이 많이 발생한다.
- datetime형을 equal하면 어떤 일이 발생하는가
  - datetime은 의외로 비용이 들지 않는다. 날짜는 1970년(정확하지 않음)부터 현재까지를 숫자로 저장하기 때문에 숫자끼리 값의 크기를 비교하면 된다.

- 인덱스 스터디 내용 별도 작성

## Vue

- Vue 라이프사이클 객체간의 관계 설계 방법 필수적으로 배워두기

- UI 통일할 예정. 주문 쪽 UI처럼

- Vuex 이용해서 세션 관리도 할 예정

- 어떻게 해야 가독성이 높은 컴포넌트와 효율성을 올릴 수 있을까?

  **컴포넌트화 해야 하는 부분을 명확하게 나눠야 한다.**

  A라는 부분이 N개의 페이지에서 사용하는 공통적인 항목이라면 컴포넌트로 분리해야하는 것이 바람직하다. 그리고

  **컴포넌트화 해야 하는 덩어리(chunk)를 굳이 세분화해서 나눌 필요는 없다.**

  우리는 컴포넌트를 작성할 때 과감하게 큰 chunk 단위로 나눌 필요도 있다.

## Python

```python
# except 확인하고 싶을 떄 try 강제로 except로 빼기
try:
    raise Exception('aaaa')
except:
    print('예외 발생')
    logging.critical(r.headers)
    exit()
```

## PHP

python과 다르게 php curl은 connection timeout과 readtimeout을 별도로 구분하지 않는 문제가 있다.

Php에서 readtimeout을 사용하기 위해서는 socket통신을 사용한다.

```php
$context = stream_context_create();
stream_socket_client($domain, $errno, $errstr, $timeout, STREAM_CLIENT_CONNECT, $context)
fwrite($socket, $out);
stream_set_timeout($socket,0, 10000); // readtimeout (second, nano second)
fread($socket, 2000); // fread를 해주지 않으면 readtimeout이 동작하지 않는다.
fclose($socket);
```

## Git

- 다른사람의 코드를 바로 pull할 때는 충돌이 나고 stash후에 pull하고 stash를 다시 받을 때 충돌이 안나는 이유?
  - pull을 할 때는 파일의 충돌을 검사하기 때문에 내가 같은 파일을 변경했다면 코드가 겹치지 않아도 충돌이 난다.