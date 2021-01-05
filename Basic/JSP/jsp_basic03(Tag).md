# JSP
- jsp는 servlet으로 봐도 된다 
    - why? jsp를 자바 내부적으로는 servlet으로 바꾸기 때문
    - **servlet : java를 사용하여 웹페이지를 동적으로 생성하는 서버측 프로그램, 웹 서버의 성능을 향상하기 위해 사용되는 java class의 종류 중 하나** <br>
    
    ☞ 차이점 
    jsp는 html문서 안에 java코드 포함, servlet은 java코드 안에 html 포함
    
- **동적문서와 정적문서의 차이** (jsp안에서! 다른 언어랑 용어섞지말기: 언어가 다르기 때문에 같은 용어여도 다른 의미로 사용됨)
    1. 정적 페이지 : html , 웹서버에 **미리 저장된 파일**을 그대로 전달(사용자가 데이터를 변경하지 않는 한 고정된 웹페이지)
    2. 동적 페이지 : 페이지 안에 프로그래밍 요소가 중간 중간에 들어가는 것 , 사용자의 요청을 해석해서 데이터를 가공한 후 생성되는 웹페이지 전송 <brb>(현재시간, 날짜 등 기능이 필요해 프로그래밍을 넣은 페이지)
        - 동적페이지를 만드는 서버 관련 언어 : jsp(java언어), asp(C#), php / 클라이언트 쪽 언어 : java script
<br>

## - JSP 태그
### 1. **지시자(페이지속성): <%@    %>**
    - page: 해당 페이지의 전체적인 속성 지정
    - include : 별도의 페이지를 현재 페이지에 삽입(include안하고 페이지의 코드를 긁어와서 넣어주면 html 중복! 
	(뿌려질때는 에러나지 않지만 굉장히 복잡해지고 꼬여서 이상하게 출력될 가능성이있음 따라서 include를 사용해서 내용 삽입해서 사용)
    - taglib: 태그라이브러리의 태그 사용 
<details><summary>지시자 코드 EX</summary>
```jsp
<!-- 지시자 -->
<%@ page import="java.util.Arrays" %> <!-- page: 해당 페이지의 전체적인 속성 지정 -->
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
		int[] iArr = {10, 20, 30};
		out.println(iArr);	
	%>
	
</body>
</html>
```
</details>

### 2. **주석: <%--   --%>**
    - 종류 : **<%-- -->, //, /* */** (자바문법이니까 주석 세개 다 사용가능)
    - html 주석: <!-- --> 
    
### 3. **선언(변수, 메소드선언): <%!	    %>**
<details><summary>선언 코드 EX</summary>
```jsp
<!-- 선언 -->
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<%! <!--! 느낌표 안써주면 오류남 왜냐면 함수사용이기 때문에 ! 들어가야함-->
		int i =10;
		String str = "abc";
			
		public int sum(int a, int b) {
			return a+b;
		}
	%>
	
	<%
		out.print("i = " + i + "<br>");
		out.print("str = " + str + "<br>");
		out.print("sum = " + sum(1,5) + "<br>");
	%>
</body>
</html>
```
</details>

### 4. **표현식(출력out.println): <%=     %>**
<details><summary>표현식 코드 EX</summary>
```jsp
<!-- 표현식 -->
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
		int i =10;
		String str = "abc";
			
		public int sum(int a, int b) {
			return a+b;
		}
	%>
	
	<%= i %> <br>
	<%= str %> <br>
	<%= sum(1,5) %> <br>
</body>
</html>
```
</details>

### 5. **스크립트릿(JAVA 코드): <%      %>** 
<details><summary>스크립트릿 코드 EX</summary>
```jsp
<!-- 스크립트릿 -->
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
	int i = 0;
	while(true) {
		i++;
		out.println("2*" + i + "=" +(2*i) + "<br>"); <!--out.println은 내부객체이기 때문에 System없이 사용 -->  
	
%>
	=========<br> 
	<!-- 자바문법이 아니기 때문에 <% %> 나눠서 넣어준것 만약 <% %> 나누지 않고 하고싶다면 out.println("========="+"<br>"); 이렇게 넣으면됨 -->
<%  
		if( i >= 9) break;
	}
%>
</body>
</html>
```
</details>

### 6. **액션태그(자바빈연결): < jsp:action >	< /jsp:action > or < jsp:action / >**
- 액션태그란 jsp페이지 내에서 어떤 동작을 하도록 지시하는 태그
- 종류 : 
	1. forword : 현재 페이지를 다른 페이지로 전환할 때 사용
	>> [중요!] forward는 서버내에서 페이지를 넘겨주기 때문에 **클라이언트가 치고 들어오는 주소가 바뀌지 X** <br> redirect는 클라이언트가 다시 접근하게 하는거라서 주소 바뀜!!!
	<details><summary>forward 예제 소스 코드</summary>
	```html
	<!-- main.jsp-->
	<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
	<!DOCTYPE html>
	<html>
	<head>
	<meta charset="EUC-KR">
	<title>Insert title here</title>
	</head>
	<body>
		<h1>main 페이지입니다</h1>
		<jsp:forward page= "sub.jsp"/>
	</body>
	</html>

	<!--sub.jsp-->
	<%@ page language="java" contentType="text/html; charset=EUC-KR"
		pageEncoding="EUC-KR"%>
	<!DOCTYPE html>
	<html>
	<head>
	<meta charset="EUC-KR">
	<title>Insert title here</title>
	</head>
	<body>
		<h1>sub페이지 입니다.</h1>
	</body>
	</html>
	```
	▶ 출력
	![forward](https://user-images.githubusercontent.com/74290204/103608840-a98cda80-4f5f-11eb-81db-a5e62537dbc6.PNG)
	</details>

	2. param : forword 액션태그와 param을 이용해서 다른 페이지에 데이터를 전달할 수 있음
	<details><summary>param 예제 소스 코드</summary>
	```jsp
	<!--main.jsp-->
	<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
	<!DOCTYPE html>
	<html>
	<head>
	<meta charset="EUC-KR">
	<title>Insert title here</title>
	</head>
	<body>
		<jsp:forward page="sub.jsp">
			<jsp:param name = "id" value="abcdef"/>
			<jsp:param name = "pw" value = "1234"/>
		</jsp:forward>	
	</body>
	</html>

	<!--sub.jsp-->
	<%@ page language="java" contentType="text/html; charset=EUC-KR"
    	pageEncoding="EUC-KR"%>
	<!DOCTYPE html>
	<html>
	<head>
	<meta charset="EUC-KR">
	<title>Insert title here</title>
	</head>
	<body>
			
		<%! String id, pw; %>
		<% 
			id = request.getParameter("id");
			pw = request.getParameter("pw");
		%>	
		
		아이디 : <%= id %>
		비밀번호 : <%= pw %>
	</body>
	</html>
	```
	▶ 출력 <br>

	![param](https://user-images.githubusercontent.com/74290204/103609503-5f0c5d80-4f61-11eb-9204-5e3415eb23ec.PNG)
	</details>

	3. include : jsp페이지 내에서 다른 페이지를 삽입하는 태그
	<br>
			
	>> **지시자include와는 다름** <br> 
	지시자 include: jsp를 포함하여 java파일을 생성하지만 액션태그를 사용할 경우 실행 중에 동적으로 포함시킴 
		
	4. useBean : 자바빈을 JSP 페이지에서 사용할 때 사용 <br>
	▶[자바빈즈참조링크] https://m.blog.naver.com/pjok1122/221728877690
			
	5. setProperty : property의 값을 세팅할 때 사용 
	6. getProperty : property의 값을 얻어낼 때 사용 
			
	

