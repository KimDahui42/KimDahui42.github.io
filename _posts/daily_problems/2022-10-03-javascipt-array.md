---
layout: post
title: "[자바스크립트] 배열에 값을 추가, 수정, 삭제하는 방법"
date: 2022-10-03
excerpt: "push(`item`) : 배열의 맨 뒤에 요소를 추가한다, unshift(`item`) : 배열의 맨 앞에 요소를 추가하고 배열의 전체 개수를 반환한다."
tags: [study, javascript, es6+]
category: [Daily Problem]
comments: false
---

# [자바스크립트] 배열에 값을 추가, 수정, 삭제하는 방법

### 배열에 추가 경우

-   push(`item`) : 배열의 맨 뒤에 요소를 추가한다.
-   unshift(`item`) : 배열의 맨 앞에 요소를 추가하고 배열의 전체 개수를 반환한다.

### 배열을 수정하는 경우

-   fill(`value[, start[, end]]`) : 메서드는 배열의 시작 인덱스부터 끝 인덱스의 이전까지 정적인 값 하나로 채운다.

```jsx
[1, 2, 3].fill(4); // [4, 4, 4]
[1, 2, 3].fill(4, 1); // [1, 4, 4]
[1, 2, 3].fill(4, 1, 2); // [1, 4, 3]
[1, 2, 3].fill(4, 1, 1); // [1, 2, 3]
[1, 2, 3].fill(4, 3, 3); // [1, 2, 3]
[1, 2, 3].fill(4, -3, -2); // [4, 2, 3]
[1, 2, 3].fill(4, NaN, NaN); // [1, 2, 3]
[1, 2, 3].fill(4, 3, 5); // [1, 2, 3]
Array(3).fill(4); // [4, 4, 4]
[].fill.call({ length: 3 }, 4); // {0: 4, 1: 4, 2: 4, length: 3}

// Objects by reference.
var arr = Array(3).fill({}); // [{}, {}, {}]
arr[0].hi = "hi"; // [{ hi: "hi" }, { hi: "hi" }, { hi: "hi" }]
```

-   slice(**`start 시작인덱스 , deleteCount 지울 요소 개수, itemN 추가할 요소 나열`**) : **배열 객체의 지정 데이터를 삭제**하고, 그 구간에 **새 데이터를 삽입할 수 있다.**
-   splice(`start[, deleteCount[, item1[, item2[, ...]]]]`) : 배열의 기존 요소를 추가, 변경, 삭제해 원본 배열을 변경한다.

```jsx
var myFish = ["angel", "clown", "trumpet", "sturgeon"];
var removed = myFish.splice(0, 2, "parrot", "anemone", "blue");

// myFish is ["parrot", "anemone", "blue", "trumpet", "sturgeon"]
// removed is ["angel", "clown"]
```

-   **concat(`array1, array2`) : 2개의 배열을 하나로 결합한**다.
-   [filter(`callback(element[, index[, array]])[, thisArg]`)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) : **특정 조건에 만족**하는 값을 filter하여 생성한 **배열을 반환한다.**

```jsx
function isBigEnough(value) {
    return value >= 10;
}

var filtered = [12, 5, 8, 130, 44].filter(isBigEnough);
// filtered 는 [12, 130, 44]
```

-   **callback :** 각 요소를 시험할 함수, true를 반환하면 요소를 유지하고, false를 반환하면 버린다.
-   callback 함수의 매개변수
    -   **element :** 처리할 현재 요소.
    -   **index _Optional_ :** 처리할 현재 요소의 인덱스.
    -   **array _Optional_ :** filter를 호출한 배열.
    -   **thisArg _Optional_ :** callback을 실행할 때 this로 사용하는 값.

### 배열의 요소를 삭제하는 경우

-   shift() : 배열의 맨 앞에 위치한 요소를 삭제하고 해당 요소를 반환한다.
-   pop() : 배열의 맨 마지막에 위치한 요소를 삭제하고 해당 요소를 반환한다.
