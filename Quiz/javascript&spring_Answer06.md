# 1. 아래 annotation 의 용도는?
## @ModelAttribute
- command 객체의 **이름을 변경**해주는 annotation
- 이름 변경  + Model을 넘겨준다 
    - modle.addAttribute()를 할 필요가 없다(@ModleAttribute를 사용하면 Model을 같이 넘겨주기 때문)


# 2. id 와 pw 를 두개를 만든후 아래와 같이 유효성 검사를 하시오 (404에러가 자꾸난다,..)
```
-클라이언트쪽 체크: id 가 Null이거나 없으면 서버로 보내지 않으면서 - 해당 페이지에 '다시 입력하세요' 라는 문구 출력
-서버쪽 체크: id에 10자 초과이거나 숫자로만 되어 있어 있으면 다시 입력하는 페이지로 이동하여 '다시 입력하세요' 라는 문구 출력

-클라이언트쪽 체크: pw 가 널이거나 없으면 서버로 보내지 않으면서 - 해당 페이지에 '패스워드 다시 입력하세요' 라는 문구 출력
-서버쪽 체크: pw에 8자 미만이거나, 숫자로만 되어 있어 있거나, 문자로만 되어 있으면, 다시 입력하는 페이지로 이동하여 '패스워드 다시 입력하세요' 라는 문구 출력

-성공시 '로그인이 되었습니다' 라는 페이지로 이동
```
```java
//LoginValidator
package edu.bit.ex;

import org.springframework.validation.Errors;
import org.springframework.validation.Validator;

public class LoginValidator implements Validator {

	@Override
	public boolean supports(Class<?> clazz) {
		
		return false;
	}

	@Override
	public void validate(Object target, Errors errors) {
		System.out.println("validate()");
		Login login = (Login) target;
		
		String [] id = login.getId();
		//id에 10자 초과이거나 숫자로만 되어 있어 있으면
		for(int i = 0; i < id.length; i++) {
			if(id.length > 10 ) {
				System.out.println("ID로 사용할 수 있는 길이를 초과했습니다");
				errors.reject("id", "Not correct!");
				
			}else if(id[i].matches(".*[0-9].*")) {
				System.out.println("ID에 문자를 포함하세요");
				errors.reject("id", "Not correct!");
			}
		}
		
		String [] pw = login.getPw();
		
		for(int i = 0; i < pw.length; i++) {
			if(id.length > 10 ) {
				System.out.println("PW로 사용할 수 있는 길이를 초과했습니다");
				errors.reject("pw", "Not correct!");
				
			}else if(id[i].matches(".*[0-9].*")) {
				System.out.println("PW에 문자를 포함하세요");
				errors.reject("pw", "Not correct!");
			}
		}
	}

}
```
```java
//LoginController
package edu.bit.ex;

import java.text.DateFormat;
import java.util.Date;
import java.util.Locale;

import javax.validation.Valid;

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
public class LoginController {
	
	private static final Logger logger = LoggerFactory.getLogger(LoginController.class);
	
	@RequestMapping("/login")
	public String loginForm() {
		return "loginPage";
	}
	
	@RequestMapping("/loginOk")
	public String loginOk(@ModelAttribute("loginOk") Login loginOk, BindingResult result ) {
		String page= "loginOkPage";
		
		LoginValidator validator = new LoginValidator();
		validator.validate(loginOk, result);
		
		if(result.hasErrors()) {
			page = "loginRePage";
		}
		
		return page;
	}

}
```
```java
//Login
package edu.bit.ex;

import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.Setter;
import lombok.NoArgsConstructor;

@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
public class Login {
	
	private String [] id;
	private String [] pw;

}
```
```jsp
//loginPage

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>LoginPage</h1>
	
	<form action="loginOk" method="post">
		아이디 : <input type="text" name="id" value=${loginOk.id }><br>
		비밀번호 : <input type="password" name="pw" value=${loginOk.pw }> <br>
		<button type="submit" value="전송">
		<button type="reset" value="초기화">
	</form>
</body>
</html>
```

# 3. 마이바티스를 활용하여, emp 12개를 뿌리시오(@Resource가 import가 안된다)

---

# Q. 수업 중 문제 
## 1. 버튼을 클릭하여 랜덤으로 색상이 나오게 하시오 
## - 색상 "#ff0", "#6c0", "#fcf", "#cf0", "#39f"

### Answer[1] : 처음 생각한 로직
```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
	pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Click</title>

	<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
																<!-- ↑ min은 축소시켰다는 뜻! 
																인터넷으로 이용하려면 공간이랑 용량을 축소시켜야 하니까 -->

	<script type="text/javascript">
		function randomColor() {
			
				$("div").css("width", "300px").css("height", "300px");
				$("div").css("border", "1px solid black").css("height", "300px");
				var randomColor = ["#ff0", "#6c0", "#fcf", "#cf0", "#39f"];
				var colorNum = Math.floor(Math.random()*randomColor.length);
				$("div").css("backgroundColor", randomColor[colorNum]);
                                    // "randomColor[colorNum]" 처음에 이렇게 사용해서 안됨 "" 단순 문자열값이기 때문에
				
		}
		
	</script>

</head>

<body>
	
	<input type="button" value="배경 색 바꾸기" onclick="randomColor()">
	<div></div>
	
</body>
</html>
```

### Answer[2] : 안되서 참고한 로직
```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
	pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Click</title>

	<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>

	<script type="text/javascript">
		function randomC() {
		
				var randomColor = ["#ff0", "#6c0", "#fcf", "#cf0", "#39f"];
				var colorNum = Math.floor(Math.random()*randomColor.length);
				var color = document.getElementById("random")
				color.style.backgroundColor = randomColor[colorNum]
	
		}
		
	</script>
	

</head>

<body>

	<div id="random">
		<input type="button" value="배경 색 바꾸기" onclick="randomC();">
	</div>
    
</body>
</html>
```
