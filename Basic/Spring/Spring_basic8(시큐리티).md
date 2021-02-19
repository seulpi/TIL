>>[에러났을때 정확한 정보 확인 사이트]  https://stackoverflow.com/
# user객체 라이브러리 따라가서 코드 보는거 추가하기!!!!!!!!!!
# Spring Security : 인증과 권한에 대한 프레임워크
- 보안과 관련 
- 스프링 시큐리티라는 것은 core의 넣는 하나의 조립품으로 **하나의 기능일 뿐**<br>(스프링 시큐리티도 하나의 프레임워크 : 개발자가 사용하기 편하게 캡슐화한 것) 
    - Q. **스프링시큐리티**가 뭔가요?  스프링 시큐리티란 **인증과 권한에 대한 프레임워크**
>> core?  Mapven Dependencies  - spring-core-5.0.7RELEASE-jar 이 부분 → 이 안에 IOC 컨테이너가 다 존재 

<details><summary>Mapven Dependencies 캡처</summary>
	
![화면 캡처 2021-02-18 185143](https://user-images.githubusercontent.com/74290204/108339292-99347480-721a-11eb-9777-279f1a531385.png)
	
![화면 캡처 2021-02-18 185158](https://user-images.githubusercontent.com/74290204/108339303-9c2f6500-721a-11eb-83c9-d4832544e941.png)
</details>

### ▶ jar 한개 한개가 다 조립품이기 때문에 설정이 다 다름  → Log4j, 스프링시큐리티도 설정이 다 다름 → 다 다르게 세팅해야된다는 소리
- 주입 → 소스 상에서 .xmld에 bean 객체생성해서 만드는 부분
- 코어 안에 로드존슨이 만든 법칙을 따라서 DI,IOC(조립)을 한다 = ★ 스프링시큐리티라는것은 다른 사람이 만든것 
    - 대표적인 예)  /mybatis - root.xml에 객체생성 , AOP, log4j등 
<br>


## @ 스프링 시큐리티 알아야할 개념(하위개념들에 대해 캡슐화한 것 = 시큐리티)
- 시큐리티를 스프링 라이브러리안에 사용하고 컨트롤 하는 것 =  스프링 시큐리티 

### 1. 인증 : 자신을 증명, 로그인에서 아이디와 비밀번호
### 2. 권한 : 남에 의한 자격부여(리소스 접근에 대한 권한) , admin과 일반 유저는 리소스에 대한 접근 권한이 달라짐 
- ex) 회사에 들어가려면 사원증으로 입장 → 인증 (로그인) <br>속한 부서에 사원증으로 한번 더 입장 → 권한 
>> ★소스상에서 인증과 권한이 어떻게 표현이 되는지?
### 3. 암호화 : 라이브러리로 제공됨 (어떻게 암호화시키는지: 패스워드를 받으면 인코딩, 디코딩 → 암호화)
### 4. CSRF, XSS : 공격
- [링크 참조]
### 5. 스프링시큐리티 → Session내에 저장하는 정보들을 만듦 , Session 객체(이 객체를 커스터마이징하는게 최종 목표) 
- 시큐리티 세션 메모리

### 6. jsp에서 스프링 시큐리티 태그 쓰는 방법
<br>

## @ 설정
- 사용을 위해 jar파일을 가져와야함(총4개의 dependency추가)
    - 버전을 타기 때문에 버전이 상당히 중요 <br>
    (※ Spring보다 security는 **낮은 버전을 사용**해야함)
    ```xml
    <!--현재 학원,집에서 사용하는 spring 버전, 그 버전에 맞춘 시큐리티 버전-->
     <org.springframework-version>5.0.7.RELEASE</org.springframework-version>
     <org.security-version>5.0.6.RELEASE</org.security-version>
     ```
#### ※ maven repository에서 spring core version과 호환 확인!!
- 맞춰도 에러가 날때 있음(오픈소스이다보니까)
- https://mvnrepository.com/
<details><summary>maven repository캡처</summary>
	
