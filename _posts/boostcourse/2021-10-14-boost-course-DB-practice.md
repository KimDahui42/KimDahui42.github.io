---
layout: post
title: "[부스트코스] 2. DB 연결 웹앱"
date: 2021-10-14
excerpt: "2장 수강하며 해결했던 사항들"
tags: [web,boostcourse]
category: [Boostcourse]
comments: true
---
# maven 프로젝트 실습

## 실행을 하지만 오류가 나면서 실행이 안 되는 경우

* 이클립스의 버그로, 수정되기 전의 데이터와 수정된 데이터가 섞여서 실행되기 때문

	* 이 경우 웹 어플리케이션을 깔끔히 초기화하고 실행하는 것이 좋을 수 있다

	    1. 기존 tomcat을 종료
	    2. 혹시 바뀌지 않았다면 프로젝트를 선택하고, 우측버튼을 눌러서 Maven 메뉴 아래의 update project를 선택한 후 확인
	    3. Servers view에서 기존 Tomcat Runtime을 삭제
	    4. Project 메뉴의 Clean선택
	    5. 프로젝트 익스플로러에서 Servers 삭제
	    6. Run on Server로 실행(서버 포트 추가-8005)

## JDBCExam1 실행 오류
* mysql 버젼이 달라서 발생
	* mysql 8.0.26 dependency 코드
	```xml
	<dependency>
    	<groupId>mysql</groupId>
    	<artifactId>mysql-connector-java</artifactId>
    	<version>8.0.26</version>
	</dependency>
	```
	* RoleDao.java
	```java
	private static String dburl="jdbc:mysql://localhost:3306/connectdb?serverTimezone=Asia/Seoul&useSSL=false";
	Class.forName("com.mysql.cj.jdbc.Driver");
	
	```
## webAPI 실습
> 500 - java.lang.NoClassDefFoundError: com/fasterxml/jackson/databind/ObjectMapper

* .m2 파일 삭제후 재실행, dependency 최신 버젼으로 코드 수정, project update=> 오류 유지
    * <a href="https://mvnrepository.com/artifact/com.fasterxml.jackson.core"> dependency 코드는 이곳에서 확인했다</a>
    * `databind`, `annotation`, `core`와 dependant하다
 
```
<!-- databind -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.13.0</version>
</dependency>
<!-- core -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-core</artifactId>
    <version>2.13.0</version>
</dependency>
<!-- annotations -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-annotations</artifactId>
    <version>2.13.0</version>
</dependency>

```

* `.classpath` 파일 수정으로 해결

```
<classpathentry kind="con" path="org.eclipse.m2e.MAVEN2_CLASSPATH_CONTAINER">
	<attributes>
		<attribute name="maven.pomderived" value="true"/>
		<attribute name="org.eclipse.jst.component.dependency" value="/WEB-INF/lib"/>
	</attributes>
</classpathentry>

```
