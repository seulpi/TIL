# 시큐리티 암호화 
- 사용자의 패스워드를 개인정보와 보완의 이유로 있는 그대로 DB에 저장하지 않고 **알고리즘을 통해 저장하는것**

## @ 실행 원리 
- 모든 암호화 단계는 인코딩과 디코딩의 과정을 거친다 
```
ex) 암호 자리 바꾸기 
pw : 123456 → 암호화 pw : 456123 
- pw의 반을 잘라서 자리를 바꾸고 DB에 저장(인코딩)
- 유저가 로그인할때 다시 원래 pw로 비교해 들어와야하기 때문에 다시 자리를 바꾼다(디코딩)
    - 디코딩 : 원래대로 되돌리는 것!
```
- 사실상 이런 쉬운 알고리즘을 털리기 쉽기 때문에 전문가들이 만든 알고리즘을 이용함

## @ 스프링에서의 암호화 

### Q. 회원가입을 이용해 패스워드를 암호화해보자! (test용)

#### 1. Conttoller
```java
package edu.bit.ex;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import edu.bit.ex.service.UserService;
import edu.bit.ex.vo.UserVO;
import lombok.AllArgsConstructor;
import lombok.extern.log4j.Log4j;

@Log4j
@Controller
@AllArgsConstructor
public class UserController {
	
	private UserService userService;
	
	@GetMapping("/user/userForm")
	public void userHome() {
		log.info("welcome userForm!");
	}
	
	@PostMapping("/user/addUser")
	public String addUser(UserVO userVO) {
		log.info("post register");
		
		userService.addUser(userVO);
		
		return "redirect:/";
	}
	
	
}
```

#### 2. Service
```java
package edu.bit.ex.service;

import javax.inject.Inject;

import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.stereotype.Service;

import edu.bit.ex.mapper.UserMapper;
import edu.bit.ex.vo.UserVO;
import lombok.NoArgsConstructor;
import lombok.extern.log4j.Log4j;


@Log4j
@NoArgsConstructor
@Service
public class UserService {
   
   @Inject
   private BCryptPasswordEncoder passEncoder;
   
   @Inject
   private UserMapper userMapper;
   
 @Transactional(rollbackFor = Exception.class) // 에러 생기면 롤백을 위해서( 함수 하나라도 에러나면 롤백! 반드시 정의해줘야함)
   public void addUser(UserVO userVO){
      String password = userVO.getPassword();
      String encode = passEncoder.encode(password);  //패스워드 인코딩(암호화)
      
      userVO.setPassword(encode);  //암호화시킨것을 저장
      
      userMapper.insertUser(userVO); // 암호화된 패스워드를 insert
      userMapper.insertAuthorities(userVO);
   }
   
}
```

#### 3. Mapper
```java
package edu.bit.ex.mapper;

import org.apache.ibatis.annotations.Insert;

import edu.bit.ex.vo.UserVO;

public interface UserMapper {
	
	// xml쓰기 귀찮아서 마이바티스4번째 방법인 바로 sql문 주입 (간단한거는 상관없지만..실무에서는 되도록이렇게 안쓰기~)
	@Insert("insert into users(username,password,enabled) values(#{username},#{password},#{enabled})")
	public int insertUser(UserVO userVO);

	@Insert("insert into AUTHORITIES (username,AUTHORITY) values(#{username},'ROLE_USER')")
	public void insertAuthorities(UserVO UserVO);

}
```

#### 4. VO 
```java
package edu.bit.ex.vo;

import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.Setter;
import lombok.ToString;
import lombok.extern.log4j.Log4j;

@Log4j
@Getter
@Setter
@AllArgsConstructor
@ToString
public class UserVO {

	private String username;
	private String password;
	private int enabled;

	public UserVO() { // 아무것도 없으면 이거 디폴트로 넣기 위해서 그냥 넣어놓음
		this("user", "1111", 1);
	}

}
```

#### 5. JSP
<details><summary>home.jsp</summary>

