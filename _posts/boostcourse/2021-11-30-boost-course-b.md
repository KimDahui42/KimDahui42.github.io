---
layout: post
title: "[부스트코스] 웹프로그래밍 시작하기"
date: 2021-11-30
excerpt: "두번째 프로젝트 제출"
tags: [web,boostcourse]
category: [Boostcourse]
project: true
comments: true
---
## 투두리스트
> '할 일 관리'는 해야 할 일, 하고 있는 일, 한 일을 웹 브라우저에서 확인할 수 있는 웹 애플리케이션

### 기술요구사항
#### 웹프론트엔드 기술요구사항
* 총 2개의 화면
	* 할 일 목록 화면 (리스트)
	* 할 일 등록 화면 (쓰기)
* CSS는 기획서와 동일한 수준으로 만들어야한다
	* HTML 엘리먼트간의 배치와 간격은 일정하고 반듯해야함
	* 글자의 크기는 일정한 수준을 유지
	* CSS는 외부 라이브러리(부트스트랩)을 사용하지 않는다.
* jQuery를 사용하지 않고, querySelector, addEventListener, innerHTML을 사용해서 DOM, EVENT 처리
* Ajax는 XMLHTTPRequest를 사용
 
#### 웹백엔드 기술요구사항
* 프로젝트는 maven프로젝트로 생성
* 제공된 테이블 생성 SQL을 이용해서 테이블을 생성
* TodoDto 클래스와 TodoDao클래스를 주어진 스펙에 맞게 작성
* 메인화면을 보여주기 위한 MainServlet과 main.jsp를 작성
* MainServlet은 TodoDao를 이용해 결과를 조회해서 main.jsp 에 전달
* 새로운todo등록 버튼을 클릭하면 해당 요청을 서블릿이 받아서 jsp로 포워딩하여 할 일 등록 화면 출력
* 할일등록폼에서 값을 입력하고 제출 버튼을 누르면 post 방식으로 요청
* 해당 요청을 서블릿이 받아서 처리하게하고, 요청에 대한 모든 일이 끝나면 메인화면으로 리다이렉트
* 메인화면에서 todo 상태변경 버튼(->)을 클릭하여 요청을 보낼 때, Todo 의 Id와 상태값을 전달하여 다음 상태로 (현재 상태가 Todo라면 doing으로 doing 이라면 done) 상태를 나타내는 컬럼값을 변경하고 응답결과로 "success"를 보냄

### 실행화면
<table>
	<tr>
		<td>메인화면</td><td>상태변경</td><td>할일등록</td>
	</tr>
	<tr>
		<td>
			<img href="/assets/etc/boostcourePhoto/mainPage.JPG" alt="메인화면"/>
		</td>
		<td>
			<img href="/assets/etc/boostcourePhoto/stateChange.JPG" alt="상태변경"/>
		</td>
		<td>
			<img href="/assets/etc/boostcourePhoto/whatTodo.JPG" alt="할일등록"/>
		</td>
	</tr>
</table>

### 피드백
<details>
	<summary>
		1. input type="text" id="title" name="actionTodo" placeholder="swift 공부하기(24자까지)" maxlength='24' required
	</summary>
	<p>
		<br> 만약 악용을 목적으로하는 사용자가 개발자 도구를 열고 maxlength를 지워버린다면 어떻게 대응하실 건가요?<br>
		maxlength가 지워진다면 24자 이상 입력이 가능할 것이고, 이것은 기획서에 위배됨과 동시에 DB 도메인 무결성에도 영향을 미칠 것입니다. 막을 수 있는 방법에 대해 생각해 보시면 도움 되실 것같습니다.
	</p>
</details>
<details>
	<summary>
		2. button onclick="location.href='./MainServlet'" class="goBackButton"
	</summary>
	<p>
		<br> history.go(-1)나 history.back()을 사용해도 브라우저 자체적인 '뒤로가기'를 구현할 수 있죠.하지만 이 경우 UX의 관점에서 생각해 봐야 할 것 입니다. Todo 등록창에서 뒤로가기 버튼을 눌렀을 때 사용자가 기대하는 화면은 메인 화면일거에요.
		<br>하지만, 브라우저에 URL을 직접 입력해 Todo등록창에 접속한 사용자는 뒤로가기 버튼을 눌렀을 때 메인 화면이 아니라 URL을 입력하기 전 화면이 나오겠죠.<br>
		즉, '뒤로가기' 라는 사용자의 기대에 어긋나는 경우가 발생할 수 있는 것 입니다.즉, 학습자님의 구현처럼 location.href나 a태그 등을 이용해 target을 명확히 명시하는 것이 더 효용성있는 구현이라고 생각합니다.<br>
		별 생각 없이 작성할 수도 있지만 이렇게 UX의 관점에서 한 번 더 생각해보시면 개발에 있어서 인사이트를 넓힐 수 있을 것 같습니다.
	</p>
