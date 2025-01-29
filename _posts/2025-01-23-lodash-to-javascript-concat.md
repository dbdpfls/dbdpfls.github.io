---
layout: post
title: Lodash to JavaScript
subtitle: concat
categories: frontend
tags: [frontend, develop, lodash]
---

## Lodash 소개

[Lodash](https://lodash.com/)는 JavaScript 개발자들이 자주 사용하는 유틸리티 라이브러리입니다. 데이터 처리와 배열, 객체, 함수 등의 조작을 간단하게 만들어주는 다양한 기능을 제공합니다. 이번 포스트에서는 Lodash의 `concat` 기능을 JavaScript로 직접 구현해보며, 내부 동작 원리를 이해하고자 합니다.

## concat 함수란?

[concat](https://lodash.com/docs/4.17.15#concat) 함수는 배열을 결합하는 데 사용됩니다. 배열 또는 값을 결합하여 새로운 배열을 반환하며, **원본 배열을 변경하지 않는다**는 특징이 있습니다. 이러한 점에서 immutability를 유지하며 데이터를 다뤄야 할 때 유용합니다.

### 사용 예제

다음은 Lodash의 `concat` 함수를 사용하는 간단한 예제입니다.

```javascript
var array = [1];
var other = _.concat(array, 2, [3], [[4]]);

console.log(other);
// => [1, 2, 3, [4]]

console.log(array);
// => [1]
```

### 특징

1. 원본 배열은 변경되지 않습니다.

2. 전달된 값이 배열일 경우, 해당 배열의 요소를 새 배열에 추가합니다.

3. 전달된 값이 배열이 아니라면 단순히 새 배열에 추가합니다.

## JavaScript로 구현하기

```javascript
function concat(...array) {
  let result = [];

  for (const item of array) {
    if (item == null) continue; // null이나 undefined는 무시
    if (Array.isArray(item)) {
      result.push(...item); // 배열일 경우 요소를 펼쳐서 추가
    } else {
      result.push(item); // 단일 값일 경우 그대로 추가
    }
  }

  return result;
}
```

<img width="854" alt="Image" src="https://github.com/user-attachments/assets/b346550a-fff7-43a2-a21e-64ce56ce903a" />
