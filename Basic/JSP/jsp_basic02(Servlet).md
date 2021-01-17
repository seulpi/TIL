# Servlet : 자바에서 제공하는 http프로토콜을 지원하는 라이브러리
- MVC 모델 : Model View Controller
- HttpServlet을 상속받는 모든 클래스를 servlet이라 한다
- http를 쉽게 사용하기 위한 캡슐화시킨 라이브러리(클래스들의 집합)
- 자바1.5 버전부터는 @Annotation이 제공됨
    ```html
    <servlet-name>hello</servlet-name> 
    <servlet-class>edu.bit.ex.HelloWorld</servlet-class> class
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hw</url-pattern>   
    </servlet-mapping>
    따라서 위에 코드를 @WebServlet("/HWorld") 이 한줄로 대체 시켜줌
    ```
- URL mapping 변경 가능 
>> mapping : 간단하게 경로를 표현해주는 것
- web.xml은 톰캣이 웹서비스를 읽어들이기 전에 먼저 읽고 설정하는 파일
```html
<servlet-name>hello</servlet-name> 은 이름이 달라도되지만 
<servlet-class>edu.bit.ex.HelloWorld</servlet-class> class이름은 똑같아야함 (대소문자까지)
```
```html
<servlet-name>hello</servlet-name> 
<servlet-class>edu.bit.ex.HelloWorld</servlet-class> class
▶ HelloWorld hello = new HelloWorld(); 객체생성하는것과 같음!

  <servlet-mapping>
     <servlet-name>hello</servlet-name>
     <url-pattern>/hw</url-pattern>   
  </servlet-mapping>
: url이 /hw 치고들어오면 hello를 실행시켜라 
서버프로그램은 자바방식을 기반으로 더해진 새로운 프로그램이기 때문에 자바방식으로 접근하면 안됨!!!
``` 
<br>

## @ Servlet 작동 순서 
: 웹브라우저 → 웹서버 → 웹어플리케이션서버 → Servlet컨테이너 (1.thread 생성 2. Servlet객체 생성) 
<br>

