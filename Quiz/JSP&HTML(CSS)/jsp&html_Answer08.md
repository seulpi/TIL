# 1. session 이란?
: 쿠키와 마찬가지로 웹브라우저와 웹서버를 연결해주는 하나의 수단

## @ 특징
1. session은 쿠키와 달리 클라이언트 쪽에 저장되는 게 아니라 서버상에 객체로 저장한다 
2. 데이터 한계가 없으며 서버에 저장되기 때문에 서버에서만 접근이 가능하고 보완에 용이하다 (단, 서버메모리 측면에서는 많이 쓰는게 좋은것은 아님)
3. session은 내부객체이며 서버는 session 객체를 클라이언트마다 가지고 있다 
 
## @ session.getId(); : 웹브라우저마다 고유의 session ID를 가진다 (ID는 자동생성)
- 시간이 지나면 자동으로 소멸되고(30분 기준:1800) 시간이 지나고 다시 접근해도 똑같은 ID를 부여하는 게 아님 (다른 ID를 부여)
- session ID는 클라이언트를 구분하는 기준 (브라우저 당 1개씩 할당)
- session ID는 Coolkie와 session을 동시에 가져다 사용 
>> 웹서버에서는 session이라는 공간에 접속하는 사람마다 session ID라는 게 존재하며 <br> Coolkie를 담아서 클라이언트 쪽에 똑같은 session ID를 전달 <br> 따라서 30분 동안에 session ID로 동일한 접근자라고 인식해서 로그아웃이 안되는 것

```
[session 관련 함수]
1. setAttribute() : 세션에 데이터를 저장한다(=메모리에 올린다) , 이 함수를 사용하면 addCookie처럼 데이터를 따로 저장할 필요가 없다                 
2. getAttribute() : 세션에서 데이터를 얻는다
3. getAttributeNames() : 세션에 저장되어 있는 모든 데이터의 이름(유니크한 키값)을 얻는다
4. getId() : 자동 생성된 세션의 유니크한 아이디를 얻는다
5. isNew() : 세션이 최초 생성되었는지, 이전에 생성된 세션인지를 구분 
6. getMaxInactiveInterval() : 세션의 유효시간을 얻고 가장 최근 요청시점을 기준으로 카운트
7. removeAttribute() : 세션에서 특정 데이터 제거
8. Invalidate() : 세션의 모든 데이터를 삭제 (싹 밀어버리는 것)
```

# 2. DBMS의 의미와 종류는?
## @ 의미
:  DBMS(DataBase Managemnent System) 은 DB(DataBase)를 관리하는 시스템 
>> DB : 여러 사람이 공유하고 사용할 목적으로 관리되는 정보 (데이터의 집합)
- DBMS는 DB를 구축하는 틀, 검색 및 저장 , DB가 접근할 수 있는 인터페이스, 복구기능과 보안성 기능을 제공한다 <br>

## @ 종류
### 1. oracle
 - 오라클에서 만들어 판매중인 상업용 DB 
 - Window, Linux, Unux 등 다양한 운영체제에 설치 가능
 - MySQL, MSSQL보다 대량의 데이터를 처리하기 용이
 - 대기업에서 주로 사용하며, DB시장 점유율 1위 
 - 학습용으로는 많은 걸 오픈하지 않고 비용이 비쌈

### 2. MySQL 
 - MySQL사에서 개발, 현재 오라클과 합병됨
 - Window, Linux, Unux 등 다양한 운영체제에 설치 가능
 - 오픈소스로 무료프로그램(상업적 사용시 비용 발생)
 - 중소기업에서 주로 사용하며, 상대적으로 비용이 저렴
 - 5천만건 이하의 데이터를 다루는 데 적합 

### 3. MSSQL 
 - 마이크로소프트사에서 개발한 상업용 DB
 - 비공개 소스(Linux 버전은 오픈 소스)
 - 윈도우에 특화됨 (다른 운영체제도 사용 가능)
 - 중소기업에서 주로 사용

### 4. MariaDB 
 - MySQL이 오라클에 합병된 후 불확실한 라이선스 문제를 해결하려고 나온 RDBMS
 - 언어는 C++
 - MySQL과 동일한 소스 코드 기반
 - MySQL과 비교하여 어플리케이션 부분 속도가 약 4-5천배 빠름

