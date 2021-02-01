# 정적 리소스 처리 
## @ webapp / resources : 정적 리소스 처리를 위한 폴더 <br> → servlet-context.xml에서 resource 컨트롤 하는 구문
```jsp
	<!-- mapping="/resources/**" : http://IP:8282/context/resources/로 치고들어오는 모든 폴더 이름 ' / ' 는 context까지 '**' 는 all
    location="/resources/" :  물리적인 경로  첫번째 '/ '  → webappRoot 마지막 '/ '  → all (='**) , wepapp폴더 안에 resource 폴더안에있는 모든것을 탐색  -->

	<resources mapping="/resources/**" location="/resources/" />

	▶ url context 명 뒤에오는 경로들은 loacation 경로랑 맞춰주면된다 
    - 확장자 대소문자 타기 때문에 경로에 구분해서 넣어줘야함
```
## @ WEP-INF : 보안이 필요한것들을 위한 폴더(클라이언트에게 보이면 안되는것들-세팅, 소스파일 등)

```jsp	
<resources mapping="/resources/**" location="/WEB-INF/img" />

▶ 이렇게 절대 하면 안됨! (정적리소스 처리하는곳이 아니기 때문에 주의할것!!!)
```
![정적리소스](https://user-images.githubusercontent.com/74290204/106417763-21372200-6498-11eb-89e5-b4be65f34952.PNG)

- 주소 url을 절대경로로 만드는게 제일 보기 쉽고 좋음 
### <절대경로 만드는 방법> 
1. ${pageContext.request.contextPath} = /ex(컨텍스트명)
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>

<html>
<head>
	<title>Home</title>
</head>
<body>
<h1>
	Hello world!  
</h1>
<img alt ="" src="${pageContext.request.contextPath}/board/resouces/img/dog.jpg">
<!--$앞에 '/' 붙여주는거 X  -->
<!-- /사이에/ 공백 들어가면 공백도 숫자로 인식하기 때문에 경로가 달라짐 -->

</body>
</html>
```

2. /ex(컨텍스트명)
```jsp
<body>
<h1>
	Hello world!  
</h1>
<img alt ="" src="/board/resouces/img/dog.jpg">

</body>
</html>
```
