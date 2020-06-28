---
title: "브라우저에 url을 치면 일어나는 일들"
date: "2020-02-07T22:40:32.169Z"
template: "post"
draft: false
slug: "what-happens-when-you-type-in-a-url"
category: "Web"
tags:
  - "wecode"
  - "web"
  - "browser"

description: "브라우저에 www.wecode.co.kr을 치면 어떤 일이 일어날까?"
socialImage: "/media/browser-dns-host.jpg"
---
## 주요 키워드
+ Hosting
+ IP
+ Domain
+ DNS

## 브라우저 통신 방법
![workflow](/media/homepage.png)
간단하게는 사용자가 브라우저에 **www.wecode.co.kr**을 치면 서버에 요청해서 응답을 받는 과정이다.


### Hosting
사용자가 서버에게 요청할 경우 항상 켜져있는 서버가 html, css, js를 다시 사용자에게 보낸다.
이렇게 할 수 있도록 하려면 컴퓨터가 항상 켜져있어야 하기 때문에 절대 꺼지지 않는 컴퓨터를 빌려서 파일을 저장해놔야 한다.
컴퓨터의 일부를 빌려서 거기에 파일들을 넣어놓는 것을 **Hosting**이라고 한다.
계속 켜져있는 서버를 **Host server**라고 한다.

서비스 예) AWS ec2/S3, cafe 호스팅센터 등

### IP
![workflow](/media/ip-address.png)
**IP 주소**는 Internet으로 통신하는 각 device에 부여된 고유한 값이다.
스마트폰이나 노트북부터 대규모 웹사이트의 콘텐츠를 서비스하는 서버에 이르기 까지 인터넷 상의 모든 컴퓨터는 **IP 주소**를 이용해 서로를 찾고 통신한다.
디바이스마다 고유한 값을 가지고 있기 때문에 각각은 서버가 될 수 있다.

### Domain
![workflow](/media/url-structure.png)
String으로 된 고유 주소. IP주소를 사람이 외워서 접속할 수 없기 때문에 Domain주소를 사용한다.   
ex) www.wecode.com, www.google.com


### DNS (Domain Name System)
DNS서버는 Domain name을 머신이 읽을 수 있는 IP 주소로 변환한다.   
DNS 요청 세부 과정은 다음과 같다.
![workflow](/media/dns-flow.png)

1. 사용자가 웹 브라우저를 열어서 주소 표시줄에 www.wecode.com을 입력하고 Enter키를 눌렀을 때, PC는 미리 설정되어 있는 Local DNS에게 IP 주소를 물어본다.   
2. 만약 Local DNS에 호스트 네임에 대한 정보가 없을 경우에 각 Local DNS에 설정된 Root DNS에 질의를 시작한다.   
   (Root DNS는 전 세계 13대, 우리나라에는 3대의 미러 서버가 설정되어 있다.)   
3. Root DNS에도 호스트에 대한 정보가 없으면   
4. 다른 DNS 서버에게 질의할 수 있도록 요청한다. (.com DNS)   
5. Local DNS는 .com을 관리하는 DNS에게 호스트 네임에 대한 질의를 요청하고 요청한 결과가 없으면 다시 질의할 다른 DNS서버의 주소를 알려준다.   
6. Local DNS는 wecode.co.kr을 관리하는 DNS에게 호스트 네임에 대한 질의를 요청하고   
7. 결과가 있을 경우 IP주소에 대한 결과를 반환한다.   
8. Local DNSsms 'www.wecode.co.kr'에 대한 IP주소를 캐싱하고, 클라이언트에게 전달한다.   

위와 같이 Local DNS 서버가 여러 DNS 서버를 차례대로 물어봐서 답을 찾는 과정을 **Recursive Query** 라고 한다.   
서비스 예) Amazon Route 53, Cafe 24 도메인 관리, 가비아 네임 서버 관리

## 전체 통신 과정
![workflow](/media/browser-dns-host.png)
사용자가 브라우저를 열고 www.wecode.co.kr를 입력하면 DNS 서버를 거쳐서 IP주소를 찾는다.
브라우저에서는 IP주소를 가지고 타겟 서버로 호출 해서 최종적으로 HTML CSS JavaScript를 응답받아서 브라우저에 나타낸다.

###배포
앞으로 배포한다 or Deploy한다. 라는 말을 자주 듣게 될 것이다.
배포란, 그동안 개발하던 것을 인터넷에 공개하고 모든 사람들이 볼 수 있게 하는 것을 의미한다.