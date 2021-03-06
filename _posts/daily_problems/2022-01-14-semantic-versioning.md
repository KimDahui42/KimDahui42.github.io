---
layout: post
title: "버전을 기록하는 방식"
date: 2022-01-14
excerpt: "버젼을 업데이트하며 버전 번호를 매기는 방법 Semantic versioning"
tags: [study,semantic_versioning]
category: [Daily Problem]
comments: false
---
## 등장 배경
의존성이 높은 시스템에서 의존성을 적절하게 관리하며 버전을 업데이트 할 수 있게 하기 위해 버전 번호를 매기는 규칙과 요구사항이 제안되었다. 필수적으로 따를 필요는 없으나 의도를 표현하는데 효과적이다.

## 형식
**<center>Major.Minor.Patch</center>**
1. semantic versioning을 사용하는 소프트웨어는 반드시 공개 API를 선언한다. API는 코드 자체로 선언하거나 문서로 정확하고 이해하기 쉽게 명시되어야한다.
2. 반드시 `Major.Minor.Patch`형태로 하고 각 숫자는 자연수여야 한다.
3. 각각은 반드시 증가하는 수여야한다.

### Major
* 0 (0.x.x)은 초기 개발을 위해서 쓴다. 
* 아무 때나 수정이 가능하며 이 공개 api는 안정판으로 보지 않는게 좋다.
* 1.0.0 버전은 공개 api를 정의한다. 이후의 버전 번호는 이때 배포한 공개 api 변경 내용에 따라 올라간다.
* 공개 api에 기존과 호환되지 않는 변화가 있을 때는 반드시 major 버전을 올린다. 
	* minor버전이나 patch버전 급 변화를 포함할 수 있다.
	* major 버전을 올릴 때는 반드시 minor, patch 버전을 0으로 초기화한다.
	
### Minor
* 반드시 그 전 버전 api와 호환되는 버그 수정의 경우에만 올린다.
	* 버그 수정은 잘못된 내부 기능을 고치는 것이라 정의한다.
* 공개 api에 기존과 호환되는 새로운 기능을 추가할 때는 반드시 부버전을 올린다.
* 공개 api의 일부를 앞으로 제거할 것으로 표시한 경우에도 반드시 올리도록한다. 
* 내부 비공개 코드에 새로운 기능이 대폭 추가되거나 개선사항이 있을 때도 올릴 수 있다. 
* 부버전을 올릴 때 patch 버전을 올릴 때만큼의 변화를 포함할 수도 있다.
* 부버전이 올라가면 수버전은 반드시 0에서 다시 시작한다.

### Patch
* patch 버전 바로 뒤에 붙임표(-)를 붙이고 마침표(.)로 구분되니 식별자를 더해서 pre-release버전을 표기할 수 있다. 
	* 식별자는 반드시 ASCII 문자, 숫자, 붙임표로만 구성한다[0-9A-Za-z-].
	* 식별자는 반드시 한 글자 이상으로 한다.
	* 숫자 식별자의 경우 절대 앞에 0을 붙인 숫자로 표기하지 않는다.
		* 정식배포 전 버전은 관련 보통 버전보다 우선순위가 낮다
		* 정식배포 전 번전은 아직 불안정하며 연관된 일반 버전에 대해 호환성 요구사항이 충족되지 않을 수도 있다.
	* 빌드 메타데이터는 patch 버전이나 정식배포 전 식별자 뒤에 더하기(+) 기호를 붙인 뒤에 마침표로 구분된 식별자를 덧붙여 포현할 수 있다.
		* 식별자는 반드시 한 글자 이상으로 한다.
		* 빌드 메타데이터는 버전 간의 우선순위를 판단하고자 할 때 반드시 무시해야한다.
* 버그 수정 있을 때 수를 올린다.
* 하위 버전과 호환된다.
<br>
<a href="https://semver.org/lang/ko/">참고문서</a>
