---
layout: post
title: 솔리디티 기본 문법 2
subtitle:
categories: blockchain
banner:
  image: https://github.com/dbdpfls/dbdpfls.github.io/assets/103565462/b1732173-b6c4-4a90-96c3-f9ebbe38f12b
tags: [blockchain]
---

## 솔리디티(Solidity)

솔리디티는 이더리움 플랫폼의 스마트 컨트랙트를 구현하기 위한 객체 지향 프로그래밍 언어입니다.

### 자료형

솔리디티 자료형은 Value Types과 Reference Types이 있습니다.

- **값 타입(Value Types)**

1. 불(bool)
   true 또는 false

2. 정수(int, uint)
   int : 부호가 있는 정수
   uint : 부호가 없는 정수

3. 실수
   fixed : 부호가 있는 실수
   ufixed : 부호가 없는 실수

4. 주소(address)
   address : 송금 불가능 주소값 (0.8버전부터)
   address payable : 송금 가능한 주소값

5. 바이트 배열(고정 크기)
   bytes1 ~ bytes32까지 고정된 크기의 배열 선언

6. 열거형(enum)
   특정값들로 집합을 정하고, 집합에 있는 데이터만을 값으로 가짐

- **참조 타입(Reference Types)**

1. 배열(array) : 저장하고자 하는 데이터 형식에 '[ ]'를 붙여 선언

- 정적배열 : 배열의 크기를 지정하여 선언
- 동적배열 : 배열의 크기를 지정하지 않고 선언

2. 구조체(struct) : 사용자 정의 자료형

3. 매핑(mapping) : 키-값 구조로 데이터 저장 시 사용하는 자료형
