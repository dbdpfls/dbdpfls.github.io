---
layout: post
title: 솔리디티 기본 문법 1
subtitle:
categories: blockchain
banner:
  image: https://github.com/dbdpfls/dbdpfls.github.io/assets/103565462/11da367f-8986-46d8-ae02-5865aa9f2742
tags: [blockchain]
---

## 솔리디티(Solidity)

솔리디티는 이더리움 플랫폼의 스마트 컨트랙트를 구현하기 위한 객체 지향 프로그래밍 언어입니다.

### 기본 구조

```java
// 1. SPDX 라이센스 명시
// SPDX-License-Identifier: GPL-3.0

// 2. 버전 표기, 0.7.0 이상 0.9.0 미만 버전 사용
pragma solidity >=0.7.0 <0.9.0;

// 3. 컨트랙트 작성
contract Storage {
	// 코드 작성
}
```

**1. SPDX 라이센스 명시**
스마트 컨트랙트의 신뢰를 높이고, 저작권 문제를 해결하기 위해 솔리디티 코드 최상단에 SPDX 라이센스를 주석으로 표기합니다.

**2. pragma**
모든 소스코드 최상단에 솔리디티 특정 컴파일러의 버전을 명시합니다. 이는 솔리디티 컴파일러와의 호환성을 지칭하는 것으로 새로운 컴파일러 버전이 나와도 기존 코드가 깨지지 않도록 해줍니다.

**3. 컨트랙트 작성**
컨트랙트 코드를 작성합니다.

### 변수 종류

솔리디티에는 3가지 변수 타입이 있습니다.

**1. 상태변수**
함수 밖에 선언되며 블록체인에 저장되는 변수입니다.

```java
// SPDX-License-Identifier: GPL-3.0

pragma solidity >=0.7.0 <0.9.0;

contract Storage {
    uint stateNum = 123;
}
```

#### 상태변수의 가시성(Visibility)

- internal(default)
  - 상태 변수에 기본적으로 사용합니다.
  - 컨트랙트 내부 및 상속 컨트랙트에서 접근 가능합니다.
- public
  - 컴파일러가 자동으로 getter 함수를 생성해줍니다.
  - 컨트랙트 내부 및 외부에서 접근 가능합니다.
- private
  - 컨트랙트 내부에서만 접근이 가능합니다.
- constant
  - 상태변수를 상수로 선언할 수 있습니다.
- immutable
  - 생성자(constructor) 안에 선언될 수 있지만 값을 수정할 수 없습니다.

**2. 전역변수**
블록체인 관련 정보를 얻을 수 있는 특수 변수입니다.

```java
function store(uint num) public {
	uint globalNum = block.timestamp;
}
```

- block : 블록 정보를 가지고 있습니다.
- msg : 컨트랙트를 시작한 트랜잭션 콜이나 메시지 콜에 대한 정보를 가지고 있습니다.
- tx : 트랜잭션 정보를 가지고 있습니다.
- this : 현재 컨트랙트를 참조합니다.

| 전역변수                          | 데이터 형식   | 설명                               |
| --------------------------------- | ------------- | ---------------------------------- |
| block.blockhash(uint blockNumber) | bytes32       | 블록 해시값                        |
| block.basefee                     | uint          | 블록의 기본 수수료                 |
| block.coinbase                    | address       | 블록 채굴자 주소                   |
| block.chainid                     | block.chainid | 해당 블록체인 ID                   |
| block.difficulty                  | uint          | 해당 블록 난이도                   |
| block.gaslimit                    | uint          | 해당 블록 가스 한도                |
| block.number                      | uint          | 해당 블록 번호                     |
| block.timestamp                   | uint          | 해당 블록 타임스탬프               |
| msg.data                          | bytes         | 전체 calldata                      |
| msg.sender                        | address       | 송금자 주소                        |
| msg.sig                           | bytes4        | calldata의 첫4바이트(=함수 식별자) |
| msg.value                         | uint          | 송금액                             |
| tx.gasprice                       | uint          | 트랜잭션 가스비                    |
| tx.origin                         | address       | 트랜잭션 발신자                    |
| now                               | uint          | block.timestamp 의 별칭            |
| gasleft()                         | uint256       | 남은 가스 양                       |

**3. 지역변수**
함수 안에 선언되며 블록체인에 저장되지 않는 변수입니다.

```java
function retrieve(uint num) public {
	uint localNum = 456;
}
```
