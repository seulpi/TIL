# Quiz

## 1. 
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body> 
<!-- http://localhost:8282/Answer/1230/A01?dan=6 -->
	<form action="A01" method="get">
	출력한 구구단 단수 입력: <input type="text" name="num" size="10">
	<input type="submit" value="실행">
	</form>
</body>
</html>
```
```java
package ex1230;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class A01
 */
@WebServlet("/1230/A01")
public class A01 extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public A01() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("EUC-KR");
		int dan = Integer.parseInt(request.getParameter("num"));
		
		response.setContentType("text/html;charset=EUC-KR");
		// 함수넣는것 주의!response.setCharacterEncoding("text/html;charset=EUC-KR"); 이렇게 나오면 문자 출력이 안됨
		
		PrintWriter write = response.getWriter();
		write.println("<html><head><tltle>");
		write.println("</title></head><body>");
		write.println("<table border=\"1\">");
		write.println("<tr>");
		write.println("<td>"+ dan + "단</td>");
		write.println("</tr>");
		for(int i= 1; i <10; i++) {
			// for문 돌아가면서 tr,td가 같이 생기면서 칸이 생기는것 for문 밖에 있으면 tr td 한번생겨서 한칸에 값을 다 넣는것
			write.println("<tr>");
			write.print("<td>" + dan + "*" + i + "=" + (dan*i) +"<br/>");
			write.println("</td></tr>");
		}
		
		write.println("</table>");
		write.println("<form action='A01' method='get'>");
		write.println("<input type='submit' value='돌아가기'>");
		// 돌아가기해서 주소 리턴이 안됨
		write.println("</form></body></html>");//
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);//
	}
}
```
### ▶ 출력 결과 
![1](https://user-images.githubusercontent.com/74290204/103345700-74066e00-4ad5-11eb-9462-c72602a0218c.PNG)
<br>

## 2. 아래를 프로그래밍 하시오
### - 가로 세로를 입력받는 페이지를 만든 후 서버로 전송하면 <br> 사각형의 넓이를 전송
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<form action="rec02" method="get">
	사각형의 가로: <input type = "text" name="weight" size="5">
	사각형의 세로: <input type = "text" name="height" size="5">
	<input type="submit" value="넓이 출력">
	</form>
</body>
</html>
```
```java
package ex1230;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class rec02
 */
@WebServlet("/1230/rec02")
public class rec02 extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public rec02() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("EUC-KR");
		
		double weightP = Double.parseDouble(request.getParameter("weight"));
		double heightP = Double.parseDouble(request.getParameter("height"));
		
		double area = weightP*heightP;
		
		response.setContentType("text/html; charset=EUC-KR"); // 이거 없으면 문자 출력이 안됨!
		
		PrintWriter w = response.getWriter();
		w.println("<html><head><title></title></head><body>");
		w.println("넓이: " + area);
		w.println("</body><html>");
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
### ▶ 출력 결과 
![Q2](https://user-images.githubusercontent.com/74290204/103342485-ec1c6600-4acc-11eb-97b9-6afc0b7bcbcc.PNG)

## 3. 아래를 짜시오 (글씨 hidden으로 맞춰놔서 다 안나옴)
![3](https://user-images.githubusercontent.com/74290204/103342565-284fc680-4acd-11eb-95d5-524ccc52bacb.PNG)

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>

	<style>
		#header, #middle, #footer {
			width: 506px;
			margin: auto;
			border-radius: 10px 10px 10px 10px;
			background-color: gray;
			overflow: hidden; 
		}
		
		.one0 {
			width: 100px;
			height: 100px;
			border: 1px solid red;
			border-radius: 10px 0 10px 10px; /*0이 둥글게 되는게 아니라 값을 준만큼 둥글해지는것 */
			float: left;
		}
		
		.one1 {
			width: 300px;
			height: 100px;
			border: 1px solid red;
			border-radius: 0 0 10px 10px;
			float: left;
		}
		
		.one2 {
			width: 100px;
			height: 100px;
			border: 1px solid red;
			border-radius: 0 10px 10px 10px;
			float: right;
		}
		
		.one3 {
			width: 100px;
			height: 100px;
			border: 1px solid red;
			border-radius: 10px 10px 10px 10px;
			float: left;
		}
		
		.one4 {
			width: 300px;
			height: 100px;
			border: 1px solid red;
			border-radius: 10px 10px 10px 10px;
			float: left;
			
		}
		
		.one5 {
			width: 100px;
			height: 100px;
			border: 1px solid red;
			border-radius:10px 10px 10px 10px;
			float: right;
		}
		
		.one6 {
			width: 100px;
			height: 100px;
			border: 1px solid red;
			border-radius: 10px 10px 0 10px;
			float: left;
		}
		
		.one7 {
			width: 300px;
			height: 100px;
			border: 1px solid red;
			border-radius: 10px 10px 0 0;
			float: left;
		}
		
		.one8 {
			width: 100px;
			height: 100px;
			border: 1px solid red;
			border-radius: 10px 10px 10px 0;
			float: right;
		}
		ul li {
			list-style: none;
			float: left;
			margin-left: 10px;
			font-size: 16px;
			font-weight: bold;
		}
	
	</style>
</head>
<body>
	<div id="header">
		<div class="one0"></div>
		<div class="one1"></div>
		<div class="one2"></div>
	</div>
	
	<div id="middle">
		<div class="one3"></div>
		<div class="one4">
			<ul>
				<li>menu1</li>
				<li>menu2</li>
				<li>menu3</li>
				<li>menu4</li>
			</ul><br/>
			<h1>하이서울브랜드</h1>
			<p>서울시와 서울산업진흥원(SBA)이 공동으로 지원하는 하이 서울브랜드 사업은 우수한
			기술력과 상품력을 보유하고 있으나 고유브랜드 육성에 어려움을 겪고 있는 우수기업들이 서울시가
			인정한 중소기업 공동브랜드인 하이서울브랜드를 활용하여 제품 경쟁력을 강화할 수 있도록
			각종 홍보 마케팅을 지원하는 사업입니다.</p>
		</div>
			
		<div class="one5"></div>
	</div>
	
	<div id="footer">
		<div class="one6"></div>
		<div class="one7"></div>
		<div class="one8"></div>
	</div>

</body>
</html>
```
<br>

## 4. 아래를 프로그래밍하시오
```
<별찍기!>
1. input박스에 숫자를 하나 넣어 서버로 전송한다
2. 응답으로 별을 찍어준다
ex) input box에 5를 입력 후 전송 → 아래와 같이 석탑을 찍어 클라이언트에 전송
*
**
***
*****
```
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<form action="star" method="get">
	
		만들고 싶은 별의 층 수 입력 : <input type="text" name="star" size="5">
		<input type="submit" value="전송">
	
	</form>
</body>
</html>
```
```java
package ex1230;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class star
 */
@WebServlet("/1230/star")
public class star extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public star() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("EUC-KR");
		int s = Integer.parseInt(request.getParameter("star"));
		
		response.setContentType("EUC-KR");
		PrintWriter w = response.getWriter();
		w.println("<html><head><tltle></title></head><body>");
		
		for(int i = 0; i < s; i++) {
			for(int j=0; j<=i; j++) {
				w.println("*"); // 왜 print 하면 옆으로 나열되는지????
			}w.println("<br/>"); // 여기서는 println은 개행 목적이 없는건가?
		}
		w.println("</body></html>");
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
### ▶ 출력 결과 
![4](https://user-images.githubusercontent.com/74290204/103348737-4245d500-4ade-11eb-8b1a-6313eaf42011.PNG)

