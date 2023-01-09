# - 쿠키(Cookie) 
- http프로토콜은 서버쪽과 **연결된 상태에서** 정보를 주고받음 (그래야 전송이 가능!)
- 웹브라우저에서 요청(request)하면 서버에서 **응답(response)하면 서버는 웹브라우저와의 관계를 종료시킨다(연결끊음: 소켓을 끊어버린다)** <br> 
    why → 서버 한대에 클라이언트는 몇천만명인데 다 연결하게 두면 서버 다운되니까 끊을 수 밖에 없음 

1. 쿠키와 세션이 나오게 된 배경 <br>
: html이나 채팅 프로그램이나 프로그래밍하는 방법은 완.똑! **차이가 있다면 연결성의 차이가 존재** 
``` 
채팅 : 채팅을 안한다고 해서 연결이 끊기지 않음, 계속 살아있음 (핸드폰 전화통화도 마찬가지 이치)
인터넷 :  서버 1  = ∞ (서버 die)

▶ 그렇다보니 response를 받고나서 계속 유지되야 할 필요성이 존재 (ex. 로그인) 
따라서, 나온 게 Cookie & Session
```
2. 정의 : 쿠키와 섹션은 연결을 유지시켜주기 위한 수단이다

3. 용량 : 4kb , 300개까지 정보 저장 가능하다

4. 저장 공간 : 서버가 아닌 클라이언트측에 특정 정보를 저장한다 (key value형태로 웹브라우저에 저장)
    - 웹브라우저 안에는 2개의 영역이 존재 
    1. 쿠키를 담을 영역 (서버 이름별로 쿠키를 저장한다)
    2. 캐시 공간(캐시는 웹브라우저마다 존재 , 웹브라우저 자체가 성능을 위해 만드는 공간)
    <details><summary>성능을 위해 만든 공간에 대한 의미 click!</summary>
	
    - 웹브라우저에 접속하면 웹페이지에 display된 이미지같은 것들이 있어야 불러오는데 
     <br> 접속할때마다 불러오기에는 생각보다 이미지의 크기가 크기 때문에 성능이 떨어지게됨
     <br> 따라서 저장 공간에 담고 있으면 다음에 접속했을 때 빠르게 접근할 수 있기 때문에 공간에 저장하는 것  </details>

5. 문법 
```jsp
Cookie cookie = new Cookie("name", "value"); // 객체생성
cookie.setMaxAge(60); // 속성 설정 
response.addCookie(cookie); // response 객체의 쿠키 저장
```
```jsp
<!--cookietest.jsp-->
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	
	<form action="loginOk.jsp" method="post">
		아이디 : <input type="text" name="id" size="10"><br>
		비밀번호 : <input type="password" name="pw" size="10"><br>
		<input type="submit" value="로그인">
	</form>
</body>
</html>


<!--loginOk.jsp-->
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
		
		if(id.equals("abcde") && pw.equals("12345")) {
			Cookie cookie = new Cookie("id", id);
			cookie.setMaxAge(60);
			response.addCookie(cookie); 
			//여기까지 쿠키가 서버에서 저장되어있고 클라이언트에게 redirect하기 전
			response.sendRedirect("welcome.jsp");
			//redirect를 해야 클라이언트 쿠키에 저장됨
		}
		
		else {
			response.sendRedirect("cookietest.jsp");
		}
	%>
</body>
</html>

<!--welcome.jsp-->
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
		Cookie[] cookies = request.getCookies(); 
		
		for(int i = 0; i <cookies.length; i++) {
			String id = cookies[i].getValue();
			if(id.equals("abcde")) {
				out.println(id + "님 안녕하세요." + "<br>");
			} // 클라이언트쪽에서 들어오는 모든 쿠키를 확인
		}
	%>
	<a href ="logout.jsp">로그아웃</a>
</body>
</html>

<!--logout.jsp-->
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
		Cookie[] cookies = request.getCookies();
	
		if(cookies != null) {
			for(int i =0; i <cookies.length; i++) {
				if(cookies[i].getValue().equals("abcde")) {
					cookies[i].setMaxAge(0);
					response.addCookie(cookies[i]);
				}
			} 
		}
		
		response.sendRedirect("cookietest.jsp");
	%>
</body>
</html>
```

