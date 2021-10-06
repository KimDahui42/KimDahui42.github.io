---
layout: post
title: "[부스트코스] 웹프로그래밍 시작하기"
date: 2021-05-01
excerpt: "덮어뒀던 부스트코스 먼지털고 완주하기"
project: true
tags: [web,boostcourse]
category: [Boostcourse]
comments: true
---
포스트 여러개로 나누어서 하나씩 올리려다가 생각해보니 그렇게까지 할 시간이 없길래 프로젝트로 두고 계속 추가해가기로 결정했다. 날짜를 기록해두면 되겠지..?
<br><br>---주의사항---
<br>정말 들으면서 까먹지 말자며 기록해둔걸 지금 여기까지 들었어요~하고 올려 두서없는 내용이 특징인 파일이라 혹시라도 보게 될 사람이 있다면 조금 걱정이 됨
## 1일차
### 1.Web 개발의 이해-FE/BE
##### 2021.05.01.SAT
<a href="https://kimdahui42/assets/etc/1. Web 개발의 이해.pdf">수강하며 필기한 노트</a>
<br> *21.05.06.파일수정*

## 2~5일차
### 2.HTML-FE/3.CSS-FE
##### 2021.05.02/03/04/05 SUN/MON/TUE/WED
<a href="/assets/etc/2. HTML-FE.pdf">수강하며 필기한 노트</a>
<br> *21.05.06.파일수정*

## 6~13일차
### 4.개발환경 설정-BE~Summary
##### 2021.05.06 THU - 2021.05.13 THU
<a href="/assets/etc/4.개발환경 설정-BE.pdf">수강하며 필기한 노트</a><br>
이래저래...오류도 많고 과제도 있어서 생각보다 오래걸렸다 그래도 첫번째 이론 파트는 끝!!!

***
## 오류파티
### Hello World
##### 2021.05.12 WED

톰캣하나 설치하려는데 너무 수많은 고난과 역경을 거쳤기에 남기는 기록
<br><br>
* Dynamic Web Project가 없음!<br>
>Help-install new Software-Work with: 최근 이클립스 업데이트 버젼-Web,XML,JAVA EE and OSGi Enterprise Development로 들어가서
* Eclipse Java EE Developer Tools
* Eclipse Java Web Developer Tools
* Eclipse Java Web Developer Tools-JavaScript Support
* Eclipse Web Developer Tools
* Eclipse XML Editors and Tools
을 설치했다 안되면 관련항목을 이것저것 더 설치해보면서 찾는 편이 나은듯<br>
<figure>
	<a href="/assets/etc/1_웹프로그래밍기초.PNG"><img src="/assets/etc/1_웹프로그래밍기초.PNG"></a>
</figure>
<br>
* startup.bat파일을 실행하려 했으나 실행이 안됨
>jdk 설치때 JAVA_HOME 과 CLASSPATH를 추가를 하지않아서 추가했음
>>여전히 실행이 안 됨
>>다른 버젼의 톰캣을 설치, 변화 없음
>>zip 파일이 아니라 WIndows Service Installer를 다운 받아 설치하는 것으로 해결<br>
><figure><a href="/assets/etc/error/톰캣.PNG"><img src="/assets/etc/error/톰캣.PNG"></a><figcaption><a href="https://tomcat.apache.org/download-80.cgi" title="톰캣8.5 버젼">톰캣8.5 버젼을 사용했다</a>.</figcaption></figure>

<br>
* 코드를 짜고 실행을 했더니 resource /Server does not exist 로 막혔음
>>이클립스 Servers에서 서버 클릭-overview에서 Sever Options (Publish module contexts to separate XML files를 선택하고 Ports 탭에서 Tomcat admin port는 8005, HTTP/1.1은 8080으로 포트넘버를 수정하는 것으로 해결했다.<br>
><figure class="half"><a href="/assets/etc/error/포츠.PNG"><img src="/assets/etc/error/포츠.PNG"></a><a href="/assets/etc/error/서버옵션.PNG"><img src="/assets/etc/error/서버옵션.PNG"></a></figure>

<br>
* "port 8080 required by tomcat v8.5 server at localhost is already in use. the server may already be running in another process, or a system process may be using the port. to start this server you will need to stop the other process or change the port number(s)."
라면서....포트를 사용중이라 사용할 수 없다고 오류가 떴음 너무 화가났지만 이거 아님 아무것도 할 수 없다는 걸 알고있지...
>cmd에 netstat -a -n -o -p tcp를 입력해 로컬주소 8080이 LISTENING 상태로 사용중인걸 발견하고 해당 PID를 확인했음
taskkill /f /pid 해당 PID를 입력해서 포트를 죽이려고 하니깐 액세스가 거부되어서 종료할 수 없었음 cmd를 관리자 모드로 다시 실행해 절차를 반복했더니 포트를 종료할 수 있었음<br>
><figure><a href="/assets/etc/error/cmd.PNG"><img src="/assets/etc/error/cmd.PNG"></a></figure>

그리고 저는 실행할 수 있게 되었답니다.
<br> *21.05.13.파일수정*

***

## 14~일차
### 프로젝트A:홈페이지
##### 2021.05.06 THU - 2021.05.13 THU
*과제 완료했습니다*
🚙 <a href="">해당 포스트로 이동</a>
