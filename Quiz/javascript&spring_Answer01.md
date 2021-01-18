# 1. 자바스크립트 타입의 종류는?
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
