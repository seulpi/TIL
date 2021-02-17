# AOP(Aspect Oriented Programming) : 관점 지향 프로그래밍
- 스프링 간판 개념 
- 관점 지향 프로그래밍이란 어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 **나누어서 보고** 그 관점을 기준로 각각 모듈화시키는 것 <br> 즉 공통된 로직에 대해서 함수가 실행될 때 로직이 실행되게 하는 것(로직 주입) 
```
EX) 
계좌이체, 입출금, 이자계산을 할 때마다 로그인해야함 
→ 각각 로그인하지 말고 로그인 로직을 따로 빼서 정의하면 계좌이체등 기능을 사용할때마다(함수가 실행될때마다) 로그인 로직을 실행하게 하는것
```

>> 모듈화 : 공통된 로직이나 기능을 하나의 단위로 묶는 것<br> 모듈 : 특정 기능별로 나누어지는 프로그램 덩어리) 

## @ 용어 
- OOP(객체지향프로그래밍)와 다름을 표현하기위해 용어를 분리 (OOP: '객체'라는 단어 사용 / AOP : '관점'이라는 단어 사용)

1. 핵심관점(핵심로직) : 객체(함수+데이터)
2. 횡단관점(공통로직)
	- 함수가 호출되기 전에 (service에 핵심기능) 공통로직을 먼저 실행시키겠다 → AOP
3. Aspect : 공통기능이 들어있는 클래스(예제, 로깅, 트랜잭션 etc)
4. Advice : Aspect 클래스에 들어있는 공통 기능(한마디로 Aspect안의 함수), 실질적으로 어떤 일을 해야할지에 대한 것
5. Target : Aspect를 적용하는 곳 (클래스,메소드...)
6. ★ JoinPoint : Advice가 적용되는 함수 
	- expression : joinpoint설정
7. ★ PointCut : JoinPoint의 부분으로 실제로 적용되는 함수내의 **지점** (JointPoint의 상세한 스펙을 정의한 것)
	- *Before*(메소드 실행**전에** 동작)
	- *After*(메소드 실행**후에** 동작)
	- ★*Around*(메소드 호출 이전&이후&예외발생 등 **모든 시점에서** 동작)
	- *After-returning*(메소드가 **정상적으로 실행된 후** 동작)
	- *After-throwing*(**예외가 발생한 후** 동작)
8. weaving : Advice를 적용하는 행위


## @ AOP 사용 방법
### 1 .xml설정 → 객체(bean) 생성

#### *- 기본 설정&사용*
- pom.xml
```xml
 <!-- AspectJ -->
 <dependency>
	<groupId>org.aspectj</groupId>
	<artifactId>aspectjrt</artifactId>
	<version>${org.aspectj-version}</version>
</dependency>
      
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
	<groupId>org.aspectj</groupId>
	<artifactId>aspectjweaver</artifactId>
	<version>${org.aspectj-version}</version>
</dependency>
```
- root-context.xml 복붙 후 밑에 코드 복사(위치: root-context.xml과 같은 위치 → WEP-INF/spring)
```xml
//aop-context.xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xmlns:mybatis-spring="http://mybatis.org/schema/mybatis-spring"
   xmlns:context="http://www.springframework.org/schema/context"
   xmlns:aop="http://www.springframework.org/schema/aop"
   xsi:schemaLocation="http://mybatis.org/schema/mybatis-spring http://mybatis.org/schema/mybatis-spring-1.2.xsd
      http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
      http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd
      http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.3.xsd">
   
 <!-- Root Context: defines shared resources visible to all other web components -->
<!--    <aop:aspectj-autoproxy></aop:aspectj-autoproxy> -->
<!--       
   <bean id="logAop" class="edu.bit.board.aop.LogAop" /> --> <!-- aruond를 위한 객체-->
   <bean id="logAdvice" class="edu.bit.ex.aop.LogAdvice" />  <!--패키지명 맞춰주기!-->
   
<!--    AOP설정    -->
   <aop:config>
      <aop:aspect ref="logAdvice">
         <aop:pointcut id="publicM" expression="within(edu.bit.ex.board.service.*)"/> <!-- edu.bit.board.service.*안에 있는 모든 것 (joinpoint)-->
<!--          <aop:around pointcut-ref="publicM" method="loggerAop" /> -->
         <aop:before pointcut-ref="publicM" method="printLogging" />
      </aop:aspect>
   </aop:config>
   
   <!-- aruond를 위한 객체-->
  <!--  <aop:config> 
      aspect id는 logger이고, logAop를 참조함
      <aop:aspect  ref="logAop">
          pointcut(핵심 기능)의 id는 publicM이고, edu.bit.ex.* 패키지에 있는 모든 클래스에 공통 기능을 적용
         <aop:pointcut id="publicM" expression="within(edu.bit.board.service.*)"/>
         loggerAop()라는 공통 기능을 publicM라는 pointcut에 적용
         <aop:around pointcut-ref="publicM" method="loggerAop" />          
      </aop:aspect>
   </aop:config>  --> 
          
</beans>
```
- servlet-context.xml 에서 interceptor 객체 생성 설정 삭제
- web.xml (servlet으로 설정했는지 헷갈림 학원에서 코드 확인 필요!)
```xml
<!-- aop.xml도 읽어야하니까 추가, 두개일때는 param-value에 추가 -->
<context-param>
	<param-name>contextConfigLocation</param-name>
	<param-value>
		/WEB-INF/spring/root-context.xml
		/WEB-INF/spring/aop-context.xml
	</param-value>
</context-param>

<!-- 2개이상이라고 이렇게 하면 절대안됨!!, 이렇게 사용하면 하나만 읽어들임-->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>
			/WEB-INF/spring/root-context.xml
		</param-value>
	</context-param>
	
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>
			/WEB-INF/spring/aop-context.xml
		</param-value>
	</context-param>
```
```java
// Aspect, Advice
package edu.bit.ex.aop;

public class LogAdvice {
	
	public void printLogging() {
		System.out.println("=====로그기록=====");

	}
}
```
#### *- Around 설정&사용*
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xmlns:mybatis-spring="http://mybatis.org/schema/mybatis-spring"
   xmlns:context="http://www.springframework.org/schema/context"
   xmlns:aop="http://www.springframework.org/schema/aop"
   xsi:schemaLocation="http://mybatis.org/schema/mybatis-spring http://mybatis.org/schema/mybatis-spring-1.2.xsd
      http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
      http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd
      http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.3.xsd">
   <!-- Root Context: defines shared resources visible to all other web components -->
