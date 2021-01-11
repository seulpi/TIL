# - EL 
스크립트릿은 사실상 실무에서는 사용 잘 안함, EL(Expression)과 JSTL을 혼용해서 사용함
## @ 정의
- EL (Experssion Language) : 액션태그나 자바스트립을 사용하지 않고 좀 더 간결하게 사용하기 위한 언어 <br>
" 다양한 위치에 있는 데이터에 접근하기 위한 언어로 **JSP의 기본 문법을 보완하는 역할**을 한다 "
- EL은 기본객체를 제공하는데 기본객체는 객체를 생성하지 않아도 사용 가능하다 <br>
    ▶ EL의 기본 객체 <br>

 ![EL기본객체](https://user-images.githubusercontent.com/74290204/104171681-5bd40e80-5446-11eb-8631-db3992637166.PNG)

- 문법  : ${ }
>> ${member.name} = < jsp:getProperty name = "member" property="name"/ > <br> 객체 생성하고 객체안에 값을 호출하는 것 

```jsp
${10}
${"ABC"}
${true}
${1+2} 연산도 가능!
${1>2} <!-- true or false 출력 -->
${(1>2) ? 1 : 2}
${(1>2) || (1<2) }
```
```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<jsp:useBean id="member" class="edu.bit.ex.MemberInfo" scope="page" />
/* [tip]
" " 안에는 개발자 영역이니까 프로그램상에서 에러 잡아주지않음 
따라서 사실상 손으로 치는 것보다 복사해서 사용하는게 좋음(오타나서 에러날수있기때문에)*/
<jsp:setProperty name="member" property="name" value="홍길동"/>
<jsp:setProperty name="member" property="id" value="abc"/>
<jsp:setProperty name="member" property="pw" value="123"/>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>

	이름: <jsp:getProperty name="member" property="name"/><br>
	아이디 : <jsp:getProperty name="member" property="id"/><br>
	비밀번호 : <jsp:getProperty name="member" property="pw"/><br>
	
	<hr>
	이름 : ${member.name}<br> <!-- getName호출 -->
	아이디 : ${member.id}<br>
	비밀번호 : ${member.pw}	 

</body>
</html>

<!--obj_el.jsp-->
<%@ page language="java" contentType="text/html; charset=EUC-KR" pageEncoding="EUC-KR"%>

<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<form action="result.jsp" method="get">
		아이디: <input type="text" name="id"><br>
		비밀번호: <input type="password" name="pw"><br>
		<input type="submit" value="login">
	
	</form>
	

</body>
</html>

<!-- result.jsp-->
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
		String id = request.getParameter("id");
		String pw = request.getParameter("pw");
	%>
	
	아이디: <%= id %><br>
	비밀번호: <%= pw %>
	
	아이디: ${ param.id }<br> 
	비밀번호: ${ param.pw } 
	<!-- param = request.getParameter()의 약자, 따라서 위에 담긴 값이 출력 가능-->
	
	아이디: ${ param["id"] }<br>  <!-- param["id"] = param.id -->
	비밀번호: ${ param["pw"] } 

</body>
</html>
```

# - 기본 객체 Scope
## - Scope : 영역
- 영역에는 크게 4가지가 존재 : page, request, session, aplication 
- EL로 사용할때는 scope를 항상 붙여준다 
    - ex) pageScope, requestScope, sessionScope, aplicationScope
    ```jsp
    ${pageScope.page_name}
    ```

### 1. page : 객체 사용 범위 **현재 페이지까지만** 가능
### 2. request : page보다는 영역이 넓음, **응답이전까지** 가능 
- 클라이언트가 주소를 치고 들어오면 온갖 정보를 다 가져옴 따라서 그 정보들을 서버가 request객체로 담고 <br> request 객체는 응답받기 전까지 메모리에서 저장하고 보관한다(response하는 순간 request는 메모리 공간에서 Bye~)
### 3. session : **지정된 시간 안에서만** 가능 , 웹브러우저 당 1개
### 4. aplication  : 구동되는 **어플리케이션 전체**에서 다 사용 가능(영역의 범위가 가장 넓다)

>>[영역 범위 크기] aplication > session > request > page

