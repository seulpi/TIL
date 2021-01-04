# 1. css 에서의 display 종류와 속성에 대하여 설명하시오
- **block :** 크기 지정이 가능하고 선언한 크기에 대해 공간을 차지함, 자동개행이 이루어짐! 
- **inline : 크기를 지정해도 속성이 적용되지 않는다**, 자동 개행이 이루어지지 않기 때문에 영역을 block처럼 차지하지 않음
- **inline-block :** inline요소 + block요소 , inline처럼 자동개행이 이루어지지 않지만 block처럼 크기 설정이 가능하다(크기를 설정한만큼 영역을 차지한다)
- **none :** 아예 화면에 출력하지 않는다 (보여주지 않는 것) 

# 2. px 과 em 의 차이는?

### - px : pixel 기준 사이즈를 말함, 웹에서의 기본 단위
	- 픽셀이란? 디스플레이를 구성하고 있는 최소 단위(가장 작게 찍을 수 있는 점)로 하나의 글꼴이 화면에 출력될 때 <br> 
	그 글꼴이 차지하는 화면 최소 구성 요소 (픽셀)의 수를 나타냄 (picture elemet의 합성 신조어) 
	- px값으로 폰트 크기를 지정하면 기본 크기(16px)는 무시되고 지정한 px 값으로 크기가 설정된다
	- 하나의 요소를 수정할 경우 어우러지는 나머지 요소의 값들도 전부 수정해야 하기 때문에 유지보수가 어려운 측면이 있다
	- 픽셀 단위로 값을 설정할 경우 클라이언트가 브라우저의 폰트 기본 값을 변경할 경우 적용되지 않아 사용성을 떨어트리기 때문에 경우에 따라 권장하지 않기도 함
	- 주로 컴퓨터 화면에서 사용
### - em : 상대적인 크기 단위 (장치에 따라 사이즈 **변경이 가능**), 부모 요소나 다른 요소의 크기에 영향을 받아 상대적으로 크기가 변함
	- 주로 웹문서에서 사용
	- 1em = 12pt = 16px = 100%
	- em : M의 크기를 뜻함
	- 상위 요소를 기준으로 해서  배수로** 크기를 정함(상대적)  → 그래서 사용하기 어렵기도 하다
	- 상대적인 비율 단위로 움직이기 때문에 px단위에 비해 유지 보수가 훨씬 쉽다. → 활용 빈도가 가장 높다

>> 절대단위(pt, cm, mm, in, pc) / 상대단위(px, em, rem, %)

# 3. inline-block 태그의 종류는?
< button > , < select >, < input >, < textarea > <br>
▶ 태그 종류(block,inline) 링크 참조
- https://www.w3schools.com/css/css_display_visibility.asp
- https://calmdawnstudio.tistory.com/51

# 4. opacity 속성의 사용법은?
: opacity - 불투명도 설정하는 속성
>> [문법] opacity : 값 (값의 허용 범위 0~1 / default값=1)

# 5. display:none 과 visibility:hidden 의 차이는?
- display:none → 화면 출력을 아예 하지 않는 것 (영역설정X)
- visibility:hidden → 화면 출력을 하지 않지만 영역은 설정하는 것 (빈공간으로 보이지만 사실상 공간차지O)

# 6. https://www.youtube.com/watch?v=O-n1EjDEuIc 시청하기

# 7. 동적문서와 정적문서의 차이는?
- 동적문서 : 사용자로부터 받은 정보를 토대로 **기능을 추가하여 출력하는 것**(html+기능) / jsp 
- 정적문서 : 사용자로부터 받은 정보를 웹서버가 그대로 출력하는 것 / html

# 8. 아래 JSP 태그에 대해 설명하시오
## 지시자 / 주석 / 선언 / 표현식 / 스크립트릿 / 액션태그 
- 지시자 : 페이지 속성 <%@   %>
- 주석 : <%  --> , // , /* */ 
- 선언 : <%!   %>
- 표현식(출력) : <%=   %>
- 스크립트릿(java코드) : <%   %>
- 액션태그 : < jsp:action > < /jsp:action >
	- 액션태그란 jsp페이지 내에서 어떤 동작을 하도록 지시하는 태그
	- 종류 : 
		1. forword : 현재 페이지를 다른 페이지로 전환할 때 사용
		2. param : forword 액션태그와 param을 이용해서 다른 페이지에 데이터를 전달할 수 있음
		3. include : jsp페이지 내에서 다른 페이지를 삽입하는 태그
		>> 지시자include와는 다름 - 지시자 include: jsp를 포함하여 java파일을 생성하지만 액션태그를 사용할 경우 실행 중에 동적으로 포함시킴 
		4. useBean : 자바빈을 JSP 페이지에서 사용할 때 사용
		5. setProperty : property의 값을 세팅할 때 사용 
		6. getProperty : property의 값을 얻어낼 때 사용 
---
# Quiz 

