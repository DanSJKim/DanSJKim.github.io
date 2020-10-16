---
title: '[MySQL] EXISTS'
date: '2020-10-16T16:09:32.169Z'
template: 'post'
draft: false
slug: 'mysql-exists'
category: 'MySQL'
tags:
  - 'mysql'
description: 'EXISTS 쿼리문 이용하기'
socialImage: 'https://i.kym-cdn.com/entries/icons/facebook/000/019/513/til.jpg'
---

# EXISTS

- 데이터의 유무만 조회하고 싶을 땐 EXISTS 쿼리를 이용한다.

- 중간에 하나의 데이터라도 조회되면 쿼리를 종료시키기 때문에 비용을 절약할 수 있다.

예시

```mysql
SELECT EXISTS 
 (SELECT 1
  FROM TB_C_MD 
  WHERE C_MD_PRTNR_YN = 'Y' 
  	AND C_MD_NO = ?
  LIMIT 1)
  AS isExistingHelpi
```

SELECT 1 < 데이터 유무만 알면 되므로 의미없는 1을 넣어 준다.