```jsp 
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<form action="result.jsp" method="get">
		<input type="submit" value="결과">
	</form>
	
	<%
	//4가지 다 내장객체 그말인즉슨 , 메모리에 이미 올라가 있단 얘기고 jsp에서 기본적으로 제공한단 소리
	
	application.setAttribute("application_name", "application_value"); 
	/*application_name 으로 application_value을 저장 (key, value)형태
	application이 종료되면 사라짐*/
	session.setAttribute("session_name","session_value" );
	/* 웹브라우저당 1개씩 메모리 공간을 할당(시간지나면 메모리에서 Bye~)
	기본적으로 새로고침하면 시간이 다시 바꿔서 시간을 다시 할당하는데 서버에서 어떻게 정하냐에 따라 다름*/
	pageContext.setAttribute("page_name","pageContext_value" );
	// page는 page마다 메모리 공간을 할당
	request.setAttribute("request_name","request_value" );
	
	%>
	
	${applicationScope.application_name} <br>
	${sessionScope.session_name} <br>
	${pageScope.page_name} <br>
	<!-- 본인페이지니까 출력 -->
	${requestScope.request_name} 
	
	
	<jsp:forward page="result2.jsp" />
	<!-- forward시키면 왜 출력되는가? forward는 서버내에서 접근하기 때문에 request가 살아있기 때문에
	redirection은 클라이언트 재접속하기 때문에 request 사라짐 -->

</body>
</html>

<!-- result2.jsp -->
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	${applicationScope.application_name} <br>
	${sessionScope.session_name} <br>
	${pageScope.page_name} <br>
	<!-- 본인페이지를 벗어나기 때문에 출력하지않음(세팅은 obj_el.jsp에서 했으니까) -->
	${requestScope.request_name} 
	<!-- 출력하지 않음 why? 응답주기 전까지이기 때문에, 
	이 문법들 자체를 그대로 출력해서 보여주는 게 아니라 html언어로 바꿔서 출력해주는원리
	(따라서 request객체에서 response로 받아서 html언어로 바꿔서 화면 출력) 
	request객체는 response하면 메모리에서 사라짐(request의 영역 결정)-->

</body>
</html>
```

# - JSTL(Jsp Standard Tag Library) : "<c: >"
- html과 java코드를 같이 사용할 경우 가독성이 떨어지기 때문에 좀 더 보기쉽게 하기 위해 나온 태그 라이브러리 
- 라이브러리이기 때문에 다운로드 받아서 사용해야하고 파일 복사가 완료된 상태에서 문법 사용이 가능하다
- <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %> 
태그라이브러리 명시해줘야함 

<details><summary>다운로드 방법 click</summary>
< 다운로드 후 복사 방법 >
1. 폴더안에 복사한 후 이클립스에서 복사하는 방법
2. 다이렉트로 이클립에 복사 + 붙여넣기 하는 방법

![JSTL설치01](https://user-images.githubusercontent.com/74290204/104145765-9bc9d000-540b-11eb-8d7a-8220674a0356.PNG)

![JSTL설치02](https://user-images.githubusercontent.com/74290204/104145766-9d939380-540b-11eb-9fe9-14dcad5b2dfa.PNG)

![JSTL설치03](https://user-images.githubusercontent.com/74290204/104145767-9e2c2a00-540b-11eb-9e75-7410b0cff391.PNG)
</details>

## @ JSTL 종류

![jspl](https://user-images.githubusercontent.com/74290204/104172149-21b73c80-5447-11eb-8409-253fadeb98b8.PNG)
<br>

### ▶ 자세한 설명은 jsp&DB_Answer03 <br> https://github.com/Lee-sb92/TIL/blob/main/Quiz/jsp%26DB_Answer03.md

```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %> 
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	
	<c:set var="varName" value="varValue"/>
	<!-- var=variable 변수명 , var varName = varValue; / 값 세팅-->
	varName: <c:out value ="${varName}"/> 
	<!-- value에 varName 출력 / 값 출력-->
	
	<br> 
	
	<c:remove var="varName"/> 
	<!-- 메모리에 올린 값 삭제 -->
	varName: <c:out value ="${varName}"/> 
	
	<!-- @예외처리를 제공한다 -->
	<c:catch va="error">
		<%=2/0 %>
	</c:catch>
	<c:out value="${error}"/>
	
</body>
</html>
```
