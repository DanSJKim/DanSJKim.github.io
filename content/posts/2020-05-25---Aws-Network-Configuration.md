---
title: 'AWS 네트워크 구성하기'
date: '2020-05-25T15:09:32.169Z'
template: 'post'
draft: false
slug: 'aws-network-configuration'
category: 'aws'
tags:
  - 'AWS'
  - 'Network'

description: ''
socialImage: 'https://i.kym-cdn.com/entries/icons/facebook/000/019/513/til.jpg'
---

# AWS 네트워크 구성하기

![network-config](/Users/seungjune/blog/DanSJKim.github.io/static/media/network-config.png)

기본적인 네트워크 구성 순서는 크게 다음과 같다.

- VPC 생성

- 서브넷 생성

- 인터넷 게이트웨이 생성

- 인터넷 게이트웨이 VPC 연결

- 라우팅 테이블 구성

- 보안 그룹 생성

- 웹 서버 1 생성

- 웹 서버 1의 AMI 생성

- AMI 기반 웹 서버 2 생성

- 로드밸런서 구성