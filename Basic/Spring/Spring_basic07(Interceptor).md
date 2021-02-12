# Interceptor
: 특정 url에 대해 controller 가기 이전에 가로채는 역할을 한다법(서버로 보내기전 가로채기)

![interceptor](https://user-images.githubusercontent.com/74290204/107745016-fd58c380-6d56-11eb-9344-bb6b98d09c8a.png)

- interceptor는 주로 '로그인'을 하는데 많이 사용된다 
- intercepror를 사용하는 이유는 공통된 로직에 대해서 interceptor를 사용함으로써 중복된 코드를 최소화하는데 있다 
```
[ex]
게시물에 대해 로그인한 사람만 글작성, 수정, 보기, 삭제 등을 처리해야할때
login정보를 체크해야하기 때문에 요청마다 login에 대한 확인 작업을 거쳐야함 100개면 100개다! 
이러한 중복을 interceptor를 사용하면 한번의 처리로 중복적인 코드를 요청시마다 남길 필요가 없어짐
interceptor에서 로그인한 사람만 controller로 넘기기 때문
이러한 장점 때문에 interceptor를 사용하는 것 !!
```

## @ Interceptor 객체 생성 (설정)
- servlet-context.xml 추가
```xml
<!-- 인터셉터 객체 생성 -->
   <beans:bean id="boardInterceptor" class="edu.bit.ex.board.interceptor.BoardInterceptor">
   </beans:bean>
   
   <!-- Interceptor 설정 -->
   <interceptors>
       <interceptor>
           <mapping path="/list"/> <!-- http://localhost:8282/ex/list , list로 치고들어오는거에 대해서는 interceptor하겠다, 전체다 오는걸 interceptor하겠다 하면 /list/** 이렇게 주고 controller와의 맵핑 경로 맞춰줘야함-->     
           <exclude-mapping path="/resources/**"/>
           <beans:ref bean="boardInterceptor"/>
       </interceptor>
   </interceptors> 
```

## @ Login을 위한 DB처리(예시로 생성)
```sql
--AUTHORITY, USERNAME -> DB에 생성( 인증과 권한)
create table users(
   username varchar2(50) not null primary key,
   password varchar2(100) not null,
   enabled char(1) DEFAULT '1'
);
create table authorities (
   username varchar2(50) not null,
   authority varchar2(50) not null,
   constraint fk_authorities_users foreign key(username) references users(username)
);
create unique index ix_auth_username on authorities (username,authority);

commit;

insert into users (username,password) values('user','user');
insert into users (username,password) values('member','member');
insert into users (username,password) values('admin','admin');

commit;

insert into AUTHORITIES (username,AUTHORITY) values('user','ROLE_USER');
insert into AUTHORITIES (username,AUTHORITY) values('member','ROLE_MANAGER');
insert into AUTHORITIES (username,AUTHORITY) values('admin','ROLE_MANAGER');
insert into AUTHORITIES (username,AUTHORITY) values('admin','ROLE_ADMIN');

commit;
```

## @ 구현 : vo → controller(uri설계) → service → mapper → interceptor설정 → view 
>> 쿠키 & ★ 세션 개념 다시 보고 이해하고 암기 

### Q. spring에서 session 활용 어떻게 하나요(어떻게 프로그래밍하나요)?
### *Answer*
- 인터셉터할때 코드를 요렇게 짰습니다
<details><summary>인터셉터할때 코드를 요렇게 짰습니다</summary>

```java
@PostMapping("login")
	public String login(HttpServletRequest req, RedirectAttributes rttr) { 
		//public String login(UserVO userVO) 줘도되는데 예전 방식으로 해봄(까먹을까봐) -> 이렇게 하면 주의할 게 view단에 name과 객체의 변수명이 같아야함 
		log.info("post login()");
		String id = req.getParameter("id");
		String pw = req.getParameter("pw");
		
		HttpSession session = req.getSession(); 
		/* ★session 각각 개인유저를 연결하기 위해서 session영역을 정해주는데 그 메모리 영역을 서버에서 가지고있음
		  ↑ Spring에서 session객체 가지고 오는 방법 */
		
		UserVO user = loginService.loginUser(id, pw);
		
		if(user == null) { 
			session.setAttribute("user", null);
			//해당 섹션영역의 값 저장 가능 , user의 대한 객체 저장
			
			  /*
	          * Spring3 에서 제공하는 RedirectAttributes를 사용하면 redirect post 구현이 가능합니다.
	          * 
	          * 하지만 일회성입니다. 새로고침하면 날라가는 데이터이므로 사용목적에 따라서 사용/불가능 판단을 잘 하셔야 할거 같습니다.
	          */
			rttr.addFlashAttribute("msg", false);
		} else {
			session.setAttribute("user", user);
		}
		return "redirect:/";
	}
```
</details>

1. VO
```java
//UserVO
package edu.bit.ex.board.vo;

import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;


@Setter
@Getter
@AllArgsConstructor
@NoArgsConstructor
public class UserVO {
	private String username;
	private String password;
	private char enabled;

}
```

2. Cotroller
```java
package edu.bit.ex.board.controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpSession;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.mvc.support.RedirectAttributes;

import edu.bit.ex.board.service.LoginService;
import edu.bit.ex.board.vo.UserVO;
import lombok.AllArgsConstructor;
import lombok.extern.log4j.Log4j;

@Log4j
@AllArgsConstructor
@Controller
public class LoginController {

	private LoginService loginService;
	
	@GetMapping("/") //홈으로 들어오게 되면 
	public String home() {
		log.info("home()");
		
		return "login"; //login.jsp 이동 
	}
	
	@PostMapping("login")
	public String login(HttpServletRequest req, RedirectAttributes rttr) { 
		//public String login(UserVO userVO) 줘도되는데 예전 방식으로 해봄(까먹을까봐) -> 이렇게 하면 주의할 게 view단에 name과 객체의 변수명이 같아야함 
		log.info("post login()");
		String id = req.getParameter("id");
		String pw = req.getParameter("pw");
		
		HttpSession session = req.getSession(); 
		/* ★session 각각 개인유저를 연결하기 위해서 session영역을 정해주는데 그 메모리 영역을 서버에서 가지고있음
		  ↑ Spring에서 session객체 가지고 오는 방법 */
		
		UserVO user = loginService.loginUser(id, pw);
		
		if(user == null) { 
			session.setAttribute("user", null);
			//해당 섹션영역의 값 저장 가능 , user의 대한 객체 저장
			
			rttr.addFlashAttribute("msg", false);
			//Spring3 에서 제공하는 RedirectAttributes를 사용하면 redirect post 구현이 가능
	        // 일회성, 새로고침하면 날라가는 데이터 , 없어도됨
		} else {
			session.setAttribute("user", user);
		}
		return "redirect:/";
	}
	
	 // 로그아웃
	   @RequestMapping(value = "/logout")
	   public String logout(HttpSession session) throws Exception {
	      log.info("/member/logout");

	      session.invalidate();

	      return "redirect:/";
	   }
}
```

3. Service
```java
package edu.bit.ex.board.service;

import org.springframework.stereotype.Service;

import edu.bit.ex.board.mapper.LoginMapper;
import edu.bit.ex.board.vo.UserVO;
import lombok.AllArgsConstructor;

@Service
@AllArgsConstructor
public class LoginService { //interface 사용하는게 정석인데 간단하게 하기위해 다이렉트로 class 사용
	
	LoginMapper loginMapper; 
	
	public UserVO loginUser(String id, String pw) {
		return loginMapper.logInUser(id, pw);
	}	
}
```

4. Mapper 
```java
 // LoginMapper
package edu.bit.ex.board.mapper;

import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.annotations.Select;

import edu.bit.ex.board.vo.UserVO;

@Mapper
public interface LoginMapper {
	
	@Select("select * from users where username = #{username} and password = #{password}") // mybatis 4번째 방법 = mapper생성하기 귀찮아서 이거 사용함
	 public UserVO logInUser(@Param("username") String username,@Param("password") String password);
	//일반 변수가 2개일때는 @param붙여주기!
}
```

5. Interceptor
```java
//BoaedInterceptor
package edu.bit.ex.board.interceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.handler.HandlerInterceptorAdapter;
import org.springframework.web.servlet.mvc.support.RedirectAttributes;

import edu.bit.ex.board.service.LoginService;
import edu.bit.ex.board.vo.UserVO;
import lombok.AllArgsConstructor;
import lombok.extern.log4j.Log4j;

@Log4j // 컨트롤러가 아니기 때문에 @Controller가 필요X
public class BoardInterceptor extends HandlerInterceptorAdapter { // 규칙1. HandlerInterceptorAdapter상속을 해줘야함 2. 상속을 하면
	// 규칙2. 상속받으면 2개의 함수를 오버라이딩 함 preHandle, postHandle
	
	
	// preHandle() : 컨트롤러보다 먼저 수행되는 메서드
	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception {
		System.out.println("preHandle 실행");
		// session 객체를 가져옴
		HttpSession session = request.getSession();

		// login처리를 담당하는 사용자 정보를 담고 있는 객체를 가져옴, user정보가 있는지 check
		UserVO user = (UserVO) session.getAttribute("user");

		if (user == null) {
			log.info("user가 null");
			// 로그인이 안되어 있는 상태이므로 로그인 폼으로 다시 돌려보냄(redirect)
			response.sendRedirect(request.getContextPath());

			return false; // 더이상 컨트롤러 요청으로 가지 않도록 false로 반환함
		}

		// preHandle의 return은 컨트롤러 요청 uri로 가도 되냐 안되냐를 허가하는 의미임
		// 따라서 true로하면 컨트롤러 uri로 가게 됨.
		return true;
	}
	
	
	// postHandle, preHandle 객체를 생성하게 되면 spring이 호출 -> postHandle Controller 다 지나가고 response 할때 실행(dispatcher에게 응답해줄때)
	@Override
	public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
			ModelAndView modelAndView) throws Exception {

		super.postHandle(request, response, handler, modelAndView);
		System.out.println("postHandle 실행");
	}

}
```

6. View 
```jsp
/login.jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<html>
<head>
<title>로그인</title>
</head>
<body>
	<%
	String path = request.getContextPath();
	%>
	
	<%=path%>
	<c:if test="${user == null}"> <!-- session에 저장했던 user, session은 전역에서 제한시간동안 사용가능하기 때문에 사용이 이렇게 가능한것 -->
		<form role="form" method="post" autocomplete="off" action="<%=path%>/login">
			<p>
				<label for="userId">아이디</label> <input type="text" id="userId"
					name="id" />
			</p>
			
			<p>
				<label for="userPass">패스워드</label> <input type="password"
					id="userPass" name="pw" />
			</p>
			
			<p>
				<button type="submit">로그인</button>
			</p>
			<!--    <p><a href="/member/register">회원가입</a></p> -->
		</form>
	</c:if>

	<c:if test="${msg == false}">
		<p style="color: #f00;">로그인에 실패했습니다. 아이디 또는 패스워드를 다시 입력해주십시오.</p>
	</c:if>

	<c:if test="${user != null}">
		<p>${user.username}님환영합니다.</p>

		<!-- <a href="member/modify">회원정보 수정</a>, <a href="member/withdrawal">회원탈퇴</a><br/> -->
		<a href="<%=path%>/list">게시판 리스트</a>
		<br>
		<a href="<%=path%>/logout">로그아웃</a>

	</c:if>

</body>
</html>
```
