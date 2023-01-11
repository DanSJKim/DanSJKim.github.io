---
title: '[Database] Index'
date: '2020-11-12T16:09:32.169Z'
template: 'post'
draft: false
slug: 'database-index'
category: 'Database'
tags:
  - 'database'
  - 'index'
description: '인덱스란?'
socialImage: 'https://i.kym-cdn.com/entries/icons/facebook/000/019/513/til.jpg'
---

# Index

## Index란 무엇인가?

![book_index](/Users/seungjune/blog/DanSJKim.github.io/static/media/book_index.png)

- RDBMS에서 검색 속도를 높이기 위한 기술

- 책의 맨 마지막에 있는 색인이라고 할 수 있다.

- 데이터베이스 테이블의 모든 데이터를 검색해서 원하는 결과를 가져 오려면 시간이 오래걸린다.

  그래서 **칼럼의 값과 해당 레코드가 저장된 주소를 키 값 쌍으로 인덱스를 만들어 둔다.**



### 어느 경우에 효과적인가?

DBMS의 인덱스는 항상 정렬된 상태를 유지하기 때문에 원하는 값을 탐색하는 데는 빠르지만 새로운 값을 추가하거나 삭제, 수정하는 경우에는 쿼리문의 실행 속도가 느려진다.

결론적으로 인덱스는 데이터의 저장 성능을 희생하고 그 대신 데이터의 읽기 성능을 높이는 기능이다.

insert는 인덱스가 정렬된 상태로 저장되어야 하기 떄문에 어느자리에 insert를 할 지 찾아서 저장을 해야 한다

그리고 테이블에만 insert update하는게 아니라 인덱스에도 똑같이 해야하기 떄문에 두군데에 하니까 오히려 느려진다.



### 인덱스를 사용하면 빨라지는 이유?

테이블을 생성할 떄 오브젝트를 생성한다고 말하는데

인덱스도 오브젝트의 하나이고 인덱스가 생성되면 테이블과 매핑된 또다른 테이블이 생성된다.

**테이블이 생성되는데 어떻게 속도가 빨라지나?**

인덱스는 인덱스 컬럼을 기준으로 소팅이 되어 저장되기 때문에 특정 조건의 데이터들을 검색할 때 시작점을 지정해서 거기서부터 스캔할 수 있다.

인덱스에서 먼저 데이터를 찾은 다음에 테이블로 매핑된 곳을 가서 데이터를 꺼내오는 방식이다.

인덱스가 해당 테이블 블럭의 주소를 가지고 있다. (블럭: 데이터가 저장되는 최소 단위)

테이블의 데이터들은 로우단위로 저장되어 있다.



### 어떤 컬럼을 인덱스로 설정해야 효율적일까?

- where절에 자주 등장하는 컬럼

- order by 절에 자주 등장하는 컬럼. 인덱스는 소팅되어 저장되기 때문에 따로 orderby를 수행할 필요 없이 인덱스에서 바로바로 꺼낼 수 있다.
- 인덱스는 단일 컬럼으로 구성할 수 있지만 여러 컬럼을 조합해서 결합 인덱스 구성 가능
- select절에 등장하는 컬럼을 잘 조합해서 인덱스로 구성해 놓으면 별도로 테이블에 가서 데이터를 꺼내올 필요 없이 인덱스만 스캔해서 출력을 하면 되니까 빠르게 된다.



### 결합인덱스는 결합하는 컬럼들의 순서가 중요하다

- where절에서 equal조건으로 많이 쓰이는 컬럼이 앞으로 오는게 효율적

- 분별력이 높은 컬럼이 앞으로 오는게 효율적

- 성별같은 컬럼보단 아이디처럼 분별력이 높은 컬럼이 앞으로 와야 바로바로 인덱스를 스캔해서 찾을 수 있다.