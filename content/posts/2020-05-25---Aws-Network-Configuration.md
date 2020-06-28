---
title: AWS 네트워크 구성하기
date: '2020-05-25T15:09:32.169Z'
template: post
draft: false
slug: aws-network-configuration
category: aws
tags:
  - AWS
  - Network
socialImage: 'https://i.kym-cdn.com/entries/icons/facebook/000/019/513/til.jpg'
published: true
---

# AWS 네트워크 구성하기

![network-config](/Users/seungjune/blog/DanSJKim.github.io/static/media/network-config.png)

기본적인 네트워크 구성 순서는 크게 다음과 같다.

1. VPC 생성

2. 서브넷 생성

3. 인터넷 게이트웨이 생성

4. 인터넷 게이트웨이 VPC 연결

5. 라우팅 테이블 구성

6. 보안 그룹 생성

7. 웹 서버 1 생성

8. 웹 서버 1의 AMI 생성

9. AMI 기반 웹 서버 2 생성

10. 로드밸런서 구성
