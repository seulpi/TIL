# Restful : URI 활용
- Rest란 HTTP URI를 통해 자원(Resource)를 명시하고 HTTP Method(POST, GET, PUTM DELETE)를 통해 해당 자원에 대한 CRUD Operation을 적용하는 것
- Restful이란 Rest원리를 따르는 시스템

```
URL : Location, 리소스 위치
URI : URL을 확장, Idenfication  
```
▶ https://dabinnn1011.tistory.com/17

## @ 특징
- **2000년도 로이필딩**이 개념을 만듦
- URI는 정보의 자원을 표현해야함
- 자원에 대한 행위는 **HTTP Method(GET, POST, PUT, DELETE)** 로 표현
	- get(조회), post(생성), put(수정), delete(삭제)의 메소드를 활용해 **Create Read Update Delete**
- 특정 행위의 표현은 JSON, XML등을 이용
- URI 마지막에 "/"를 포함하지 X ("/"는 계층관계를 나타낼때 사용)
- "_" 대신 "-(하이픈)"을 사용
- 소문자 사용 


## @ PathVariable : Restful 관련 Annotation
- 경로에 변수를 넣어 요청메소드에서 파라미터로 이용하는 어노테이션
```java
@RequestMapping(value = "/stydent/{studentId}")
	public String getStudent(@PathVariable String studentId, Model model) {
		model.addAttribute("studentId", studentId);
		
		return "student/studentView"; 
	}
```

## @RequestMapping에서의 get과 post방식 
- RquestMapping에서 요청을 받을 때 get방식과 post방식을 구분
```java
@RequestMapping(value = "/student/{studentId}", method= RequestMethod.GET)
	public String getStudent(@PathVariable String studentId, Model model) {
		model.addAttribute("studentId", studentId);
		
		return "student/studentView"; 
	}
```

## @ModelAttribute 
- 커맨드 객체의 이름을 변경해주는 어노테이션
```java
@RequestMapping(value = "studentView", method= RequestMethod.GET)
	public String studentView(@ModelAttribute("studentInfo") StudentInformation studentInformation) {
		return "student/studentView"; 
	}
```

