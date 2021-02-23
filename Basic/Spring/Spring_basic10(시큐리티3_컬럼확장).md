# 시큐리티 정보 확장 (Customizing) → 암기! (이해의영역X)
- 기본적으로 로그인 처리할때 **시큐리티는 username, password, enabled만 디폴트로 가져올 수 있음** → 다른 컬럼 정보들 못 가져옴(**시큐리티 인증과 권한까지만 처리를 해줌**) 
    - 시큐리티는 로그인을 시키면 로그인 정보를 session 메모리에 올려서 jsp에서 principal로 사용(전에는 EL사용했었음)
    - ★ contextholder, principal, authorfication → 세션 메모리에 있는 객체 암기
    ```jsp
    <p>principal: <sec:authentication property="principal"/></p>
    <p>principal: <sec:authentication property="principal.username"/></p>
    ```

- **로그인의 기본목적은 유저의 정보를 올리고 써먹기 위함** 따라서 DB설계를 할때도 회원에 대한 정보에 관련된 컬럼이 여러개 존재 가능함(이메일, 성별 등) 

### ▶ 따라서 나머지 컬럼들은 커스텀마이징해서 가져와야함

>> 예전에는 로그인을 interceptor를 이용했었는데 스프링시큐리티를 이용하면서 스프링시큐리티에게 넘겨줌 <br> → 따라서 로그인을 이제 interceptor로 할 필요가 없는 것


## @ 확장 방법

### 1. 다이렉트로 UserDetails 설정하는 방법
```java
public class MemberUser2 implements UserDetails { } 
```

### ★ 2. UserDetailsService, User 로 확장
- User안에 UserDetails 상속해서 정의해놨기 때문에 User를 implements가 가능한 것

- [x] security-custom-context.xml에 객체생성
```xml
<!-- <jdbc-user-service> 이거 사용 안하고 내가 확장시켜서 쓰겠다-->
<beans:bean id="memberDetailsService"   class="edu.bit.ex.security.MemberDetailsService" />


<!--MemberDetailsService.java 를 끌고 온 것 (MemberDetailsService 객체생성)-->
<beans:bean id="memberDetailsService"   class="edu.bit.ex.security.MemberDetailsService" />  

   <authentication-manager>
      <authentication-provider user-service-ref="memberDetailsService">
         <password-encoder ref="customNoOpPasswordEncoder"/>      
      </authentication-provider>
   </authentication-manager>
```
- 세션 메모리 공간인 Authentication 안에 UserDetails를 메모리에 올리는 것

---

1. VO

- *AuthVO*
```java
import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;
import lombok.ToString;

@AllArgsConstructor
@NoArgsConstructor
@Setter
@Getter
@ToString
public class AuthVO {
   private String username;
   private String authority;
}
```

- *MemberVO/8
```java
package edu.bit.ex.vo;

import java.util.List;

import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;
import lombok.ToString;

@AllArgsConstructor
@NoArgsConstructor
@Setter
@Getter
@ToString
public class MemberVO {
   private String username;
   private String password;
   private String enabled;
   
   // user, authority는 1:N
   private List<AuthVO> authList;
}
```

- *MemberUser*
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

@Setter
@Getter
@ToString
public class MemberUser extends User {
	
	   private MemberVO member;
	   
	   //기본적으로 부모의 생성자를 호출해야만 정상적으로 작동
	   public MemberUser(String username, String password, Collection<? extends GrantedAuthority> authorities) {
	      super(username, password, authorities);
	   }
	   
	   public MemberUser(MemberVO memberVO) {
	            
	      super(memberVO.getUsername(), memberVO.getPassword(),getAuth(memberVO)); // username, password는 반드시 넘겨줘야하고 getAuth는 선생님이 만든 함수 

	      this.member = memberVO;
	   }
	   
