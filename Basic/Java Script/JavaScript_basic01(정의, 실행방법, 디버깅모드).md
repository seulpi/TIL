# - Java Script
### 1. 정의 : 클라이언트쪽 웹브라우저 페이지를 동적으로 만들기 위해 나온 언어(=클라이언트쪽 웹브라우저가 해석해주는 언어)
### 2. 목적 : html 태그들을 동적으로 변환시키기 위함
### 3. 특징
- ; (세미콜론) 사용하지않아도 에러나지 않는다 → 개발자의 습관적 ; 붙이기 (현대언어 예를들어 파이썬같은거는 ; 문법 따지지X)
-  자바스크립트는 웹브라우저 언어이기 때문에 컴파일 하지않고 바로 코드를 전송한다 → 따라서 컴파일 에러가 나도 그냥 뿌려진다
- 주석 : 자바와 동일하게 사용 //, /* */
- 자바와 관계가 전혀 없음 ^____^ , 별개의 언어로써 혼동하지 말 것! 
- 문자열 표시 : ' ', " " 둘 다 사용 가능
- 기본 문법
```html
	<script type="text/javascript"></script>
```

## @ Java Script 실행 테스트 방법 
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<script type="text/javascript">  <!-- 자바스크립트를 사용하겠다는 선언 -->
		alert("Hello World"); 
	</script>
</body>
</html>
```
![11](https://user-images.githubusercontent.com/74290204/104862733-46477300-5977-11eb-80ea-452c05c4f3a1.PNG)

![22](https://user-images.githubusercontent.com/74290204/104862735-46e00980-5977-11eb-8100-478f2de31d0c.PNG)

![33](https://user-images.githubusercontent.com/74290204/104862736-4778a000-5977-11eb-8346-6514e530b2c6.PNG)

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<script type="text/javascript">  <!-- 자바스크립트를 사용하겠다는 선언 -->
		alert("Hello World"); 
		console.log("Hello World"); /* System.out.println("Hello World"); */
	</script>
</body>
</html>
```

![4](https://user-images.githubusercontent.com/74290204/104863668-fd44ee00-5979-11eb-9707-3a13dabf5954.PNG)

## @ Java Script 디버깅 모드 확인 방법
![디버깅모드](https://user-images.githubusercontent.com/74290204/104863666-fb7b2a80-5979-11eb-9c88-dd0f854eb7f8.PNG)