![어노테이션](https://user-images.githubusercontent.com/74290204/106417911-6fe4bc00-6498-11eb-9238-119755e7dd0c.PNG)

## @Redirect
- redirect:studentOk → studentOk는 jsp 파일이 아닌 URL을 뜻함
- redirect : 서버에서 클라이언트로 하여금 다시 주소 요청 / window.location.assign("URL") : 함수로 접근(클라이언트) → 이거 다시 체크해서 정리해놓기, 제대로 못들음
```java
@RequestMapping("/studentConfirm") 
	public String studentRedirect(HttpServletRequest httpServletRequest, Model model) {
		String id = httpServletRequest.getParameter("id");
		if(id.equals("abc")) {
			return "redirect:studentOk";
		}
		return "redirect:studentNg";
		//forward로 바꿔주면 forwarding된다 (forward니까 url에 주소창이 변경되지는않음)
	}
	
	@RequestMapping("/studentOk")
	public String studentOk(Model model) {
		return "student/studentOk";
	}
	

	@RequestMapping("/studentNg")
	public String studentNg(Model model) {
		return "student/studentNg";
	}
```

## @Validator : 해당 데이터가 유효한지 check, 서버&클라이언트 둘다 검사 가능
1. validator를 이용한 검증
```java
//StudentController.java
package edu.blt.ex;

import java.text.DateFormat;
import java.util.Date;
import java.util.Locale;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

/**
 * Handles requests for the application home page.
 */
@Controller
public class StudentController {

	private static final Logger logger = LoggerFactory.getLogger(StudentController.class);

	@RequestMapping("/studentForm")
	public String studentForm() {
		return "createPage";
	}

	@RequestMapping("/student/create")
	public String studentCreate(@ModelAttribute("student") Student student, BindingResult result) {
		// ModelAttribute는 이름을 바꿀수도 있지만 student이름으로 Model을 넘겨준다 model.addAttribute할 필요가 없다는 것
		String page = "createDonePage";

		StudentValidator validator = new StudentValidator();
		validator.validate(student, result);

		if (result.hasErrors()) { // result.hasErrors()는 rejectValue에서 한개라도 존재하면 true를 리턴(null로 정의해놨으니까)
			page = "createPage";
		}

		return page;
	}

}
```
```java
//StudentValidator.java
package edu.blt.ex;

import org.springframework.validation.BindingResult;
import org.springframework.validation.Errors;
import org.springframework.validation.Validator;

// Validator 만드는 방법
public class StudentValidator implements Validator {

	@Override
	public boolean supports(Class<?> clazz) {

		return false;
	}  // return false는 검증할 객체에 타입 정보를 적는 건데 일단 신경안써도됨

	@Override
	public void validate(Object obj, Errors errors) { 
		/*BindingResult를 Errors로 받음 Errors가 부모 
		BindingResult result에서 result가 errors에 들어간다 (부모=자식)*/
		System.out.println("validate()");
		Student student = (Student)obj;
		
		// 검증할 로직
		String studentName = student.getName();
		if(studentName == null || studentName.trim().isEmpty()) { 
		/* trim해줘야하는이유? 유저는 공백그런거 신경안쓰고 입력하지만 받는 우리 입장에서는 공백까지 문자로 포함하기 때문에 
		DB저장할때나 활용할때 문제가 생기기때문에 반드시 trim을 써야한다*/
			System.out.println("student is null or empty");
			
			errors.rejectValue("name", "trouble");
			//error가 있다면 name에서 trouble이 발생했다고 출력되는것, 이 부분은 개발자가 정해주는것(명명하는부분)
		}
		
	    int studentId = student.getId();
	      if(studentId == 0) {
	         System.out.println("studentId is 0");
	         
	         errors.rejectValue("id", "trouble");
	      }
		
	}

}
```
```jsp
createPage.jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=EUC-KR">
<title>Insert title here</title>
</head>
<body>
createPage.jsp
<br />

<% 
   String conPath = request.getContextPath();
%>

<form action="student/create"> <!--<%=conPath%>/student/create 절대경로안만들어주면 student가 계속 경로에 추가되는 에러나기 때문에 앞에 conPath사용해서 절대경로 만들어서 에러잡아줌 -->
   이름 : <input type="text" name="name" value="${student.name}"> <br />
   아이디 : <input type="text" name="id" value="${student.id}"> <br />
   <input type="submit" value="전송"> <br />
</form>

</body>
</html>
```
```jsp
//createDonePage.jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=EUC-KR">
<title>Insert title here</title>
</head>
<body>

이름 : ${student.name } <br />
아이디 : ${student.id }

</body>
</html>
```

2. ValidationUtils 클래스
```java
public void validate(Object obj, Errors errors) { 
		System.out.println("validate()");
		Student student = (Student)obj;
		
		/*
		String studentName = student.getName();
		if(studentName == null || studentName.trim().isEmpty()) {
			System.out.println("student is null or empty");
			
			errors.rejectValue("name", "trouble");
		}*/
		
		ValidationUtils.rejectIfEmptyOrWhitespace(errors, "name", "trouble");
```

## @Valid, @InitBinder
### - web.xml 에 dependecy 추가
```xml
 <dependency>
         <groupId>org.hibernate</groupId>
         <artifactId>hibernate-validator</artifactId>
         <version>4.2.0.Final</version>
      </dependency>
```
```java
package edu.blt.ex;

import java.text.DateFormat;
import java.util.Date;
import java.util.Locale;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;
import org.springframework.validation.annotation.Validated;
import org.springframework.web.bind.WebDataBinder;
import org.springframework.web.bind.annotation.InitBinder;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

/**
 * Handles requests for the application home page.
 */
@Controller
public class StudentController {

	private static final Logger logger = LoggerFactory.getLogger(StudentController.class);

	@RequestMapping("/studentForm")
	public String sudentForm() {
		return "createPage";
	}

	@RequestMapping("/student/create")
	public String studentCreate(@ModelAttribute("student") @Valid Student student, BindingResult result) { 
		/* view로 넘길때 <form:form command 객체 사용하는 방법, hasBindErrors 사용방법
		또는 여기서 model로 넘겨도됨
		Model model 추가해서 model.addAttribute("erroeId", "id가 이상합니다"); */
		
		String page = "createDonePage";

		if (result.hasErrors()) { 
			page = "createPage";
		}

		return page;
	}
	
	/*
	 * StudentValidator validator = new StudentValidator();
	 * validator.validate(student, result);를 InitBinder로 
	 */

	
	@InitBinder
	protected void initBinder(WebDataBinder binder) {
		binder.setValidator(new StudentValidator());
	}
}
```