6. 검증헤더 : 캐시가 만료된 후에도 서버에서 데이터를 변경X -> 저장해 두었던 캐시 사용이 더 효율적 -> 이 과정에서 **클라이언트 데이터와 서버의 데이터가 같다는것을 확인할 방법이 필요 -> 검증헤더를 이용하자!** <br>
- ① Last-Modified : 데이터가 최종적으로 마지막에 수정된 시간
	>> If-Modified-Since : 웹브라우저가 서버로부터 마지막으로 최종 업데이트라고 받은 최종 시간 (이후에 데이터가 수정되었는지 물어보는 것) <br>If-Unmodified-Since
	```text
		HTTP/1.1 200 OK
		Content-Type: image/jpeg
		cache-control: max-age=60
		Last-Modified: Monday, Jan 09 2023 11:28:39
		Content-Length: 34012
		
		HTTP Body...
		-------------------------
		[지정된 캐시 유효시간 만료후 요청]
		GET /star.jpg
		if-midified-since: Monday, Jan 09 2023 11:28:39
		
		-> if-modified-since로 수정여부 체크가 가능 
		-------------------------
		[수정이 안된 경우 서버 -> 웹브라우저]
		HTTP/1.1 304 Not Modified	// 304: 실패, 200: 성공(수정되었는지 물어본건데 아니니까 304를 보내고 수정되었다면 200을 보냄)
		Content-Type: image/jpeg
		cache-control: max-age=60
		Last-Modified: Monday, Jan 09 2023 11:28:39
		Content-Length: 64012
		
		-> HTTP Body X -> ★수정된게 없기 때문에 바디를 빼고 헤더만 만들어서 전송★ -> 			네트 워크 부하 ↓
	```
	- 1초 미만 단위로 캐시 조정 X
	- 날짜 기반의 로직 사용 <br> ( a-> b -> a  수정 => 결국 데이터 수정 X , 단 날짜는 변경되었음 => 실제 파일의 변경 날짜는 달라졌지만 데이터 변경은 없음 이 경우 날짜가 변경되었기 때문에 수정되었다고 인식되어 다시 다운로드 받게 되는 현상이 일어남 )

- ② ETag(Entity Tag) : Last-Modified의 단점을 보완
	>> If-None-Match , If-Match
	```text
		HTTP/1.1 200 OK
		Content-Type: image/jpeg
		cache-control: max-age=60
		ETag: "aaaa"
		Content-Length: 34012
		
		HTTP Body...
		-------------------------
		[캐시 시간 초과]
		GET /star.jpg
		If-None-Math: "aaaa"	
	```
	- 캐시용 데이터에 날짜가 아닌 임의의 고유한 버전 이름 사용 O
		```text
			[EX]
			Etag:"v1.0", Etag:"a2jiodskj3"
		```
	- 데이터가 변경되면 이 이름을 바꾸어서 변경함 (Hash를 다시 생성)
		>> Hash 라이브러리는 파일을 넣었을때 동일한 컨텐츠면 똑같은 Hah값이 나옴, 파일의 컨텐츠가 조금이라도 다르면 완전 다른 Hash값이 나옴
		```text
			[EX]
			Etag: "aaaaa" -> Etsg:"bbbbb"
		```
7. 프록시 캐시 
8. 캐시 무효화 (확실한 캐시 무효화 응답 = 아래 하위 4가지를 다 사용하는것)
	>> Cache-contrl: no-cache, no-store, must-revalidate <br> Pragma: no-cache
	- Cache-Control: no-cache -> 데이터는 캐시가능, **단, 항상 원서버에 검증하고사용**
	- Cache-Control: no-store -> 데이터에 민감한 정보는 저장하면 안되기때문에 메모리에 사용하고 최대한 빨리 삭제
	- Cache-Control: must-revalidate -> 캐시 만료 후 최초 조회 원서버에 검증해야함, 원서버 접근 실패 시 반드시 오류발생(504: Gateway Timeout) 
	- Pragma: no-cache -> HTTP 1.0하위 호환



# - 세션(Session) : 내장 객체
- 웹브라우저와 서버를 연결시키기 위한 하나의 수단 
- session은 서버상에 객체로 저장(클라이언트쪽에 저장하는 게 X)
- 대표적인 EX : 자동 로그인, 장바구니 등

## @ 특징
1. 서버에만 존재하고 session ID 값만 클라이언트에게 넘겨주기 때문에 Cookie보다 보완이 우수함
2. 저장할 수 있는 데이터 한계 X 
    - 그렇다고 자주 쓰는 건 좋은 접근이 아님 → why? 메모리를 많이 잡아먹으니까 
3. 서버는 session 객체를 클라이언트마다 자동으로 생성 (클라이어트 당 1개)

## @ session 메소드()
```
- setAttribute() : session에 데이터를 저장(=메모리에 올린다)
                   setAttribute를 활용하면 addCookie처럼 데이터를 따로 저장할 필요가 없다 
- getAttribute() : session에 저장되어있는 데이터를 얻는다
- getAttributeNames() : session에 저장되어 있는 모든 데이터의 이름(고유의 key값)을 얻는다
- ★getID() : 자동 생성된 session의 고유의 ID를 얻는다
- isNew : session이 최초로 생성된건지 이전에 생성된것인지를 구분
- getMaxInactiveInterval() : session의 유효시간 / 가장 최근 요청시점을 기준으로 카운트(접근할때마다 유효시간이 바뀜)
- removeAttribute() : session의 특정 데이터를 제거 
- Invaildate() : session의 모든 데이터를 삭제 (싹 밀어버리는 것)
```