## 1. 아래를 Servlet으로 작성하시오 (1,2번 테이블 정렬이 안되는데..)
![q1](https://user-images.githubusercontent.com/74290204/103401786-b12e3700-4b8d-11eb-9b9e-727e415471d7.PNG)

```java
package ex1231;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class oneInput
 */
@WebServlet("/oneInput")
public class oneInput extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public oneInput() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
			request.setCharacterEncoding("UTF-8");
			/* response.setContentType("text/html;charset=EUC-KR"); */
			response.setContentType("text/html; charset=UTF-8");
			PrintWriter dan = response.getWriter();
			
			dan.println("<html><head><title></title></style></head>");
			dan.println("<body><h1>구구단 출력</h1>");
			
			for(int i = 2; i <10; i++) {
				dan.println("<table border='1'><tr>");
				for(int j =1; j <10; j++) {
					dan.println("<td border='1'>");
					dan.println( i + "*" + j + "=" + (i*j));
				}dan.println("<br></td>");
			}
			
			dan.println("</tr></table></body></html>");
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}
}
```
<br>

## 2. 1번 문제를 jsp로 작성하시오 
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
	
			<%
				for(int i = 2; i <10; i++) {
			%>      <table border="1"> // 행을 for문 돌릴때마다 하나씩 만들어야 되서 for문 안으로 들어와야함 
					<tr>
			<% 		
					for(int j = 1; j <10; j++) {
			%>
				<td border="1">
			<% 		
					out.println(i + "*" + j + "=" + (i*j));
					}out.println("<br></td>");
				}
			%> 
		</tr>
	</table>
</body>
</html>
```
<br>

## 3. 위의 프로그램을 아래의 순서로  작성하시오
```
1. include.jsp파일을 생성합니다.
- <h4> 태그에 '구구단 출력하기'를 작성합니다.
- include 액션 태그로 구구단을 출력하는 include_data.jsp 파일로 이동하도록 작성합니다.
- param 액션 태그로 숫자 5를 출력하는 include_data.jsp 파일에 전달하도록 작성합니다.

2. include_data.jsp 파일을 생성합니다.
- 전달받은 숫자 5의 구구단을 출력하도록 작성합니다.

3. 웹 브라우저에 http://localhost:8080/Exercise/ch04/include.jsp를 입력하여 실행 결과를 확인합니다
```
```jsp
// include.jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<h4>구구단 출력하기</h4>
	
	<jsp:include page="include_data.jsp" flush="false">
		<jsp:param value="5" name="dan"/>
	</jsp:include>

</body>
</html>
```
```jsp
// includ_data.jsp
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
	String dan = request.getParameter("dan");
	int danPrint = Integer.parseInt(dan);
	
	for(int i =1; i <10; i++) {
		out.println(danPrint + "*" + i + "=" + (danPrint*i) + "<br>");
	}

%>
</body>
</html>
```
<br>

## 4. jsp로 작성하시오
![3](https://user-images.githubusercontent.com/74290204/103455898-6bf83980-4d34-11eb-9cf1-c9eb58aee29d.PNG)
```jsp
// form04.jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<form action=formOut.jsp" method="get">
	이름: <input type="text" name="uname" size="10"><br>
	주소: <input type="text" name="uadress" size="10"><br>
	이메일: <input type="text" name="uemail" size="10"><br>
	전송: <input type="submit" value="전송">
	</form>
</body>
</html>
```
```jsp
// formOut.jsp
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
	String name = request.getParameter("uname");
	String ad = request.getParameter("uadress");
	String e = request.getParameter("uemail");
	%>
	
	<%=name %>
	<%=ad %>
	<%=e %>

</body>
</html>
```
<br>

## 5. jsp로 작성하시오
![4](https://user-images.githubusercontent.com/74290204/103455928-ac57b780-4d34-11eb-8d8a-5fde6aa83d5c.PNG)
```jsp
//form05.jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<form action="formOut.jsp" method="get">
	오렌지<input type="checkbox" name="fruit" value="오렌지">
	사과<input type="checkbox" name="fruit" value="사과">
	바나나<input type="checkbox" name="fruit" value="바나나">
	<input type="submit" value="전송">
	</form>
</body>
</html>
```
```jsp
//formOut.jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<h4>선택한 과일</h4>
	<%
	String[] fruit = request.getParameterValues("fruit");
	for(String f:fruit) {
		out.println(f + "<br>");
	}
	%>
</body>
</html>
```
<br>

## 6. jsp로 작성하시오
![5](https://user-images.githubusercontent.com/74290204/103455929-af52a800-4d34-11eb-95e1-fcca67edef93.PNG)
```jsp
<%@page import="java.text.SimpleDateFormat"%>
<%@page import="java.util.Calendar"%>
<%@page import="java.util.Date"%>
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<h1>몇시 일까요??</h1>
	<% 
		// 현재시간 구하는 방법 1. Date 객체
		Date date = new Date();
		out.println(date);
	%>
	
	<% 
		// 현재시간 구하는 방법 2. Calendar getInstance()활용
		Calendar date = Calendar.getInstance();
		out.println(date);
		// 필요한 정보만 얻고싶다면 int year = now.get(Calnedar.YEAR); 이런식으로도 가능
	%>
	
	<% 
		// 현재시간 구하는 방법 3. SimpleDateFormat 활용(생성자의 매개변수가 필요)
		SimpleDateFormat now = new SimpleDateFormat("yyyy / MM / dd / HH:mm:ss");
		String date = date.format(cal.getTime());
		out.println(date);
	%>
</body>
</html>
```
<br>
▶ 참조한 링크 https://m.blog.naver.com/PostView.nhn?blogId=kj930519&logNo=221440967773&proxyReferer=https:%2F%2Fwww.google.com%2F