## @ Servlet Life Cycle(생명주기: 하나의 클래스 단위)
![servlet 생명주기](https://user-images.githubusercontent.com/74290204/103363063-91e7c900-4afd-11eb-880b-9546fe1811af.png)

▶ 이미 메모리에 올라가 있는 상태기 때문에 새로 올릴필요가 X 따라서, Servlet객체 생성 & Init()은 한번만 호출한다 <br>
request가 한명한테만 오는게 아닌 다수의 request이기 때문에 할때마다 호출하면 비용이 비싸짐

- destoy()는 어플리케이션이 꺼질 때 한 번 호출 (마무리지어야할 때) / Servlet이 수정되거나 서버를 재가동하는 경우 호출한다 

- 선처리 : @PostConstruct 
    - 객체의 초기화 부분 
    - 객체가 생성된 후 별도의 초기화 작업을 위해 실행하는 메소드를 선언한다
    - @PostConstruct Annotation을 설정해 놓은 init메소드는 WAS가 띄어질 때 실행된다 
    >> init 메소드가 호출되기 전에 실행되길 원하는 메소드는 @PostConstruct 라는 Annotation을 사용

- 후처리: @PreDestroy
    - 마지막 소멸단계
    - 객체를 제거하기 전에 해야할 작업이 있다면 메소드 위에 사용하는 Annotation
    - close()하기 직전에 실행 → ((AbstractApplicationContext) context).close()
    >> destroy()가 호출된 후에 실행되길 원하는 메소드는 @PreDestroy라는 Annotation을 사용 
<br>

## @ServletContextListener : 프로젝트 단위별 생명 주기 감시
- 프로그램이 죽기전에 반드시 해야할 일이 있을 때 사용 
- destroy 서버를 강제로 끊으면 contextInitialized 작동안됨

## @ HttpServletRequest request / HttpServletResponse response
: 클라이언트가  requst한것을 response해주려면 클라이언트의 주소와 정보를 알아야하는데 웹브라우저에서 자동으로 프로토콜로 만들어 전달해줌 <br> 그러면 이걸 받기위해서는 웹서버가 정보를 저장할 내용을 가지고 있어야하는데 그게 바로 HttpServletRequest request, HttpServletResponse response
```java
System.out.println("HelloWorld~~"); 콘솔에서 출력
```
```html
response.setContentType("text/html; charset=euc-kr");
<!--response니까 다시 돌려주는것 (클라이언트의 IP로)-->
PrintWriter writer = response.getWriter();
		
writer.print("<html>");
writer.print("<head>");
writer.print("</head>");
writer.print("<body>");
writer.print("<h1>post방식입니다</h1>");
writer.print("</body>");
writer.print("<html>");
		
writer.close();
```
<br>

## @ Servlet Parameter
: 클라이언트의 정보들을 톰캣 서버가 전달 받으면 request로 정리하는데 객체이기 때문에 데이터멤버와 함수를 가진다
<br>

- response객체는 클라이언트에게 주기위한 객체 
- request객체는 클라이언트의 정보를 담는 객체 

▶ getParameter로 갖고 오는 정보는 인터넷을 타고 들어오기 때문에 숫자를 집어넣더라도 무조건 String으로 들어오게됨(인터넷에서는 타입 그런거 없음/int 그런거 없음) <br>
checkbox같은거는 여러개를 받아야하기 때문에 배열로 들어오게 된다(getParameterValues함수를 사용)

```html
<!-- formex.html -->
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<form action="FormEx" method="post">
		이름 : <input type="text" name="name" size="10"><br/>
		아이디 : <input type="text" name="id" size="10"><br/>
		비밀번호 : <input type="password" name="pw" size="10"><br/>
		취미 : <input type="checkbox" name="hobby" value="read">독서
		<input type="checkbox" name="hobby" value="cook">요리
		<input type="checkbox" name="hobby" value="run">조깅
		<input type="checkbox" name="hobby" value="swim">수영
		<input type="checkbox" name="hobby" value="sleep">취침<br/>
		<input type="radio" name="major" value="kor">국어
		<input type="radio" name="major" value="eng" checked="checked">영어
		<input type="radio" name="major" value="mat" >수학
		<input type="radio" name="major" value="des" >디자인<br/>
		<select name="protocol">
			<option value="http">http</option>
			<option value="ftp" selected="selected">ftp</option>
			<option value="smtp">smtp</option>
			<option value="pop">pop</option>
		</select><br/>
		<input type="submit" value="전송"><input type="reset" value="초기화">
	</form>
</body>
</html>
```
```java
// FormEx.java
package com.javalec.ex;

import java.io.IOException;
import java.io.PrintWriter;
import java.util.Arrays;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class FormEx
 */
@WebServlet("/FormEx")
public class FormEx extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public FormEx() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		System.out.println("doGet");
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		System.out.println("doPost");
		
		String id = request.getParameter("id");
		String pw = request.getParameter("pw");
		
		String[] hobbys = request.getParameterValues("hobby");
		String major = request.getParameter("major");
		String protocol = request.getParameter("protocol");
		
		response.setContentType("text/html; charset=EUC-KR");
		PrintWriter writer = response.getWriter();
		
		writer.println("<html><head></head><body>");
		writer.println("아이디 : " + id + "<br />");
		writer.println("비밀번호 : " + pw + "<br />" );
		writer.println("취미 : " + Arrays.toString(hobbys) + "<br />");
		writer.println("전공 : " + major + "<br />");
		writer.println("프로토콜 : " + protocol);
		writer.println("</body></html>");	
	}
}
```
<br>

## @ ServletConfig / ServletContext
- ServletConfig : Servlet 초기화 파라미터  
- ServletContext : 데이터 공유 (여러개의 Servlet을 파라미터를 공유해서 사용할 수 있음)

## @한글처리 
: 한글 인코딩 방식은 크게 2가지 "UTF-8" , "EUC-KR"
<details markdown="1">
<summary>▶ 한글 인코딩 방식 설명 click! </summary>
- UTF-8 : 유니코드 / 조합형(문자를 조합해서 출력) ' ㅇ ㅏ ㄴ ㅕ ㅇ '
- EUC-KR : 한글&기호만 가능(다른 언어 불가능) / 완성형(문자를 완성해서 출력) '안 녕'
>> 웹에서는 보통 UTF-8을 주로 사용 (세게적인 유니코드이니까)
</details>

1. 서버 활용 (왠만하면 서버 건드리지 않는게 최선^_____^!!! ※dangerous※) <br>
    : sever.xml 수정(톰캣) [웹로직이나 제우스는 세팅 방법이 다를 수 있음]
```java
<Connector connectionTimeout="20000" port="8282" protocol="HTTP/1.1" redirectPort="8443"/>
↑ 여기에 URIEncoding="EUC-KR" 삽입

▶ <Connector connectionTimeout="20000" URIEncoding="EUC-KR" port="8282" protocol="HTTP/1.1" redirectPort="8443"/>
```

2. request 객체의 setCharactrerEncoding() 메소드 이용 
```java
request.setCharacterEncoding("EUC-KR");
```
