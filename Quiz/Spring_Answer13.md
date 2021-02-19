# 1. emp 테이블을 스프링 시큐리티에서 커스텀마이징 하시오
<br>

## - VO 
### *+ EmpVO*
- emp table은 1:N이 아니기 때문에 List로 자식을 가져올 필요가 없기 때문에 변수를 한번에 다 넣었다 (변수명 → 컬럼명)
```java
package edu.bit.ex.vo;

import java.sql.Timestamp;

import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;
import lombok.ToString;

@Getter
@Setter
@ToString
@AllArgsConstructor
@NoArgsConstructor
public class EmpVO {
	
	private String empno;
	private String ename;
	private String job;
	private Timestamp hiredate;
	private int sal;
	private int comm;
	private int deptno;
	private String authority;

}
```

### *+ EmpUser*
- 학원실습예제는 1:N이어서 List로 담은 값을 for문을 사용해 add해줬는데 List형식으로 저장한게 없기 때문에 그냥 add를 한줄로 처리해도된다(요 케이스에 한해)
- return null;로 해서 에러남 당연히 값을 저장했으면 그 값을 돌려줘야함! (return 확인 잘 할것)
```java
package edu.bit.ex.vo;

import java.util.ArrayList;
import java.util.Collection;
import java.util.List;

import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.User;

import lombok.Getter;
import lombok.Setter;
import lombok.ToString;

@Getter
@Setter
@ToString
public class EmpUser extends User {
	
	private EmpVO emp;
	
	public EmpUser(String username, String password, Collection<? extends GrantedAuthority> authorities) {
		super(username, password, authorities);
	}
	
	public EmpUser(EmpVO empVO) {
		super(empVO.getEname(), empVO.getEmpno(), getAuth(empVO)); // Sting, String 형식이어서 일단 empno를 String으로 바꿔줬는데 될지..?
		this.emp = empVO;
		
	}
	
// 권한이 두개 이상일때 적용하려고 만드는 함수(현재 만들어놓은 emp authority에는 한개씩만 넣어서 상관없지만 위에 함수 오버라이딩하는게 Collection으로 들어가야하기 때문에 정의해줘야함)
	private static Collection<? extends GrantedAuthority> getAuth(EmpVO empVO) {

		List<GrantedAuthority> authorities = new ArrayList<GrantedAuthority>();
	
		authorities.add(new SimpleGrantedAuthority(empVO.getAuthority()));
	     
		return authorities;
	}
}
```

## - Service
### *+ EmpDetailsService*
```java
package edu.bit.ex.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

import edu.bit.ex.mapper.EmpMapper;
import edu.bit.ex.vo.EmpUser;
import edu.bit.ex.vo.EmpVO;
import lombok.Setter;
import lombok.extern.log4j.Log4j;

@Log4j
@Service
public class EmpDetailsService implements UserDetailsService{
	
	@Setter(onMethod_ = @Autowired)
	private EmpMapper empMapper;

	@Override
	public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
		
		 log.warn("Load User By EmpVO number: " + username);      
	      EmpVO vo = empMapper.getEmp(username);      
	      
	      log.warn("queried by EmpVO mapper: " + vo);
	      
	      return vo == null ? null : new EmpUser(vo);
	}

}
```

## - Mapper
### *+ EmpMapper.java*
```java
package edu.bit.ex.mapper;

import edu.bit.ex.vo.EmpVO;

public interface EmpMapper {

	EmpVO getEmp(String username);

}
```

### *+ EmpMapper.xml*
- 역시 마찬가지로 1:N 관계 아니기 때문에 resultMap해줄 이유가 없음
- 변수명 잘못써서 에러 한번 났다 → empname으로 씀 변수명 ename으로 주고 컬럼명도 ename인데..ㅎ
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
	PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
	"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="edu.bit.ex.mapper.EmpMapper">

	<select id="getEmp" resultType="edu.bit.ex.vo.EmpVO">
		select * from emp where ename = #{ename}
	</select>

</mapper>
```

## - context.xml
- web.xml에 security-custom-context.xml로 등록되어있고 학원에서 진행하는 것에 객체만 바꾸면 되서 객체만 바꿔줌 
### *+ security-custom-context.xml*
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
   
   <beans:bean id="bcryptPasswordEncoder"
      class="org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder" />

      
   <beans:bean id="customNoOpPasswordEncoder" class="edu.bit.ex.security.CustomNoOpPasswordEncoder"/>
  <!--  <beans:bean id="memberDetailsService"   class="edu.bit.ex.security.MemberDetailsService" />   -->
 	<beans:bean id="empDetailsService"   class="edu.bit.ex.service.EmpDetailsService" />
 
   <authentication-manager>
      <!-- <authentication-provider user-service-ref="memberDetailsService"> -->
       <authentication-provider user-service-ref="empDetailsService"> 
         <password-encoder ref="customNoOpPasswordEncoder"/>      
      </authentication-provider>
   </authentication-manager>
</beans:beans>
```

## - View
### *+ userHome*
```jsp
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
<%-- <p>principal: <sec:authentication property="principal.member.username"/></p> --%>
<%-- <p>EmpVO: <sec:authentication property="principal.emp"/></p> --%>
<p>사용자이름: <sec:authentication property="principal.emp.ename"/></p>
<p>사용자월급: <sec:authentication property="principal.emp.sal"/></p>
<p>사용자입사일자: <sec:authentication property="principal.emp.hiredate"/></p>
<p><a href="<c:url value="/" />">홈</a></p>

</body>
</html>
```

<details><summary>controller, login 페이지 처리(기존에 있는 코드 활용)</summary>

- HomeController
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

</details>

## ▶ 화면 출력

![화면 캡처 2021-02-19 173402](https://user-images.githubusercontent.com/74290204/108480881-d8c29580-72da-11eb-94dc-06a720f16b59.png)

![화면 캡처 2021-02-19 173413](https://user-images.githubusercontent.com/74290204/108480885-d9f3c280-72da-11eb-9f95-d4f166927331.png)

