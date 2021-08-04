# 1. 아래가 에러가 나는 이유는?
```sql
SELECT DEPTNO, SUM(SAL), AVG(SAL) FROM EMP GROUP BY DEPTNO;
```

# 2. 그룹 함수의 종류는?
; 그룹함수란 하나의 로우를 그룹으로 묶어 연산해서 하나의 결과를 나타내는 함수
- sum : 합계를 내는 함수
- count : 조건에 맞는 로우의 갯수를 반환하는 함수 , count함수는 null값의 갯수를 count하지 않는다
- avg : 평균을 내는 함수 
- min : 조건에 맞는 최소값을 나타내는 함수
- max : 조건에 맞는 최댓값을 나타내는 함수 
<br>
☞ 그룹 함수는 group by를 할 때 사용 될 수 있다

# 3. 오라클에서 형의 종류와 변환 함수에 대하여 설명하시오
- 형의 종류 :
    1. NUMBER : 숫자
    2. DATE : 날짜
    3. VARCHAR : 문자
- 변환 함수 :
    1. TO_NUMBER : 숫자형으로 형변환 하는 함수 (숫자로~) 
        문자형은 연산할 수 없기때문에 연산을 하려면 숫자형으로 변환해줘야한다<br>(문자를 DATE로 바꾸는 이유도 마찬가지)
    2. TO_DATE : 날짜형으로 형변환 하는 함수 (날짜로~)
    3. TO_CHAR : 숫자나 날짜형을 문자형으로 변환하는 함수(문자로~)

# 4. decode 함수에 대하여 설명하시오
: 여러가지 조건에 대해서 선택할 수 있도록 하는 기능을 제공한다. 즉, **선택을 위한 함수(java에서 swutch-case문과 같은 역할)**

# 5. CASE 함수에 대하여 설명하시오
: 조건에 따라 서로 다른 처리가 가능한 함수, decode와 동일한 기능을 한다
- if-else문과 유사한 구조를 가진다 

### * decode함수와 case의 차이점 
- deacode함수는 비교연산자 '=' 경우에 대해서만 적용 가능하나 case함수는 다양한 비교 연산자가('<> , >='등)가 가능 

# 6. 아래를 프로그래밍 하시오
### - 객체 생성 
### - input box 2개를 생성해 가로, 세로 를 입력
### - 결과를 뿌리는 페이지

```html
<!--recInfo.html-->
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>

	<h1> 사각형의 가로와 세로를 입력하세요.</h1>
	<form action = "recArea.jsp" method = "get"> 
		가로 : <input type="text" name="width" size="10">
		세로 : <input type="text" name="height" size="10"><br>
		<input type="submit" value="전송">
	</form>

</body>
</html>
```
```jsp
//recArea.jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<jsp:useBean id="rec" class="answer2.Rectangle" />

<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	
	<%
		request.setCharacterEncoding("EUC-KR");
		rec.setWidth(Double.parseDouble(request.getParameter("width")));
		rec.setHeight(Double.parseDouble(request.getParameter("height")));
	
	%>
	
	사각형의 넓이: <%=rec.recArea()%>

</body>
</html>
```
```java
package answer2;

public class Rectangle {
	
	private double width, height;
	
	public Rectangle() {
		
	}

	public double getWidth() {
		return width;
	}

	public void setWidth(double width) {
		this.width = width;
	}

	public double getHeight() {
		return height;
	}

	public void setHeight(double height) {
		this.height = height;
	}
	
	public double recArea() {
		return getWidth()*getHeight();
	}

}
```

# 7. MVC 모델에 대하여 설명하시오
- MVC 모델은 MVC에서의 애플리케이션의 정보를 나타낸다
- MVC는 디자인패턴의 한 종류로 디자인패턴이란 프로그램을 개발하는 중에 발생한 문제점을 정리해 캡슐화한 것을 말한다
>> 디자인패턴의 목적 = 좀 더 쉽고 편리하게 사용할 수 있게 하는 것 (캡슐화의 목적)

- MVC(Model View Controller) 

## MVC 원리
사용자가 controller 조작 → controller는 model을 통해 데이터 get → 정보를 바탕으로 시각적인 표현을 하는 view 제어 → 사용자에게 전달

## @ Model 
- 어플리케이션의 정보, 데이터를 나타냄(DB, 처음 정의하는 상수, 초기화값, 변수 등을 말함)<br>
    → 이러한 DATA, 정보들의 가공을 책임지는 컴포넌트
### - 특징
1. 사용자가 편집하기 원하는 모든 데이터를 가지고 있어야한다
>> 예를들어, 네모박스 안에 글자를 넣는다고 가정하면 네모박스의 화면 위치정보, 박스의 크기정보,<br>
글자내용, 글자위치, 글자포맷 정보등을 가지고 있어야 하는 것
2. view 나 controller에 대한 어떠한 정보도 알고 있으면 안된다 
>> 데이터 변경이 일어났을 때 Model에서 화면 UI를 직접 조정&수정할 수 있도록 <br> 
view 를 참조하는 내부속성 값을 가지면 안된다는 것
3. 변경이 일어나면, 변경 통지에 대한 처리방법을 구현해야 한다
4. Model은 재사용 가능해야하고 다른 interface에서도 변하지 않아야한다

## @ View 
- 사용자 인터페이스 요소를 나타냄 , model에서 정보만 받아서 보여주는 담당 (출력역할만 할 뿐)
- html/css/javascript를 모아둔 컨테이너

## @ Controller
: 데이터와 사용자 인터페이스 요소들을 연결하는 다리 역할 <br>
즉 , 사용자가 데이터를 클릭하고 수정하는것에 대한 '이벤트'들을 처리하는 부분(흐름제어)
>> 사용자가 접근한 URL에 따라서 사용자의 요청사항을 파악한 후, 그 요청에 맞는 데이터를 Model에 의뢰하고, <br> 데이터를 View에 반영해서 사용자에게 알려준다

- Model 과 View와 달리 두가지의 정보를 알고 있어야한다 
- 어플리케이션의 메인 로직은 컨트롤러가 담당

## Model 1 방식
### *Model 1 방식이란 Controller에 View를 같이 구현하는 방식*
- 사용자의 요청을 jsp가 bean이나 class를 사용하여 전부 처리하는 방식
![model1](https://user-images.githubusercontent.com/74290204/103993276-b36d4280-51d8-11eb-80d0-8710f7a657bf.PNG)

## model 2 방식
### *Model 2 방식이란 Controller 와 View 영역을 분리해 구현하는 방식*
- 사용자의 요청을 Servlet이 받고 Servlet은 요청을 View로 보여줄건지 Model로 보여줄건지 정해 전송한다<br>
여기서 View 페이지는 사용자에게 보여주는 역할만 담당 / 실질적인 기능 → Model에서 담당
![model2](https://user-images.githubusercontent.com/74290204/103993610-37bfc580-51d9-11eb-8d7c-7e0c1b464f53.PNG)
