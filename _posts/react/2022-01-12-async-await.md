---
layout: post
title: "async/await"
date: 2022-01-12
excerpt: "비동기 연관 MDN Web Docs를 읽고 정리한 문서입니다."
tags: [react,web,javascript]
category: [React] 
comments: false
---
# 동기와 비동기
## 비동기

## async 함수
async 함수 선언은 AsyncFunction 객체를 반환하는 하나의 비동기 함수를 정의한다
* 비동기 함수
	* 이벤트 루프를 통해 비동기적으로 작동하는 함수
	* 암시적으로 <a href="#promise">Promise</a>를 사용해 결과를 반환한다
	* 비동기 함수를 사용하는 코드의 구문과 구조는 표준 동기 함수 사용과 유사
* asyn
* async 함수 정의
	* async function 표현식
		* async function 키워드는 표현식 내에서 async 함수를 정의하기 위해 사용된다
		* 문법 : `async function [name]([param1[, parma2[, ...,paramN]]]){statements}` arrow functions를 사용할 수도 있다
	* 






1) <a name="promise">Promise 객체는 비동기 작업이 맞이할 미래의 완료 또는 실패와 그 결과 값을 나타낸다.
	* 프로미스는 프로미스가 생성된 시점에는 알려지지 않았을 수도 있는 값을 위한 대리자
	* 비동기 연산이 종료된 이후에 결과 값과 실패 사유를 처리하기 위한 처리기를 연결할 수 있다
	* 최종 결과를 반환 x, 미래 어떤 시점에 결과를 제공하겠다는 `약속`을 반환한다
	* 다음 중 하나의 상태를 가진다
		* `대기(Pending)` : 초기 상태
		* `이행(Fulfilled)` : 연산이 성공적으로 완료됨
		* `거부(Rejected)` : 연산 실패
	* 프로미스가 대기에서 벗어나 이행 또는 거부된다면 `프로미스가 처리(settled)됐다`고 표현
	* 이행되거나 거부될 때, 프로미스의 then 메서드에 의해 대기열(큐)에 추가된 처리기들이 호출된다
		* 이미 이행했거나 거부된 프로미스에 처리기를 연결해도 호출되어 비동기 연산과 처리기 연결 사이에 경합 조건이 없다
	* Promise.prototype.then() 및 Promise.prototype.catch() 메서드의 반환 값은 새로운 프로미스이므로 서로 연결할 수 있다<br><br>
	<img width=550 height=200 src="/assets/etc/react/promises.png" alt="Promise 설명"><br>
	<a href="https://url.kr/r6yebc">해당 문서로 이동</a>

