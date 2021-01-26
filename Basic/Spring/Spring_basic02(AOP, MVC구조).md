# AOP(Aspect Oriented Programming) : 관점 지향 프로그래밍
- 스프링 간판 개념 
- 관점 지향 프로그래밍이란 어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 **나누어서 보고** 그 관점을 기준으로 각각 모듈화시키는 것 
>> 모듈화 : 공통된 로직이나 기능을 하나의 단위로 묶는 것
```
예로들어 핵심적인 관점은 결국 우리가 적용하고자 하는 핵심 비즈니스 로직
부가적인 관점은 핵심 로직을 실행하기 위해서 행해지는 데이터베이스 연결, 로깅, 파일 입출력 등이 예
```
- 흩어진 관심사(Crosscutting Concerns) : 소스 코드상에서 다른 부분에 계속 반복해서 쓰는 코드들 
### ▶ AOP의 취지 : Aspect로 모듈화하고 핵심적인 비스니스 로직에서 분리하여 코드들을 재사용하기 위함

- doGet, doPost를 캡슐화해서 만든 게 스프링MVC


# ★★★★★스프링 MVC 구조★★★★★
![Sprin_MVC](https://user-images.githubusercontent.com/74290204/105818387-358fa080-5ffa-11eb-9a28-d541915d12e1.png)

# @ Controller
- 기본 작동 방법

![mapping](https://user-images.githubusercontent.com/74290204/105436455-963d7700-5ca2-11eb-998e-6fca313b08f0.PNG)

![Model](https://user-images.githubusercontent.com/74290204/105436461-976ea400-5ca2-11eb-9405-ef5453677db7.PNG)

### √ 디버깅 체크하는 방법 
<details><summary>click!</summary>
- 체크하는 방법은 get방식이니까 주소창에 값을 같이 넣으면 됨
(이제 이런건 감으로 아는 센스~!..get방식이 어떻게 값을 넘기는지 알면 할 수 있는 것)

![check](https://user-images.githubusercontent.com/74290204/105436737-119f2880-5ca3-11eb-8bde-40dbd98656fc.PNG)
</details>

## ▶ Form 데이터를 Cotroller 가 받아오는 방식
- 값 넘어올 때 어떻게 받아오는지에 대한 정의
- 서버쪽에서는 어떻게 넘길것인가? 사용자의 request값을 어떻게 받아올것인가
    - → HttpServletRequest 으로 요청된 값을 받아오고 Model로 값을 넘긴다

### 1. HttpServlet request 이용 
```java
@RequestMapping("board/confirmId")
	public String confirmId(HttpServletRequest request, Model model) {
		
		String id = request.getParameter("id");
		String pw = request.getParameter("pw");
		
		model.addAttribute("id", id);
		model.addAttribute("pw", pw);
		
		return "board/confirmId";
	}
```

### 2. @RequestParam 이용
```java
@RequestMapping("board/checkId")
	public String checkId(@RequestParam("id") String id, @RequestParam("pw") int pw , Model model) {
		// �� ����� �޾Ƴ� �����Ͱ� �������� �ڵ���� �ʹ� ������ ���� '�����Ͱ�ü'�� ������� ����
		// null���¿����� �ѱ�� ���⶧���� ��������ü���� ������ ������ ���� ȭ�� ����� �ȵǰ� 400���(������������ �������ʴ°�)
		// param ������̼��� �����ʾ������� ������������ �Ѱ���µ�? �� �ٸ�����?
		model.addAttribute("ID", id);
		model.addAttribute("PW", pw);
		
		return "board/checkId";
	}
```
<br>

### 3. Command 객체(데이터 객체) 이용: 우리가 아는 커맨드 아님, 다른 개념의 스프링 커맨드
- 기존에 방식을 이용해보니 데이터가 많아지면 코드가 길어지고 복잡해짐 → 해결방안으로 등장!
- 코드가 훨씬 간결해진다
```java
// LoginContoroller.java
@RequestMapping("/member/join")
	public String joinData(Member member) {
		// Model로 값을 넘기지 않았는데도 데이터객체를 쓰면 한번에 값을 받아오고 넘기는것까지 처리해줌
		return "member/join";
	}

/* Member.java  → 데이터 객체를 위한 클래스 ,
★[원리] 멤버변수의 setter 함수가 하나라도 없으면 내부안에서 만들때 에러남 
get함수가 하나라도 없으면 값을 받아올 함수가 없기 때문에 값을 요청해도 나오지않는 에러발생 
객체 생성을 하기위해서는 디폴트 생성자로 객체 생성을 하기로 약속했기 때문에 디폴트 생성자 반드시 있어야 커맨드 객체를 사용할 수 있고 없다면 에러남
(자바에선 인자가 있는 생성자 하나라도 있으면 디폴트 생성자를 만들어주지 않기 때문에 
디폴트 생성자를 꼭 만들어준다는 논리적인 기초가 있어야함)*/
package edu.bit.ex.member;

public class Member {

	private String id;
	private String pw;

	public Member() {

	}

	public Member(String id, String pw) {
		this.id = id;
		this.pw = pw;

	}

	public String getId() {
		return id;
	}

	public void setId(String id) {
		this.id = id;
	}

	public String getPw() {
		return pw;
	}

	public void setPw(String pw) {
		this.pw = pw;
	}

}

// join.jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR" pageEncoding="EUC-KR" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<html>
<head>
	<title>Home</title>
</head>
<body>
<h1>
	아이디 : ${member.id} <br>
	비밀번호 : ${member.pw} <br>
	<!-- 객체 변수명.데이터 객체 안에 변수명 -->
</h1>

</body>
</html>
```
