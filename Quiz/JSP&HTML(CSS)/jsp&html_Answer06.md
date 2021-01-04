# 1. box-sizing 속성들에 대하여 설명하시오
## : box-sizing이란 padding,margin,border등을 활용했을때 <br>기존 width와 height에 포함해서 계산하는지 아닌지에 대한 정의
### 1. box-sizing : border-box → 기존 크기 안에 속성을 포함해서 내부에서 해결
### 2. box-sizing : content-box → default, 기존 크기를 건드리지 않고 content내용들이 더해진다 (기존에 정의했던 크기가 속성들이 더해질때마다 커짐)
<br>

# 2. margin 과 padding의 차이는?
### - margin : 외부 간격
### - padding : 내부 간격
![padding margin](https://user-images.githubusercontent.com/74290204/103519895-a6d7aa00-4eb9-11eb-8fb4-1c08c6ed82c6.PNG)
<br>

# 3. 내장객체에 대하여 설명하시오
### : 객체생성(new)을 따로 하지 않아도 객체가 이미 jsp안에 만들어져 있어서 기본적으로 제공하는 객체 
▶ 대표적인 내장 객체 : request / response / out
<br>

# 4. 구구단을 세로로 나타내도록 jsp 로 짜시오
## - out.println 을쓰지 말고 <%= expression을 사용 하시오
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
	<h1>구구단 출력</h1>
	<table border="1">
	<%
		for(int i = 1; i < 10; i++) {
			out.println("<tr>");
			for(int j = 2; j <10; j++) {
	%>
	
	<%="<td>"%>
	<%=j + "*" + i + "=" + (i*j)%>
	
	<% } %>
	<%="<br></td>"%>
	<% } %>
	<%="</tr>"%>

	</table>

</body>
</html>
```
<details><summary> 출력 </summary>

![A4](https://user-images.githubusercontent.com/74290204/103521092-8d376200-4ebb-11eb-8d77-7c97b4148cfc.PNG)
</details>
<br>

# 5. redirect forward 의 차이는?
## 활용: redirect와 forward는 페이지에서 조건문을 줘서 나눌 때 사용!
### 차이점 : redirect는 클라이언트로 다시 접근하게 하는 것 / forward는 자기자신이 접근해서 클라이언트에게 넘겨주는 것

---
# Quiz

## 1. 아래를 구현하시오 
![Q1](https://user-images.githubusercontent.com/74290204/103512091-8274d100-4eab-11eb-8ab9-bc1c3bb00c8f.PNG)

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
	<style>
		#box {
			width: 800px;
			height: 165px;
			margin:0 auto;
			border: 1px solid gray;
			overflow: hidden;
		}
		
		div {
			width: 150px;
			height: 150px;
			float: left;
			margin: 5px;
			line-height: 150px;
			border-top: 1px solid gray;
			border-bottom: 1px solid gray;
			text-align: center;
			font-size: 1.2em;
			font-weight: bold;
		}
	</style>

</head>
<body>
	<div id="box">
		<div>menu1</div>
		<div>menu2</div>
		<div>menu3</div>
		<div>menu4</div>
		<div>menu5</div>
	</div>

</body>
</html>
```
<details><summary>출력 </summary>

![A1](https://user-images.githubusercontent.com/74290204/103512228-b6e88d00-4eab-11eb-90b5-25b93611e419.PNG)
</details>
<br>

## 2. 아래를 구현하시오 
![Q2](https://user-images.githubusercontent.com/74290204/103512367-e7302b80-4eab-11eb-8560-143e66895036.PNG)

```html
<!-- 방법1 -->
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
	<style>
		#table {
			width: 800px;
			height: 400px;
			border: 1px solid black;
		}
		
		div p:nth-child(1) {
			height: 100px;
			font-size: 2em;
			font-weight: bold;
			text-decoration: underline;
			text-align: center;
			
		}
		
		div p:nth-child(2) {
			font-weight: bold;
			font-size: 1.2em;
			text-align: left;
		}
		
		div p:nth-child(4) {
			font-weight: bold;
			font-size: 1.2em;
			text-align: right;
		}
		
		div p:nth-child(6) {
			height:100px;
			line-height: 70px;
			text-align: center;
			
		}
		
		a {
			font-weight: bold;
			font-size: 1.5em;
			
			text-decoration: none;	
		}
	</style>
</head>
<body>
	<div id = "table">
		<p>HTML5,CSS3 Document</p>
		<p>To. all member</p>
		<p>html5, CSS5 study is very easy</p>
		<p>From. SBA</p>
		
		<hr/>
		
		<p><a href="www.sba.kr">서울산업진흥원</a></p>
	
	</div>

</body>
</html>
```
```html
<!-- 방법2 -->
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
	<style>
		#nav {
			width:800px;
			height:400px;
			margin:0 auto;
			border: 1px solid gray;
			
			overflow:hidden;
		}
		
		.one {
			height: 100px;
			text-align: center;
			font-size: 2em;
			font-weight: bold;
			line-height: 100px;
		}
		
		.two {
			height: 50px;
			text-align: left;
			font-size: 1em;
			font-weight: bold;
		}
		
		.three {
			height: 50px;
			text-align: left;
			font-size: 1em;
		}
		
		.four {
			height: 50px;
			text-align: right;
			font-size: 1em;
			font-weight: bold;
			border-bottom: 1px solid gray;
		}
		
		.five {
			height: 50px;
			text-align: center;
			font-size: 1.5em;
			font-weight: bold;
			line-height: 150px;
		}
		
		.one a {
			text-decoration: none;
			border-bottom: 5px solid black;
		}
		
		.five a {
			text-decoration: none;
		}
		
	
	</style>
</head>
<body>
	<div id ="nav">
		<div class='one'><a href="https://www.w3schools.com/css/">HTML5, CSS3 Document</a></div>
		<div class='two'>&nbsp To.all member</div>
		<div class='three'>&nbsp html5, CSS5 study is very easy</div>
		<div class='four'>Form. SBA &nbsp</div>
		<div class='five'><a href="www.sba.kr">서울산업진흥원</a></div>
	</div>

</body>
</html>
```
<details><summary> 출력 </summary>

![A2](https://user-images.githubusercontent.com/74290204/103514155-68d58880-4eaf-11eb-8b3e-09f1f7fe30c6.PNG)
</details>
<br>

## 3. 아래를 구현하시오 
![Q3](https://user-images.githubusercontent.com/74290204/103514204-8276d000-4eaf-11eb-8068-9d8e2ed0910c.PNG)

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
	<style>
		#headbox {
			width: 500px;
			border: 2px solid black;
			overflow: hidden;
		}
		
		div p:nth-child(1) {
			font-size: 2em;
			font-weight: bold;
			color: green;
			border: 1px solid gray;
		}
	</style>

</head>
<body>
	<div id = "headbox">
		<p>세계 3대 미항</p>
		<img src="http://www.obsnews.co.kr/news/photo/201904/1153128_357045_5944.jpg" width="80%">
		<p>시드니(Sydney), 호주</p>
		<p>리우데자네이루(Rio de Janeiro), 브라질</p>
		<p>나폴리(Napoles), 이탈리아</p>
	</div>

</body>
</html>
```
<details><summary> 출력 </summary>

![A3](https://user-images.githubusercontent.com/74290204/103515467-1f3a6d00-4eb2-11eb-82fb-1f6896ea145f.PNG)
</details>

## 4. jsp로 아래를 구현하시오(필수)
<br>

![Q4-1](https://user-images.githubusercontent.com/74290204/103515566-4db84800-4eb2-11eb-8ffc-1fb684698dd9.PNG)
![Q4-2](https://user-images.githubusercontent.com/74290204/103515581-5741b000-4eb2-11eb-98f5-178276f6ee4b.PNG)

```html
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<form action="formsubmit.jsp" method="post">
		이름: <input type="text" name="name" size="5"><br>
		아이디: <input type="text" name="id" size="5"><br>
		비밀번호: <input type="password" name="pw" size="5"><br>
		취미: <input type="checkbox" name="hobby" value="read">독서
		<input type="checkbox" name="hobby" value="cook">요리
		<input type="checkbox" name="hobby" value="run">조깅
		<input type="checkbox" name="hobby" value="swim">수영
		<input type="checkbox" name="hobby" value="sleep">취침<br>
		전공: <input type="radio" name="major" value="kor">국어
		<input type="radio" name="major" value="eng">영어
		<input type="radio" name="major" value="math">수학
		<input type="radio" name="major" value="design">디자인<br>
		
		<select name="protocol">
		<option value="ftp">ftp</option>
		<option value="http">http</option>
		</select>
		
		<input type="submit" value="전송">
		<input type="reset" value="초기화">
		
	</form>

</body>
</html>
```
```jsp
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
		String uname, uid, upw, umajor, uprotocol;
		String[] uhobby;
	%>

	<%
		request.setCharacterEncoding("EUC-KR");
		uname = request.getParameter("name");
		uid = request.getParameter("id");
		upw = request.getParameter("pw");
		uhobby = request.getParameterValues("hobby");
		umajor = request.getParameter("major");
		uprotocol = request.getParameter("protocol");
		response.setContentType("text/html; charset=EUC-KR");
	%>
	
	이름: <%=uname%><br>
	아이디: <%=uid%><br>
	비밀번호: <%=upw%><br>
	취미: <%= Arrays.toString(uhobby) %> <br>
	전공: <%=umajor%><br>
	프로토콜: <%=uprotocol%>

</body>
</html>
```
<br>

### [주의] ▶ 한글처리할때 method를 get으로 보내면 깨지는 현상이 종종 발생(post는 깨지지 않음)

- 처리 방법은 encoder, decoder함수를 사용해 처리할 수 있음 
```jsp
<!--입력하는쪽(보내는쪽)-->
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<%@page import="java.net.URLEncoder"%>
 
<%
    String text = "홍길동" ;
 
    // 전송 문자 UTF-8 인코딩
    string encText = URLEncoder.encode(text, "UTF-8") ;
%>

<!--데이터 받는쪽-->
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<%@page import="java.net.URLDecoder"%>
 
<%
    // Get 전송 파라미터
    String rText = request.getParameter("text") ;
 
    // 문자 디코딩
    string text = URLDecoder.decode(text, "UTF-8") ;
%>
```