	   //유저가 갖고 있는 권한 목록
	   public static Collection<? extends GrantedAuthority> getAuth(MemberVO memberVO) { 

	      List<GrantedAuthority> authorities = new ArrayList<GrantedAuthority>();
	      
	      for (AuthVO auth: memberVO.getAuthList()) {
	         authorities.add(new SimpleGrantedAuthority(auth.getAuthority()));
	      }
	      
	      return authorities;
	   }   
}
```

2. Service
```java
package edu.bit.ex.security;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

import edu.bit.ex.mapper.MemberMapper;
import edu.bit.ex.vo.MemberUser;
import edu.bit.ex.vo.MemberVO;
import lombok.Setter;
import lombok.extern.log4j.Log4j;

@Log4j
@Service
public class MemberDetailsService implements UserDetailsService  {
   
   @Setter(onMethod_ = @Autowired)
   private MemberMapper memberMapper;
   
   @Override
   public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
		/*
		 * 이 함수가 핵심! UserDetailsService를 implements를 하면 loadUserByUsername 구현하게됨
		 * loadUserByUsername ↑username에는 유저로부터 id가 넘어옴(로그인할때 받드시 loadUserByUsername호출하게
		 * 되어있음) 이 함수를 오버라이딩하는 것은 내가 정의해서 리턴시켜주겠다는 뜻임
		 */
      log.warn("Load User By MemberVO number: " + username);      
      MemberVO vo = memberMapper.getMember(username);      
      
      log.warn("queried by MemberVO mapper: " + vo);
      
      return vo == null ? null : new MemberUser(vo);
   }
}
```

3. Mapper, Mapper.xml

- *interface Mapper*
```java
package edu.bit.ex.mapper;

import org.apache.ibatis.annotations.Mapper;
import edu.bit.ex.vo.MemberVO;

@Mapper
public interface MemberMapper {
   MemberVO getMember(String username);
}
```

- *Mapper.xml*
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
	PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
	"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="edu.bit.ex.mapper.MemberMapper">

	<resultMap id="memberMap" type="edu.bit.ex.vo.MemberVO">
	    <result property="username" column="username"/>
	    <result property="password" column="password"/>
	    <result property="enabled" column="enabled"/>
		<collection property="authList" resultMap="authMap"></collection>
	</resultMap>
	
	<resultMap id="authMap" type="edu.bit.ex.vo.AuthVO">
		<result property="username" column="username"/>
		<result property="authority" column="authority"/>
	</resultMap>
	
	<select id="getMember" resultMap="memberMap">
		select * from users , authorities 
		where users.username = authorities.username and users.username = #{username}
	</select>
</mapper>
```

4. *security-custom-context.xml*
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
   <beans:bean id="memberDetailsService"   class="edu.bit.ex.security.MemberDetailsService" />  

   <authentication-manager>
      <authentication-provider user-service-ref="memberDetailsService">
         <password-encoder ref="customNoOpPasswordEncoder"/>      
      </authentication-provider>
   </authentication-manager>

</beans:beans>
```
▶ 실행 전에 web.xml에 실행할 xml 설정되어있는지 반드시 확인

# - UserDetails
- **loadUserByUsername을 누군가 호출! = 'Security'**
- 유저가 로그인할 때 시큐리티가 수많은 함수들을 호출하는데 'loadUserByUsername'도 호출 → 유저 ID만 주면 UserDetails에 맞춰 return을 해줘야함 
```java
return vo == null ? null : new MemberUser(vo);
```

## @  리턴하는 방법
### 1. UserDetails를 'implements'하는 방법(다이렉트로 상속받는 것)
### 2. User → UserDetails를 상속받아서 정의해놓은 클래스를 'implements'하는 방법
- ※ 주의] 객체생성은 반드시 getter&setter 만들어줘야함
```java
private MemcberVO member;
```

## @ UserDetails와 principal
```jsp
<p>principal: <sec:authentication property="principal.member.username"/>
```
▶  principal과 UserDetails의 관계과 뭐길래 jsp에서 principal로 UserDetails 사용이 가능한가? → principal.member는 getMember() 호출 <br>
- 시큐리티는 로그인 정보들을 session에 저장 → 저장하면 전역에 사용 가능 → session에 어떤걸 올렸는지 알아야 사용이 가능(시큐리티가 메모리에 올릴때 이름 : key를 알아야 사용이 가능) 
#### ★ UserDetails의 key가 Principal!! ☞ principal로 UserDetails 꺼내온다
- UserDetails-value의 세션 메모리 이름 

### [ 증명하는 방법: 세션을 어떻게 관리하는가 ]
>> (한번 읽어보기) https://velog.io/@sa833591/Spring-Security-4-Authentication-SecurityContextHolder%EC%9D%98-%EC%9D%B4%ED%95%B4

- pincipal=UserDetails
```java
//Controller

