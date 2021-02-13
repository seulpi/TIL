# 1. restful을 적용하여 게시판을 구현을 완성하시오 (URL 설계 포함)
## - 댓글 구현
- 하면서 부족했던것 
   1. js안에 var name = $("#bName").val(), **';'안쓰고 ','사용**
   2. var name = $("#bName").val(); → var name = ${"#bName"}.val(); **괄호 잘못 사용** 
   3. ajax안에 변수들 갯수 맞춰서 저장 안함 **- bGroup: bGroup 빼먹음**

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>

<html>
<head>
<title>List</title>

<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script type="text/javascript"></script>

<script>

	$(document).ready(function(){
		
		$('#replyContent').submit(function(event) {
			event.preventDefault(); 
	
			var name = $("#bName").val();
			var bTitle = $("#bTitle").val();
			var bContent = $("#bContent").val();
			var bId = $("#bId").val();
			var bGroup = $("#bGroup").val();
			var bStep = $("#bStep").val();
			var bIndent = $("#bIndent").val();
			
			var form = {
					bName: name,
					bTitle: bTitle,
					bContent: bContent,
					bId: bId,
					bGroup: bGroup,
					bStep: bStep,
					bIndent: bIndent
					
			};
			
			$.ajax({
				type: "POST", 
				url: $(this).attr("action"),
				cache:  false,
				contentType: 'application/json; charset=UTF-8', //contentType 은 data 설명 put을 쓰면 json형태로 넘겨야하고 stringify은 json으로 바꿔주는것
				data: JSON.stringify(form), //마임타입? 보안문제때문에 그냥 넘기면 안되고 json으로 넘겨줘야함
				success: function(result) {
					
					if (result == ("SUCCESS")) {
						/* windows.location.href = ${pageContext.request.contextPath}/restful/board */
						$(location).attr('href', '${pageContext.request.contextPath}/restful/board')
						// ↑ BOM 객체 
	
					}
				},
				error: function(e) {
					console.log(e);
				}
			})
		}); //end.submit()
	}); //end.ready()
</script>


</head>
<body>
  
  <table width="500" cellpadding="0" cellspacing="0" border="1">
		<form id="replyContent" action="${pageContext.request.contextPath}/restful/board/reply/${reply_view.bId}" method="post">
			
			<input type="hidden" id="bId" value="${reply_view.bId}"> 
			<input type="hidden" id="bGroup" value="${reply_view.bGroup}"> 
			<input type="hidden" id="bStep" value="${reply_view.bStep}">
		 <input type="hidden" id="bIndent" value="${reply_view.bIndent}">
			<tr>
				<td>번호</td>
				<td>${reply_view.bId}</td>
			</tr>
			<tr>
				<td>히트</td>
				<td>${reply_view.bHit}</td>
			</tr>
			
			<tr>
				<td>이름</td>
				<td><input type="text" id="bName" value="${reply_view.bName}"></td>
			</tr>
			
			<tr>
				<td>제목</td>
				<td><input type="text" id="bTitle" value="${reply_view.bTitle}"></td>
			</tr>
			
			<tr>
				<td>내용</td>
				<td><textarea rows="10" id="bContent">${reply_view.bContent}</textarea></td>
		<tr>
			<td colspan="2"><input type="submit" value="답변업로드">&nbsp;
				&nbsp; <a href="list">목록보기</a></td>
		</tr>

		</form>
	</table>


</body>
</html>
```
```java
package edu.bit.ex.board.controller;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.servlet.ModelAndView;

import edu.bit.ex.board.service.BService;
import edu.bit.ex.board.service.BServiceImpl;
import edu.bit.ex.board.vo.BoardVO;
import lombok.AllArgsConstructor;
import lombok.extern.log4j.Log4j;



//REST : Representational State Transfer
//하나의 URI가 하나의 고유한 리소스를 대표하도록 설계된 개념

