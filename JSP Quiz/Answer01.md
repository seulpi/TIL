# 1. 아래의 용어에 대해서 설명하시오
## - 웹서버
## - WAS
## - JSP
## - DNS
## - 포트번호
- 웹서버 : 웹 사용자가 어떻게 호스트 파일들에 접근하는지를 관리하는 것(IP 주소를 찾아가 해당하는 번호에 대해 접근하게 해주는 것) <br> 
**User가 requset하면 프로토콜을 이용해 request에 대한 response를해 브라우저에 출력시키는 역할(HTML을 처리한다)**
   - 대표적인 웹서버로는 Aphach(HTTP서버) / IIS(Internet Information Sevices) : 마이크로 소프트 윈도우를 사용하는 서버들을 위한 인터넷 기반서비들의 모임이 있다

- WAS(웹어플리케이션서버) : WAS는 user의 request를 웹서버로부터 request받아 JAVA언어로 바꾸어 웹서버에게 다시 전달한다

▶ 웹서버와 WAS를 같이 처리하는 것이 바로 톰캣과 제우스

- JSP(Java Server Page) : HTML페이지 내에 자바코드를 삽입하여 동적인 웹페이지를 구현할 수 있도록 도와주는 하나의 도구

- DNS(Domain Name Server) : 컴퓨터의 주소를 User가 기억하기 쉽게 이름지은 것

- 포트번호 : 해당 컴퓨터의 주소(IP)로 찾아가 그 주소안에 프로그램의 번호를 말한다

# 2. 프로토콜이란 무엇이며,프로토콜의 종류는?
: 프로토콜(Protocol)이란 네트워크에서 **각각의 목적에 맞게 약속된 통신들이다** 
- 종류 : HTTP, FTP(파일 전송과 관련), SMTP(Simple Mail Transger Protocol), POP, DHCP(IP동적으로 줄 때) 이 있다 

# 3. ★ .jsp 가 컴파일 되는 과정에 대하여 설명하시오 ★
```
1. user가 requset (도메인 주소를 친다)
2. Wep Server가 requset를 받아 → Wep Application에게 requset → DB 
3. DB response →  Wep Application response → Wep Server responce → User (화면 출력)

▶ .jsp file → Java file(.java)로 변환 → 컴파일(.class)
```

# 4. WAS란 무엇이며, 종류는?
: WAS란 웹어플리케이션 서버라고 하며 user가 request하면 웹서버로부터 request받아 JAVA언어로 바꾸어 웹서버에게 다시 전달한다 <br>
종류로는 톰캣과 제우스가 있다

# 5. 아래를 프로그래밍 하시오
```
아래의 주소를 접속시 아래가 웹브라우져에 나타나도록 하시오.
http://localhost:8282/jsp_programer/programer.jsp

I am programer(웹브라우져 출력)
```

![5](https://user-images.githubusercontent.com/74290204/103076814-e547ac00-4611-11eb-9c27-81892e72e734.PNG)


# 6. servlet에 대하여 설명하시오
: 자바에서 제공하는 http 프로토콜을 지원하는 라이브러리 (HttpServlet을 상속바는 모든 클래스를 말한다) <br> http를 캡슐화시킨(쉽게 사용할수있게 만든) 라이브러리이다
>> 라이브러리: 클래스들의 집합

# 7. web.xml에 대하여 설명하시오
: 톰캣이 웹서비스를 읽어들이기 전 프로젝트를 실행하기 위해 설정하는 파일 (설정은 개발자가 지정)
