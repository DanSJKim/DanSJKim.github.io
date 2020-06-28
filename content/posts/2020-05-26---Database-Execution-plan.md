---
title: '[Database] 실행 계획 - Execution Plan'
date: '2020-05-26T16:09:32.169Z'
template: 'post'
draft: false
slug: 'database-execution-plan'
category: 'Database'
tags:
  - 'database'
  - 'mySQL'
description: '실행 계획의 사용법과 필드의 의미'
socialImage: 'https://i.kym-cdn.com/entries/icons/facebook/000/019/513/til.jpg'
---

# [Database] 실행 계획 - Execution Plan

DBMS에서는 쿼리를 최적으로 실행하기 위해 각 테이블의 데이터가 어떤 분포로 저장돼 있는지 통계 정보를 참조하며, 그러한 기본 데이터를 비교해 최적의 실행 계획을 수립하는 작업이 필요하다. DBMS에서는 **옵티마이저**가 이러한 기능을 담당한다.

쿼리 비용 판단 기준에 대해 물어보면 많은 사람들이 실행 시간을 이야기 한다.   
아주 틀린 이야기는 아니지만 기기의 상황과 데이터의 분포에 따라 실행 시간은 천차만별로 달라진다.   
데이터베이스 비용 보기의 기본은 ```실행 계획```이다.

## 개요

### 쿼리 실행 절차

MySQL 서버에서 쿼리 실행 과정은 크게 3가지로 나눌 수 있다.

1. 사용자로부터 요청된 SQL 문장을 잘게 쪼개서 MySQL 서버가 이해할 수 있는 수준으로 분리한다.
2. SQL의 파싱 정보(파스 트리)를 확인하면서 어떤 테이블부터 읽고 어떤 인덱스를 이용해 테이블을 읽을지 선택한다. 여기서 실행 계획이 만들어진다.
3. 두번째 단계에서 결정된 테이블의 읽기 순서나 선택된 인덱스를 이용해 스토리지 엔진으로부터 데이터를 가져온다. MySQL 엔진에서는 스토리지 엔진으로부터 받은 레코드를 조인하거나 정렬하는 작업을 수행한다.



![optimizer](/Users/seungjune/blog/DanSJKim.github.io/static/media/optimizer.png)

![mysql_engine](/Users/seungjune/blog/DanSJKim.github.io/static/media/mysql_engine.png)

첫번째와 두번째 단계는 거의 MySQL 엔진에서 처리하며, 세 번째 단계는 MySQL 엔진과 스토리지 엔진이 동시에 참여해서 처리한다.



## 실행 계획 분석

MySQL에서 쿼리의 실행 계획을 확인하려면 EXPLAIN 명령을 사용하면 된다. EXMPLAIN EXTENDED나 EXPLAIN PARTITIONS 명령을 이용해 더 상세한 실행 계획을 확인할 수도 있다.

표의 각 라인(레코드)은 쿼리 문장에서 사용된 테이블의 개수만큼 출력된다.

출력된 실행 계획에서 위쪽에 출력된 결과일 수록 쿼리의 바깥 부분이거나 먼저 접근한 테이블이고, 아래쪽에 출력된 결과일수록 쿼리의 안쪽부분 또는 나중에 접근한 테이블에 해당된다. 하지만 쿼리 문장과 직접 비교해 가면서 실행 계획의 위쪽부터 테이블과 매칭해서 비교하는 편이 더 쉽게 이해될 것이다.

| id   | select_type        | table | type  | key     | key_len | ref   | rows | Extra       |
| ---- | ------------------ | ----- | ----- | ------- | ------- | ----- | ---- | ----------- |
| 1    | PRIMARY            | e     | const | PRIMARY | 4       | const | 1    |             |
| 2    | DEPENDENT SUBQUERY | s     | ref   | PRIMARY | 4       | const | 17   | Using index |

### id

- SELECT 쿼리별로 부여되는 식별자 값

- 하나의 SELECT 문장에서 여러 테이블을 조인하면 테이블 숫자만큼 실행 계획 레코드가 출력되지만 id는 같다.

- 쿼리문장이 여러개의 SELECT로 구성되어 있으면 각 레코드마다 다른 id를 가지게 된다.

  만약 하나의 SELECT문장 안에서 여러 개의 테이블을 조인하면 조인되는 테이블 개수만큼 실행 계획 레코드가 출력되지만 같은 id가 부여된다.

```
EXPLAIN
SELECT e.emp_no, e.first_name, s.from_date, s.salary FROM employees e, salaries s
WHERE e.emp_no=s.emp_no
LIMIT 10;
```
| id   | select_type | table | type  | key          | key_len | ref                | rows   | Extra       |
| ---- | ----------- | ----- | ----- | ------------ | ------- | ------------------ | ------ | ----------- |
| 1    | SIMPLE      | e     | index | ix_firstname | 44      |                    | 300584 | Using index |
| 1    | SIMPLE      | s     | ref   | PRIMARY      | 4       | employees.e.emp_no | 4      |             |



### select_type

각 단위 select 쿼리가 어떤 타입의 쿼리인지 표시되는 칼럼이다.

쿼리가 아무리 복잡하더라도 실행 계획에서 select_type이 SIMPLE인 단위 쿼리를 반드시 하나만 존재한다.

일반적으로 제일 바깥 SELECT 쿼리의 select_type이 SIMPLE로 표시된다.

