---
layout: post
title: "[자바스크립트] Map, Object와 비교"
date: 2022-10-01
excerpt: "키 기반 컬렉션 : 입력된 키값을 기준으로 정렬되는 데이터의 집합(자료구조)"
tags: [study, javascript, es6+]
category: [Daily Problem]
comments: false
---

# 키 기반 컬렉션

-   입력된 키값을 기준으로 정렬되는 데이터의 집합(자료구조)
    -   컬렉션 : 객체의 모음, 그

## Object와 Map 비교

-   Object의 키는 String이나 Symbol, Map은 모든 값을 키로 가질 수 있다.
-   Object는 prototype을 가지고 있어 기본 키가 존재할 수 있는데, 이로 인해 새로 만든 키와 충돌을 야기할 수 있다.
-   Obejct는 크기를 수동으로 추적(O(n)의 시간복잡도)하지만 Map은 크기를 쉽게 얻을 수 있다(O(1)의 시간복잡도| .size 속성)
-   Map은 삽입된 순서대로 정렬된다.
-   Object는 not iterable하지만 Map은 iterable하다.
-   Map은 key-value 쌍 추가/제거에 더 좋은 성능을 보인다.

## Map 객체

-   ES6 새로 소개된 데이터 구조
-   간단한 키와 값을 서로 연결(매핑)시켜 저장
-   저장된 순서대로 각 요소들을 반복적으로 접근할 수 있도록 한다.
-   unique한 key를 가진다.

### 생성자

```jsx
var m = new Map();
var m2 = new Map(iterable);
```

위의 형식으로 새로운 Map 객체를 생성한다.

### 메서드

-   set(key,value) : 맵에 데이터를 저장한다. `map[key]=value`형식으로 저장한다면 console.log로 출력했을 때 문제없이 출력되기 때문에 정상적으로 저장된 것처럼 보이나 Map의 다른 동작들을 사용할 수 없는 상태로 남아있어 set 메서드를 이용해 데이터를 저장해야한다.
-   has(key) : 맵에 주어진 key가 존재하는 지 확인하고 boolean 값을 반환한다.
-   keys() : 각 요소의 key 로 구성된 iterable 객체를 반환한다.
-   values() : 각 요소의 value로 구성된 iterable 객체를 반환한다.
-   clear() : 모든 요소 삭제
-   delete(key) : key에 해당하는 요소 삭제하고 결과 boolean 반환

### 속성

-   size : Map 객체의 key/value 쌍의 개수를 반환한다.

### forEach 접근

```jsx
myMap.forEach((value, key) => {
    console.log(`${key} = ${value}`);
});
```
