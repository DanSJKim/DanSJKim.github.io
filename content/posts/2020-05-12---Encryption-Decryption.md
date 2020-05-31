---
date: '2020-05128T15:09:32.169Z'
template: 'post'
draft: false
slug: 'encryption-decryption'
category: 'database'
tags:
  - '암호화'
  - 'AES'
description: '데이터 암호화, 복호화'
socialImage: 'https://i.kym-cdn.com/entries/icons/facebook/000/019/513/til.jpg'
---

# 암호란?

## Cryptography

평문을 암호문으로 변환하거나 암호문을 평문으로 변환하는 기술

**Plaintext**

해독 가능한 형태의 텍스트

ex) ("123456")

**Ciphertext**

해독 불가능한 형태의 텍스트

ex("aBD#fefa$fae!")



## Encryption&Decryption

**Encryption**

평문을 암호문으로 변환하는 과정

**Decryption**

암호문을 평문으로 변환하는 과정



## 암호화의 종류

**단방향**

암호화 후 복호화할 수 없다.

**양방향**

암호화와 복호화 모두 가능하다.

ex) 사용자 주소, 이메일, 번호 또는 전자서명 같이 재사용성이 있는 정보는 암호화 복호화 모두 이루어져야 한다.



## 암호 알고리즘

### 단방향

PBKDF2

bcrypt

scrypt

### 양방향 

- **대칭형(비밀키 암호) 알고리즘**

  **AES**

  고급 암호화 표준(Advanced Encryption Standard)이라고 불리는 AES 암호 알고리즘은 DES를 대체한 암호 알고리즘이며 암호화와 복호화 과정에서 동일한 키를 사용하는 **대칭 키 알고리즘**이다.

  현재 가장 보편적으로 쓰이는 암호화 방식이며 현 미국 표준 방식인 AES.128~256비트 키를 적용할 수 있어 보안성이 뛰어나며 공개된 알고리즘이라 누구나 사용할 수 있디.

  Rijmen과 Daemen이 개발한 Rijndael 알고리즘이 AES 공모에서 선정되면서 AES로 암호화 표준이 되었다.

  **AES는 SPN(Substituion-Permutation Network)라는 구조를 사용**한다. 

  SPN구조는 암호화와 복호화 과정이 서로 다르다.

  - **Feistel 구조와 SPN 구조의 차이**

    

    **Feistel 구조**는 암복호화 과정에서 **역함수가 필요 없다는 장점**이 있지만 구현시 스왑(Swap)단계 때문에 **연산량이 많이 소요 되며 암호에 사용되는 라운드 함수를 안전하게 설계해야 한다는 단점**이 있다. 

    

    대표적인 암호로는 **DES**가 있으며 Single DES는 **안전성 문제**로 현재 사용하고 있지 않다.

    

    **SPN 구조**는 암복호화 과정에서 **역함수가 필요하도록 설계되어야 한다는 단점**이 있지만 중간에 비트의 이동없이 한번에 암복호화가 가능하기 때문에 **페스탈 구조에 비해 효율적으로 설**계할 수 있다. 대표적인 암호로는 **AES**가 있으며 AES는 현재 널리 상용되고 있다.

    출처: https://www.crocus.co.kr/1230 [Crocus]

- **비대칭형(공개키 암호) 알고리즘**

  **RSA**

  공개키 암호시스템의 하나로 암호화뿐만 아니라 전자서명이 가능한 최초의 알고리즘