- **SIMPLE**
  
  UNION이나 서브 쿼리를 사용하지 않는 단순한 SELECT 쿼리인 경우 SIMPLE로 표시된다.

  ```
  SELECT * FROM USER;
  ```

- **PRIMARY**

  UNION이나 서브 쿼리가 포함된 SELECT 쿼리의 실행 계획에서 가장 바깥쪽에 있는 단위 쿼리는 select_type이 PRIMARY로 표시된다. 하나만 존재한다.
  하나만 존재한다.

  ```
  SELECT * FROM (SELECT * FROM USER) t;
  ```

- **UNION**

  UNION으로 결합하는 단위 SELECT 쿼리 중 두번째 이후 단위쿼리의 select_type는 UNION으로 표시된다.

- **DEPENDENT UNION**

  내부 쿼리가 외부 값의 영향을 받을 때 DEPENDENT가 붙는다.
  DEPENDENT 키워드를 포함하는 서브쿼리는 외부 쿼리에 의존적이므로 절대 외부 쿼리보다 먼저 실행될 수 없다.
  그래서 DEPENDENT가 포함된 서브쿼리는 비효율적인 경우가 많다.

  ```
  EXPLAIN
  SELECT
    e.first_name,
    ( SELECT CONCAT(’Salary change count : ‘, COUNT(*)) AS message
      FROM salaries s WHERE s.emp_no=e.emp_no
      UNION
      SELECT CONCAT(‘Department change count : ‘, COUNT(*)) AS message
      FROM dept_emp de WHERE de.emp_no=e.emp_no
    ) AS message
  FROM employees e
  WHERE e.emp_no=10001;	
  ```
  위 쿼리를 보면 UNION으로 결합되는 각 쿼리가 이 서브쿼리의 외부에서 정의된 employees 테이블의 emp_no 칼럼을 사용하고 있음을 알 수 있다. 이렇게 내부 쿼리가 외부의 값을 참조해서 처리될 때 DEPENDENT 키워드가 select_type에 붙는다.


|id|select_type|table|type|key|key_len|ref|rows|Extra|
|--|-----------|-----|----|---|-------|---|----|-----|
|1 |PRIMARY|e|const|PRIMARY|4|const|1||
|2|DEPENDENT SUBQUERY|s|ref|PRIMARY|4|const|17|Using index|
|3|DEPENDENT UNION|de|ref|ix_empno_fromdate|3||1|Using where; Using index|
||DEPENDENT RESULT|<union2,3>|ALL||||||

- **UNION RESULT**
  
  UNION 결과를 담아두는 테이블을 의미한다.

- **SUBQUERY**

  FROM절 이외에서 사용되는 서브 쿼리를 의미한다.

  ```mysql
  EXPLAIN SELECT
  e.first_name,
  (SELECT COUNT(*) FROM dept_emp de, dept_manager dm WHERE dm.dept_no=de.dept_no) AS cnt FROM employees e
  WHERE e.emp_no=10001;
  ```

| id   | select_type | table | type  | key     | key_len | ref                  | rows  | Extra       |
| ---- | ----------- | ----- | ----- | ------- | ------- | -------------------- | ----- | ----------- |
| 1    | PRIMARY     | e     | const | PRIMARY | 4       | const                | 1     |             |
| 2    | SUBQUERY    | dm    | index | PRIMARY | 16      |                      | 24    | Using index |
| 3    | SUBQUERY    | de    | ref   | PRIMARY | 12      | employees.dm.dept_no | 18603 | Using index |
|      |             |       |       |         |         |                      |       |             |

FROM절에 사용된 서브 쿼리는 select_type이 DERIVED라고 표시되고, 그밖의 위치에서 사용된 서브쿼리는 전부 SUBQUERY로 표시된다.

DERIVED는 파생 테이블과 같은 의미로 이해하면 된다.

- **DEPENDENT SUBQUERY**
  
  서브 쿼리가 바깥쪽 SELECT 쿼리에서 정의된 칼럼을 사ㅛㅇ하는 경우를 DEPENDENT SUBQUERY라고 한다.
  
  마찬가지로 DEPENDENT 키워드가 붙기 떄문에 일반 서브쿼리보다 처리 속도가 느릴 때가 많다.

  ```mysql
  SELECT * FROM user u1 WHERE EXISTS (
      SELECT * FROM user u2 WHERE u1.user_id = u2.user_id 
  );
  ```
  
- **DERIVED**

  Subquery가 FROM절에 사용된 경우 MySQL은 항상 select_type이 DERIVED인 실행 계획을 만든다.   
  DERIVED는 단위 쿼리의 실행 결과를 메모리나 디스크에 임시 테이블로 생성하는 것을 의미한다.   
  select_type이 DERIVED인 경우에 생성되는 임시 테이블을 파생 테이블이라고도 한다.   

  파생 테이블에는 인덱스가 전혀 없으므로 다른 테이블과 조인할 때 성능상 불리할 때가 많다.

  ```mysql
  SELECT * FROM (SELECT * FROM USER) t; 
  ```

