# - 배열 
- index는 '0'부터 시작 
- 배열 객체는 **new로 생성**
- 값 다이렉트로 초기화 → [ ] 사용 
- lengrh() : Ok
- indexOf() : 인덱스 검색
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
