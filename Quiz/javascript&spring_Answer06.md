# 1. 아래 annotation 의 용도는?
## @ModelAttribute
- command 객체의 **이름을 변경**해주는 annotation
- 이름 변경  + Model을 넘겨준다 
    - modle.addAttribute()를 할 필요가 없다(@ModleAttribute를 사용하면 Model을 같이 넘겨주기 때문)


# 2. id 와 pw 를 두개를 만든후 아래와 같이 유효성 검사를 하시오
```
-클라이언트쪽 체크: id 가 Null이거나 없으면 서버로 보내지 않으면서 - 해당 페이지에 '다시 입력하세요' 라는 문구 출력
-서버쪽 체크: id에 10자 초과이거나 숫자로만 되어 있어 있으면 다시 입력하는 페이지로 이동하여 '다시 입력하세요' 라는 문구 출력

-클라이언트쪽 체크: pw 가 널이거나 없으면 서버로 보내지 않으면서 - 해당 페이지에 '패스워드 다시 입력하세요' 라는 문구 출력
-서버쪽 체크: pw에 8자 미만이거나, 숫자로만 되어 있어 있거나, 문자로만 되어 있으면, 다시 입력하는 페이지로 이동하여 '패스워드 다시 입력하세요' 라는 문구 출력

-성공시 '로그인이 되었습니다' 라는 페이지로 이동
```

# 3. 마이바티스를 활용하여, emp 12개를 뿌리시오

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
