# - JS
### : 외부 자바 스크립트 파일을 적용하는 방법 (import와 비슷)
>> [문법] < script src="주소"> < /script>
```html 
<!-- javascript.html -->

<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<script src="js/hello.js" type="text/javascript">  
	<!-- src="js/hello.js" 자바스크립트 파일 js 폴더 안에 있는 hello를 찾아가 실행시킨다 
	js = 외부파일 이용하는 방법 -->
	</script>
	
</body>
</html>
```
```java script
// hello.js (javascript file)
alert("HelloWorld!!");
```

# - prompt / confirm / alert 
- prompt , confirm : 입력창 
- alert : 출력창
```html
<!-- javascript.html -->

<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<script type="text/javascript">  
		alert("Hello Javascript!!");
		
		/* var = variable 자바스크립트의 데이터 타입
		(데이터 타입이 존재하나 다 받아내는 타입이고 웹브라우저가 알아서 데이터 타입을 구별해냄) */
		var inputPro = prompt("출력창입니다." , "문장을 입력하세요.") 
		// 화면 실행시켰을때 "출력창입니다" 는 보여지는 해당 문구, "문장을 입력하세요"는 해당 부분에 문장입력할 수 있는 칸 
		alert(inputPro);
		/* 위 prompt 실행후 입력한 메세지 확인 메세지
		("문장을 입력하세요"는 변수! 언제든지 다른 값 가능, 다른 값을 입력하면 여기서 그 값이 보여진다) */
		
		var inputCon = confirm("진행하겠습니다.");
		// confirm은 입력할 수 있는 칸이 아닌 해당 내용이 잘 전달되었는지 확인
		alert(inputCon);
		// confirm = ture, false 로 값을 넘긴다
	</script>
</body>
</html>
```

# - 데이터 타입 
## : 6가지지만 타입구분없이 var로 다 받아냄 
   - 알아서 웹브라우저에서 데이터 타입을 해석해냄
### 1. 문자열 : String 
### 2. 숫자 : Number
### 3. Boolean
```html 
<!-- javascript.html -->

<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<script type="text/javascript">  
		var str = "가나다라마바사";
		console.log("str : " + str);
		
		var num = 123;
		console.log("num : " + num);
		
		var boo = true;
		console.log("boo : " + boo);
	</script>
	
</body>
</html>
```
### ▶ 출력 

![데이터타입](https://user-images.githubusercontent.com/74290204/104865219-c3c2b180-597e-11eb-92c3-80062cacdd44.PNG)

### 4. **함수 : 함수도 한개의 데이터 타입!**
### 5. 객체 
>> [객체 표현 방식] → { }
   - 어떤언어의 객체는 반드시 알고 있어야함 따라서 까먹지말자
### 6. undefined : 아무것도 없는 상태, null과 비슷
```html
<!-- javascript.html -->

<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<script type="text/javascript">  
		
		var varFun = function fun() { };
		console.log("varFun: " + varFun);
		//function fun() { }; 는 함수를 변수에 대입이 가능하다, 리턴타입이 function 인 이유는 변수표현을 var 1개이기 때문에
		
		var varObj = {}; //자바에서는 배열 초기화 표시지만 자바스크립트에서는 객체를 표현한다
		console.log("varObj : " + varObj);
		  
		var varUnd = undefined;
		console.log("varUnd : " + varUnd);
		
		//undfined: 선언만하고 초기화하지 않은 상태
		var varUndfined;
		console.log("varUndfined : " + varUndfined);
		varUndfined = 1;
		console.log("varUndfined : " + varUndfined);
		
	</script>
	
</body>
</html>
```

### ▶ 출력 

![데이터타입2](https://user-images.githubusercontent.com/74290204/104867635-aabcff00-5984-11eb-9697-83359ed1b96f.PNG)


## @ typeof() : 해당 변수의 타입을 명시해준다
```html
<!-- javascript.html -->

<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<script type="text/javascript">  
		
		var varType = "123"; //String 타입이기 때문에 문자+숫자는 한줄로 붙여서 연산(문자형으로 따른것)
		console.log("varType type: " + typeof(varType));
		console.log("varType : " + (varType + 1000));
		
		varType = Number(varType); // varType을 숫자로 형변환했기 때문에 숫자+숫자로 연산해서 결과값 출력
		console.log("varType type: " + typeof(varType));
		console.log("varType : " + (varType + 1000));
	</script>
	
</body>
</html>
```
