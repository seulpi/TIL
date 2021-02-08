# 1. 선택자(Seletor)란?
## : style을 주기 위해 html안에 요소들을 선택하는 것 
<br>

# 2. CSS 문법은?
>> <style> <br> 선택자 { 속성명: 속성값; } <br> </style>
```html 
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
    <style> 
        font-size: 20px; <!-- 적용시킬style 구현 부분-->
    </style>
</head>
<body>

</body>
</html>
```
<br>

# 3. 시멘틱 태그에 대하여 설명하고, 그 종류는?
: 레이아웃을 구성하는 태그 종류로는 div / span이 존재하는데 구분짓는 요소는 block과 inline의 차이 
- block 태그 : 자동으로 개행이 일어남 → **div**
- inline 태그(unblock): 자동으로 개행이 일어나지 X → **span**

▶ 개발자들이 div를 많이 사용하다보니 발전되서 나온 게 시멘틱태그(가장 upgrade된 게 오디오와 비디오 삽입) 
>> 시멘틱태그 : div + 의미 <br> 문서 구조를 태그 이름만 봐도 쉽게 이해 할 수 있도록 해 놓은것

![시멘틱태그](https://user-images.githubusercontent.com/74290204/103324602-9378a900-4a8b-11eb-91cb-4e03301d6ad3.png)

☞ 더 많은 종류 참고
https://thrillfighter.tistory.com/492
<br>

# 4. bootstrap 에 대하여 설명하시오
## 웹사이트를 쉽게 만들 수 있게 도와주는 HTML, CSS, JS프레임워크이다
< 다양한 기능을 제공하여 쉽게 웹사이트를 제작, 유지, 보수할 수 있도록 도와준다>
- 무료로 사용가능한 오픈소스

☞ bootstrap 링크 https://getbootstrap.com/
<br>

# 5. overfolw 에 대하여 설명하시오
: CSS 특정요소의 자식요소가 부모요소의 범위를 초과 할 때 이러한 상황을 어떻게 처리 할지를 결정할 수 있는 속성
- overflow 의 기본값은 visible
- overflow 속성은 가로부분과 세로부분 모두에 일괄적으로 적용되는 속성값
- overflow의 속성 값으로는 visible, hidden, scroll, auto 등을 사용한다
```
1. visible
특정요소가 부모의 범위를 넘어가더라도, 그대로 보여준다

2. hidden
부모요소의 범위를 넘어가는 자식요소의 부분은 보이지 않도록 처리한다

3. scroll
부모요소의 범위를 넘어가는 자식요소의 대부분은 보이지 않지만, 
그 대신 사용자가 확인할 수 있도록 스크롤 바를 표시하여 확인할 수 있게 한다

4. auto
부모요소의 범위를 넘어가는 자식요소의 부분이 있을경우 해당 부분은 보이지 않도록 처리하고, 
사용자가 해당 부분을 확인 할 수 있도록 스크롤 바를 표시하여준다

5. overflow-x / overflow-y
가로부분 overflow-x / 세로 부분 overflow-y

#parent { overflow-x : hidden; overflow-y : visible } 
▶ 가로부분은 감추고 세로부분은 보여줘라란 뜻 
```
<br>

# 6. class 와 id 선택자의 차이와 어떨때 사용하는가?
본질적인 기능은 같으나 중복에서 차이가 있다
- id : 중복 사용 불가 (한 페이지에 하나의 정의로 하나의 태그만 가능) : java HashMap에서 key가 중복이 안되는것처럼!
>> 로고, 상단메뉴, 하단정보같은 스타일 등
- class : 중복 사용 가능 (한 페이지에 반복적으로 사용되는 스타일 가능)

▶ 여러요소의 스타일을 원하면 class / 한 요소만 사용하고 싶다면 id
<br>

# 7. servlet의 생명주기에 대하여 설명하시오
클라이언트가 요청하면 Servlet이 바로 호출되는 게 아니라 객체를 생성하고 초기화 작업을 거친 후,
요청을 처리하는 생명주기를 갖는다 
```
1. 요청이 오면 Servlet 클래스가 로딩되어 요청에 대한 Servlet 객체가 생성
2. 서버는 init() 메소드를 호출해서 Servlet을  초기화한다
3. service() 메소드를 호출해서 Servlet이 브라우저의 요청을 처리하도록 한다
4. service() 메소드는 특정 HTTP 요청(GET, POST 등)을 처리하는 메서드 (doGet(), doPost() 등)를 호출
5. 서버는 destroy() 메소드를 호출하여 Servlet을 제거


Servlet 객체를 생성하고 초기화하는 작업은 비용이 많이 들기 때문에 
톰캣은 다음에 또 요청이 올 때를 대비하여 이미 생성된 Servlet 객체는 메모리에 남겨두고 
톰캣이 종료되기 전이나 reload 전에 모든 Servlet을 제거한다
```
<br>

---
# Quiz.

## 1.

![Q1](https://user-images.githubusercontent.com/74290204/103273133-f02f8180-4a01-11eb-9b70-62dff53e70c9.PNG)

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
	<style>
		#header {
			width:500px;
			background:#FFBF00;
			text-align: center;
			border: 2px solid gray;
			
			
		}
		#wrap {
			width:500px;
			height: 300px;
			background:#2ECCFA;
			border: 2px solid gray;
			
		}
		
		#contents {
			width:250px;
			height:300px;
			float:left;
			text-align: center;
			border: 1px solid red;
		
		}
	
		#banner {
			width:245px;
			float:Right;
			text-align: center;
			border: 1px solid red;
		
		}
		
		#footer {
			width:500px;
			background:#FFBF00;
			text-align: center;
			border: 2px solid gray;
		}
		
		li.menu1 {
			font-size: 16pt;
			color: blue;
		}
		
		li.menu3, li.menu5 {
			font-size: 16pt;
		}
	
	</style>

