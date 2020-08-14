---
title: 'Pagination'
date: '2020-07-05T16:09:32.169Z'
template: 'post'
draft: false
slug: 'pagination'
category: 'algorithm'
tags:
  - 'dfs'
  - 'coding-test'
description: '페이징 기능 정리'
socialImage: 'https://i.kym-cdn.com/entries/icons/facebook/000/019/513/til.jpg'
---

# Pagination

페이징을 구현할 때 마다 필요한 값들과 계산방법이 헷갈려서 정리해두려고 한다.

### 페이지네이션 구현에 필요한 요소 

currentList: 현재 리스트

listCount: 전체 리스트 길이

page: 현재페이지

rowLen: 페이지당 보여줄 아이템 수

pageLen: 보여줄 페이지 수

offset: 아이템 시작 인덱스

offset 구하는 공식: offset = (page-1) * rowLen



현재 페이지 요소 가져오기

```javascript
nowList() {
    let offset = (this.page-1) * this.rowLen;
    return this.totalList.slice(offset, offset+this.rowLen);
},
```

마지막 페이지 구하기

```javascript
let maxPage = Math.ceil(this.totalList.length/this.rowLen);
```

마지막 페이지를 초과하거나 1페이지 아래로 접근했을 경우 페이지 재설정하기

```javascript
setPage(page) {
    let maxPage = Math.ceil(this.exclList.length/this.rowLen);
    if (page > maxPage) page = maxPage;
    if (page < 1) page = 1;
    this.page = page;
},
```

