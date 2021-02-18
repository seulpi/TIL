# Q. DB에 있는 EMP테이블로 스프링시큐리티 로그인을 구현하시오
- ename(사원명)을 ID로 empno(사원번호)를 비밀번호로 설정할 것 
- mgr인 사람은 admin관리자로 사원인 사람들은 그냥 user로 권한 설정해서 리소스접근을 다르게 할 것

## *Answer* 
+ DB 컬럼추가, xml설정 수정만 내가 하고 나머지는 수업시간에 한걸로 진행함

## 1. 일단 DB EMP 테이블에 Autority, Enabled 컬럼 추가 
- 처음에 Enabled 컬럼 추가없이 했더니 로그인이 안되었다<br>
 → **Enabled**가 계정에 대한 **활성, 비활성 여부(0-비활성화 1-활성화)**이기 때문에 컬럼을 추가해서 체크해줘야한다 <br> **users-by-username-query에서 username, password, enabled는 필수이기 때문에 DB에 이 컬럼들이 반드시 있어야함**

```sql
--컬럼추가
alter table emp add AUTHORITY varchar2(50);
alter table emp add ENABLED char(1) DEFAULT '1';

-- 컬럼에 대한 값 추가(권한 설정)
-- mgr
update  emp set authority = 'ROLE_ADMIN' where ename in (select DISTINCT(M.ename) from emp M, emp E where M.empno = E.mgr);
-- 사원
update  emp set authority = 'ROLE_USER' where ename not in (select DISTINCT(M.ename) from emp M, emp E where M.empno = E.mgr);

commit;

--테이블 잘 들어갔는지 확인
select * from emp;
```

![화면 캡처 2021-02-18 175449](https://user-images.githubusercontent.com/74290204/108331755-6c7c5f00-7212-11eb-92a0-9b7cf423c07e.png)

## 2. security-db-context.xml에서 쿼리 수정
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns="http://www.springframework.org/schema/security"
    xmlns:beans="http://www.springframework.org/schema/beans"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://www.springframework.org/schema/security http://www.springframework.org/schema/security/spring-security.xsd
      http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   
   
     <http auto-config="true" use-expressions="true">
        <intercept-url pattern="/login/loginForm" access="permitAll" />
        <intercept-url pattern="/" access="permitAll" />
        <intercept-url pattern="/admin/**" access="hasRole('ADMIN')" />
        <intercept-url pattern="/**" access="hasAnyRole('USER, ADMIN')" />
      
      
      <!--로그인 페이지 커스텀 화    -->
      <form-login login-page="/login/loginForm"
                    default-target-url="/"
                    authentication-failure-url="/login/loginForm?error"
                    username-parameter="id"
                    password-parameter="password" />
      
      <logout logout-url="/logout" logout-success-url="/" /> 
                
      <!-- 403 에러 처리 -->
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
               users-by-username-query="select ename as username, empno as password, enabled from emp where ename = ?"
               authorities-by-username-query="select ename as username, authority from emp where ename = ?"
        /> 
      </authentication-provider>
   </authentication-manager>
    
    
</beans:beans>
```

## 3. contorller mapping
```java
package edu.bit.ex;

import java.util.Locale;

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

## 4. jsp 설정

- home.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
<%@ taglib prefix="sec" uri="http://www.springframework.org/security/tags" %>
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
   <title>메이페이지</title>
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

- userHome

```jsp

<h1>유저 페이지 입니다.</h1>

<p>principal: <sec:authentication property="principal"/></p>
<%-- <p>EmpVO: <sec:authentication property="principal.emp"/></p>
<p>사용자이름: <sec:authentication property="principal.emp.ename"/></p>
<p>사용자월급: <sec:authentication property="principal.emp.sal"/></p>
<p>사용자입사일자: <sec:authentication property="principal.emp.hiredate"/></p> --%>
<p><a href="<c:url value="/" />">홈</a></p>
```

- adminHome.jsp

```jsp
<h1>관리자 페이지 입니다.</h1>

<h3>[<a href="<c:url value="/" />">홈</a>]</h3>
```

### ▶ 화면 출력
![화면 캡처 2021-02-18 172646](https://user-images.githubusercontent.com/74290204/108330973-9e40f600-7211-11eb-9c59-5e113ae00683.png)

![화면 캡처 2021-02-18 172707](https://user-images.githubusercontent.com/74290204/108330989-a305aa00-7211-11eb-8375-fd8b35d47635.png)


![화면 캡처 2021-02-18 172741](https://user-images.githubusercontent.com/74290204/108331180-d8aa9300-7211-11eb-92d7-b7a5af7bc859.png)

![화면 캡처 2021-02-18 172826](https://user-images.githubusercontent.com/74290204/108331016-aef16c00-7211-11eb-85f8-0afff2e3098a.png)

![화면 캡처 2021-02-18 172849](https://user-images.githubusercontent.com/74290204/108331040-b44eb680-7211-11eb-9a19-02b5c6b96e66.png)
