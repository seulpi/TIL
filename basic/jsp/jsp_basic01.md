# 들어가기 전 
<details markdown="1">
<summary>파일 import하는 방법 click!</summary>
이클립스 → Packacge → 우클릭 Import → Exisiting Projects into Workspace → Import할 폴더 위치 copy paste (Select root directory-Browse) 
→ option -Copy projects into workspace

> [import 했을 때 컴파일 에러 잡는 법: 사람마다 깔린 버전이 다르고 환경설정이 달라서 생기는 것]
프로젝트 우클릭 → Properties → Java Build Path →Libraries (unbound) 체크 → edit 
→  내 컴퓨터의 깔린 자바프로그램(Workspace default JRE)과 서버의 version을 맞춰 주기 (내컴퓨터에 맞춰서 환경설정)
</details>
<br>

## @ 용어 정리
- 웹 : 인터넷 기반 서비스
- 웹프로그래밍 : 웹어플리케이션을 구현하는 행위
- ★ **프로토콜(Protocol) :** 네트워크상에서 **약속한 통신규약**, 각각의 목적에 맞게 약속된 통신들
>> 종류: Http, FTP:파일전송과 관련, SMTP(Simple Mail Transfer Protocol), POP,DHCP:IP 동적으로 줄때
- IP 
- DNS(Domain Name Server) : EX) www.naver.com
- Port : 프로그램 번호 , IP를 찾아가서 해당 IP의 프로그램의 번호를 찾아라
```
// EX1)
www.naver.com → 네이버 서버로 들어감 → 네이버가 사용자에게 메인화면을 전송
* 네이버 서버 컴퓨터를 찾는 방법 1. IP 2. 웹주소

www.naver.com : DNS, 네이버 IP주소로 변경(IP를 다 외우지 못하기 때문에 DNS이용하는 것)

// EX2)
http://www.sba.seoul.kr:80/kr/index

- http: 인터넷 통신 규약
- www.sba.seoul.kr : DNS
- 80 : IP의 프로그램의 번호
- kr/index : 톰캣, 제우스가 관리하는 경로 
```
>> 클라이언트(웹브라우저) ↔ 서버(다수를 기다리는 곳) <br> http://127.0.0.1:8282 (자기자신에게 접속하는 IP:locallhost)