//http://localhost/spring02/list?bno=1 ==> url+파라미터
//http://localhost/spring02/list/1 ==> url
//RestController은 스프링 4.0부터 지원
//@Controller, @RestController 차이점은 메서드가 종료되면 화면전환의 유무

@Log4j
@AllArgsConstructor
@RestController
@RequestMapping("/restful/*")
public class RestBoardController {

	private BServiceImpl bSerivce;
	
	//기존 @Controller 방식으로 넘기려면 ModelAndView를 사용해 데이터와 view를 넘겨줘야함
	// 1. list(처음 진입 화면이므로 화면이 깜박여도 상관없기 때문에 @Controller방식으로 접근 -> 꼭 json으로 넘겨야 된다 그런거아님
	@GetMapping("/board")
	public ModelAndView list(ModelAndView mav) {
		mav.setViewName("rest_list");
		mav.addObject("list", bSerivce.getList());
		
		return mav;
		
	}
	
	@GetMapping("/board/{bId}") 
	public ModelAndView rest_content_view(BoardVO boardVO, ModelAndView mav) {
		
		log.info("rest_content_view");
		mav.setViewName("rest/rest_content_view"); //view 물리적인 경로
		mav.addObject("content_view", bSerivce.getBoard(boardVO.getbId()));
		
		return mav;
	}

	@PutMapping("/board/{bId}")
	public ResponseEntity<String> rest_update(@RequestBody BoardVO boardVO, ModelAndView model) {
		// @RequestBody는 body 태그로 넘어오는걸(json으로 넘어오고있음) BoardVO 객체로 바꿔주는 것
		ResponseEntity<String> entity = null;

		log.info("rest_update");

		try {
			bSerivce.modify(boardVO);
			entity = new ResponseEntity<String>("SUCCESS", HttpStatus.OK);
		
		} catch (Exception e) {
			e.printStackTrace();
			entity = new ResponseEntity<String>(e.getMessage(), HttpStatus.BAD_REQUEST);
		}
		return entity;
	}

	@DeleteMapping("/board/{bId}")
	// delete는 기본적으로 form 태그에서 지원하지 x(get, post만 지원) -> list에서 클릭을 하게되면 js&jquery-ajax로 처리 (submit이런거말고)
	// 어떨 때 사용하느냐? 댓글 삭제 -> ajax
	public ResponseEntity<String> rest_delete(BoardVO boardVO, Model model) {
		
		ResponseEntity<String> entity = null;
		
		log.info("rest_delete");
		
		try {
			bSerivce.remove(boardVO.getbId());
			//삭제가 성공하면 성공 상태 메세지 저장
			entity = new ResponseEntity<String>("SUCCESS", HttpStatus.OK);
											//json (X), 그냥 SUCCESS라는 일반 문자열
		}catch (Exception e) {
			e.printStackTrace();
			//삭제가 실패하면 실패 상태 메세지 저장
			entity = new ResponseEntity<String>(e.getMessage(), HttpStatus.BAD_REQUEST);
		}
		//삭제처리 http 상태 메세지 리턴
		return entity;
	}
	
	//댓글 보기
	@GetMapping("/board/reply/{bId}") //bId가 고유키
	public ModelAndView reply_view(BoardVO boardVO, ModelAndView mav) {
		log.info("reply_view");
		
		mav.setViewName("rest/rest_reply_view"); //view 물리적인 경로
		mav.addObject("reply_view", bSerivce.getBoard(boardVO.getbId()));
		// 일단 reply작성화면을 보여줘야하니까 댓글이므로 원글 아이디 가져와서 작성화면 보여주기
		
		return mav;
	}
	
	//댓글 작성
	@PostMapping("/board/reply/{bId}") 
	public ResponseEntity<String> replyWrite(@RequestBody BoardVO boardVO, Model model) {
		
		ResponseEntity<String> entity = null;

		log.info("replyWrite");

		try {
			bSerivce.replySet(boardVO);
			entity = new ResponseEntity<String>("SUCCESS", HttpStatus.OK);
		
		} catch (Exception e) {
			e.printStackTrace();
			entity = new ResponseEntity<String>(e.getMessage(), HttpStatus.BAD_REQUEST);
		}
		return entity;
	}
}
```
<br>

# 2. 부트스트랩으로 로그인 화면구현후 interceptor 적용하여 로그인한 유저에게만 게시판이 보이도록 하시오
>> [참조사이트] https://rongscodinghistory.tistory.com/2

1. LoginVO
```java
package edu.bit.ex.vo;