<!--    <aop:aspectj-autoproxy></aop:aspectj-autoproxy> -->
      
   <bean id="logAop" class="edu.bit.board.aop.LogAop" /> <!-- aruond를 위한 객체-->
   <!-- <bean id="logAdvice" class="edu.bit.ex.aop.LogAdvice" />  --> <!--패키지명 맞춰주기!-->
   
<!--    AOP설정    -->
<!--    <aop:config>
      <aop:aspect ref="logAdvice">
         <aop:pointcut id="publicM" expression="within(edu.bit.ex.board.service.*)"/> edu.bit.board.service.*안에 있는 모든 것 (joinpoint)
         <aop:before pointcut-ref="publicM" method="printLogging" />
      </aop:aspect>
   </aop:config> -->
   
   <!-- aruond를 위한 객체-->
   <aop:config> 
     <!--  aspect id는 logger이고, logAop를 참조함 -->
      <aop:aspect  ref="logAop">
          <!-- pointcut(핵심 기능)의 id는 publicM이고, edu.bit.ex.* 패키지에 있는 모든 클래스에 공통 기능을 적용 -->
         <aop:pointcut id="publicM" expression="within(edu.bit.board.service.*)"/>
       <!--   loggerAop()라는 공통 기능을 publicM라는 pointcut에 적용 -->
         <aop:around pointcut-ref="publicM" method="loggerAop" />          
      </aop:aspect>
   </aop:config> 
          
</beans>
```
```java
//LogAop.java
package edu.bit.ex.aop;

import org.aspectj.lang.ProceedingJoinPoint;

//가장 대표적인 aruond에 대한 예제
public class LogAop {
	
	public Object loggerAop(ProceedingJoinPoint joinpoint) throws Throwable {
		String signatureStr = joinpoint.getSignature().toShortString();
		System.out.println( signatureStr + " is start.");
		
		long st = System.currentTimeMillis();
		
		try {
			Object obj = joinpoint.proceed(); //핵심로직을 사이에 두고 공통로직 삽입
			return obj;
			
		} finally {
		
			long et = System.currentTimeMillis();
			
			System.out.println( signatureStr + " is finished.");
			System.out.println( signatureStr + " 경과시간 : " + (et - st));
		}
	}	
}
```
#### 가장 대표적인 around 예제
- 함수 실행될 때 걸리는 시간 체크하기
- ▼ java 시간체크하는 대표적인 코드 → aop를 사용하지 않으면 이 코드를 실행할때마다 넣어줘야함
```java
long beforeTime = System.currentTimeMillis(); //코드 실행 전에 시간 받아오기
      
//실험할 코드 추가  
     