![화면 캡처 2021-02-18 190547](https://user-images.githubusercontent.com/74290204/108340966-8458e080-721c-11eb-8479-5c330edf2561.png)
	
![화면 캡처 2021-02-18 190656](https://user-images.githubusercontent.com/74290204/108340969-858a0d80-721c-11eb-9cfe-e6590b6bc004.png)
</details>
  

### 1. xml 설정
#### *- pom.xml* → 이 라이브러리를 **왜 다운로드** 받는가? **인증과 권한을 컨트롤**하기 위함! 
>> 소셜로그안할때 핵심개념 = ★ 인증과권한 (시큐리티의 기본)

<details><summary>pom.xml</summary>
	
```xml
	<!-- Spring Security -->
	<dependency>
		<groupId>org.springframework.security</groupId>
		<artifactId>spring-security-core</artifactId>
		<version>${org.security-version}</version>
	</dependency>

	<dependency>
		<groupId>org.springframework.security</groupId>
		<artifactId>spring-security-web</artifactId>
		<version>${org.security-version}</version>
	</dependency>

	<dependency>
		<groupId>org.springframework.security</groupId>
		<artifactId>spring-security-config</artifactId>
		<version>${org.security-version}</version>
	</dependency>

	<dependency>
		<groupId>org.springframework.security</groupId>
		<artifactId>spring-security-taglibs</artifactId>
		<version>${org.security-version}</version>
	</dependency>
```
</details>

#### *- web.xml* → filter 설정
- filter name도 바뀌서는 안됨(그냥 내비두기) / 위치 : 한글 필터 밑
<details><summary>web.xml</summary>
	
```xml
	 <!-- Spring Security Filter spring 12개의 필터 객체가 생성된다-->
	<filter>
	    <filter-name>springSecurityFilterChain</filter-name>
	    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
	</filter>

	 <filter-mapping>
	    <filter-name>springSecurityFilterChain</filter-name>
	    <url-pattern>/*</url-pattern>
	</filter-mapping>
```
</details>

#### *- security-context.xml* → root-context.xml를 복붙(※같은자리에 붙여넣기)
<details><summary>security-context.xml</summary>
	
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<beans:beans xmlns="http://www.springframework.org/schema/security"
	    xmlns:beans="http://www.springframework.org/schema/beans"
	   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	   xsi:schemaLocation="http://www.springframework.org/schema/security http://www.springframework.org/schema/security/spring-security.xsd
	      http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">   

	<!-- http와 auth~는 시큐리티의 기본적인 설정 -->
		<http>
			<form-login />
		</http>

		<!-- provider -->
		<authentication-manager>

		</authentication-manager>

	</beans:beans>

	- web.xml -> /WEB-INF/spring/security-context.xml 추가 

	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/spring/root-context.xml
			/WEB-INF/spring/security-context.xml</param-value>
	</context-param>
```
</details>

---

### ▶ 여기까지 설정하고 "context명"/login하면 화면 출력

![화면 캡처 2021-02-16 121256](https://user-images.githubusercontent.com/74290204/108015218-0f867a80-7053-11eb-82e0-d4da638a8ccd.png)

#### why] form을 만들어 주지도 않았는데 어떻게 이런 form형태의 페이지가 출력되는지?
- 내가 만들어 준 페이지가 아닌데 누군가 응답해서 페이지를 출력 <br> → login이라는 경로를 누가 받아줌, interceptor처럼 누군가가 낚아채가는 중! <br>(내가 controller에서 경로 처리 안해줬는데 페이지가 구현중이니까)

- **★ 응답 해주는 주체 : 스프링 시큐리티** <br> → 스프링 시큐리티 필터에서 낚아챔(필터니까 Controller가기 전에 낚아채기 가능)

- 페이지는 어디서 구현되는가? Maven 라이브러리
<br>

## @ 시큐리티 실행되는 위치
- 로그인을 어디서 낚아채는가? Dispather Servlet 가기전에!
    - why? - 해킹 때문에 filter에서 작동

![security](https://user-images.githubusercontent.com/74290204/108144440-fe01a900-710c-11eb-97bd-b4278a67237b.png)
<br>


## @ 시큐리티 사용 
### ① interceptor : 권한 설정1
- form-login처럼 디폴트로 생성해주지않기 때문에 직접 controller, view등을 세팅해줘야함

1. *security-context.xml*
```xml
<intercept-url pattern="/security/all" access="permitAll" />
<intercept-url pattern="/security/member" access="hasRole('ROLE_MEMBER')" /> 
```

<details><summary>security-context.xml에 interceptor 추가된 코드</summary>
	
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<beans:beans
		xmlns="http://www.springframework.org/schema/security"
		xmlns:beans="http://www.springframework.org/schema/beans"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://www.springframework.org/schema/security http://www.springframework.org/schema/security/spring-security.xsd
	      http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

		<http>
			<intercept-url pattern="/security/all" access="permitAll" /> <!-- 모두 접근 허용 -->
			<intercept-url pattern="/security/member" access="hasRole('ROLE_MEMBER')" /> <!-- ROLE_MEMBER 권한을 가진 사람만 접근 허용 -->
			<form-login />
		</http>

		<!-- provider -->
		<authentication-manager>

		</authentication-manager>

	</beans:beans>
```
</details>


2. *controller*

- void 로 함수를 설정했기 때문에  view페이지는 all.jsp, member.jsp로 만들어줘야하고<br> /securuty/*로 맵핑되어있기 때문에 views/security폴더를 생성해 jsp파일을 그 안에 둬야함! 

```java
package edu.bit.ex;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;

import lombok.extern.log4j.Log4j;

@Log4j
@Controller
@RequestMapping("/security/*")
public class SecurityController {
	
	@GetMapping("/all")
	public void doAll() {
		log.info("do all can access everybody");
	}
	
	@GetMapping("/member")
	private void doMember() {
		log.info("logined member");
	}
}
```
3. *view(jsp)*

- all.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>    
<%@ taglib uri="http://www.springframework.org/security/tags" prefix="sec" %>    
    
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
	<!-- all or member or admin -->
	<h1>/sample/all page</h1>

	<sec:authorize access="isAnonymous()"> <!-- security 태그 , isAnonymous() = permitAll로 했기 때문에 누구든지 올 수 있는 함수로 받음
	+sec : security  태그 ->사용하려면 라이브러리 명시
	<%@ taglib uri="http://www.springframework.org/security/tags" prefix="sec" %>    
	-->
		<a href="/customLogin">로그인</a>
	</sec:authorize>

	<sec:authorize access="isAuthenticated()"> <!-- isAuthenticated() 인증된유저만 : 인증처리 안해서 로그아웃 화면에 출력X -->
		<a href="/customLogout">로그아웃</a>
	</sec:authorize>

</body>
</html>
```

- member.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>/sample/member page</h1>
</body>
</html>
```
<details><summary>화면 출력</summary>
	
![화면 캡처 2021-02-17 112009](https://user-images.githubusercontent.com/74290204/108149013-52a92200-7115-11eb-9c96-7a1a2936d50c.png)
</details>


#### *★시큐리티 내부 동작*
- 체크가 안되는 권한에 대해서는 내부적으로 인증을 하기 위한 로그인을 다시하라고 되돌려 보냄 <br> (ROLE_MEMBER권한을 가지고 있는지)
- 인증 → 권한 → 리소스 접근
>> 리소스가 뭔지? all.jsp 같은 파일


### ② < user-service >에 user 권한 설정2
+ 실무에서는 이렇게 잘 사용안하지만 테스트를 위함 (실무에서는 DB에서 가져옴)
- 여기서는 직접 사용자를 지정해서 권한을 부여하는 것(DB에서 가져오는 게 X)
	- member라는 유저, admin이라는 유저 여기서 권한 설정해주고 아이디랑 비밀번호 설정 하는 것 <br>(그래서 실무에서는 이렇게 안한다는 거)
```xml
<authentication-manager>
    <authentication-provider> 
    	<user-service> 
            <user name="member" password="{noop}member" authorities="ROLE_MEMBER" /> 
        </user-service> 
    </authentication-provider>
</authentication-manager>
```
<details><summary> 'user-service' 추가된 코드 </summary>
	
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<beans:beans
		xmlns="http://www.springframework.org/schema/security"
		xmlns:beans="http://www.springframework.org/schema/beans"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://www.springframework.org/schema/security http://www.springframework.org/schema/security/spring-security.xsd
	      http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

		<http>
			<intercept-url pattern="/security/all" access="permitAll" /> <!-- 모두 접근 허용 -->
			<intercept-url pattern="/security/member" access="hasRole('ROLE_MEMBER')" /> <!-- ROLE_MEMBER 권한을 가진 사람만 접근 허용 -->
			<form-login />
		</http>

		<!-- provider -->
		<authentication-manager>
			<authentication-provider>
				<user-service>
					<user name="member" password="{noop}member"
						authorities="ROLE_MEMBER" />
				</user-service>
			</authentication-provider>
		</authentication-manager>

	</beans:beans>
```
</details>

▶ 화면 출력 

![화면 캡처 2021-02-17 121531](https://user-images.githubusercontent.com/74290204/108151386-e41a9300-7119-11eb-943e-9a08541ef446.png)

- {noop} 5.0부터는 패스워드를 인코딩하게 되어있어서(패스워트 암호화) 이걸 줘야 에러 안나고 패스워드를 읽어들임
- noop는 Spring Security에서 텍스트 그대로 비밀번호로 인식하게 해줌<br>
noop을 붙이지 않으면 다른 인코딩으로 암호화를 해줘야함 

>> [spring security 공식블로그 설명 참조]https://spring.io/blog/2017/11/01/spring-security-5-0-0-rc1-released#password-encoding 
<br>

<details><summary>< user-service > {noop} 사용안했을 때 에러 </summary>

![화면 캡처 2021-02-17 121849](https://user-images.githubusercontent.com/74290204/108151555-555a4600-711a-11eb-80c3-47bb62561a7e.png)
</details>

### Q. manager로 로그인하는것을 스프링시큐리티를 적용하세요
<details><summary> 정답! </summary>
	
+ 기존 소스코드에 user하나만 추가함 
	```xml
	<authentication-manager>
		<authentication-provider>
			<user-service>
				<user name="member" password="{noop}member" authorities="ROLE_MEMBER" />
				<user name="manager" password="{noop}manager" authorities="ROLE_MEMBER" />
			</user-service>
		</authentication-provider>
	</authentication-manager>
	```
</details>


### ④ principal : jsp에서 써먹기 위한 용도 

1. *security-context.xml*

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans
	xmlns="http://www.springframework.org/schema/security"
	xmlns:beans="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/security http://www.springframework.org/schema/security/spring-security.xsd
    http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

	<http>
		<intercept-url pattern="/security/all" access="permitAll" /> <!-- 모두 접근 허용 -->
		<intercept-url pattern="/security/member" access="hasRole('ROLE_MEMBER')" /> <!-- ROLE_MEMBER 권한을 가진 사람만 접근 허용 -->
		<intercept-url pattern="/security/admin" access="hasRole('ROLE_ADMIN')" />
		<form-login />
	</http>

	<!-- provider -->
	<authentication-manager>
		<authentication-provider>
			<user-service>
				<user name="member" password="{noop}member" authorities="ROLE_MEMBER" />
				<user name="manager" password="{noop}manager" authorities="ROLE_MEMBER" />
				<user name="admin" password="{noop}admin" authorities="ROLE_MEMBER,ROLE_ADMIN" /> <!-- 권한을 2개 줄수도 O / 두개 다 권한을 줬기 때문에 두개의 권한에 대한 페이지 이동 가능 -->
			</user-service>
		</authentication-provider>
	</authentication-manager>

</beans:beans>
```

2. *controller*

```java
package edu.bit.ex;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;

import lombok.extern.log4j.Log4j;

@Log4j
@Controller
@RequestMapping("/security/*")
public class SecurityController {
	
	@GetMapping("/all")
	public void doAll() {
		log.info("do all can access everybody");
	}
	
	@GetMapping("/member")
	private void doMember() {
		log.info("logined member");
	}
	
	@GetMapping("/admin")
	public void doAdmin() {
		log.info("admin only");
	}
}
```

3. *view(jsp)*

```jsp

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>    
<%@ taglib uri="http://www.springframework.org/security/tags" prefix="sec" %>
    
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
<h1>/sample/admin page</h1>


<p>principal : <sec:authentication property="principal"/></p>
<!-- 전에는 DB안에서 Model안에 넣어서 jsp로 넘김 근데 스프링시큐리티를 쓰면 그럴필요가 없음! 
user 정보를 메모리에 올리는데 기본적으로 세션안에 시큐리티 관련된 것들을 메모리에 올리고 세션 시간다되거나 날릴 때 or 필요가 없어질때 날림
 + 설정만으로 알아서 스프링시큐리티가 몇가지 객체를 만들어놓고 관리 -> 그 중 하나가 principal: jsp페이지에서 사용 가능  -->

<p>사용자 아이디 : <sec:authentication property="principal.username"/></p>

<a href="/customLogout">Logout</a>


</body>
</html>
```
<br>

<details><summary>화면 출력 </summary>

![화면 캡처 2021-02-17 142355](https://user-images.githubusercontent.com/74290204/108160142-da019000-712b-11eb-8979-5604ef478778.png)
</details>


#### ★ - 전에는 DB안에서 Model안에 넣어서 jsp로 넘김 → 근데 스프링시큐리티를 쓰면 그럴 필요가 없음!!!!
- user 정보를 메모리에 올리는데 기본적으로 세션안에 시큐리티 관련된 것들을 메모리에 올리고 <br> 세션 시간다되거나 날릴 때 or 필요가 없어질때 날림

- 설정만으로 알아서 스프링시큐리티가 세션안에 우리가 써먹을 수 있는 몇가지 객체를 만들어놓고 관리
    - 그 중 하나가 principal: jsp페이지에서 사용 가능  
<br>

### ④ 에러 처리
- 객체 생성해서 처리
- 권한 에러 처리 = 403 에러 처리(권한이 없는데 페이지 접속을 해서 나는 에러)

```xml
<!-- 403에러처리, 에러가 나면 /security/accessError로 이동  -->
<access-denied-handler error page="/security/accessError"/>
```

#### [★시큐리티 에러 처리 과정] 
- 시큐리티는 Dispatcher Servlet으로 가기 전에 낚아채서 권한 있는지 확인 → *403에러 발생*(권한이 없는데 접근) <br> → page(url)를 시큐리티가 Controlller에게 전달 → 이 에러 개발자에게 맡길게!  → 개발자가 Controller에서 맵핑해서 처리

1. *security-context.xml*

```xml
<http>
	<intercept-url pattern="/security/all" access="permitAll" /> <!-- 모두 접근 허용 -->
	<intercept-url pattern="/security/member" access="hasRole('ROLE_MEMBER')" /> <!-- ROLE_MEMBER 권한을 가진 사람만 접근 허용 -->
	<intercept-url pattern="/security/admin" access="hasRole('ROLE_ADMIN')" />
	<form-login />
		
	<!-- 403에러처리 -->
	<access-denied-handler error-page="/security/accessError"/>
</http>
```

2. *Conroller*

```java
@GetMapping("/accessError")
public void accessError(Authentication auth, Model model) {
//public void accessError(Principal principal, Model model) : Principal 객체도 컨트롤러에서 사용가능
	log.info("access denied" + auth);
	model.addAttribute("msg", "Access Denied");
}
```

3. *accessError.jsp*

```jsp
<h1>Access Denied Page</h1>

<!-- 내장객체 이런게 있다고 보여주려고 사용-->
<h2><c:out value="${SPRING_SECURITY_403_EXCEPTION.getMessage()}"/></h2>

<h2><c:out value="${msg}"/></h2>
```
<details><summary> 화면 출력 </summary>

![화면 캡처 2021-02-18 111143](https://user-images.githubusercontent.com/74290204/108295295-bfd2bb00-71da-11eb-914b-638425314999.png)

![화면 캡처 2021-02-18 110531](https://user-images.githubusercontent.com/74290204/108295298-c103e800-71da-11eb-85d3-0848197bd2c0.png)

- member는 admin에 대한 권한이 없는데 admin으로 접근 → 403에러 발생 → 시큐리티 Controller로 넘김 → Controller에서 개발자가 맵핑해준 페이지로 이동
</details>
<br>

### ⑤ logout : 시큐리티 내부적으로 Session을 알아서 날림

```xml
<http>
    <logout logout-url="/logout" logout-success-url="/" />
</http>
```

>> **url = "logout" 이렇게 반드시 맞춰줘야함** 안에 함수가 **디폴트로** 그렇게 **정의**되어있기 때문에! 
<br>

### ⑥ < form-login /> → customizing 하는 방법

```xml
<form-login login-page="/login/loginForm"/>  이 부분 커스텀마이징
```
1. *security-context.xml*
```xml
<http>
	<intercept-url pattern="/security/all" access="permitAll" /> <!-- 모두 접근 허용 -->
	<intercept-url pattern="/security/member" access="hasRole('ROLE_MEMBER')" /> <!-- ROLE_MEMBER 권한을 가진 사람만 접근 허용 -->
	<intercept-url pattern="/security/admin" access="hasRole('ROLE_ADMIN')" />
	
	<form-login login-page="/login/loginForm"
		default-target-url="/"
		authentication-failure-url="/login/loginForm?error"
		username-parameter="id" password-parameter="password" />
      
	<!-- login폴더안에 loginForm.jsp가 아니고 로그인 인증을 처리할 주소!! Controller 처리 주소
		default-target-url : 로그인이 완료되면 default-target-url로 이동 (이거 사용안해도 디폴트로  "/"로 이동) -->
</http>
```

2. *Controller*

```java
@GetMapping("/login/loginForm")
public String loginForm() {
	logger.info("Welcome Login Form!");
		
	return "login/loginForm";
}

@GetMapping("/login/loginForm")
public String loginForm() {
	logger.info("Welcome Login Form!");
		
	return "login/loginForm2";
}
```

3. *jsp*

```jsp
<!-- loginForm.jsp -->
<body onload="document.f.id.focus();">

<h3>아이디와 비밀번호를 입력해주세요.</h3>

<c:url value="/login" var="loginUrl" /> 
<p>${loginUrl}</p>
<form:form name="f" action="${loginUrl}" method="POST">
    <c:if test="${param.error != null}">
        <p>아이디와 비밀번호가 잘못되었습니다.</p>
    </c:if>
    <c:if test="${param.logout != null}">
        <p>로그아웃 하였습니다.</p>
    </c:if>
    <p>
        <label for="username">아이디</label>
        <input type="text" id="id" name="id" /> <!-- name은 xml에서 설정한 username-parameter명과 반드시 맞춰줘야함!-->
    </p>
    <p>
        <label for="password">비밀번호</label>
        <input type="password" id="password" name="password"/>
    </p>
    <%-- <input type="hidden" name="${_csrf.parameterName}" value="${_csrf.token}" /> --%>
    <button type="submit" class="btn">로그인</button>
</form:form>
```
<details><summary> 화면 출력 </summary>

![화면 캡처 2021-02-18 120336](https://user-images.githubusercontent.com/74290204/108299460-d03a6400-71e1-11eb-875e-9e0086e12b49.png)
</details>

```jsp
<!-- loginForm2.jsp (부트스트랩 적용) -->
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
<!DOCTYPE html>
<html lang="ko">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<%@ include file="/WEB-INF/include/header.jspf"  %>
<title>Login</title>
</head>
<body onload="document.f.id.focus();">
    <br><br>
    <div class="container text-center">
        <h1>로그인 페이지</h1><br>
    </div>
    <c:url value="/login" var="loginUrl" />
    <div class="container col-md-4">
	    <form:form name ="f" class="px-4 py-3" action="${loginUrl}" method="post">
	         <c:if test="${param.error != null}">
        		<p>아이디와 비밀번호가 잘못되었습니다.</p>
    		</c:if>
    			
    		<c:if test="${param.logout != null}">
        		<p>로그아웃 하였습니다.</p>
    		</c:if>
    			
	        <div class="form-group">
	            <label for="exampleDropdownFormEmail1">ID</label>
	            <input type="text" class="form-control" name="id" placeholder="example">
	        </div>

	        <div class="form-group">
	            <label for="exampleDropdownFormPassword1">Password</label>
	            <input type="password" class="form-control" name="password" placeholder="Password">
	        </div>

	        <div class="form-check">
	            <label class="form-check-label">
	            <input type="checkbox" class="form-check-input">Remember me</label>
	        </div>
<%-- 	          <input name="${_csrf.parameterName}" type="hidden" value="${_csrf.token}"/> --%>
	        <button type="submit" class="btn btn-primary">Sign in</button>
	   </form:form>
	      <div class="dropdown-divider"></div>
	      <a class="dropdown-item" href="#">New around here? Sign up</a>
	      <a class="dropdown-item" href="#">Forgot password?</a>
	</div>

</body>
</html>
```
<details><summary> 화면 출력 </summary>

![화면 캡처 2021-02-18 120000](https://user-images.githubusercontent.com/74290204/108299462-d16b9100-71e1-11eb-8c9b-041edaa77368.png)
</details>
<br>

### ③ 시큐리티 활용해 DB에 있는 데이터 가져와서 권한 설정3
- 미리 생성해 둔 AUTHORITIES, USERS 테이블 이용
>> [Spring_basic07 →  @Login을 위한 DB처리 https://github.com/seulpi/TIL/blob/main/Basic/Spring/Spring_basic07(Interceptor).md
] : 전자정부 프레임워크 참조했다고 함

#### ex1] 
*@ security-db-context.xml*
- ref="dataSource" → 커넥션풀 참조 

```xml
<beans:bean id="userDetailsService" class="org.springframework.security.core.userdetails.jdbc.JdbcDaoImpl">
    <beans:property name="dataSource" ref="dataSource"/>
</beans:bean> 
   
<!-- provider --> 
<authentication-manager>
    <authentication-provider> 
        <jdbc-user-service 
            data-source-ref="dataSource"
            role-prefix=""
            users-by-username-query="select username, password, enabled from users where username = ?"
            authorities-by-username-query="select username, authority from authorities where username = ?"
        /> 
    </authentication-provider>
</authentication-manager>
```

- **JdbcDaoImpl을 참조해** users-by-username-query="select username, password, enabled from users where username = ?" 를 **쿼리를 맵핑시킴** → UserDatailService를 호출 
	- **맵핑안하면** JdbcDaoImpl 안에 **정의된 디폴트 쿼리 실행**
	```xml
	<jdbc-user-Service> 는 JdbcDaoImp객체 생성
	<user-service> -> User 객체 생성 Userdetails 를 implements 함
	```

- [x] 실행시키기 위해 web.xml에서 context설정 체크
<details><summary> web.xml </summary>
	
```xml
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/spring/root-context.xml
		/WEB-INF/spring/security-db-context.xml</param-value>
	</context-param>
```
</details>
	
- [x]  root-context.xml 에 커넥션풀 확인(DB데이터 가져올거니까 DB연결되어있는지 확인하는 것)
<details><summary>root-context.xml</summary>

```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:mybatis-spring="http://mybatis.org/schema/mybatis-spring"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="http://mybatis.org/schema/mybatis-spring http://mybatis.org/schema/mybatis-spring-1.2.xsd
		http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd
		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.3.xsd
		http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.3.xsd">
	
	<!-- Root Context: defines shared resources visible to all other web components -->
		<!-- Root Context: defines shared resources visible to all other web components -->
	
	<bean id="hikariConfig" class="com.zaxxer.hikari.HikariConfig">
		<property name="driverClassName"
			value="net.sf.log4jdbc.sql.jdbcapi.DriverSpy"></property>
		<property name="jdbcUrl"
			value="jdbc:log4jdbc:oracle:thin:@localhost:1521:XE"></property>
		<property name="username" value="scott"></property>
		<property name="password" value="tiger"></property>
	</bean>

	<!-- HikariCP configuration -->
	<bean id="dataSource" class="com.zaxxer.hikari.HikariDataSource"
		destroy-method="close">
		<constructor-arg ref="hikariConfig" />
	</bean>

	<!-- 1.번방법을 위하여 mapperLocations 을 추가 함 -->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource"/>

	</bean>
	<!-- 1번 방식 사용을 위한 sqlSession -->
	<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
		<constructor-arg index="0" ref="sqlSessionFactory" />
	</bean>
	
	<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
			<property name="dataSource" ref="dataSource" />
		</bean>
		
		<tx:annotation-driven transaction-manager="transactionManager" />    
	</beans>
```
</details>

- [x]  spring5 부터는 **패스워드 인코딩 처리(암호화) 반드시 해줘야함**
	- 학원에서 암호화 설명 전이라 일단 테스트용 이렇게 설정 (실무에서는 이렇게 사용x) 
	- for 암호화: CustomNoOpPasswordEncoder.java
	<details><summary>CustomNoOpPasswordEncoder.java 코드</summary>

	```java
	package edu.bit.ex.security;

	import org.springframework.security.crypto.password.PasswordEncoder;

	import lombok.extern.log4j.Log4j;

	@Log4j
	public class CustomNoOpPasswordEncoder implements PasswordEncoder {

		@Override //굉장히 간단한 암호화
		public String encode(CharSequence rawPassword) {

			log.warn("before encode :" + rawPassword);

			return rawPassword.toString();
		}

		@Override
		public boolean matches(CharSequence rawPassword, String encodedPassword) {

			log.warn("matches: " + rawPassword + ":" + encodedPassword);

			return rawPassword.toString().equals(encodedPassword);
		}
	}
	```
	</details>
	<details><summary>security-db-context.xml 암호화 추가 부분</summary>
	
	![화면 캡처 2021-02-18 130939](https://user-images.githubusercontent.com/74290204/108304159-bf422080-71ea-11eb-9ca3-f1285e52f4a7.png
	</details>
	
	- test진행 : security/admin → DB에 admin만 권한 조건이 맞기 때문에 admin으로 test
	- <details><summary>화면 출력</summary>
	
	![화면 캡처 2021-02-18 130801](https://user-images.githubusercontent.com/74290204/108510540-0111ba80-7302-11eb-990b-a6fe74995ce6.png)
	</details>


	```xml
	role-prefix=""
		       users-by-username-query="select username, password, enabled from users where username = ?"
		       authorities-by-username-query="select username, authority from authorities where username = ?
	<!--이 코드가 없어도 돌아감 why? JdbcDaoIml 객체 안에 디폴트 쿼리가 실행되기 때문 -->
	```


#### ex2]
1. *Controller*

```java
package edu.bit.ex;

import java.text.DateFormat;
import java.util.Date;
import java.util.Locale;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import lombok.extern.log4j.Log4j;

@Log4j
@Controller
public class HomeController {
	
	@RequestMapping(value = "/", method = RequestMethod.GET)
	public String home(Locale locale, Model model) {
		log.info("Welcome home! The client locale is {}.");
		return "home";
	}
	
	@GetMapping("/user/userHome")
	public void userHome() {
		log.info("userHome..");
	}
	
	@GetMapping("/admin/adminHome")
	public void adminHome() {
		log.info("adminHome..");
	}
	
	@GetMapping("/login/acessDenied")
	public void accessDenied(Model model) {
		log.info("accessDenied..");
	}

	@GetMapping("/login/loginForm")
	public String loginForm() {
		log.info("Welcome Login Form!");
		
		return "login/loginForm2";
	}	
}
```

2. *jsp*
-  ※ 주의) controller에서void로 처리했을시 jsp는 url 경로에 따라 맞춰줘야함 → "/login/home"이면 login 폴더 안에 home.jsp 

```jsp
<!--home-->
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
<%@ taglib prefix="sec" uri="http://www.springframework.org/security/tags" %>
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
   <title>메인페이지</title>
</head>

<body>

<h1>메인페이지</h1>

<sec:authorize access="isAnonymous()">
   <p><a href="<c:url value="/login/loginForm" />">로그인</a></p>
</sec:authorize>

<sec:authorize access="isAuthenticated()">
   <form:form action="${pageContext.request.contextPath}/logout" method="POST">
       <input type="submit" value="로그아웃" />
   </form:form>
   <p><a href="<c:url value="/loginInfo" />">로그인 정보 확인 방법3 가지</a></p>
</sec:authorize>

<h3>
    [<a href="<c:url value="/user/userForm" />">회원가입</a>]
    <!--  c:url 절대 경로 만들어줌 <c:url value="/user/userForm" /> = ${pageContext.request.contextPath}/user/userForm -->
    [<a href="<c:url value="/user/userHome" />">유저 홈</a>]
    [<a href="<c:url value="/admin/adminHome" />">관리자 홈</a>]
</h3>
</body>
</html>
```
<details><summary>화면 출력</summary>

![화면 캡처 2021-02-18 151459](https://user-images.githubusercontent.com/74290204/108313686-16042600-71fc-11eb-8940-6f59c944556c.png)
</details>

```jsp
<!-- userHome -->
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
<%@ taglib prefix="sec" uri="http://www.springframework.org/security/tags" %>
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
   <title>유저 페이지</title>
</head>

<body>

<h1>유저 페이지 입니다.</h1>

<p>principal: <sec:authentication property="principal"/></p>
<%-- <p>EmpVO: <sec:authentication property="principal.emp"/></p>
<p>사용자이름: <sec:authentication property="principal.emp.ename"/></p>
<p>사용자월급: <sec:authentication property="principal.emp.sal"/></p>
<p>사용자입사일자: <sec:authentication property="principal.emp.hiredate"/></p> --%>
<p><a href="<c:url value="/" />">홈</a></p>

</body>
</html>
```
```jsp
<!-- adminHome -->
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
<%@ taglib prefix="sec" uri="http://www.springframework.org/security/tags" %>
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <title>관리자 홈</title>
</head>

<body>

<h1>관리자 페이지 입니다.</h1>

<h3>[<a href="<c:url value="/" />">홈</a>]</h3>

</body>
</html>
```
```jsp
<!-- accessDenied -->
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <title>Access Denied</title>
</head>

<body>

<h1>Access Denied!</h1>

<h3>[<a href="<c:url value="/" />">홈</a>]</h3>

</body>
</html>
```

3. *security-db-context.xml*
-  jsp 페이지에서 에러 페이지 경로 바뀌었기 때문에 xml error 경로 수정!
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans
	xmlns="http://www.springframework.org/schema/security"
	xmlns:beans="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/security http://www.springframework.org/schema/security/spring-security.xsd
      http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

	<http auto-config="true" use-expressions="true">
		<!-- 제한은 좁은데서 큰쪽으로와야 좁은 권한부터 체크한다 (큰데서 좁은데 오면 밑에는 안먹힘 예를 들어 Exception처럼 에러 조상님 선언하면 밑에 하위에러 처리해도 안먹는것처럼) -->
		<intercept-url pattern="/login/loginForm" access="permitAll" />
		<intercept-url pattern="/" access="permitAll" />
		<intercept-url pattern="/admin/**" access="hasRole('ADMIN')" /> <!-- ROLE_MEMBER 권한을 가진 사람만 접근 허용 -->
		<intercept-url pattern="/**" access="hasAnyRole('USER, ADMIN')" />
	
		<form-login login-page="/login/loginForm"
		default-target-url="/"
		authentication-failure-url="/login/loginForm?error"
		username-parameter="id" 
		password-parameter="password" />
     
		<logout logout-url="/logout" logout-success-url="/" /> 
		
		<!-- 403에러처리 -->
		<access-denied-handler error-page="/login/accessDenied"/>
	</http>
	
	<beans:bean id="userDetailsService" class="org.springframework.security.core.userdetails.jdbc.JdbcDaoImpl">
        <beans:property name="dataSource" ref="dataSource"/>
    </beans:bean> 
    
   <beans:bean id="customNoOpPasswordEncoder" class="edu.bit.ex.security.CustomNoOpPasswordEncoder"/>
   
	<!-- provider --> 
   <authentication-manager>
      <authentication-provider> 
         <password-encoder ref="customNoOpPasswordEncoder"/> 
         <jdbc-user-service 
               data-source-ref="dataSource"
             
           /> 
      </authentication-provider>
   </authentication-manager>
</beans:beans>
```

▶ 화면 출력

![화면 캡처 2021-02-18 150919](https://user-images.githubusercontent.com/74290204/108313251-6038d780-71fb-11eb-9c4a-57f085fc8a67.png)
