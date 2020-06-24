---
date: '2020-05258T15:09:32.169Z'
template: 'post'
draft: false
slug: 'aws-network-configuration'
category: 'aws'
tags:
  - 'AWS'
  - 'Network'

description: 'AWS 네트워크 구성하기'
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



## VPC 생성

### VPC 이해하기

VPC를 이해하기 전에 VPN(Virtual Private Network)먼저 이해해야 한다.





VPC는 AWS 클라우드의 격리된 부분으로서, Amazon EC2 인스턴스와 같은 AWS 객체로 채워진다. 

나만의 데이터센터를 만든다.

VPC에 대한 IPv4 주소 범위를 지정해야 합니다. IPv4 주소 범위를 CIDR(Classless Inter-Domain Routing) 블록으로 지정합니다(예: 10.0.0.0/16). /16보다 큰 IPv4 CIDR 블록은 지정할 수 없습니다. 또는 Amazon 제공 IPv6 CIDR 블록을 VPC에 연결할 수 있습니다.





CIDR블록