public class LoginVO {
	private String id;
	private String pw;
	private char enabled;
	
	
	public char getEnabled() {
		return enabled;
	}
	public void setEnabled(char enabled) {
		this.enabled = enabled;
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
	
	public LoginVO(String id, String pw, char enabled) {

		this.id = id;
		this.pw = pw;
		this.enabled = enabled;
	}
	
	public LoginVO() {
		
	}	
}
```

2. Controller
```java
package edu.bit.ex;

import javax.servlet.http.HttpSession;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.servlet.mvc.support.RedirectAttributes;

import edu.bit.ex.service.LoginService;
import edu.bit.ex.vo.LoginVO;
import lombok.AllArgsConstructor;
import lombok.ToString;
import lombok.extern.log4j.Log4j;


@Log4j
@AllArgsConstructor
@Controller
public class LoginController {
	
	LoginService service;
	
	@GetMapping("/") //처음 들어오면 보이는 login창
	public String loginHome() {
		System.out.println("logingHome()실행");
		return "loginSuccess";
	}
	
	@PostMapping(value="/loginProcess")
	public String loginProcess(HttpSession session, LoginVO dto, RedirectAttributes rttr) throws Exception {
		
		LoginVO user = service.login(dto);
		
		if(user == null) { //로그인 실패
			session.setAttribute("user", null);
			rttr.addFlashAttribute("msg", false);
		
		}else { //로그인 성공
			session.setAttribute("user", user);
		}
				
		return "redirect:/";
	}
	
	@RequestMapping("logout") //로그아웃
	public String logout(HttpSession session) throws Exception {
		
		log.info("logout()");
		session.invalidate(); // 세션 전체를 날림 
		
		return "redirect:/";
	}
}
```

3. Service
```java
package edu.bit.ex.service;

import edu.bit.ex.vo.LoginVO;

public interface LoginService {
	
	public LoginVO login(LoginVO dto);

}
```

4. ServiceImpl
```java
package edu.bit.ex.service;

import org.springframework.stereotype.Service;

import edu.bit.ex.mapper.LoginMapper;
import edu.bit.ex.vo.LoginVO;
import lombok.AllArgsConstructor;

@Service
@AllArgsConstructor
public class LoginServiceImpl implements LoginService {
	
	LoginMapper loginMapper;

	@Override
	public LoginVO login(LoginVO dto) {
	
		return loginMapper.loginOK(dto);
	}	
}
```

5. Mapper
```javapackage edu.bit.ex.mapper;

import edu.bit.ex.vo.LoginVO;

public interface LoginMapper {

	public LoginVO loginOK(LoginVO dto);

}
```
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="edu.bit.ex.mapper.LoginMapper">

	<select id="loginOK" resultType="edu.bit.ex.vo.LoginVO">


<![CDATA[
      select * from users where username=#{id} and password=#{pw}
]]>
	</select>

</mapper>
```

6. Interceptor
```java
package edu.bit.ex.interceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.handler.HandlerInterceptorAdapter;

import edu.bit.ex.vo.LoginVO;
import lombok.extern.log4j.Log4j;

@Log4j
public class LoginInterceptor extends HandlerInterceptorAdapter {

	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception {
		System.out.println("preHandle()");
		
		HttpSession session = request.getSession();
		
		LoginVO user = (LoginVO)session.getAttribute("user");
		
		if(user == null) {
			log.info("user == null");
			response.sendRedirect(request.getContentType());
			//response.sendRedirect("/login");
			return false;
		}
		return true;
	}
	
	@Override
	public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
			ModelAndView modelAndView) throws Exception {
		// TODO Auto-generated method stub
		super.postHandle(request, response, handler, modelAndView);
		System.out.println("postHandle()");
	}
}
```

7. View 
- bootstrap 활용

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<html>
<head>
	<title>BootStrap Login</title>
	
