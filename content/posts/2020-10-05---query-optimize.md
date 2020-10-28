---
title: '[MySQL] 쿼리 최적화'
date: '2020-10-05T16:09:32.169Z'
template: 'post'
draft: false
slug: 'quick-sort'
category: '알고리즘'
tags:
  - 'recursive'
description: '쿼리 최적화 알아보기'
socialImage: 'https://i.kym-cdn.com/entries/icons/facebook/000/019/513/til.jpg'
---

# 쿼리 최적화

애플리케이션에서 데이터를 저장, 조회하기 위해 데이터베이스와 통신할 때 데이터베이스 서버로 전달되는 것은 SQL 뿐이다. SQL은 **어떠한(What)** 데이터를 요청하기 위한 언어지, **어떻게(How)** 데이터를 읽을지 표현하는 언어는 아니므로 C, 자바와 같은 언어와 비교했을 때 상당히 제한적으로 느껴질 수 있다.

하지만 쿼리가 빠르게 수행되게 하려면 쿼리가 어떻게 데이터를 가져올 지 예측할 수 있어야 한다.

그래서 SQL을 작성하는 방법과 규칙은 물론, 내부적인 처리 방식**(옵티마이저)** 에 어느정도 지식이 필요하다.

애플리케이션 코드를 튜닝해서 성능을 2배 개선하는 것은 쉽지 않은 일이지만, DBMS에서 몇십 배 몇백 배 성능 향상은 상당히 흔한 일이다.

SQL 처리에서 "어떻게(How)" 를 이해하고 쿼리를 작성하는 것이 그만큼 중요하다는 의미다.



### SELECT 시 꼭 필요한 컬럼만 불러 온다.

```mysql
-- X
SELECT * FROM movie; 
-- O
SELECT id FROM movie;
```

많은 필드 값을 불러올 수록 데이터베이스는 더 많은 로드를 부담하기 때문에 꼭 필요한 컬럼만 불러오는 것이 좋다.



### 조건 부여 시, 가급적이면 기존 DB 값에 별도의 연산을 걸지 않는다.

```mysql
-- X
SELECT m.id, ANY_VALUE(m.title) title, COUNT(r.id) r_count 
FROM movie m 
INNER JOIN rating r 
ON m.id = r.movie_id 
WHERE FLOOR(r.value/2) = 2 
GROUP BY m.id;
-- O
SELECT m.id, ANY_VALUE(m.title) title, COUNT(r.id) r_count 
FROM movie m 
INNER JOIN rating r 
ON m.id = r.movie_id 
WHERE r.value BETWEEN 4 AND 5 
GROUP BY m.id;
```

상단의 쿼리는 Full Table Scan을 하면서 모든 셀을 탐색하고 수식을 건 뒤에 조건 충족 여부를 판단해야 한다.



### LIKE사용 시 와일드카드 문자열(%)을 String 앞부분에는 배치하지 않는 것이 좋다.

```mysql
-- Inefficient
SELECT g.value genre, COUNT(r.movie_id) r_cnt 
FROM rating r 
INNER JOIN genre g 
ON r.movie_id = g.movie_id 
WHERE g.value LIKE "%Comedy"  
GROUP BY g.value;
-- Improved(1): value IN (...)
SELECT g.value genre, COUNT(r.movie_id) r_cnt 
FROM rating r 
INNER JOIN genre g 
ON r.movie_id = g.movie_id 
WHERE g.value IN ("Romantic Comedy", "Comedy") 
GROUP BY g.value;
-- Improved(2): value = "..."
SELECT g.value genre, COUNT(r.movie_id) r_cnt 
FROM rating r 
INNER JOIN genre g 
ON r.movie_id = g.movie_id 
WHERE g.value = "Romantic Comedy" OR g.value = "Comedy"
GROUP BY g.value;
-- Improved(3): value LIKE "...%"
-- 모든 문자열을 탐색할 필요가 없어, 가장 좋은 성능을 내었습니다
SELECT g.value genre, COUNT(r.movie_id) r_cnt 
FROM rating r 
INNER JOIN genre g 
ON r.movie_id = g.movie_id 
WHERE g.value LIKE "Romantic%" OR g.value LIKE "Comed%"
GROUP BY g.value;
```

이전 예시와 마찬가지로 `value LIKE "%..."` 는 Full Table Scan으로 조건 충족 여부를 판단해야 한다.



### SELECT DISTINCT, UNION DISTINCT와 같이 중복 값을 제거하는 연산은 최대한 사용하지 않아야 한다.

```mysql
-- Inefficient
SELECT DISTINCT m.id, title 
FROM movie m  
INNER JOIN genre g 
ON m.id = g.movie_id;
-- Improved
SELECT m.id, title 
FROM movie m  
WHERE EXISTS (SELECT 'X' FROM rating r WHERE m.id = r.movie_id);
```

### 같은 내용의 조건이라면, GROUP BY 연산 시에는 가급적 HAVING보다는 WHERE 절을 사용하는 것이 좋다.

```mysql
-- Inefficient
SELECT m.id, COUNT(r.id) AS rating_cnt, AVG(r.value) AS avg_rating 
FROM movie m  
INNER JOIN rating r 
ON m.id = r.movie_id 
GROUP BY id 
HAVING m.id > 1000;
-- Improved
SELECT m.id, COUNT(r.id) AS rating_cnt, AVG(r.value) AS avg_rating 
FROM movie m  
INNER JOIN rating r 
ON m.id = r.movie_id 
WHERE m.id > 1000
GROUP BY id ;
```

### 3개 이상의 테이블을 INNER JOIN 할 때는, 크기가 가장 큰 테이블을 FROM 절에 배치하고, INNER JOIN 절에는 남은 테이블을 작은 순서대로 배치하는 것이 좋다.

```mysql
-- Query (A)
SELECT m.title, r.value rating, g.value genre 
FROM rating r 
INNER JOIN genre g 
ON g.movie_id = r.movie_id  
INNER JOIN movie m 
ON m.id = r.movie_id;
-- Query (B)
SELECT m.title, r.value rating, g.value genre 
FROM rating r 
INNER JOIN movie m
ON r.movie_id = m.id 
INNER JOIN genre g 
ON r.movie_id = g.movie_id;
```

### 자주 사용하는 데이터의 형식에 대해서는 미리 전처리된 테이블을 따로 보관/관리하는 것도 좋다.

### ORDER BY는 연산 중간에 사용하지 않는다.

### LIMIT를 활용한다.



참고: [링크](https://medium.com/watcha/쿼리-최적화-첫걸음-보다-빠른-쿼리를-위한-7가지-체크-리스트-bafec9d2c073)