### ★ session.getID() : 이 친구의 원리를 이해하는 게 웹프로그래밍의 핵심
- 로그인 여부를 떠나서 클라이언트가 접속하면 웹서버에서는 session이라는 공간에 접속하는 사람마다 session ID라는 것이 존재
- session.getID는 클라이언트를 구분하고 , ID 발급 기준은 **웹브라우저당 1개씩 ID를 할당한다** 
- ID의 저장시간의 유효시간은 30분을 기준으로 하고 접근할때마다 유효시간이 다시 카운트된다
    >> 시간이 지나면 자동으로 저장된 값 초기화(=소멸)
- 클라이언트에게 Cookie를 담아서 클라이언트쪽에 똑같은 session ID를 전달해서 구분하는 원리 <br>
(다시 접근할 때 서버는 session ID로 '아 저번에 온 친구'로 인식해서 알 수 있는 것)
- session ID는 Cookie와 session을 동시에 가져다 쓴다


#### ▶ Q. 로그인 했을 때 같은 브라우저에서는 로그인이 유지되고 <br> 다른 브라우저에서는 왜 로그인이 유지되지 않는 지 설명하시오 
: session ID는 웹브라우저당 1개씩 할당하기 떄문인데 같은 브라우저안에서는 유효시간까지 session ID가 있어 로그인이 유지되지만<br> 새로운 브라우저에는 session ID가 다르기 때문에 다시 로그인해야하는 것 <br>

>>[참고] 쿠키와 세션 확인 방법: 크롬 F12 - 네트워크 -F5(새로고침)

```html
<!--login2.html-->
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<form action="loginOk2" method="post">
		아이디: <input type="text" name="id" size=10><br>
		비밀번호: <input type="password" name="pw" size=10><br>
		<input type="submit" value="로그인">
	</form>

</body>
</html>
```
```jsp
<!--loginOk2.jsp-->

<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<!-- 처음으로 정보 저장 -->
	<%! String id, pw; %>
	<%
		id = request.getParameter("id");
		pw = request.getParameter("pw");
		
		if(id.equals("abcde") && pw.equals("12345")) {
			session.setAttribute("id", id); /* 정보가 일치하면 세션에 데이터 저장  */
			response.sendRedirect("welcome2.jsp"); /* 저장하고 클라이언트에게 다시 welcome2로 접근요청 */
		} else { 
			response.sendRedirect("logln2.html"); /* 정보가 일치하지 않으면 다시 로그인 */
		}
	
	%>
</body>
</html>

<!--welcome2.jsp-->
<%@page import="java.util.Enumeration"%>
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<!-- 클라이언트가 다시 접근했을때 세션에 있는 모든 값들 검사 -->
	<% 
		Enumeration enumeration = session.getAttributeNames(); /* 세션에 저장되어있는 모든 값 */
		while(enumeration.hasMoreElements()) { /* 저장된 값들을 다 돌려서 검사 */
			String sName = enumeration.nextElement().toString(); /* 받은 이름(key)을 toString해서 sName에 담는다 */
			String sValue = (String)session.getAttribute(sName); /* 세션에서 저장되어 있는 이름(key)의 값을 sVlaue에 담는다 */
			/* 캐스팅 하기 전까지 : session.getAttribute(sName)는 객체타입으로 인식하기 때문에 형변환을 맞춰서 캐스팅해줘야한다
			집어넣을때는 객체니까 어떠한 형도 다 받을 수 있음(데이터 타입은 항상 타입을 맞춰줘야함)*/ 
			if(sValue.equals("abcde")) {
				out.println(sValue + "님 안녕하세요.");
			}
		}
	%>
	
	<a href = "logout2.jsp">로그아웃</a>
</body>
</html>

<!--logout2.jsp-->
<%@page import="java.util.Enumeration"%>
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
		Enumeration enumeration = session.getAttributeNames();
		while(enumeration.hasMoreElements()) {
			String sName = enumeration.nextElement().toString();
			String sValue = (String)session.getAttribute(sName);
			
			if(sValue.equals("abcde")) {
				session.removeAttribute(sName);
			}
		}
	%>
	
	<a href = "sessiontest.jsp">sessionTest</a>
</body>
</html>

<!--sessionTest.jsp-->
<%@page import="java.util.Enumeration"%>
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
		Enumeration enumeration = session.getAttributeNames();
	
	
		int i =0;
		while(enumeration.hasMoreElements()) {
			i++; /* 세션안에 저장되어있는 갯수 count */
			String sName = enumeration.nextElement().toString();
			String sValue = (String)session.getAttribute(sName);
			
			out.println("sNmae : " + sName + "<br>");
			out.println("sVlalue : " + sValue + "<br>");
		}
		
		if(i==0) {
			out.println("해당 세션이 삭제 되었습니다");
		}
	
	%>
</body>
</html>
```