	<!-- Latest compiled and minified CSS -->
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">

<!-- jQuery library -->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>

<!-- Popper JS -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.16.0/umd/popper.min.js"></script>

<!-- Latest compiled JavaScript -->
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>
</head>


<!-- https://passauer1083.tistory.com/18 : 정렬하는 법 참고사이트 -->
<body class="text-center">
	<div class="row justify-content-center">
		<!-- 이게 있어야 블록이 전체를 차지안하고 상대적으로 크기가 맞춰지고 정렬된다,  row에 포함된 column이 중앙정렬 -->
		
		<c:if test="${user == null}">
			<form class="form-horizontal" role="form" method="post"
				autocomplete="off"
				action="${pageContext.request.contextPath}/loginProcess">
				<img class="logoimg" alt=""
					src="${pageContext.request.contextPath}/resources/부트스트랩로고.png"
					width="50px" height="50px">
				<!-- 사진리소스처리 -->
				<h1>Please sign in</h1>


				<label for="userId" class="col-sm-2 control-label"></label> <input
					type="text" class="form-control" id="userId" name="id"
					placeholder="Id"> <label for="userPass"
					class="col-sm-2 control-label"></label> <input type="password"
					class="form-control" id="userPass" name="pw" placeholder="Password">

				<div class="checkbox">
					<label><input type="checkbox"> Remember me </label>
				</div>

				<button type="submit" class="btn btn-primary btn-block">Sign
					in</button>
			</form>
		</c:if>
	</div>
		
	<c:if test="${msg==false}">
		<p style="color:#f00;"> 로그인에 실패했습니다. 아이디 or 패스워드를 다시 입력하세요</p>
	</c:if>
		
	<c:if test="${user != null}">
		<p>${user.id}님 환영합니다</p>
		<a href = "${pageContext.request.contextPath}/list">게시판 리스트</a>
		<br>
		<a href ="${pageContext.request.contextPath}/logout">로그아웃</a>
	</c:if>	
		
</body>
</html>

```
<br>

# 3. intercptor의 개념에 대하여 설명하시오
## 특정 URI 요청시 Controller로 가는 요청을 가로채는 역할 
- interceptor와 비슷한 기능 → Filter
>> interceptor, filter의 차이 : 실행되는 시점 <br> - interceptor : Dispatcher Servlet 실행된 후 (Controller로 가기 전), Spring내에 모든 객체에 대해 접근 가능, 주로 "로그인 처리"에 이용  <br> - filter : Dispatcher Servlet 실행되기 전, 같은 웹 어플리케이션 내에서만 접근이 가능, 주로 "한글처리"에 이용

- interceptor를 **사용하는 이유?**
    - interceptor를 사용하지 않으면 게시물 수정, 삭제, 작성 등의 **요청을 처리할때마다** session을 통해 로그인 정보가 있는지 **계속 확인해야되는 코드를 입력해야함** → 중복 코드 발생 → interceptor를 사용하면 **코드를 줄이고 한번에 작업이 가능한 장점** 

### @ 특징 : **HandlerInterceptorAdapter를 상속받음** → 상속받게 되면 두개의 함수를 오버라이딩함
1. preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) : 컨트롤러보다 먼저 실행, 세션 및 로그인 체크
2. postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) : 클라이언트에게 응답해줄 때 실행

- interceptor 객체 생성 ▶ xml에 bean 생성 (servlet-context.xml)
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
<br>

# 4. 부트스트랩이란?
- 트위터사에서 만든 웹사이트를 쉽게 만들게 도와주는 html, css, js 프레임워크 
- 반응형, 모바일 위주 웹개발을 위한 웹 프레임워크
>>[공식사이트] http://bootstrapk.com/