- **UNCACHEABLE SUBQUERY**

  조건이 똑같은 서브 쿼리가 실행될 때는 다시 실행하지 않고 이전의 실행 결과를 그대로 사용할 수 있게 서브 쿼리의 결과를 내부적인 캐시 공간에 담아둔다.

  SUBQUERY는 바깥쪽의 영향을 받지 않으므로 처음 한번만 실행해서 그 결과를 캐시하고 필요할 때 캐시된 결과를 이용한다.

  DEPENDENT SUBQUERY는 의존하는 바깥쪽 쿼리의 칼럼의 값 단위로 캐시해두고 사용한다.

  ![subquery-cache](/Users/seungjune/blog/DanSJKim.github.io/static/media/subquery-cache.png)

  SUBQUERY의 경우 위와같이 캐시를 사용한다. 하지만 DEPENDENT SUBQUERY는 서브 쿼리 결과가 딱 한번만 캐시되는것이 아니라 외부쿼리의 값 단위로 캐시가 만들어지는 방식으로 처리된다. 즉, 위의 그림이 차례대로 반복되는 구조다.

  하지만 아래와 같은 경우 캐시를 사용하지 못하기 때문에 "UNCACHEABLE SUBQUERY"가 나타나게 된다.

  - 사용자 변수가 서브 쿼리에 사용된 경우
  - NOT-DETERMINISTIC 속성의 스토어드 루틴이 서브 쿼리 내에 사용된 경우
  - UUID()나 RAND()와 같이 결과값이 호출할 때마다 달라지는 함수가 서브 쿼리에 사용된 경우

- **UNCACHEABLE UNION**

  UNCACHEABLE 과 UNION 키워드는 위에서 살펴보았다. UNCACHEABLE UNION은 두 속성이 혼합된 select_type을 의미한다.

### table

쿼리 문장에 에 사용한 테이블을 기준으로 표시된다.

별칭이 부여된 경우에는 별칭이 표시된다.   

<>로 둘러쌓였을 경우에는 임시테이블을 사용한 경우다.   

### type

MySQL 서버가 각 테이블의 레코드를 어떤 방식으로 읽었는지를 의미한다.
방식이라 함은 인덱스를 사용해 레코드를 읽었는지 테이블을 처음부터 끝까지 읽는 풀 테이블 스캔으로 레코드를 읽었는지 등을 말한다.
일반적으로 쿼리를 튜닝할 때 인덱스를 효율적으로 사용하는 것이 중요하므로 실행 계획에서 type칼럼은 반드시 체크해야 할 중요한 정보다.

- system
- ref
- unique_subquery 
- index_merge
- const
- fulltext
- index_subquery
- index
- eq_ref
- ref_or_null
- range
- ALL

위 타입들은 성능이 빠른 순서대로 나열된 것이다. MySQL 옵티마이저는 이런 접근 방식과 비용을 함께 계산해서 최소의 비용이 필요한 접근 방식을 선택한다.

- **system**

  레코드가 1건만 존재하는 테이블 또는 한 건도 존재하지 않는 테이블을 참조하는 형태의 접근 방법이다. system은 테이블에 레코드가 1건 이하인 경우에만 사용할 수 있는 접근 방법이므로 실제 애플리케이션에서 사용되는 쿼리의 실행 계획에서는 거의 보이지 않는다.

  ```mysql
  EXPLAIN
  SELECT * FROM tb_dual;
  ```

  | id   | select_type | table   | type   | key  | key_len | ref  | rows | Extra |
  | ---- | ----------- | ------- | ------ | ---- | ------- | ---- | ---- | ----- |
  | 1    | SIMPLE      | tb_dual | system |      |         |      |      |       |

- **const**

  테이블 레코드 건수에 관계 없이 쿼리가 프라이머리 키나 유니크 키 칼럼을 이용하는 WHERE 조건절을 가지고 있으며, 반드시 1건을 반환하는 쿼리의 처리 방식
  const type인 실행 계획은 MySQL의 옵티마이저가 쿼리를 최적화하는 단계에서 쿼리를 상수화 하기 때문에 const라고 표시되는 것이다.

  ```mysql
  EXPLAIN
  SELECT * FROM employees WHERE emp_no=10001;
  ```

| id   | select_type | table     | type  | key     | key_len | ref   | rows | Extra |
| ---- | ----------- | --------- | ----- | ------- | ------- | ----- | ---- | ----- |
| 1    | SIMPLE      | employees | const | PRIMARY | 4       | const | 1    |       |

​		프라이머리키의 일부만 조건으로 사용할 때는 접근 방식이 const가 아닌 ref로 표시된다.

- **eq_ref**

  인덱스가 UNIQUE이거나 PRIMARY KEY인 경우의 조인으로 const를 제외한 조인 중 가장 좋은 형태이다.

  **조인에서 첫번째 읽은 테이블의 칼럼 값을 이용해 두번째 테이블을 프라이머리키나 유니크키로 동등조건 검색할 때**를 eq_ref 라고 한다.(두번째 테이블은 반드시 1건의 레코드만 반환)

  두번째 이후에 읽히는 테이블을 유니크로 검색할 때 그 유니크 인덱스는 **NOT NULL**이어야 하며, 다중 칼럼으로 만들어진 프라이머리키나 유니크키라면 **모든 칼럼이 비교조건에 사용되어야 eq_ref 접근 방법이 사용될 수 있다.** 즉, 조인에서 두 번째 이후에 읽는 테이블에서 반드시 1건만 존재한다는 보장이 있어야 사용할 수 있는 접근 방법이다.

  ```mysql
  EXPLAIN
  SELECT * FROM dept_emp de, employees e
  WHERE e.emp_no=de.emp_no AND de.dept_no='d005';
  ```

  | id   | select-type | table | type   | key     | key_len | ref                 | rows  | Extra       |
  | ---- | ----------- | ----- | ------ | ------- | ------- | ------------------- | ----- | ----------- |
  | 1    | SIMPLE      | de    | ref    | PRIMARY | 12      | const               | 53288 | Using where |
  | 1    | SIMPLE      | e     | eq_ref | PRIMARY | 4       | employees.de.emp_no | 1     |             |

