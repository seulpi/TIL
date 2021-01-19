# - 배열 
- index는 '0'부터 시작 
- 배열 객체는 **new로 생성**
- 값 다이렉트로 초기화 → **[ ]** 사용 
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
	var varArr1 = new Array();
	var varArr2 = new Array(5);
	var varArr3 = new Array(123, "ABC", true, function fun(){}, {}, undefined); // 데이터타입을 다 맞추지 않아도 가능
	var varArr4 = [123, "ABC", true, function fun(){}, {}, undefined];

	console.log("varArr1: " + varArr1);
	console.log("varArr2: " + varArr2);
	console.log("varArr3: " + varArr3);
	console.log("varArr4: " + varArr4);
	</script>
	
</body>
</html>
```
## @ 배열 함수

### - length() : 배열의 길이
### - indexOf() : 처음부터 순차적인 인덱스 값 검색 

### - lastIndexOf() : 배열의 끝에서부터 인덱스 값 검색 
- 배열에 해당하는 값이 없으면 -1로 반환 

### - concat() : 배열간 값 연결
```java script
var arr1 = new Array ("배열 1", "배열 2");
var arr2 = new Array ("배열 3", "배열 4");
var arr = arr1.concat(arr2);
```

### - join() : 배열 데이터를 문자열로 반환 
- 전체를 묶어주는데 묶을 조건을 줘서 연결하는 것
```java script
var varArrJoin = new Array("ABC", "DEF", "GHI", "JKL");
	console.log("varArrJoin : " + varArrJoin.join());
	console.log("varArrJoin : " + varArrJoin.join(" | "));
	console.log("varArrJoin : " + varArrJoin.join(":"));
	console.log("varArrJoin : " + varArrJoin.toString());
```

### - sort() : 배열 정렬

### - reverse() : 배열값의 순서를 반대로~
```java script 
var varArrReverse = new Array("E", "B", "A", "C", "D");
	console.log("varArrReverse : " + varArrReverse);
	console.log("varArrReverse.reverse() : " + varArrReverse.reverse());
	console.log("varArrReverseAfter : " + varArrReverse);
```
### **▶ mutable 이기때문에 reverse하면 한 후에 그대로 값이 저장된다**

### - push() : 데이터를 추가, 배열의 크기를 리턴 (추가한 후 값이 증가)

### - shift() : 배열의 첫 번째 요소를 제거한 후, 제거한 요소를 반환

### - pop() : 배열의 마지막 요소를 제거한 후, 제거한 요소를 반환
<br>

## @ argument : 매개변수를 데이터로 하는 배열 객체 
- **함수 안에서 사용할 수 있도록 이름이나 특성이 약속되어 있는 일종의 배열(자바스크립트만의 특징)**
- 변수를 지정해주지 않아도 선언만 하게되면 배열이 사용가능하고 후에 초기화해서 사용이 가능하다(갯수 제한X)
```javascript
function funName() {
	return arguments;
	}

	var varArr;
	varArr = funName(1, 2, 3, 4, 5, 6, 7);
	console.log("varArr : " + varArr);
	console.log("varArr.length : " + varArr.length);
		
	for(var i in varArr) {
		console.log("varArr[" + i + "] : " + varArr[i]);
	}
```

<br>

# - 조건문 : Java문법과 동일 
## - if, else if
```java script
var num = 100;	
	if(num > 50 ) {
		console.log("num > 50");
	} if (num <= 50) {
		console.log("num <= 50");
	} console.log(num);
```

## - switch

## - 삼항연산자
<br>

# - 반복문 : Java문법과 동일
## - for
```java script 
var varArr = [1, 2, 3, 4, 5];
	for(var i = 0; i <varArr.length; i++) {
		console.log("varArr[" + i + "] : " + varArr[i]);
	}for(var i in varArr) 
		document.write("varArr [" + i + "] : " + varArr[i]);
```

## - forEach문 (enhanced for문)
>> for(var i in varArr) = for(int i : varArr)