```jsp
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

<details><summary>userForm.jsp</summary>

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
<%@ taglib prefix="sec" uri="http://www.springframework.org/security/tags" %>
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
	<title>회원가입</title>
</head>

<body>

<h1>회원가입</h1>

<c:url value="/user/addUser" var="addUserUrl" />
<p>${addUserUrl}</p>
<form:form name="frmMember" action="${addUserUrl}" method="POST">
    <p>
        <label for="username">아이디</label>
        <input type="text"  name="username" />
    </p>
    <p>
        <label for="password">비밀번호</label>
        <input type="password" name="password"/>
    </p>
    <button type="submit" class="btn">가입하기</button>
</form:form>
</body>
</html>
```
</details>
<br>

#### 6. 설정 Check!
- [x] root-context.xml에서 mapper 경로, 정의되어있는지 확인
```xml
<mybatis-spring:scan base-package="edu.bit.ex.mapper"/>
```

- [x] security-db-context.xml에서 service에서 정의한 암호화 객체 생성
```xml
<beans:bean id="bcryptPasswordEncoder" class="org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder" />
===========================================================================
<intercept-url pattern="/user/**" access="permitAll" /> 추가

<!--
<intercept-url pattern="/user/userForm" access="permitAll" />이렇게 줬더니 add함수를 타지X 왜냐면 또 다시 정의 해줘야하니까 근데 all로 주면 두개 다 안줘도됨 -->

<!-- 위치도 중요!      <intercept-url pattern="/**" access="hasAnyRole('USER, ADMIN')" /> 밑에 넣으면 all이니까 저 추가한게 타지 않음 -->
```

   <details><summary>security-db-context.xml 설정 완료한 코드</summary>

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
            <intercept-url pattern="/user/**" access="permitAll" />
            <!-- <intercept-url pattern="/user/userForm" access="permitAll" /> 이렇게 해주면 user/addUser를 또 넣어줘야됨 그래서 함수를 안탓네..-->

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
    <beans:bean id="bcryptPasswordEncoder" class="org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder" />
    
    <!-- provider --> 
    <authentication-manager>
        <authentication-provider>
        <password-encoder ref="customNoOpPasswordEncoder"/>  
        <jdbc-user-service 
                data-source-ref="dataSource"
                users-by-username-query="select username, password, enabled from users where username= ?"
                authorities-by-username-query="select username, authority from users where username = ?"
            /> 
        </authentication-provider>
    </authentication-manager>
</beans:beans>
```
</details>
<details><summary>화면출력(설정완료)</summary>
![화면 캡처 2021-02-19 114733](https://user-images.githubusercontent.com/74290204/108451926-01329b80-72ab-11eb-9378-ff67a0abbb57.png)

![화면 캡처 2021-02-19 120601](https://user-images.githubusercontent.com/74290204/108451936-0394f580-72ab-11eb-9b75-5d2141716393.png)
</details>
<br>

#### ※ **디코딩을 안해주면 로그인 되지 않음!!** 
- 암호화되어있기때문에 user가 치고 들어오는 게 먹히지 않음 → 디코딩 처리 해줘야함

    ![화면 캡처 2021-02-19 121248](https://user-images.githubusercontent.com/74290204/108455676-fd564780-72b1-11eb-8f6e-e9d59da6aba7.png)

## @ 디코딩 처리
```xml
<password-encoder ref="bcryptPasswordEncoder"/>

<!--이런 암호화 방법을 시켜서 DB에 저장했다는 것을 명시해주면 
내부적으로 알아서 유저가 치고온것과 암호화된 것을 matching시켜서 비교해서 넘겨줌
-->
```
### + 나는 저런 처리를 했는데 에러가 났음 
![화면 캡처 2021-02-19 125553](https://user-images.githubusercontent.com/74290204/108455665-f8919380-72b1-11eb-9072-1460c9861796.png)

- why? xml에서 쿼리 설정 오류
```sql
authorities-by-username-query="select username, authority from users where username = ?" 로 설정해서

authorities-by-username-query="select username, authority from authorities where username = ?"
이렇게 해줘야하는데

★ 콘솔 에러를 잘보자 !!!
```

