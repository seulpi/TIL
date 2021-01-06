# - 예외 페이지 
: 예외를 처리하는 페이지를 설정하는 것! 방법은 2가지 
## 1. page 지시자를 이용한 예외 처리
- [핵심] 예외페이지로 넘어가면서 예외처리한 부분으로 화면 출력 → 주소 바뀌지X 
>> ▶ 내부적으로 forward 처리 

```jsp
<!--main.jsp-->
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

## 2. web.xml 를 이용한 예외 처리 
: 예외 처리를 보내는 경로는 **절대 경로** 

```jsp
// info.jsp
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


//error404.jsp 
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


//error500.jsp
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


//web.xml [에러처리]
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
	<display-name>class_ex</display-name>
	<welcome-file-list> //경로 리스트 
		<welcome-file>index.html</welcome-file> 
		<welcome-file>index.htm</welcome-file>
		<welcome-file>index.jsp</welcome-file>
		<welcome-file>default.html</welcome-file>
		<welcome-file>default.htm</welcome-file>
		<welcome-file>default.jsp</welcome-file>
	</welcome-file-list>
	
	<error-page>
		<error-code>404</error-code>
		<location>/error404.jsp</location> // 절대경로! 경로 주의 
	</error-page>
	<error-page>
		<error-code>500</error-code>
		<location>/error500.jsp</location>
	</error-page>

</web-app>
```