package edu.bit.ex;

import java.security.Principal;

import org.springframework.security.core.Authentication;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import edu.bit.ex.service.UserService;
import edu.bit.ex.vo.MemberUser;
import edu.bit.ex.vo.UserVO;
import lombok.AllArgsConstructor;
import lombok.extern.log4j.Log4j;

@Log4j
@Controller
@AllArgsConstructor
public class UserController {
	
	private UserService userService;
	
	@GetMapping("/user/userForm")
	public String userHome() {
		log.info("welcome userForm!");
		return "user/userForm";
	}
	
	@PostMapping("/user/addUser")
	public String addUser(UserVO userVO) {
		log.info("post register");
		
		userService.addUser(userVO);
		
		return "redirect:/";
	}
	
	@GetMapping("/loginInfo")
	   public String loginInfo(Principal principal) {
	      
	    // 1.Controller를 통하여 Pincipal객체로 가져오는 방법
	    String user_id = principal.getName();
	    System.out.println("유저 아이디:" + user_id);

	    // 2.SpringContextHolder를 통하여 가져오는 방법(일반적인 빈에서 사용 할수있음 )
	    Authentication auth = SecurityContextHolder.getContext().getAuthentication();
	    user_id = auth.getName();
	    System.out.println("유저 아이디:" + user_id);

	    // 3.
	    UserDetails userDetails = (UserDetails) auth.getPrincipal();
	    System.out.println(userDetails.getUsername());

	    //4. ****polymortism 적용 MemberVO - User - UserDetails
	    MemberUser memberUser = (MemberUser) auth.getPrincipal();
	    System.out.println(memberUser.getPassword());

	      
	    // 4.User 클래스로 변환 하여 가져오는 방법
	    User user = (User) SecurityContextHolder.getContext().getAuthentication().getPrincipal();
	    user_id = user.getUsername();
	    System.out.println("유저 아이디:" + user_id);

	    return "home";

	}
	
}
```
```jsp
<!--jsp에서 사용 방법-->

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

<!-- 로그인 하지 않은 모든 사용자(로그인 중인 사용자에게는 보이지 않음) -->
<sec:authorize access="isAnonymous()">
   <p><a href="<c:url value="/login/loginForm" />">로그인</a></p>
</sec:authorize>

<!-- 로그인 중인 사용자  -->
<sec:authorize access="isAuthenticated()">
   <form:form action="${pageContext.request.contextPath}/logout" method="POST">
       <input type="submit" value="로그아웃" />
   </form:form>
   <p><a href="<c:url value="/loginInfo" />">로그인 정보 확인 방법3 가지</a></p>
</sec:authorize>

<sec:authorize access="hasRole('admin')" > 관리자 페이지 </sec:authorize>

<h3>
    [<a href="<c:url value="/user/userForm" />">회원가입</a>]
    <!--  c:url 절대 경로 만들어줌 <c:url value="/user/userForm" /> = ${pageContext.request.contextPath}/user/userForm -->
    [<a href="<c:url value="/user/userHome" />">유저 홈</a>]
    [<a href="<c:url value="/admin/adminHome" />">관리자 홈</a>]
</h3>
</body>
</html>
```
