---
layout: post
title: Lodash to JavaScript - reverse
subtitle: reverse
categories: frontend
tags: [frontend, develop, lodash]
---

이번 포스트에서는 Lodash의 `reverse`를 JavaScript로 직접 구현해보며 해당 함수의 동작 원리를 이해해보겠습니다.

## reverse 함수란?

[reverse](https://lodash.com/docs/4.17.15#reverse) 함수는 배열의 요소 순서를 뒤집는 기능입니다.

### 사용 예제

다음은 Lodash의 `reverse` 함수를 사용하는 간단한 예제입니다.

```javascript
const array = [1, 2, 3];

console.log(_.reverse(array));
// => [3, 2, 1]

console.log(array);
// => [3, 2, 1]
```

### 특징

1. 원본 배열을 변경합니다.

2. 배열 요소의 데이터 타입과 무관하게 동작합니다.

## JavaScript로 구현하기

```javascript
function reverse(array) {
  let arrLen = array.length;
  let loopVar = Math.floor(arrLen / 2);

  for (let i = 0; i < loopVar; i++) {
    let tmp = array[i];
    array[i] = array[arrLen - 1 - i];
    array[arrLen - 1 - i] = tmp;
  }

  return array;
}
```

<img width="790" alt="Image" src="https://github.com/user-attachments/assets/7eea6d70-ee94-4b8a-af27-fd73241abb12" />
