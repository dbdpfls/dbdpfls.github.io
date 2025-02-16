---
layout: post
title: Lodash to JavaScript - remove
subtitle: remove
categories: frontend
tags: [frontend, develop, lodash]
---

이번 포스트에서는 Lodash의 `remove`를 JavaScript로 직접 구현해보며 해당 함수의 동작 원리를 이해해보겠습니다.

## remove 함수란?

[remove](https://lodash.com/docs/4.17.15#remove) 함수는 주어진 predicate를 만족하는 요소들을 배열에서 제거하고 제거된 요소들의 배열을 반환합니다.

### 사용 예제

다음은 Lodash의 `remove` 함수를 사용하는 간단한 예제입니다.

```javascript
const array = [1, 2, 3, 4];
var evens = _.remove(array, function (n) {
  return n % 2 == 0;
});

console.log(array);
// => [1, 3]

console.log(evens);
// => [2, 4]
```

### 특징

1. 원본 배열을 직접 변경합니다.

2. 제거된 요소들을 새로운 배열에 담아서 반환합니다.

3. Lodash는 내부적으로 splice()를 사용하여 요소를 제거하므로 성능에 영향을 줄 수 있습니다.

## JavaScript로 구현하기

```javascript
function remove(array, predicate) {
  let removeArr = [];
  let writeIndex = 0;

  for (let i = 0; i < array.length; i++) {
    if (predicate(array[i])) {
      removeArr.push(array[i]);
    } else {
      array[writeIndex] = array[i];
      writeIndex++;
    }
  }

  array.length = writeIndex;

  return removeArr;
}
```

<img width="773" alt="Image" src="https://github.com/user-attachments/assets/27781ef3-8260-48d4-9258-6e2b2c54432e" />