</details>
<details>
	<summary>
		3. button type
	</summary>
	<p>
		<br> button타입에 type="button"을 명시하지 않으면  기본적으로 submit처럼 동작하게 됩니다.이는 side effect를 일으킬 수 있습니다. 
        <br>즉, submit용이 아니라면 type을 명확히 명시 바랍니다.
	</p>
</details>
<details>
	<summary>
		4. DB 접속 정보 
	</summary>
	<p>
		<br> 해당 DB 접속 정보는 변경되지 않아야하는 정보입니다.<br>
		접근제한자가 private이기 떄문에 해당 클래스 외에선 변경되진 않지만 해당 클래스 내에선 변경이 가능합니다.<br>
		따라서, final 키워드를 붙여 선언 및 할당과 동시에 변경되지 않도록 하는 것이 좋습니다.<br>	
		이러한 것을 상수라고하며 상수같은 경우 상수명을 대문자 스네이크 표기법으로 작성합니다.
	</p>
</details>
<details>
	<summary>
		4. Class.forName
	</summary>
	<p>
		<br> Class.forName은 구체적인 클래스의 타입을 알지 못해도 클래스의 변수 및 메소드 등에 접근하게 해주는 API이며
		런타임 단계에서 해당 클래스를 동적 로딩합니다. 또한, 한번만 로딩을 하면 계속 사용가능하기 때문에 매 메소드마다 로딩할 필요가 없습니다. 따라서, 아래 코드 처럼 객체를 생성할 때 static 영역에 호출하면 TodoDao 객체를 매번 생성해도 한번만 로딩을 하게됩니다.
	</p>
</details>
<details>
	<summary>
		5. serialVersionUID
	</summary>
	<p>
		<br> serialVersionUID는 객체 직렬화, 역직렬화 과정에서 송수신측에서 서로 맞는지 확인하는 값이며 선언하지 않는다면 내부에서 자동으로 값이 추가되기 때문에 굳이 선언을 해주지 않으셔도 되지만 직접 관리하는 것이 좋긴합니다. 
		<br>자세한 내용은 아래 링크 참고부탁드립니다.<br> https://madplay.github.io/post/java-serialization-advanced <br>또한, 위에서 말씀드린 것처럼 직렬화, 역직렬화 과정에서 해당 객체가 맞는지 확인하는 값이므로 유일한 것이 좋습니다.각 IDE에서 serialVersionUID 유니크한 값으로 생성해주는 기능이 있으니 아래 링크를 참고하시여 유니크한 값으로 생성해보시기 바랍니다.<br>이클립스 : https://javafactory.tistory.com/1388 <br>인텔리J : https://blog.naver.com/PostView.nhn?blogId=jieuni4u&logNo=222041348411&categoryNo=0&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=postView
	</p>
</details>
<details>
	<summary>
		6. @Override 어노테이션
	</summary>
	<p>
		<br> 다른 Servlet 클래스에 doGet 메소드처럼 doPost 메소드 또한  HttpServlet의 doPost 메소드를 오버라이드하여 재정의한 메소드 입니다. 따라서 오버라이드한 메소드들을 @Override 어노테이션을 붙여주는 것이 좋습니다. 이유는 해당 메소드가 오버라이드 된 메소드라는 것을 명시적으로 알 수 있으며, 컴파일 시 상속한 부모 클래스에 해당 메소드가 있는지 여부등을 통해 예외를 발생할 수 있어 오류를 인지할 수 있기 때문입니다.<br>자세한 내용을 아래 링크 참고 부탁드립니다.<br>https://onsil-thegreenhouse.github.io/programming/java/2017/12/20/java_tutorial_1-17/
	</p>
</details>
<details>
	<summary>
		7. WEB-INF 디렉토리
	</summary>
	<p>
		<br> 해당 jsp 파일이 WEB-INF 디렉토리 하위에 없기 때문에 해당 jsp 파일로 직접 접근이 가능합니다. (http://localhost:8080/main.jsp) 하지만, Servlet을 거치지 않고 바로 노출되기 때문에 노출될 데이터가 다 노출이 되지 않습니다. <br>또한, 보안의 취약점이 발생합니다.하지만 WEB-INF 디렉토리에 위치한다면 직접 접근이 불가능 하기 때문에 위와 같은 일이 발생하지 않습니다.<br>자세한 내용은 아래 링크 참고부탁드립니다.<br>https://xzio.tistory.com/1345
	</p>
</details>


