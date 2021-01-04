# - request 객체 
## request 객체는 내장 객체 
>> 내장객체란? 객체생성(new)을 따로 하지 않아도 기본적으로 jsp에서 제공하는 객체 <br> 대표적인 내장객체 : request, response, out ▶ 내장객체 종류 참고 링크 https://pathas.tistory.com/184

- [참조] jsp 페이지 실행시 servlet이 자동으로 생성되기 때문에 jsp가 변환된 .java파일 안 jspService() 메소드 내부에 내장객테 선언되어있다

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
- URL :
- URI : 절대 경로 만들 때 사용

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

### ★ redirect와 forward의 차이! 
- redirect : 클라이언트에서 요청했을 때 웹서버에서 redirect하면 **클라이언트로 하여금 다시 접근하게 하는 것(클라이언트에게 다시 요청**
- forward : **자기 자신이 접근**해서 클라이언트에게 넘겨주는 것

>> redirect와 forward는 페이지에서 조건문을 줘서 나눠서 작성해야할 때 사용한다
