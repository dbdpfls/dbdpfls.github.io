---
layout: post
title: Lodash to JavaScript - indexOf
subtitle: indexOf
categories: frontend
tags: [frontend, develop, lodash]
---

이번 포스트에서는 Lodash의 `indexOf`를 JavaScript로 직접 구현해보며 해당 함수의 동작 원리를 이해해보겠습니다.

## indexOf 함수란?

[indexOf](https://lodash.com/docs/4.17.15#indexOf) 함수는 배열에서 특정 값이 처음 등장하는 인덱스를 반환하는 기능입니다.
값이 존재하지 않으면 `-1`을 반환합니다.
fromIndex를 음수로 입력하면 배열의 끝에서부터 검색을 시작합니다.

### 사용 예제

다음은 Lodash의 `indexOf` 함수를 사용하는 간단한 예제입니다.

```javascript
const array = [1, 2, 1, 2];

console.log(_.indexOf(array, 2));
// => 1

// Search from the `fromIndex`.
console.log(_.indexOf(array, 2, 2));
// => 3
```

### 특징

1. 배열에서 특정 값이 처음 나타나는 인덱스를 반환합니다.

2. 값이 존재하지 않으면 -1을 반환합니다.

3. 검색을 시작할 인덱스를 지정할 수 있고, 기본값은 0입니다.

4. 검색을 시작할 인덱스를 음수로 지정하면 배열의 끝에서부터 계산하여 검색합니다.

5. 완전 일치(`===`)를 기준으로 값을 찾습니다.

## JavaScript로 구현하기

```javascript
function indexOf(array, value, fromIndex = 0) {
  if (fromIndex < 0) {
    fromIndex = Math.max(array.length + fromIndex, 0);
  }
  for (let i = fromIndex; i < array.length; i++) {
    if (array[i] === value) return i;
  }

  return -1;
}
```

<img width="651" alt="Image" src="https://github.com/user-attachments/assets/04860a06-d101-4950-bdeb-70c8311714de" />
