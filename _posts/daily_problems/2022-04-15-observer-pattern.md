---
layout:post
title:"observer pattern"
date: 2022-04-15
excerpt: "바닐라 자바스크립트로 옵저버 패턴 흉내내기"
tags: [web,study,javascript]
category: [Daily Problem]
comments: false
---
## 옵저버 패턴
옵저버 패턴은 객체의 상태 변화를 관찰하는 관찰자들, 즉 옵저버들의 목록을 객체에 등록하여 상태 변화가 있을 때마다 메서드 등을 통해 객체가 직접 목록의 각 옵저버에게 통지하도로고 하는 디자인 패턴이다. 주로 분산 이벤트 핸들링 시스템을 구현하는데 사용된다. 