---
layout: post
title: Lodash to JavaScript
subtitle: slice
categories: frontend
tags: [frontend, develop, lodash]
---

이번 포스트에서는 Lodash의 `slice`를 JavaScript로 직접 구현해보며 동작 원리를 이해해보겠습니다.

## slice 함수란?

[slice](https://lodash.com/docs/4.17.15#slice) 함수는 배열의 특정 범위를 잘라서 새로운 배열을 생성하는 기능입니다. 주어진 start 인덱스부터 end 인덱스 이전까지의 요소를 포함하는 새로운 배열을 반환합니다.

### 사용 예제

다음은 Lodash의 `slice` 함수를 사용하는 간단한 예제입니다.

```javascript
const array = [1, 2, 3, 4, 5];

console.log(_.slice(array, 1, 4));
// => [2, 3, 4]
```

### 특징

1. 원본 배열을 변경하지 않습니다.

2. start 인덱스와 end 인덱스를 지정할 수 있습니다.

   - start: 잘라낼 시작 인덱스 (기본값은 0)
   - end: 잘라낼 끝 인덱스 (기본값은 array.length)

3. 음수 인덱스를 사용하면 배열의 끝에서부터 계산합니다.

4. 범위를 벗어난 경우 자동으로 조정됩니다.
   - start가 array.length보다 크면 빈 배열 반환
   - end가 start 보다 작으면 빈 배열 반환

## JavaScript로 구현하기

```javascript
function slice(array, start = 0, end = array.length) {
  if (start < 0) start = array.length + start;
  if (end < 0) end = array.length + end;

  let result = [];

  for (let i = start; i < end; i++) {
    result.push(array[i]);
  }

  return result;
}
```

<img width="830" alt="Image" src="https://github.com/user-attachments/assets/31db591f-5138-40f9-b145-a3e88f5c94fa" />
