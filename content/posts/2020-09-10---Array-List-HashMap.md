---
title: 'Array, List, Map'
date: '2020-09-10T16:09:32.169Z'
template: 'post'
draft: false
slug: 'array-list-map'
category: '자료구조'
tags:
  - 'Array'
  - 'List'
  - 'Map'
description: '배열, 리스트, 맵의 차이'
socialImage: 'https://i.kym-cdn.com/entries/icons/facebook/000/019/513/til.jpg'
---

## Array와 List

같은 자료형을 가진 데이터를 하나의 변수에 담아서 관리하기 위한 자료구조



## Array

- 인덱스로 바로 접근할 수 있어서 검색이 빠르다.
- 연속된 메모리 공간으로 이루져 있기 떄문에 빠른 성능을 보인다.
- **크기가 고정**되어 있어서 크기 변경이 불가능하다. 메모리가 낭비됨
- 연속된 메모리 공간에 할당된다.



## List

- 삽입/삭제 시 전후의 노드의 참조 주소만 수정하면 되기 때문에 효율적이다.
- 크기가 **가변적**이다.
- 검색이 비효율적이다. 처음부터 **순차적으로 검색**해야 하기 때문에
- 메모리가 연속되어있지 않고 다음 노드를 가리키는 주소값을 가지고 있다.
- 흩어져 있는 메모리를 노드로 연결하여 쓰기 때문에 불필요한 메모리를 사용하지 않아 메모리낭비를 하지 않는다.



## Map

- Key와 Value의 쌍으로 연관지어 저장하는 객체

### HashMap

- Map 인터페이스를 구현하기 위해 해시테이블을 사용한 클래스
- 중복을 허용하지 않고 순서를 보장하지 않는다.
- 키와 값으로 null이 허용된다.

### HashTable

- HashMap보다 느리지만 동기화가 지원된다.
- 키와 값으로 null이 허용되지 않는다.