long afterTime = System.currentTimeMillis(); // 코드 실행 후에 시간 받아오기
long secDiffTime = (afterTime - beforeTime)/1000; //두 시간에 차 계산
System.out.println("시간차이(m) : "+secDiffTime);
```

### 2. annotation 활용
- 사용하기 위해 servlet-context.xml 에 < aop:aspectj-autoproxy >< /aop:aspectj-autoproxy > 필요
```xml
<aop:aspectj-autoproxy></aop:aspectj-autoproxy> 
<!-- annotation을 읽어들이기 위해서는 객체가 필요한데 이 한줄이 그 객체를 생성하는 것-->
```
- @Component : < context:component-scan base-package="edu.bit.ex" /> scan하면서 객체 생성
- @Aspect 
- @Pointcut("within(edu.bit.ex.*)")
- @Before("within(edu.bit.ex.*)")
```java
package edu.bit.ex.aop;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

@Component
@Aspect
public class LogAop2 {
	
	@Around("within(edu.bit.ex.board.service.*)")
	public Object loggerAop(ProceedingJoinPoint joinpoint) throws Throwable {
		String signatureStr = joinpoint.getSignature().toShortString();
		System.out.println( signatureStr + " is start.");
		
		long st = System.currentTimeMillis();
		
		try {
			Object obj = joinpoint.proceed(); //핵심로직을 사이에 두고 공통로직 삽입
			return obj;
			
		} finally {
		
			long et = System.currentTimeMillis();
			
			System.out.println( signatureStr + " is finished.");
			System.out.println( signatureStr + " 경과시간 : " + (et - st));
		}
		
	}
	
}
```

## @ AOP 안먹힐때
```
+ aop 실행 안될때 )
1. web.xml에서 context.xml 한개만 두기

	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>
			/WEB-INF/spring/root-context.xml
		</param-value>
	</context-param>

2. servlet-context.xml에 aop 내용 맨밑에 추가 
xmlns:aop="http://www.springframework.org/schema/aop"
xsi:schemaLocation= http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.3.xsd

→ 맨위에<beans>에 이 두개도 추가해줘야함
→  namespace에 aop 체크 되어있는지 확인 (안되면 그냥 손으로 치거나 복붙하면됨)

https://velog.io/@songi_jeon/Spring-namespace-%ED%83%AD%EC%9D%B4-%EC%95%88-%EB%B3%B4%EC%9D%BC-%EB%95%8C

+ <beans:bean id="logAdvice" class="edu.bit.ex.aop.LogAdvice" />
beans:bean 으로 붙여줘야함
```

## @ AOP 표현식 
>> ' * ' : 모든 / ' . ' : 현재 / ' .. ' : 0개이상

### 1. Execution
- @Pointcut("execution(public void get*())") : public void의 모든 get메소드
- @Pointcut("execution(* com.javalec.ex.*.*())") : com.javalec.ex 패키지에 파라미터가 없는 모든 메소드 
- @Pointcut("execution(* com.javalec.ex..*.*())") : com.javalec.ex 패키지에 &com.javalec.ex 하위 패키지에 파라미터가 없는 모든 메소드
- @Pointcut("execution(* com.javalec.ex.Woerker.* ())") :com.javalec.ex.Woerker안에 모든 메소드

### 2. within
- @Pointcut("within(edu.bit.ex.* )") : 패키지내에 모든 메소드
- @Pointcut("within(edu.bit.ex..* )") : 패키지의 하위패키지를 포함한 모든 클래스의 메소드에 적용
- @Pointcut("within(edu.bit.ex.Worker)") : edu.bit.ex.Worker안의 메소드

### 3. bean 
- @Pointcut("bean(* ker)") : ~ ker로 끝나는 빈에만 적용
- @Pointcut("bean(student)") : student 빈에만 적용

```
실제 aop를 만드는 방법은 크게 3가지

1. 컴파일 단계에서 하는 방법
0101덩어리를 미리 만들어서 컴파일해놓은상태

2. 실행중에 proxy라는 객체를 통해 실행시키는 방법(컴파일을 미리 해놓지X)
→ spring에서 aop생성하는 방법
```


# ★★★★★스프링 MVC 구조★★★★★
- doGet, doPost를 캡슐화해서 만든 게 스프링MVC
![Sprin_MVC](https://user-images.githubusercontent.com/74290204/105818387-358fa080-5ffa-11eb-9a28-d541915d12e1.png)

# @ Controller 
*Controller는 ModelAndView만 전달해주는 영역이기 때문에 소스코드는 Command(Service)쪽에서 다 정리해서 와야함*
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
