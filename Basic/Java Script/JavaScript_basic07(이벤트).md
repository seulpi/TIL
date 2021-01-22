# 이벤트
- 이벤트란 마우스,버튼, 포커스 ,웹페이지로드, 양식전송등과 같은 행위를 말한다 (특정버튼을 클릭했을때, 양식을 전송할때와 같은 행위) 
- 이벤트 핸들러(→ 함수)를 통하여 이벤트가 발생했을 때 원하는 함수에 연결해 실행시킬수 있다 

## @ 이벤트 모델
### 1. 인라인(inline) 모델 : 이벤트를 HTML 요소의 속성으로 직접 지정하는 방식
- onclick 이벤트 = 마우스 클릭
```jsp
<!DOCTYPE html>
<html>

   <head>
      <title>Javascript</title>
      <script>
      	function headerClick() {
      		console.log("click!!");
      	/* 	var ce = document.getElementById("cEvent");
      		ce.onclick = null;
      		하면 더이상 onclik의 이벤트 기능 실행하지 않게 만드는것
      		*/
      	}
      </script>
   
		<style>
		
		#cEvent {
		width: 200px;
		height: 100px;
		line-height: 100px;
		text-align: center;
		font-size: 1.2em;
		background-color: red;
		color: white;
		font-weight: bold;
		}
		</style>
   </head>
   
   <body>
	<diV id="cEvent" onclick="headerClick();">click event</diV>
	<!-- 마우스 클릭했을때 headerClick()함수를호출하겠다 -->
   
   </body>
</html>
```

### 2. 기본 모델 : 다이렉트로 함수를 넣어 이벤트를 주는 방법
- onclick의 함수 연결해주는 방법
```jsp
<!DOCTYPE html>
<html>

<head>
<title>Javascript</title>
<script>
	window.onload = function() {
		// 익명함수 방법1(다이렉트로 id 연결)
		var ce = document.getElementById("cEvent");
		ce.onclick = function() {
			console.log("click!!");
		
		// 명시적함수 방법2(onclick의 함수 연결)
		ce.onclick = clickEventHandler;
		function clickEventHandler() {
			console.log("clickEvent!!");
		}

		}
	}
</script>

<style>
#cEvent {
	width: 200px;
	height: 100px;
	line-height: 100px; text-align : center;
	font-size: 1.2em;
	background-color: red;
	color: white;
	font-weight: bold;
	text-align: center;
}
</style>

</head>

<body>
	<div id="cEvent">click event</div>

</body>
</html>
```

### 3. 표준 모델 
- Listener  : 콜백함수를 넣는것
```jsp
<!DOCTYPE html>
<html>

<head>
<title>Javascript</title>
<script>
	window.onload = function() {
		function addHandler() {
			console.log("click!!");
		};
		
		function removeHandler() {
			console.log("event remove!!");
			ceA.removeEventListener("click", addHandler, false);
		};
		
		var ceA = document.getElementById("cEventAdd");
		ceA.addEventListener("click", addHandler, false);
		
		var ceR = document.getElementById("cEventRem");
		ceR.addEventListener("click", removeHandler, false);
	}
</script>

<style>
#cEventAdd {
	width: 200px;
	height: 100px;
	line-height: 100px; text-align : center;
	font-size: 1.2em;
	background-color: red;
	color: white;
	font-weight: bold;
	text-align: center;
}

#cEventRem {
	width: 200px;
	height: 100px;
	line-height: 100px; text-align : center;
	font-size: 1.2em;
	background-color: blue;
	color: white;
	font-weight: bold;
	text-align: center;
}
</style>

</head>

<body>
	<div id="cEventAdd">click event</div>
	<div id="cEventRem">click event</div>

</body>

</html>
```