- 톰캣 : 하나의 프로그램으로 웹서버 & 웹어플리케이션 서버를 같이 처리한다
    - 하나의 컴퓨터가 다수의 사용자를 받아내기 위해서는 웹서버가 필요(ex.톰캣-세계적으로 사용/제우스-국내 사용)
    <details markdown="1">
    <summary>톰캣 설치 방법 click!</summary>

    톰캣 설치 (버전주의)
    https://tomcat.apache.org/download-90.cgi
    - 64-bit Windows zip (pgp, sha512)
    - C이의 드라이브에서 설정 
    - C - tomcat폴더 안에 zip파일 여기서 풀기

    ![캡처](https://user-images.githubusercontent.com/74290204/103077016-55563200-4612-11eb-97b8-9edf5a5d0592.PNG)

    - 설치할때 통일! 폴더면도 버전이 나오게 (zip이름으로 풀면됨)
    - 이클립스 열고 PPT랑 똑같이 하면됨 New Server 에서 JRE - jdk-15.0.1

    ![캡처2](https://user-images.githubusercontent.com/74290204/103077054-6737d500-4612-11eb-9664-44520ab9bdd7.PNG)

    - Port 번호 8282로 맞춘 후 ctrl +s http://localhost:8282/로 확인
    - 이클립 환경변수 C:\Java\jdk-15.0.1\bin
    - window -preference - general - web Browser - Use external web browser - chrome체크<br> (크롬안깔려있으면 디버깅안됨)
    크롬 체크하고 서버 실행시키면 자동으로 크롬 열어서 보여줌
    브라우저 창에서 F12(개발자모드)

    ![캡처3](https://user-images.githubusercontent.com/74290204/103077109-7e76c280-4612-11eb-85cc-d526c3659e8c.PNG)

    ![캡처4](https://user-images.githubusercontent.com/74290204/103077111-7fa7ef80-4612-11eb-81e1-287a3376ba67.PNG)
    </details>

## @ ★ JSP 구조 
```
.jsp file → (java파일로 변환) .java file → (컴파일) .class file
```
- java 서버 페이지이기 때문에 jsp파일은 **반드시 java파일로 변환한다**
- java 파일을 이클립스에서 자동으로 컴파일했다면 jsp파일을 java파일로 바꾸고 컴파일해주는 주체 : 톰캣!
- .class가 메모리에 올라가고 채팅 프로그램이 돌아가는 원리(request한 곳으로 html을 다시 돌려보내주는 것)

## @웹서버 & 웹어플리케이션
```
<클라이언트 request>
클라이언트 서버 → 웹서버 (html을 처리하는 부분) → WAS: 웹어플리케이션 서버 (java를 처리하는 부분) → DB (데이터 베이스)

<request를 다시 클라이언트에게 response>
DB → WAS : 웹어플리케이션 서버 → 웹서버 → 클라이언트 서버

☞ java부분이 html로 변환되서 웹서버로 전달하고 웹서버에서 html로 사용자에게 전달 
```
<br>

# ★★ ★  do post/ do get ★ ★ ★ 
- doGet: get방식, 내부적으로 실행
- doPost: post방식, 내부적으로 실행

```
naver 창에 검색단어를 친다 → 검색단어를 어디에 보내? 네이버 서버로~ → 어떻게 보내는데? 2가지 방식 !! 1. get 2. post
```
## 1. get방식 : 주소창 뒤에 '?key value'정보를 함께 전송 (URL안에 값이 존재)
>> http:// locallhost:8080/boardList?name=제목&contents=내용

![get](https://user-images.githubusercontent.com/74290204/103291896-12d88f00-4a30-11eb-89df-8654e9bc76f7.PNG)

## 2. post방식 : BODY영역에 숨어서 들어간다 (주소로 들어가는게 X)
- post는 get과 달리 URL에 데이터가 노출되지않음(보완이 필요할때 사용)

### ▶ URL을 치면 doget이 나오는 이유?(=post방식이 아닌 이유?) <br> post로 따로 지정하지 않는다면 디폴트값으로 get이 설정되어 넘어오기 때문이다 

# 태그 
## - form 태그
- form은 action과 method를 지정할 수 있음 
>> action : 클릭했을때 주소 / method : post or get 방식 중 하나 선택 
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<form action="hw" method="post">
		<input type="submit" value="post"> <!-- input : 버튼을 만들어주는 역할 / value : 버튼안에 글자-->
	</form> 
<!-- ▶ 버튼을 누르게되면 post방식으로 hw주소를 보내라 --> 
</body>
</html>
```
![html tag05](https://user-images.githubusercontent.com/74290204/103292253-ce99be80-4a30-11eb-99a5-d88102e1b41c.PNG)

![html tag06](https://user-images.githubusercontent.com/74290204/103292259-cfcaeb80-4a30-11eb-8988-faec158cc9e0.PNG)
<br>

# Context Path
 <주의!> 여기서는 톰캣 서버 기준으로 설명 (OS에서의 context path는 또 다른 용어의 의미를 가짐)
 - context : 프로젝트 , 웹 어플리케이션을 구분하기 위한 path
```java
Servers → server.xml → <Context docBase="servlet_hello3" path="/servlet_hello3" reloadable="true" source="org.eclipse.jst.jee.server:servlet_hello3"/></Host> // 현재 돌리고 있는 server를 표시 
http://192.168.0.96:8282/servlet_hello3
							↑ 프로젝트명(실무에선 context명이라 부름)

** server 파일은 함부로 건들지 말 것 !
```
