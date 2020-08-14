---
title: 'Semantic의 정의'
date: '2020-08-03T16:09:32.169Z'
template: 'post'
draft: false
slug: 'what-is-semantic'
category: 'web'
tags:
  - 'semantic'
  - 'wecode'
description: 'Semantic이란?'
socialImage: 'https://i.kym-cdn.com/entries/icons/facebook/000/019/513/til.jpg'
---

# Semantic이란?

사전적 의미: 의미론의, 어의의, 의미의

- 웹 데이터간의 관계. 사람이 인식하고 이해하는 관계가 아닌 기계가 이해할 수 있는 관계를 의미한다.
- 의미있는 요소를 뜻하며, 브라우저와 개발자 모두에게 요소의 의미나 목적을 명확하게 설명해줄 수 있는 요소다.
- 기존 HTML에서는 웹 문서의 구획을 나눌때 사용되던 <div>, <span> 태그는 구획을 명확하게 나누지 못했고 모호한 표현과 일관되지 않단느 특징을 가지고 있었고, 단순히 컨테이너 역할만 수행했다.
- 그 이후 등장한 HTML5에서는 <article>, <section>, <header>, <footer>등과 같이 시맨틱 요소를 정의해 구조적인 부분을 정의할 수 있는 구획 태그를 사용할 수 있게 되었다.

## Semantic web이 무엇인가?

- 인터넷이 발전하면서 방대한 양의 문서가 생산되어, 정보는 매우 다양해졌지만, 원하는 정보를 찾기가 어려워졌다.
- 그리하여 나온 것이 시맨틱 웹인데, 웹에 존재하는 수많은 웹페이지들에 메타데이터(Metadata)를 부여하여, 기존의 잡다한 데이터 집합이었던 웹페이지를 ‘의미’와 ‘관련성’을 가지는 거대한 데이터베이스로 구축하고자 하는 발상이다.
- 즉, 사용자가 찾고싶은 정보를 검색하면 컴퓨터가 사람을 대신해 정보를 찾아 뜻을 이해하고 이를 가공, 추론하는 것 까지 가능한 '차세대 지능형 웹'을 말한다.

## **Semantic을 사용해야하는 이유는 무엇인가?**

- 검색 엔진 노출에 적합함
- 개발자가 의도한 의미가 명확히 드러남
- 코드의 가독성을 높이고, 유지보수에 용이함

https://poiemaweb.com/html5-semantic-web

## Semantic과 표준과의 관계

웹 표준

- 웹에서 표준적으로 사용되는 기술이나 규칙
- W3C가 권고한 표준안에 따라 웹사이트를 작성할 때 이용하는 HTML, CSS, Javascript등에 대한 규정이 담겨있다.
- 웹 표준을 지킴으로써 검색봇을 통한 효율적 노출과 같은 검색엔진 최적화가 가능하다.

**W3C의 시맨틱 웹 관련 표준화 활동은 “Semantic Web Activity” 산하의 다음과 같은 3개의 그룹에서 수행되고 있다.**

- RDF Core Working Group : RDF 모델과 문법, RDF 스키마 등 RDF에 대한 요구사항 정의와 표준 규격 정의
- RDF Interest Group : RDF 기반의 응용개발 중심 그룹
- Web Ontology Working Group : 웹 온톨로지 언어규격 정의