</head>
<body>
	<div id= "header">
		<h1>HEADER</h1>
	</div>
	
	<div id = "wrap" > 
		<div id = "contents">
			<h1>CONTENTS</h1>
				<ul>
					<li class="menu1">menu1</li>
					<li class="menu2">menu2</li>
					<li class="menu3">menu3</li>
					<li class="menu4">menu4</li>
					<li class="menu5">menu5</li>
				</ul>
		</div>
	
		<div id="banner">
			<h1>BANNER</h1>
			<a href="http://www.sba.seoul.kr" target="_blank"><img src="http://www.sba.seoul.kr/kr/images/footer/f_logo.png"></a> 
		</div>
	</div>
	
	<div id="footer">
		<h1>FOOTER</h1>
	</div>
</body>
</html>
```

## 2. 필수!

![2-1](https://user-images.githubusercontent.com/74290204/103277578-b9129d80-4a0c-11eb-8cc3-16e0e4ead779.PNG)

![2-2](https://user-images.githubusercontent.com/74290204/103277580-ba43ca80-4a0c-11eb-9285-d8a89155501c.PNG)

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<form action ="../Formexex" method=post>
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
```java
//Servlet.java
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Arrays;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class Formexex
 */
@WebServlet("/Formexex")
public class Formexex extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public Formexex() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		response.getWriter().append("Served at: ").append(request.getContextPath());
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		String name = request.getParameter("uname");
		String id = request.getParameter("uid");
		String password = request.getParameter("pw");
		String[] hobby = request.getParameterValues("hobby");
		String major = request.getParameter("major");
		String protocol = request.getParameter("protocol");
		
		response.setContentType("text/html; charset=EUC-KR");
		
		PrintWriter write =  response.getWriter();
		
		write.println("<html>");
		write.println("<head>");
		write.println("</head>");
		write.println("<body>");
		write.println("아이디: " + id + "<br>");
		write.println("비밀번호: " + password + "<br>");
		write.println("취미: " + Arrays.toString(hobby) + "<br>");
		write.println("전공: " + major + "<br>");
		write.println("프로토콜: " + protocol + "<br>");
		write.println("</body>");
		write.println("</html>");
	}
}
```
