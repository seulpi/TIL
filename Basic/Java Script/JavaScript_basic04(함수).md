# - ★ 함수 (자바와는 조금 다른 특징으로 사용되기 때문에 잘 이해하자)
: 자바스크립트에서 함수는 데이터타입으로도 사용할 수 있고 리턴타입도 가능하다 
- 데이터 타입을 명시해주는 표현이 **'var'** 하나이기 때문에 내부적으로 알아서 데이터타입을 정리하고 실행함
    - * 주의) 함수에 리턴타입이 없기 때문에 표시할 형태가 X

## @ 함수 종류
### 1. 명시적 함수 : 이름있는 함수 
```javascript
function 함수이름 () {
    실행문;
}

// 함수호출 ▶ 함수이름();
```

### 2. 익명 함수 : 이름 없는 함수
```javascript
var 변수이름 = function() {
                 실행문;
                }

// 함수호출 ▶ 변수이름();
```

### ☞ 자바스크립트는 코드를 순서대로 쭉 읽어들이는데 명시적함수를 제일 먼저 읽기때문에 뒤에 함수를 선언해도 에러가 나지않지만 익명함수는 함수가 먼저 선언되고 호출되야 에러가 나지않는다 (why? 익명함수는 변수에 함수를 대입하고 '='라는 연산을 해야하기 때문에 선언해주지않으면 null인상태) 예를들어 명시적함수는 static , 익명함수는 일반 데이터 멤버라고 생각하면 비슷할듯하다


## @ 클로저 : 함수 in 함수
- 함수는 리턴타입으로 함수를 받을 수 있기 때문에 **함수 안에 함수 사용이 가능하다** 
    - 익명함수던 명시적함수던 다 가능! 

### ▶ 즉, 클로저란 내부함수가 외부함수의 지역변수에 접근할 수 있고 외부함수는 외부함수의 지역변수를 사용하는 내부 함수가 소멸될때까지 소멸되지 않는 특성을 의미한다

```javascript
function funName() {
	return function(x) {
		for(var i =1; i <10; i++) {
			console.log(x + "*" + i + "=" +(x*i));
		}
	};
}
		
var returnFun = funName();
returnFun(4);
```
```javascript
function outFun(width, height) {
			document.write("triangle : " + triangle());
			document.write("quadrangle : " + quadrangle());
			
			function triangle() {
				return (width * height) / 2;
			}
			
			function quadrangle() {
				return width * height;
			}
		}
```

### 3. ★ 콜백(CallBack) 함수 : **매개변수로 함수** 를 전달하고 전달된 매개변수가 특정 시점에 호출되는 함수
<br> ▶ 함수안에 파라미터로 들어올 수 있는 이유? 함수도 하나의 자료형이기 때문에 <br> 

- 함수 호출이 아닌 매개변수이기 때문에 함수이름만 넣어준다
    - 함수의 첫번째 주소를 넘겨주는 것! 
```javascript
function funName (funCBF, funCBP, num) {
			for(var i =1; i<= 10; i++) {
				console.log(num + "*" + i + " = " + (num*i));
				if( i < 10) {
					funCBP(i);
				}
			}
			
			funCBF();
		};
		
		function funCallBackProgress(n) {
			console.log((n * 10) + "% 진행완료!");
		};
		
		function funCallBackFinish() {
			console.log("서버 작업 종료!");
		};
		
		funName(funCallBackFinish, funCallBackProgress, 7);
```

### 4. 내장함수 : JS에서 기본적으로 제공하는 함수 
>> console.log() , setTimeout(), clearTimeout() 등등 
- setInterval(매개변수, 시간) : 시간마다 매개변수를 실행
- setTimeOut(매개변수, 시간) : 시간 뒤에(after) 매개변수를 실행 
    - 이 함수는 딱 한번 실행됨, 반복을 원하면 → setInterval 사용
    - [tip] 1000 ms = 1 second
- clearTimeout() : setTimeout ()으로 설정된 함수가 실행되지 않도록 하는 함수
- eval() : 문자열을 코드(소스)로 이용 → 해킹의 원흉 (소스코드를 쿠키로 이용해서 해킹)
    - 왠만하면 사용하지 않는것이 좋다
```javascript
//setTimeOut, clearInterval
var printTimeOutId = setTimeout(function() {
	clearInterval(printIntervalId);
	console.log("종료!");}, 7000);
//setInterval, setInterval		
var printIntervalId = setInterval(function() {
	console.log("*")}, 2000);
		
//eval
var varEval = "console.log('eval함수!')";
	eval(varEval);
		
	varEval = "document.write('<p>eval함수</p>');";
	eval(varEval);
```