- **ref**

  조인 순서와 인덱스 종류에 관계 없이 동등 조건으로 검색(1건의 레코드만 반환된다는 보장이 없어도 됨)

  프라이머리 키나 유니크 키에 대한 제약 조건도 없다.

  인덱스 종류와 관계없이 동등 조건으로 검색할 때는 ref 접근 방법이 사용된다.

  반드시 1건이라는 보장이 없기 때문에 const나 eq_ref보다는 빠르지 않지만 동등조건으로만 비교되므로 매우 빠른 레코드 조회 방법중 하나다.

  ```mysql
  EXPLAIN
  
  SELECT * FROM dept_emp WHERE dept_no='d005';
  ```

  | id   | select-type | table    | type | key     | key_len | ref   | rows  | Extra       |
  | ---- | ----------- | -------- | ---- | ------- | ------- | ----- | ----- | ----------- |
  | 1    | SIMPLE      | dept_emp | ref  | PRIMARY | 12      | const | 53288 | Using where |

  dept_emp 테이블의 프라이머리 키를 구성하는 컬럼(dept_no + emp_no) 중에 일부만 동등조건으로 WHERE절에 명시했기 때문에 조건에 일치하는 레코드가 1건이라는 보장이 없다. 그래서 const가 아닌 ref접근 방법이 사용됐으며 실행 계획의 ref칼럼 값에는 const가 명시됐다.
  
  다시한번 type들을 비교해 보면
  
  **const**
  
  조인의 순서에 관계없이 프라이머리 키나 유니크 키의 모든 칼럼에 대해 동등(Equal) 조건으로 검색(반드시 1 건의 레 코드만 반^)
  
  **eq_req**
  
  조인에서 첫 번째 읽은 테이블의 칼럼값을 이용해 두 번째 테이블을 프라이머리 키나 유니크 키로 동등(Equal) 조건 검색(두 번째 테이블은 반드시 1 건의 레코드만 반환)
  
  **ref**
  
  조인의 순서와 인덱스의 종류에 관계없이 동등(Equal) 조건으로 검색(1 건의 레코드만 반환된다는 보장이 없어도 됨)
  
  위 세가지 접근방식 모두 WHERE 조건절에 사용되는 비교 연산자는 동등 비교 연산자여야 한다는 공통점이 있다.
  
  세가지 모두 매우 좋은 접근 방법으로 인덱스 분포도가 나쁘지 않다면 성능상의 문제를 일으키지 않는 접근 방법이다. 쿼리를 튜닝할 때도 이 세가지 접근 방법에 대해서는 크게 신경쓰지 않고 넘어가도 무방하다.
  
- **fulltext**

  MySQL의 전문 검색(full text) 인덱스를 사용해 레코드를 읽는 접근 방법을 의미한다.

  전문 검색은 “MATCH … AGAINST …”구문을 사용해서 실행하는데, 반드시 해당 테이블에 전문 검색용 인덱스가 준비돼 있어야 한다.

  ```mysql
  EXPLAIN
  SELECT *
  FROM employee_name
  WHERE emp_no=10001
  	AND emp_no BETWEEN 10001 AND 10005
  AND MATCH(first_name, last_name) A6AINST('Facello' IN BOOLEAN MODE);
  ```

  | id   | select_type | table         | type  | key     | key_len | ref   | rows | Extra       |
  | ---- | ----------- | ------------- | ----- | ------- | ------- | ----- | ---- | ----------- |
  | 1    | SIMPLE      | employee_name | const | PRIMARY | 4       | const | 1    | Using where |

  위 쿼리 문장에서 최종적으로 MySQL 옵티마이저가 선택한 것은 const타입의 조건이다.

  const타입의 첫번쨰 조건이 없으면 둘중 어떤것을 선택할까?

  | id   | select_type | table         | type     | key     | key_len | ref  | rows | Extra       |
  | ---- | ----------- | ------------- | -------- | ------- | ------- | ---- | ---- | ----------- |
  | 1    | SIMPLE      | employee_name | fulltext | fx_name | 0       |      | 1    | Using where |

  이번에는 range타입의 두번째 조건이 아니라 fulltext조건인 세번쨰 조건을 선택했다.

  MATCH AGAINST를 사용하면 MySQL은 주저없이 fulltext 접근방식을 사용하는 경향이 있다. 하지만 전문 검색 인덱스를 이용하는 fulltext보다 일반 인덱스를 이용하는 range 접근 방법이 더 빨리 처리되는 경우가 더 많기 때문에 전문 검색 쿼리를 사용할 떄는 각 조건별로 성능을 확인해 보는 편이 좋다.

- **ref_or_null**

  ref 접근 방식과 같은데 NULL 비교가 추가된 형태다.

