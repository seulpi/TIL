# - request 객체 
## request 객체는 내장 객체 
>> 내장객체란? 객체생성(new)을 따로 하지 않아도 기본적으로 jsp에서 제공하는 객체 <br> 대표적인 내장객체 : request, response, out ▶ 내장객체 종류 참고 링크 https://pathas.tistory.com/184

- [참조] jsp 페이지 실행시 jsp파일을 servlet으로 바꾸기 때문에(servlet이 자동으로 생성되기 때문에) <br> jsp가 변환된 .java파일 안 jspService() 메소드 내부에 내장객체가 선언되어있다

```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<%
		out.println("서버 : " + request.getServerName() + "<br/>");
		out.println("포트 번호 : " + request.getServerPort() + "<br/>");
		out.println("요청 방식 : " + request.getMethod() + "<br/>");
		out.println("프로토콜 : " + request.getProtocol() + "<br/>");
		out.println("URL : " + request.getRequestURI() + "<br/>");
		out.println("URI : " + request.getRequestURL() + "<br/>");
	%>
</body>
</html>
```

### ▶ URL과 URI란?  
- URI(Uniform Resource Identifier) : 절대 경로 만들 때 사용, URI는 URN과 URL을 포함(상위개념이다)
- URL(Uniform Resource Locator) : 위치 (일종의 하나의 파일 위치)
- URN(Uniform Resource Name) : 이름

```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<form action ="requestparam.jsp" method=post> 
		이름: <input type="text" name="uname" size="10"><br>
		아이디: <input type="text" name="uid" size="10"><br>
		비밀번호: <input type="password" name="pw" size="10"><br>
		취미:	<input type="checkbox" name="hobby" value="read">독서
				<input type="checkbox" name="hobby" value="cook">요리
				<input type="checkbox" name="hobby" value="running">조깅
				<input type="checkbox" name="hobby" value="swimming">수영
				<input type="checkbox" name="hobby" value="sleeping">취침 <br>
				<input type="radio" name="major" value="kor">국어
				<input type="radio" name="major" value="eng">영어
				<input type="radio" name="major" value="math">수학
				<input type="radio" name="major" value="design">디자인 <br>
	<select name="protocol">
		<option value="http">http</option>
		<option value="smtp">smtp</option>
		<option value="ftp">ftp</option>
		<option value="pop">pop</option>
	</select> <br>
	<input type="submit" value="전송">
	<input type="submit" value="초기화">
	
	</form>
</body>
</html>
```
```jsp
<!--requestparam.jsp-->
<%@page import="java.util.Arrays"%>
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>

	<%!
		String name, id, pw, major, protocol;
		String[] hobbys;
	%>
	
	<%
		request.setCharacterEncoding("EUC-KR");
		name = request.getParameter("uname");
		id = request.getParameter("uid");
		pw = request.getParameter("pw");
		major = request.getParameter("major");
		protocol = request.getParameter("protocol");
		
		hobbys = request.getParameterValues("hobby");
	%>
	이름 : <%=name%><br>
	아이디 : <%= id %><br>
	비밀번호 : <%= pw %><br>
	취미 : <%= Arrays.toString(hobbys) %><br>
	전공 : <%= major %><br>
	프로토콜: <%= protocol %><br>
</body>
</html>
```
<br>

# - response 객체
1. getCharacterEncoding() : 응답시 문자의 인코딩 형태를 구해온다 
2. addCookie(Cookie) : 쿠키 지정할때 사용 
3. sendRedirect() : 지정한 URL로 이동할 때 사용

### @ redirect와 forward의 차이! ★
- redirect : 클라이언트에서 요청했을 때 웹서버에서 redirect하면 **클라이언트로 하여금 다시 접근하게 하는 것(클라이언트에게 다시 요청)**
- forward : **자기 자신이 접근**해서 클라이언트에게 넘겨주는 것

>> redirect와 forward는 페이지에서 조건문을 줘서 나눠서 작성해야할 때 사용한다
#### _*redirect EX 코드*
```html
<!--redirectex.html-->
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<form action="request_send.jsp">
		당신의 나이는: <input type ="text" name = "age" size = "5">
		<input type ="submit" value="전송">
	</form>
</body>
</html>
```
```jsp
<!--redirect_send.jsp-->

<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<%! 
		int age;
	%>
	
	<%
		String str = request.getParameter("age");
		age = Integer.parseInt(str);
		
		if(age<20) {
			response.sendRedirect("nonpass.jsp?age=" + age);
		} else {
			response.sendRedirect("pass.jsp?age=" + age);
		}
	%>
	성인입니다. 주류구매가 가능합니다 
	<a href = "requestex.html">처음으로 이동</a>
</body>
</html>
```
```jsp
<!--pass.jsp-->

<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<%! 
		int age;
	%>
	
	<%
		String str = request.getParameter("age");
		age = Integer.parseInt(str);
	%>
	
	
	성인입니다. 주류구매가 가능합니다 
	<a href = "requestex.html">처음으로 이동</a>
</body>
</html>
```
```jsp
<!--nonpass.jsp-->

<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<%! 
		int age;
	%>
	
	<%
		String str = request.getParameter("age");
		age = Integer.parseInt(str);
	%>
	
	성인이 아닙니다. 주류구매가 불가능합니다 
	<a href = "requestex.html">처음으로 이동</a>
</body>
</html>
```

▶ 출력

![re1](https://user-images.githubusercontent.com/74290204/103597884-97517300-4f44-11eb-9a1d-c6724dfe8ea6.PNG)
![re2](https://user-images.githubusercontent.com/74290204/103597886-97ea0980-4f44-11eb-9cdf-649abe024170.PNG)
![re3](https://user-images.githubusercontent.com/74290204/103597888-991b3680-4f44-11eb-9e20-95fdd3c6417f.PNG)
![re4](https://user-images.githubusercontent.com/74290204/103597889-9a4c6380-4f44-11eb-8eaf-21c09470b65b.PNG)