### 5. Tibero 
 - 티맥스 데이터에서 제작한 국내 DBMS
 - SQL등을 포함한 오라클의 제품과 거의 동일한 호환성을 제공
 - 동기화 성능을 개선하여 다중 노드에서도 안정적인 DB 서비스 운영이 가능
 - 많은 플랫폼에 지원됨에 따라 국내 업체들의 사용빈도가 잦아짐
 - 자기튜닝을 통한 성능 최적화, 지속적인 DB모니터링, 성능 관리 지원등을 제공 

[참조] https://m.blog.naver.com/icbanq/221720815999


# 3. session id 란?
: sessin id 란 웹브라우저 당 갖는 고유의 ID이며 클라이언트가 접근할 때 자동 생성된다(sessin id로 클라이언트 구분)
- sessin id 의 제한 시간은 30분(1800)이며 웹브라우저 당 1개씩 할당한다 <br>
[ex] 로그인했을때 접근할때마다 로그인 하지 않아도 되는 부분, 다른 웹브라우저는 로그인 적용이 안되는 부분
- sessin id 는 Cookie 와 session을 같이 사용하며 Cookie를 담아서 똑같은 sessin id를 클라이언트에게 전달한다


# 4. 에러페이지 처리 방법 2가지를 설명하시오
## @ page를 지시자를 활용한 예외처리 
>> <%@ page errorPage="에러처리할 페이지명" %>

#### - page를 활용해서 예외처리를 할 때 핵심은 자체적으로 에러 페이지 이동시킬 때 **forward를 한다는 것 (주소가 변하지 X)**

```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
   <%@ page errorPage="errorPage.jsp" %> <!-- 에러가 발생하면 errorPage.jsp로 보내라-->
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content type" content="text/html; charset=EUC-KR">
<title>Insert title here</title>
</head>
<body>

	<%
		int i = 40/0; // 에러 발생 
	%>

</body>
</html>

<!-- errorPage.jsp-->
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
   <%@ page isErrorPage="true" %> 
   <% response.setStatus(200); %> <!-- 에러가 아닌 정상적으로 잘 돌아가면 200, body안에 있어도 상관없음 -->
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content type" content="text/html; charset=EUC-KR">
<title>Insert title here</title>
</head>
<body>
	에러 발생 <br>
	에러 메세지: <%= exception.getMessage() %> <!-- 에러 발생하면 띄움 --> 

	<!-- response 와 exception은 객체생성을 따로 하지 않아도 사용가능하니 "내장 객체" -->
</body>
</html> 
```

## @ web.xml를 활용한 예외처리 
: 예외처리를 web.xml에서 기술해 처리한다! 따라서 이 방법을 사용하려면 web.xml이 있어야함 

```jsp
<!--info.jsp-->
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
   <%@ page errorPage="errorPage.jsp" %> <!-- 에러가 발생하면 errorPage.jsp로 보내라-->
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=EUC-KR">
<title>Insert title here</title>
</head>
<body>

	<a href="error03.jsp">error03.jsp</a>

</body>
</html>

<!--error404.jsp--> 
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<%@ page isErrorPage="true" %>
<% response.setStatus(200); %>
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=EUC-KR">
<title>Insert title here</title>
</head>
<body>

	404에러입니다.

</body>
</html>
<!--error500.jsp-->
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<%@ page isErrorPage="true" %>
<% response.setStatus(200); %>
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=EUC-KR"s>
<title>Insert title here</title>
</head>
<body>

	500에러입니다.

</body>
</html>

<!--web.xml [에러처리]-->
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
	<display-name>class_ex</display-name>
	<welcome-file-list>
		<welcome-file>index.html</welcome-file>
		<welcome-file>index.htm</welcome-file>
		<welcome-file>index.jsp</welcome-file>
		<welcome-file>default.html</welcome-file>
		<welcome-file>default.htm</welcome-file>
		<welcome-file>default.jsp</welcome-file>
	</welcome-file-list>
	
	<error-page>
		<error-code>404</error-code>
		<location>/error404.jsp</location>
	</error-page>
	<error-page>
		<error-code>500</error-code>
		<location>/error500.jsp</location>
	</error-page>

</web-app>
```

# 5. bean 이란?
: bean이란 JAVA언어의 데이터와 기능으로 이루어진 클래스를 말한다(한마디로 객체)
- 객체 안에 일반 데이터 멤버의 기본적인 getter&setter함수를 만들어준다
- 액션태그를 이용해서 bean을 사용한다 (jsp페이지에서 사용할 java bean을 객체를 지정할 때 액션태그 사용)
>> < jsp:useBean id="객체 변수명" class="패키지 이름을 포함한 자바빈 클래스의 이름" scope="java page를 저장할 영역"> <br> 
→ scope의 default는 page 

