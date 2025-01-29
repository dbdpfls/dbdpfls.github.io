---
layout: post
title: Lodash to JavaScript - join
subtitle: join
categories: frontend
tags: [frontend, develop, lodash]
---

이번 포스트에서는 Lodash의 `join`을 JavaScript로 직접 구현해보며 동작 원리를 이해하고자 합니다.

## join 함수란?

[join](https://lodash.com/docs/4.17.15#join) 함수는 배열의 모든 요소를 문자열로 변환하고 지정한 구분자를 사용해 하나의 문자열로 결합하는 기능입니다.

### 사용 예제

다음은 Lodash의 `join` 함수를 사용하는 간단한 예제입니다.

```javascript
const array = ["a", "b", "c"];

console.log(_.join(array, "~"));
// => 'a~b~c'
```

### 특징

1. 배열의 요소를 문자열로 변환하여 하나의 문자열을 반환합니다.

2. 구분자를 지정할 수 있으며 기본값은 쉼표( , )입니다.

3. 배열의 요소가 null이나 undefined인 경우 빈 문자열로 처리됩니다.

4. 빈 배열의 경우 빈 문자열을 반환합니다.

## JavaScript로 구현하기

```javascript
function join(array, separator) {
  let result = "";

  for (const idx in array) {
    result += array[idx];

    if (Number(idx) < array.length - 1) {
      result += separator;
    }
  }

  return result;
}
```

<img width="697" alt="Image" src="https://github.com/user-attachments/assets/30b67aac-4db7-4b50-83e1-f4564a6cc6fb" />
