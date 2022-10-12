---
layout: post
title: "[자바스크립트] 구조 분해 할당"
date: 2022-10-02
excerpt: "ES6 부터 구조 분해 할당(Destructuring assignment)을 적용할 수 있게 되었다.  Perl, Python 등의 언어에서도 볼 수 있는 개념이다."
tags: [study, javascript, es6+]
category: [Daily Problem]
comments: false
---

# [자바스크립트] 구조 분해 할당

[참고 링크](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

> ES6 부터 구조 분해 할당(Destructuring assignment)을 적용할 수 있게 되었다. Perl, Python 등의 언어에서도 볼 수 있는 개념이다.

## 정의

배열이나 객체의 속성을 해체하여 그 값을 개별 변수에 담을 수 있게하는 자바스크립트의 표현식

```jsx
var [x, y, z] = [0, [{ a: 0, b: 0 }], "item"];
```

## 특징

### 배열 구조 분해

1. 변수의 선언이 한 번에 이루어지지 않아도 구조 분해 할당이 가능하다.

```jsx
var x, y;
[x, y] = [1, 2];
```

1. 변수에 기본 값(`undefiined` 일 때 사용됨)을 할당할 수 있다.

```jsx
var [x = 7, y = 3] = [1];
//x=1, y=3 으로 출력된다.
```

1. 임시 변수 없이 swap이 가능하다.

```jsx
var [x, y] = [1, 2];
[x, y] = [y, x];
//x=2, y=1 로 swap 되었다.
```

1. 필요없는 값을 무시할 수 있다.

```jsx
var [a, , b] = [1, 2, 3];
//a=1,b=3 2가 무시되었다.
```

1. 나머지 구문(rest)을 이용해 분해하고 남은 부분을 하나의 변수에 할당할 수 있다. _나머지 할당은 뒤에 쉼표가 있을 때 구문 오류가 발생하니 주의_

```jsx
var [x, ...b] = [1, 2, 3];
//x=1, b=[2,3];
```

1. 정규 표현식의 `[exec()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/RegExp/exec)`메서드과 조합할 경우 필요하지 않은 경우 정규식과 일치하는 전체 부분은 무시하고 필요한 부분만 빼올 수 있다.
    1. `[exec()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/RegExp/exec)` : 문자열에서 정규식과 일치하는 부분 전체를 배열의 맨 앞에, 그리고 그 뒤에 정규식에서 괄호로 묶인 각 그룹과 일치하는 부분을 포함하는 배열을 반환
    2. `exec()` 메서드 실행 반환 배열을 구조 분해 할당 값으로 활용하는 것

### 객체 구조 분해

1. 기본 할당 방법 : 연산자 우측엔 분해하려는 객체를, 좌측엔 객체 프로퍼티의 '패턴’을 넣어 할당한다.

```jsx
var o = { p: 42, q: true };
var { p, q } = o;
// 객체 프로퍼티 패턴 = 객체
console.log(p); // 42
console.log(q); // true
```

1. 객체 프로퍼티를 다른 이름의 변수에 저장할 수 있다.

```jsx
var o = { p: 42, q: true };
var { p: foo, q: bar } = o;

console.log(foo); // 42
console.log(bar); // true
```

1. 배열 구조 분해와 같이 기본값을 설정할 수 있다. 이를 2번 특징과 조합하면 새로운 변수명 할당과 기본값 할당을 한번에 할 수 있게된다.

```jsx
var a, b;

({ a, b } = { a: 1, b: 2 });
```

-   할당 문을 둘러싼 `( .. )`는 선언 없이 객체 리터럴 비구조화 할당을 사용할 때 필요한 구문이다.
-   좌측의 `{a, b}`는 객체 리터럴*(데이터,값)*이 아닌 중괄호 블록으로 간주되기 때문에 `{a, b} = {a:1, b:2}`는 유효하지 않다.
-   `({a, b} = {a:1, b:2})`는 `var {a, b} = {a:1, b:2}`와 같아 유효하다.