- **unique_subquery**

  WHERE 조건절에서 사용될 수 있는 IN (subquery) 형태의 쿼리를 위한 접근 방식이다.

  **서브쿼리에서 중복되지 않은 유니크한 값만 반환할 때** 이 접근방법을 사용한다.

  ```mysql
  EXPLAIN
  SELECT FROM departments WHERE dept_no IN (
  SELECT dept_no FROM dept_emp WHERE emp_no=10001);
  ```

  | id   | select_type        | table       | type            | key         | key_len | ref         | rows | Extra                     |
  | ---- | ------------------ | ----------- | --------------- | ----------- | ------- | ----------- | ---- | ------------------------- |
  | 1    | PRIMARY            | departments | index           | ux_deptname | 123     |             | 9    | Using where; Using index; |
  | 2    | DEPENDENT SUBQUERY | dept_emp    | unique_subquery | PRIMARY     | 16      | func, const | 1    | Using index; Using where  |

  emp_no=10001인 레코드 중에 부서 번호는 중복이 없기 때문에 dept_emp 테이블의 접근 방식은 unique_subquery로 표시된 것이다.

- **index_subquery**
  In (subquery)에서 subquery가 중복된 값을 반환할 수는 있지만 중복된 값을 인덱스를 이용해 제거할 수 있을 때 index_subquery 접근 방법이 사용된다.

  **unique_subquery**
   IN (subquery) 형태의 조건에서 subquery의 반환 값에는 중복이 없으므로 별도의 중복 제거 작업이 필요하지 않음

  **index_subquery**

  IN (subquery) 형태의 조건에서 subquery의 반환 값에 중복된 값이 있을 수 있지만 인덱스를 이용해 중복된 값을 제거할 수 있음

- **range**

  range는 인덱스를 하나의 값이 아니라 범위로 검색하는 경우를 의미한다.

  주로 “<, >, IS NULL, BETWEEN, IN, LIKE” 등의 연산자를 이용해 인덱스를 검색할 때 사용된다.

- **index_merge**

  index_merge 접근 방식은 2개 이상의 인덱스를 이용해 각각의 검색 결과를 만들어낸 후 그 결과를 병합하는 처리 방식이다.

  ```mysql
  EXPLAIN
  SELECT * FROM employees
  WHERE emp_no BETWEEN 10001 AND 11000
  	OR first_name='Smith';
  ```

  | id   | select_type | table     | type        | key                   | key_len | ref  | rows | Extra                                          |
  | ---- | ----------- | --------- | ----------- | --------------------- | ------- | ---- | ---- | ---------------------------------------------- |
  | 1    | SIMPLE      | employees | index_merge | PRIMARY, ix_firstname | 4,44    |      | 1521 | Using union(PRIMARY,ix_firstname); Using where |

  

<span style="color:red">성능상 문제</span>

