>>[에러났을때 정확한 정보 확인 사이트]  https://stackoverflow.com/
# user객체 라이브러리 따라가서 코드 보는거 추가하기!!!!!!!!!!
# Spring Security : 인증과 권한에 대한 프레임워크
- 보안과 관련 
- 스프링 시큐리티라는 것은 core의 넣는 하나의 조립품으로 **하나의 기능일 뿐**<br>(스프링 시큐리티도 하나의 프레임워크 : 개발자가 사용하기 편하게 캡슐화한 것) 
    - Q. **스프링시큐리티**가 뭔가요?  스프링 시큐리티란 **인증과 권한에 대한 프레임워크**
>> core?  Mapven Dependencies  - spring-core-5.0.7RELEASE-jar 이 부분 → 이 안에 IOC 컨테이너가 다 존재 

![화면 캡처 2021-02-18 185143](https://user-images.githubusercontent.com/74290204/108339292-99347480-721a-11eb-9777-279f1a531385.png)

![화면 캡처 2021-02-18 185158](https://user-images.githubusercontent.com/74290204/108339303-9c2f6500-721a-11eb-83c9-d4832544e941.png)

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
    - 밎춰도 에러가 날때 있음(오픈소스이다보니까)
    - https://mvnrepository.com/
  
    ![화면 캡처 2021-02-18 190547](https://user-images.githubusercontent.com/74290204/108340966-8458e080-721c-11eb-8479-5c330edf2561.png)
    ![화면 캡처 2021-02-18 190656](https://user-images.githubusercontent.com/74290204/108340969-858a0d80-721c-11eb-9cfe-e6590b6bc004.png)

### 1. xml 설정

#### *- pom.xml*
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
#### ▶ 이 라이브러리를 **왜 다운로드** 받는가? **인증과 권한을 컨트롤**하기 위함! 
>> 소셜로그안할때 핵심개념 = ★ 인증과권한 (시큐리티의 기본)

#### *- web.xml* → filter 설정
- filter name도 바뀌서는 안됨(그냥 내비두기) / 위치 : 한글 필터 밑

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

#### *- security-context.xml* → root-context.xml를 복붙(※같은자리에 붙여넣기)
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

### ▶ 여기까지 설정하고 "context명"/login하면 화면 출력
![화면 캡처 2021-02-16 121256](https://user-images.githubusercontent.com/74290204/108015218-0f867a80-7053-11eb-82e0-d4da638a8ccd.png)

#### why] form을 만들어 주지도 않았는데 어떻게 이런 form형태의 페이지가 출력되는지?
- 내가 만들어 준 페이지가 아닌데 누군가 응답해서 페이지를 출력 <br> → login이라는 경로를 누가 받아줌, interceptor처럼 누군가가 낚아채가는 중! <br>(내가 controller에서 경로 처리 안해줬는데 페이지가 구현중이니까)
- **★ 응답 해주는 주체 : 스프링 시큐리티** <br> → 스프링 시큐리티 필터에서 낚아챔(필터니까 Controller가기 전에 낚아채기 가능)
- 페이지는 어디서 구현되는가? Maven 라이브러리

## @ 시큐리티 실행되는 위치
- 로그인을 어디서 낚아채는가? Dispather Servlet 가기전에!
    - why? - 해킹 때문에 filter에서 작동

![security](https://user-images.githubusercontent.com/74290204/108144440-fe01a900-710c-11eb-97bd-b4278a67237b.png)


## @ 시큐리티 사용 
### ① interceptor : 권한 설정
- form-login처럼 디폴트로 생성해주지않기 때문에 직접 controller, view등을 세팅해줘야함

#### *- security-context.xml*
```xml
<intercept-url pattern="/security/all" access="permitAll" />
<intercept-url pattern="/security/member" access="hasRole('ROLE_MEMBER')" /> 
```
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
- {noop} 5.0부터는 패스워드를 인코딩하게 되어있어서(패스워트 암호화) 이걸 줘야 에러 안나고 패스워드를 읽어들임
- noop는 Spring Security에서 텍스트 그대로 비밀번호로 인식하게 해줌<br>
noop을 붙이지 않으면 다른 인코딩으로 암호화를 해줘야함 
>> [spring security 공식블로그 설명 참조]https://spring.io/blog/2017/11/01/spring-security-5-0-0-rc1-released#password-encoding 
<br>
▶ {noop} 사용안했을 때 에러 

![화면 캡처 2021-02-17 121849](https://user-images.githubusercontent.com/74290204/108151555-555a4600-711a-11eb-80c3-47bb62561a7e.png)


#### *- controller*
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

#### *- view(jsp)*
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

#### ▶ 화면 출력

![화면 캡처 2021-02-17 112009](https://user-images.githubusercontent.com/74290204/108149013-52a92200-7115-11eb-9c96-7a1a2936d50c.png)

#### *☞ ★시큐리티 내부 동작*
- 체크가 안되는 권한에 대해서는 내부적으로 인증을 하기 위한 로그인을 다시하라고 되돌려 보냄 <br> (ROLE_MEMBER권한을 가지고 있는지)
- 인증 → 권한 → 리소스 접근
>> 리소스가 뭔지? all.jsp 같은 파일


### ② DB에서 데이터를 가져오기
+ 실무에서는 이렇게 잘 사용안하지만 테스트를 위함 (실무에서는 DB에서 가져옴)
```xml
<authentication-manager>
      <authentication-provider> 
         <user-service> 
            <user name="member" password="{noop}member" authorities="ROLE_MEMBER" /> 
         </user-service> 
      </authentication-provider>
   </authentication-manager>
```

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

#### ▶ 화면 출력 

![화면 캡처 2021-02-17 121531](https://user-images.githubusercontent.com/74290204/108151386-e41a9300-7119-11eb-943e-9a08541ef446.png)

### Q. manager로 로그인하는것을 스프링시큐리티를 적용하세요
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

### ③ principal : jsp에서 써먹기 위한 용도 

1. security-context.xml
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

2. controller
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

3. view(jsp)
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

▶ 화면 출력
![화면 캡처 2021-02-17 142355](https://user-images.githubusercontent.com/74290204/108160142-da019000-712b-11eb-8979-5604ef478778.png)

#### ★ - 전에는 DB안에서 Model안에 넣어서 jsp로 넘김 → 근데 스프링시큐리티를 쓰면 그럴 필요가 없음!!!!
- user 정보를 메모리에 올리는데 기본적으로 세션안에 시큐리티 관련된 것들을 메모리에 올리고 <br> 세션 시간다되거나 날릴 때 or 필요가 없어질때 날림

- 설정만으로 알아서 스프링시큐리티가 세션안에 우리가 써먹을 수 있는 몇가지 객체를 만들어놓고 관리
    - 그 중 하나가 principal: jsp페이지에서 사용 가능  
