---
layout: post
title: Lodash to JavaScript - includes
subtitle: includes
categories: frontend
tags: [frontend, develop, lodash]
---

이번 포스트에서는 Lodash의 `includes`를 JavaScript로 직접 구현해보며 해당 함수의 동작 원리를 이해해보겠습니다.

## includes 함수란?

[includes](https://lodash.com/docs/4.17.15#includes) 함수는 컬렉션(배열, 객체, 문자열)에서 특정 값이 존재하는지 확인하는 기능입니다.

### 사용 예제

다음은 Lodash의 `includes` 함수를 사용하는 간단한 예제입니다.

```javascript
const array = [1, 2, 3];

console.log(_.includes(array, 1));
// => true

console.log(_.includes(array, 1, 2));
// => false
```

### 특징

1. 배열, 객체, 문자열에서 특정 값이 존재하는지 확인할 수 있습니다.

2. fromIndex를 설정하면 해당 인덱스부터 탐색을 시작합니다.

3. 문자열의 경우 부분 탐색이 가능합니다.

## JavaScript로 구현하기

```javascript
function includes(collection, value, fromIndex = 0) {
  if (fromIndex < 0) {
    fromIndex = Math.max(0, collection.length + fromIndex);
  }

  if (typeof collection === "string") {
    for (let i = fromIndex; i < collection.length; i++) {
      if (collection.slice(i, i + value.length) === value) return true;
    }
  } else {
    collection = Object.values(collection);
    for (let i = fromIndex; i < collection.length; i++) {
      if (collection[i] === value) return true;
    }
  }

  return false;
}
```

<img width="681" alt="Image" src="https://github.com/user-attachments/assets/71b92506-6245-429b-b0da-27f1800505ef" />