- **index**
  인덱스를 처음부터 끝까지 읽는 풀스캔을 의미한다.

  index 접근 방법 은 다음의 조건 가운데 (첫 번째+두 번째) 조건을 중족하거나 (첫 번째+세 번째) 조건을 중족하는 쿼리 에서 사용되는 읽기 방식이다.

  - range나 const 또는 ref와 길은 접근 방식으로 인덱스를 사용하지 못하는 경우
  - 인덱스에 포함된 칼럼만으로 처리할 수 있는 쿼리인경우(즉, 데이터 파일을 읽지 않아도 되는 경우
  - 인덱스를 이용해 정렬이나 그룹핑 작업이 가능한 경우(즉, 별도의 정렬 작업을 피할 수 있는 경우)

- **all**
  조인 시 모든 테이블의 모든 row를 스캔한 경우. 성능이 가장 안좋다.



### possible_key

index로 사용할 것으로 예상되는 키. 여러개가 나올 수 있다.
옵티마이저는 쿼리를 처리하기위해 여러가지 처리 방법을 고려해 비용이 가장 낮을 것으로 예상되는 실행 계획을 선택해 쿼리를 실행한다. possible_keys 칼럼에 내용은 옵티마이저가 최적의 실행계획을 만들기 위해 후보로 선정했던 인덱스 목록이다. 이 컬럼은 무시해도 좋다.



### key

최종 선택된 실행 계획에서 사용된 인덱스
index_merge 실행계획이 사용될 때는 2개 이상의 인덱스가 사용된다.



### key_len

MySQL이 사용한 인덱스의 길이를 나타낸다. 사용자가 쉽게 무시하는 정보지만 사실 매우 중요한 정보중에 하나다.

쿼리를 처리하기 위해 다중 칼럼으로 구성된 인덱스에서 몇개의 칼럼까지 사용했는지 알려준다.

인덱스의 각 레코드에서 몇 바이트까지 사용했는지 알려주는 값이다.

int가 기본형인데 기본형이 아닌 string을 인덱스로 쓰면 비용이 더 많이듬



### ref

접근 방법이 ref 방식이면 비교 조건으로 어떤 값이 제공됐는지 보여 준다.
func라고 표시될 경우엔 값을 그대로 사용한게 아니라 변환이나 연산을 거친 후에 값을 사용했다는 뜻이다.

```mysql
EXPLAIN
SELECT ★
FROM employees e, dept_emp de WHERE e.emp_no=(de.emp_no-1);
```



### rows

실행 계획의 효율성 판단을 위해 예측했던 레코드 건수를 보여준다.

이 값은 각 스토리지 엔진별로 가지고 있는 통계 정보를 참조해 MySQL 옵티마이저가 산출해 낸 예상 값이라서 정확하지는 않다.

rows 값은 반환하는 레코드의 예측치가 아니라, 쿼리를 처리하기 위해 얼마나 많은 레코드를 디스크로부터 읽고 체크해야하는지를 의미한다.

limit가 포함된 쿼리는 오차가 너무 심해서 별로 도움이 되지 않는다.



### Extra

칼럼 이름과 달리, 쿼리를 실행하면서 처리한 주요 작업에 대한 내용이 표시된다.
쿼리를 튜닝할 때 중요한 단서가 되는 내용이 많이 표시된다.

- **쿼리가 요건을 제대로 반영하고 있는지 확인해야 하는 경우**
  Full scan on NULL key
  Impossible HAVING(MySQL 5.1부터)
  Impossible WHERE(MySQL 5.1부터)
  Impossible WHERE noticed after reading const tables
  No matching min/max row(MySQL 5.1부터)
  No matching row in const table(MySQL 5.1부터)
  Unique row not found(MySQL 5.1부터)

  쿼리가 요건을 제대로 반영해서 작성됐거나 버그가 생길 가능성은 없는지 확인해야 한다.
  개발용 데이터베이스에 테스트용 레코드가 제대로 준비돼 있는지 확인해보는 것도 좋다.
  쿼리가 업무적인 요건을 제대로 반영하고 있다면 무시해도 된다.

- **쿼리의 실행 계획이 좋지 않을 경우**
  Range checked for each record (index map: N)
  **Using filesort**
  Using join buffer (MySQL 5.1부터)
  **Using temporary**
  **Using where**

  먼저 쿼리를 더 최적화할 수 있는지 검토해보는 것이 좋다.
  성능상 허용 가능하다면 넘어가도 좋다.

- **쿼리의 실행 계획이 좋은 경우**
  Distinct
  Using index
  Using index for group-by

  최적화 되어 처리되고 있음을 알려주는 지표 정도로 생각하면 된다.
  특히 Using index는 쿼리가 커버링 인덱스(인덱스만으로 쿼리를 처리)로 처리되고 있음을 알려주는 것인데, MySQL에서 제공하는 최고 성능을 보여준다.
  쿼리를 아무리 최적화해도 성능 요건에 미치지 못한다면 커버링 인덱스되는 형태로 유도해보는 것도 좋다.
  
  **Using filesort, Using temporary, Using where 세가지가 가장 많이 나오므로 왜 사용되는지 알아둔다.**

**Using filesort**

​	ORDER BY 처리가 **인덱스를 사용하지 못할 때**만 실행계획의 Extra칼럼에 'Using filesort' 코멘트가 표시된다.

​	이럴 경우에 MySQL 옵티마이저는 레코드를 읽어서 Sort buffer에 복사하고, 정렬해서 그 결과를 클라이언트에 보낸다.

​	정렬 작업이 쿼리 실행시 처리되므로 레코드 대상 건수가 많아질 수록 쿼리 응답 속도가 느려진다.

**Using temporary**

MySQL이 쿼리를 처리하는 동안 중간 결과를 담아두기 위해 임시테이블을 사용한다.

```mysql
EXPLAIN
SELECT * FROM employees GROUP BY gender ORDER BY MIN(emp_no);
```

위 쿼리는 **GROUP BY**칼럼과 ORDER BY 칼럼이 다르기 때문에 임시 테이블이 필요한 작업이다.

인덱스를 사용하지 못하는 **GROUP BY** 쿼리는 실행 계획에서 Using temporary 메세지가 표시되는 가장 대표적인 형태의 쿼리다.

Extra에 Using Temporary가 표시되지 않아도 실제 내부적으로는 임시테이블을 사용할 때도 많다.

| id   | select_type | table     | type | key  | key_len | ref  | rows   | Extra                           |
| ---- | ----------- | --------- | ---- | ---- | ------- | ---- | ------ | ------------------------------- |
| 1    | SIMPLE      | employees | ALL  |      |         |      | 300584 | Using temporary; Using filesort |



**Using where** 

![mysql-engine](/Users/seungjune/blog/DanSJKim.github.io/static/media/mysql-engine.png)

MySQL엔진 레이어에서 별도의 가공을 해서 필터링 작업을 처리한 경우에 Extra칼럼에 Using where가 표시된다.

```mysql
EXPLAIN
SELECT * FROM employees WHERE emp_no BETWEEN 10001 AND 10100 AND gender=‘F’;
```

위 쿼리에서 BETWEEN은 작업범위제한조건 gender는 체크조건이다.

작업범위제한조건은 스토리지엔진에서 처리되지만 체크조건은 MySQL엔진에서 처리된다. (5.1 InnoDB 플러그인버전부터 체크조건까지 스토리지엔진으로 전달된다.)

이 쿼리에서 작업 범위 제한 조건은 “emp_no BETWEEN 10001 AND 10100”이며 “gender=‘F”’는 체 크 조건임을 쉽게 알 수 있다. 그런데 처음의 emp_no 조건만을 만족하는 레코드 건수는 100건이지만 두 조건을 모두 만족하는 레코드는 37건밖에 안 된다. 이는 스토리지 엔진은 100개를 읽어서 MySQL 엔진에 넘겨줬지만 MySQL 엔진은 그중에서 63건의 레코드를 그냥 필터링해서 버렸다는 의미다. 여기 서 “Using where”는 63건의 레코드를 버리는 처리를 의미한다.



### EXPLAIN EXTENDED(Filtered 칼럼)



### EXPLAIN EXTENDED(추가 옵티마이저 정보)



### EXPLAIN PARTITIONS(Partitions 칼럼)



## MySQL의 주요 처리 방식

성능에 미치는 영향이 큰 실행 계획과 연관이 있는 단위 작업에 대해 살펴보자

풀 테이블 스캔을 제외한 나머지는 모두 스토리지 엔진이 아니라 MySQL 엔진에서 처리되는 내용이다. MySQL에서 부가적으로 처리하는 작업은 대부분 성능에 미치는 영향이 큰데, 안타깝게도 모두 쿼리의 성능을 저하시키는 요인들이다. MySQL엔진에서 처리하는 데 시간이 오래 걸리는 작업의 원리를 알아둔다면 쿼리를 튜닝하는데 상당한 도움이 될 것이다.

### 풀 테이블 스캔

인덱스를 사용하지 않고 테이블의 데이터를 처음부터 끝까지 읽어서 요청된 작업을 처리하는 작업을 의미한다.

풀 테이블 스캔을 선택하는 조건은 다음과 같다.

- 테이블의 레코드 건수가 너무 작아서 인덱스를 통해 읽는 것보다 풀테이블스캔을 하는 편이 더 빠른 경우 (일반적으로 테이블이 페이지 1개로 구성된 경우)
- WHERE나 ON절에 인덱스를 이용할 수 있는 적절한 조건이 없는 경우
- 인덱스 레인지 스캔을 사용할 수 있는 쿼리라 하더라도 옵티마이저가 판단한 조건 일치 레코드 건수가 너무 많은 경우
- 반대로, max_seek_for_key 변수를 특정 값으로 설정하면 MySQL 옵티마이저는 인덱스의 Cardinality나 Selectivity를 무시하고, 최대 N건만 읽으면 된다고 판단하게 한다. 이 값을 작게 설정할 수록 MySQL 서버가 인덱스를 더 사용하도록 유도한다.

### ORDER BY 처리(Using filesort)

레코드 1~2건을 가져오는 쿼리를 제외하면 대부분의 SELECT 쿼리에서 정렬은 필수적으로 사용된다.

정렬을 처리하기 위해서는 인덱스를 이용하는 방법과 쿼리가 실행될 때 Filesort라는 별도의 처리를 이용하는 방법으로 나눌 수 있다.

|               | 장점                                                         | 단점                                                         |
| ------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 인덱스를 이용 | INSERT, UPDATE, DELETE 쿼리가 실행될 때 이미 인덱스가 정렬돼 있어서 순서대로 읽기만 하면 되므로 매우 빠르다. | INSERT, UPDATE, DELETE 작업 시 부가적인 인덱스 추가/삭제 작업이 필요하므로 느리다. 인덱스 때문에 디스크 공간이 더 많이 필요하다. 인덱스 개수가 늘어날 수록 InnoDB의 버퍼 풀이나 MyISAM의 키 캐시용 메모리가 많이 필요하다. |
| Filesort 이용 | **인덱스를 생성하지 않아도 되므로** 인덱스를 이용할 때의 단점이 장점으로 바뀐다. 정렬해야 할 레코드가 많지 않으면 메모리에서 Filesort가 처리되므로 충분히 빠르다. | **정렬 작업이 쿼리 실행 시 처리**되므로 레코드 대상 건수가 많아질 수록 응답 속도가 느리다. |

MySQL이 인덱스를 이용하지 않고 별도의 정렬 처리를 수행했는지는 실행 계획의 Extra 칼럼에 “Using filesort”라는 코멘트가 표시되는지로 판단할 수 있다

**Sort buffer**(인덱스를 사용하지 않을 때?)

MySQL은 정렬을 수행하기 위해 별도의 메모리 공간을 할당받아서 사용하는데, 이 메모리 공간을 소트버퍼라고 한다.

소트 버퍼는 정렬이 필요한 경우에만 할당된다.

정렬이 문제가 되는 이유는?

![sortbuffer](/Users/seungjune/blog/DanSJKim.github.io/static/media/sortbuffer.png)

정렬할 레코드가 소량이어서 메모리에 할당된 소트버퍼만으로 정렬할 수 있으면 아주 빠르게 정렬할 수 있다. 하지만 정렬할 레코드 건수가 소트버퍼로 할당된 공간보다 크다면 정렬할 레코드를 여러 조각으로 나눠서 처리하는데, 이 과정에서 임시저장을 위해 디스크를 사용한다.

이 과정에서 디스크의 쓰기와 읽기를 유발하기 떄문에 성능이 저하된다.

### GROUP BY 처리

GROUP BY 또한 ORDER BY와 같이 쿼리가 스트리밍된 처리를 할 수 없게 하는 요소 중 하나다.

GROUP BY 절이 있는 쿼리에서는 HAVING절을 사용할 수 있는데, HAVING절은 GROUP BY 결과에 대해 필터링 역할을 수행한다. GROUP BY에 사용된 조건은 인덱스를 사용해서 처리될 수 없으므로 HAVING절을 튜닝하려고 인덱스를 생성하거나 다른 방법을 고민할 필요는 없다.



GROUP BY 작업도 인덱스를 사용하는 경우와 그렇지 못한 경우로 나뉜다.

인덱스를 이용할 때는 **인덱스 스캔** 방법과 인덱스를 건너뛰면서 읽는 **루스 인덱스 스캔**이라는 방법으로 나뉜다.

그리고 인덱스를 사용하지 못하는 쿼리에서 GROUP BY 작업은 임시 테이블을 사용한다.

- 인덱스 스캔
- 루스 인덱스 스캔



### DISTINCT 처리

특정 칼럼의 유니크한 값만 조회하려면 SELECT 쿼리에 DISTINCT를 사용한다.

Dis

### 임시 테이블(Using temporary)

MySQL 엔진이 스토리지 엔진으로부터 받아온 레코드를 정렬하거나 그룹핑할 때는 내부적인 임시 테이블을 사용한다.



### 테이블 조인

MySQL은 다른 DBMS보다 조인을 처리하는 방식이 단순하다. 현재 릴리즈된 MySQL의 모든 버전에서 조인 바잇ㄱ은 네스티드-루프로 알려진 중첩된 루프와 같은 형태만 지원한다. 그리고 조인되는 각 테이블 간의 레코드를 어떻게 연결할 지에 따라 여러 가지 종류의 조인으로 나뉜다.

**조인의 종류**

- INNER JOIN
- OUTER JOIN
  - LEFT OUTER JOIN
  - RIGHT OUTER JOIN
  - FULL OUTER JOIN

조인의 조건을 어떻게 명시하느냐에 따라 NATURAL JOIN과 CROSS JOIN(FULL JOIN, CARTESIAN JOIN)으로 구분할 수 있다.



## MySQL 실행 계획 분석 시 주의사항

쿼리의 실행계획을 확인할 때 각 칼럼에 표시되는 값 중에서 특별히 주의해서 확인해야 하는 항목

### Select_type 칼럼의 주의대상

**DERIVED**

DERIVED는 FROM절에서 사용된 서브쿼리로부터 발생한 임시 테이블을 의미한다.

임시 테이블은 메모리나 디스크에 저장된다. 일반적으로 메모리에 저장하는 경우에는 성능에 영향을 미치지 않지만

데이터 크기가 클 경우에는 임시테이블을 디스크에 저장한다. 디스크에 저장하는 경우에 성능이 떨어진다.

**UNCACHEABLE SUBQUERY**

FROM절 이외의 부분에서 사용하는 서브쿼리는 MySQL옵티마이저가 최대한 캐시되어 재사용될 수 있게 유도한다.

**하지만 사용자 변수나 일부 함수가 사용된 경우에는 이러한 캐시 기능을 사용할 수 없게 만든다.**

이런 경우에는 사용자 변수를 제거하거나 다른 함수로 대체해서 사용 가능할지 검토해보는 것이 좋다.

**DEPENDENT SUBQUERY**

FROM절 이외의 부분에서 사용하는 서브쿼리가 자체적으로 실행되지 못하고, **외부 쿼리에서 값을 전달받아 실행되는 경우** 발생한다. 이러한 경우에는 서브쿼리가 외부쿼리에 의존적이기 때문에 전체 쿼리 성능을 느리게 만든다.

**가능하다면 외부 쿼리와의 의존도를 제거하는 것이 좋다.**

### Key 칼럼의 주의 대상

쿼리가 인덱스를 사용하지 못할 때 Key칼럼에 아무것도 표시되지 않는다. 쿼리가 인덱스를 사용할 수 있게 인덱스를 추가하거나, WHERE 조건을 변경하는 것이 좋다.

### Rows 칼럼의 주의 대상

- **쿼리가 실제 가져오는 레코드 수보다 훨씬 더 큰 값이 Rows 칼럼에 표시되는 경우**

  쿼리가 인덱스를 정상적으로 사용하고 있는지 확인하고, 충분히 식별성을 가지고 있는 칼럼을 선정해 인덱스를 다시 생성하거나 쿼리의 요건을 변경하는 것이 좋다.

- LIMIT가 포함된 쿼리라 하더라도 LIMIT의 제한은 Rows칼럼의 고려 대상에서 제외된다.

  즉, 'LIMIT 1'로 1건만 SELECT하는 쿼리가 하더라도 Rows 칼럼에는 훨씬 큰 수치가 표현될 수도 있으며, 성능상 아무런 문제가 없고 최적화된 쿼리일 수도 있다는 것이다.

### Extra 칼럼의 주의 대상

**쿼리가 요건을 제대로 반영하고 있는지 확인해야 하는 경우**

- RJ scan on NULL key
- Impossible HAVING(MySQL 5.1 부터)
- Impossible WHERE(MySQL 5.1 부터)
- Impossible WHERE noticed after reading const tables
- No matching min/max row(MySQL 5.1 부터)
- No matching row in const table(MySQL 5.1 부터)
- Uniquerownotfound(MySQL5.1 부터)

위와 갈은 코멘트가 Extra 칼럼에 표시된다면 우선 쿼리가 요건을 제대로 반영해서 작성됐거나 버그가 생길 가능성은 없는지 확인해야 한다. **성능과 관계가 깊지 않고** 쿼리가 업무적인 요건을 제대로 반영하고 있다면 무시해도 된다.

**쿼리의 실행 계획이 좋지 않은 경우**

- Range checked for each record (index map: N)
- Using filesort
- Using join buffer (MySQL 5.1 부터)
- Using temporary
- Using where

위와 같은 코멘트가 Extra 칼럼에 표시된다면 먼저 쿼리를 더 최적화할 수 있는지 검토해보는 것이 좋다.

Using where가 표시될 때 Rows 컬럼이 실제 SELECT되는 레코드 건수보다 상당히 높은 경우는 반드시 보완해서 레코드 수의 차이를 줄이는 것이 중요하다. 

**쿼리의 실행 계획이 좋은 경우**

- Distinct

- Using index
- Using index for group-by

최적화되어 처리되고 있음을 알려주는 지표 정도로 생각하자.

두번 째의 Using index는 쿼리가 커버링 인덱스로 처리되고 있음을 알려주는 것인데, MySQL에서 제공할 수 있는 최고의 성능을 보여줄 것이다.

https://12bme.tistory.com/168


