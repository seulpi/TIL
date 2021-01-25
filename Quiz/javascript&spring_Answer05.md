# 1. command 객체에 대하여 설명하시오
- 데이터 객체!
- Spring에서 데이터 객체는 **HttpServlet request객체 + Model 객체의 기능**을 한번에 수행하는 객체
	- 기능이 합쳐지면서 코드가 훨씬 간결해지는 장점이 있다 
### - **데이터 객체를 생성할 때 주의할점** 
### 1. 생성자함수와 getter&setter함수를 만들어주지 않으면 에러남 
- why? 
	1. 디폴트 생성자는 생성자를 하나라도 만들어주면 자동으로 생성되지 않음! <br> 
	command객체는 디폴트 생성자를 기준으로 객체를 생성하기 때문에 디폴트 생성자를 만들어주지 않으면 객체 생성 X
	2. 값을 받고 전달하는 주체가 gette&setter함수이기 때문에 하나라도 없으면 값을 주고 받는 게 이뤄지지않음!!
	```jsp
	<body>
		이름 : ${member.name}
		//name을 호출하는것 같아보이지만 사실상 getName()을 호출하고 있는것
	</body>
	```
### 2. command객체 내부의 변수명과 호출할때 변수명이 같아야함
<br>

# 2.ModelAndView 객체에 대하여 설명하시오
- Controller에서 Model 객체와 View를 함께 처리하는 객체(Model과 View를 동시에 설정가능한 객체)
	- 즉, Model객체는 데이터만 저장하지만 ModelAndView는 데이터를 저장하고 데이터를 이동할 Viwe페이지도 같이 저장
- Controller 처리 결과 후 응답할 view와 view에 전달할 값을 저장
<br>

# 3.아래의 골뱅이에 대하여 설명하시오
```
@Controller
@RequestParam
@RequestMapping
```

## @ : annotation 
### @Controller 
- 해당 클래스가 Controller임을 나타내기 위한 annotation

### @RequestParam
- 요청받은 파라미터 값(HttpServlet request)을 메소드 파라미터에 넣어주는 annotation 
```java
public String view(@RequestParam("id") int id) { .. }
```

### @RequestMapping
- URL을 컨트롤러의 메서드와 매핑할 때 사용하는 annotation
- 클라이언트의 request로 들어오는 url을 특정 controller 클래스나 메소드로 연결시키는 역할
<br>

# 4. 아래를 프로그래밍 스프링으로 프로그래밍 하시오
## - 놓쳤던 부분 : td나 div로 점수의 색을 주게 되면 전체 색을 받게 되므로 inline 태그 < span >을 사용해서 점수에 색을 주면된다
###  → < div id="box">< span> 90< /span>< /div>  (lnline태그 묶을때 span / block태그 묶을 때 div)
```
-국영수 입력 페이지
-국영수점수를 서버에서 받아서,총점과 평균을 리턴
-해당 총점과 평균의 점수의 색깔은 각각 빨간색과 파란색으로 만들것(JS로)
```
```jsp
//gradeInput.jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%@ page session="false" %>
<html>
<head>
	<title>Home</title>
</head>
<body>
	<h1>Grade!</h1>
	<form action="gradeOutput" method="post">
		
		Kor : <input type="text" name="kor" size="10"><br>
		Eng : <input type="text" name="eng" size="10"><br>
		Math : <input type="text" name="math" size="10"><br>
		<input type="submit" value="submit">

	</form>

</body>
</html>
```
```jsp
//gradeOutput.jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%@ page session="false" %>
<html>
<head>
	<title>Grade</title>

	<script>
		window.onload = function() {
			var sumStyle = document.getElementById("sum");
			var avgStyle = document.getElementById("avg");
			
			sumStyle.style.color = "red";
			avgStyle.style.color = "blue";
	
		};
	
	</script>
</head>
<body>
<table border="1">
	<tr>
	<td>Kor</td>
	<td>Eng </td>
	<td>Math</td>
	</tr>
	
	<tr>
	<td>${grade.kor} </td>
	<td>${grade.eng} </td>
	<td>${grade.math} </td>
	</tr>
	
	<tr>
	<td>Sum</td>
	<td id="sum" colspan="2">${grade.gradeSum() }</p>
	</tr>
	
	<tr>
	<td>Avg</td>
	<td id="avg" colspan="2">${grade.gradeAvg() }</td>
	</tr>
</table>
</body>
</html>
```
```java
//GradeController
package edu.bit.answer;

import java.text.DateFormat;
import java.util.Date;
import java.util.Locale;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

/**
 * Handles requests for the application home page.
 */
@Controller
public class GradeController {
	
	private static final Logger logger = LoggerFactory.getLogger(GradeController.class);
	
	/**
	 * Simply selects the home view to render by returning its name.
	 */
	@RequestMapping("/grade/gradeInput")
	public String gradeIn( ) {
		return "/grade/gradeInput";
		
	}
	
	@RequestMapping("/grade/gradeOutput")
	public String graOut(Grade grade ) {
		return "/grade/gradeOutput";
		
	}
	
}
```
```java
//Grade
package edu.bit.answer;

public class Grade {	
	
	private double kor, eng, math;
	
	public Grade() {
		
	}
	
	public Grade(double kor, double eng, double math) {
		this.kor = kor;
		this.eng = eng;
		this.math = math;
	}

	public double getKor() {
		return kor;
	}

	public void setKor(double kor) {
		this.kor = kor;
	}

	public double getEng() {
		return eng;
	}

	public void setEng(double eng) {
		this.eng = eng;
	}

	public double getMath() {
		return math;
	}

	public void setMath(double math) {
		this.math = math;
	}
	
	public double gradeSum() {
		return kor + eng + math;
	}
	
	public double gradeAvg() {
		return (kor + eng + math) / 3.0;
	}

}
```

### ▶ 출력
![캡처](https://user-images.githubusercontent.com/74290204/105463441-65296a80-5cd3-11eb-9819-06964e4f91a9.PNG)
<br>

# 5. 아래를 프로그래밍하시오
## - 놓쳤던 부분 : 부모 자식으로 태그 관계를 만들어 사용해야 코드가 덜 복잡해진다! (addChild)
### *※ addChild, addEventListener 부분 다시 공부 (dom의 핵심이기 때문)*

```
- 제일 위에 입력버튼 하나를 만든다
- 버튼을 누르면 200*200 파란색 박스가 하나씩 계속해서 생기는 프로그램을 만드시오
```

```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
	pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Click</title>

	<script type="text/javascript">
		
		function inputbox() {
			document.getElementById("box").innerHTML += 
				 "<div style='background-color: blue; height: 200px; width: 200px'></div><br>";
		}
		
	</script>

</head>


<body>
	
	<div id ="box">	
		<input type="button" value="click me!" onclick="inputbox()"> 
		<!-- onclick에 함수명을 click으로 했더니 나오지않음 click은 키워드기 때문에
		변수명이나 함수명으로 쓰면 기능이 적용되지않음 ㅠㅠㅠㅠ!!(기초인데..ㅎㅎ알았으니다행..) -->
	</div>

</body>
</html>
```
### ▶ 출력
![캡처2](https://user-images.githubusercontent.com/74290204/105475879-3fa45d00-5ce3-11eb-8fb7-cdf3d7dbb714.PNG)