```java
package edu.bit.ex;

public class Student {
	
	private String name;
	private int age;
	private int grade;
	private int studentNum;
	
	public Student() {
		
	}

	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public int getGrade() {
		return grade;
	}
	public void setGrade(int grade) {
		this.grade = grade;
	}
	public int getStudentNum() {
		return studentNum;
	}
	public void setStudentNum(int studentNum) {
		this.studentNum = studentNum;
	}
}
```
```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
    
    <!-- Student student = new Student(); , class는 student가 참조하는 객체가 속해있는 패키지명.클래스명-->
<jsp:useBean id="student" class="edu.bit.ex.Student" scope="page" />

<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
			<!-- student.setName("홍길동"); -->
	<jsp:setProperty name="student" property="name" value="홍길동"/>
	<jsp:setProperty name="student" property="age" value="13"/>
	<jsp:setProperty name="student" property="grade" value="60"/>
	<jsp:setProperty name="student" property="studentNum" value="20121101"/>
	
				<!-- student.getName(); -->
	이름 : <jsp:getProperty property="name" name="student"/> <br>
	나이 : <jsp:getProperty property="age" name="student"/> <br>
	점수 : <jsp:getProperty property="grade" name="student"/> <br>
	학번 : <jsp:getProperty property="studentNum" name="student"/>

</body>
</html>
```


---
# Quiz 
## 1. 가위바위보 게임을 구현하라 
![q1](https://user-images.githubusercontent.com/74290204/103740872-69028f00-503b-11eb-8de7-5ec7ed1938d1.PNG)

```jsp
<!--gameInput.jsp-->
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<h1>가위바위보 게임</h1>
	<img src ="http://res.heraldm.com/content/image/2016/06/17/20160617000234_0.jpg" width="30%">
	<form action="gameoutput.jsp">
		<select name ="user">
			<option value="1">가위</option>
			<option value="2">바위</option>
			<option value="3">보</option>
		</select>
		<input type="submit" value="제출">
	</form>
</body>
</html>
```
```jsp
<!--gameoutput.jsp-->
<%@page import="java.io.PrintWriter"%>
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<%! int scissors, paper, rock; 
		int comNum;	
		String computer;
	%>

	<%
		request.setCharacterEncoding("EUC-KR");
	
		scissors = Integer.parseInt(request.getParameter("user"));
		paper = Integer.parseInt(request.getParameter("user"));
		rock = Integer.parseInt(request.getParameter("user"));
		
		response.setContentType("text/html; charset=EUC-KR");
		
		out.println("<h1>당신이 낸 것</h1>");

		if(scissors == 1) {
			out.println("<img src='https://apktada.com/storage/images/appinventor/ai_bbestest2/RSP_sjy_06/appinventor.ai_bbestest2.RSP_sjy_06_1.png'>");

		} else if(paper == 2) {
			out.println("<img src='https://blog.kakaocdn.net/dn/dWscPZ/btqFnPyjcoJ/EkFxXpYN58Xaf0HuAMWpH0/img.png'>");
		} else {
			out.println("<img src='https://mnmsoft.co.kr/aivs/images/3.png'>");
		}
		
		comNum = (int)(Math.random()*3)+1;
		out.println("<br><h1>컴퓨터가 낸 것</h1>");
		
		if(comNum == 1) {
			out.println("<br><img src='https://apktada.com/storage/images/appinventor/ai_bbestest2/RSP_sjy_06/appinventor.ai_bbestest2.RSP_sjy_06_1.png'>");
		} else if(comNum == 2) {
			out.println("<br><img src='https://blog.kakaocdn.net/dn/dWscPZ/btqFnPyjcoJ/EkFxXpYN58Xaf0HuAMWpH0/img.png'>");
		} else {
			out.println("<br><img src='https://mnmsoft.co.kr/aivs/images/3.png'>");
		}
		
		if(comNum == scissors || comNum == paper || comNum == rock) {
			out.println("<br><h1>무승부</h1>");
		} else if(comNum > paper || comNum < rock || comNum < scissors) {
			out.println("<br><h1>컴퓨터 승리</h1>");
		} else {
			out.println("<br><h1>유저 승리</h1>");
		}
	%>
	
	<a href="gameInput.jsp">다시 하기</a>

</body>
</html>
```
▶ 출력 
![a1](https://user-images.githubusercontent.com/74290204/103750272-e7fec400-5049-11eb-9e2d-95ff6b135f9a.PNG)
