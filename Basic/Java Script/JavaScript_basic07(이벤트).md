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

### 4. 이벤트 객체

- this : 이벤트가 발생한 자기자신
- event : 이벤트를 눌렀을 때 key value형태로 값을 받아온다 

```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
	pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Click</title>

	<script type="text/javascript">
		window.onload = function() {
			function addHandler(e) {
				console.log("click!");
				console.log("this : " + this);
				
				var event = e || window.event;
				for(var key in event) {
					console.log(key + " : " + event[key]);
				}
			};
			
			var objD = document.getElementById("objDiv");
			objD.addEventListener("click", addHandler, false);
			
			var objP = document.getElementById("objPar");
			objP.addEventListener("click", addHandler, false);
		};
		
		
	</script>
	
	<style>
         #objDiv {
            width: 200px; height: 100px;
            line-height: 100px;
            text-align: center;
            font-size: 1.2em;
            background-color: #f00;
            color: #fff;
            font-weight: bolder;
         }
         
         #objPar {
            width: 200px; height: 100px;
            line-height: 100px;
            text-align: center;
            font-size: 1.2em;
            background-color: #0f0;
            color: #fff;
            font-weight: bolder;
         }
         
         #eventProp {
            width: 500px;
            border: 1px solid #cccccc;
         }
      </style>
	

</head>

<body>

	<div id="objDiv">Object Division</div>
	<p id="objPar">Object Paragraph</p>

</body>
</html>
```

### 5. 마우스
- mouseover : 설정한 영역에 들어갔을때
- mouseout : 설정한 영역에 나왔을때
- mouseup:
- mousedown : 
- dbclick : 더블 클릭 

### 6. form(양식) 
- button : button으로 처리하는 이유는 이벤트로 이용하기 위함! 
    - button으로 처리할때는 데이터 전송을 위해서 value="submit" 으로 표시
```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
	pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Click</title>

	<script type="text/javascript">
		window.onload = function() {
			
			// @JS의 유효성 검사!!
			var sbmBtn = document.getElementById("sbmBtn");
			sbmBtn.onclick = function() {
				if(document.getElementById("uid").value == "") {
					alert("user id blank!!");
				} else if(document.getElementById("upw").value == "") {
					alert("user pw blank!!");
				} else {
					alert("login Ok!!");
					document.getElementById("loginForm").submit();
				}
			};
			
			var resBtn = document.getElementById("resBtn");
			resBtn.onclick = function() {
				alert("reset Ok!!!");
				document.getElementById("loginForm").reset();
			};
		}
	</script>
	

</head>

<body>

	<form id = "loginForm" action="http://www.google.com">
		USER ID : <input id="uId" type="text" name="uId"><br>
		USER PW : <input id= "upw" type="password" name="upw"><br>
		<input id="sbmBtn" type="button" value="submit">
		<input id="resBtn" type="button" value="reset">

	</form>

</body>
</html>
```

- submit : 일반적인 sublmit을 사용할때는(< input type="submit >) onsubmit을 활용
- submit의 주체는 **form 자체** (form태그를 컨트롤, 따라서 form태그 안에 submit이 주체가 X)
```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
	pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Click</title>

	<script type="text/javascript">
		window.onload = function() {
			
			// @JS의 유효성 검사!!
			var lf = document.getElementById("loginForm");
			lf.onsubmit = function() {
				if(document.getElementById("uid").value == "") {
					alert("user id blank!!");
					return false;
					// false이면 데이터 전송X true여야 전송O
				} else if(document.getElementById("upw").value == "") {
					alert("user pw blank!!");
					return false;
				} else {
					alert("login Ok!!");
					return true;
				}
			};
	
		}
	</script>
	

</head>

<body>

	<form id = "loginForm" action="http://www.google.com">
		USER ID : <input id="uId" type="text" name="uId"><br>
		USER PW : <input id= "upw" type="password" name="upw"><br>
		<input id="sbmBtn" type="submit" value="SUBMIT">
		<input id="resBtn" type="reset" value="RESET">

	</form>

</body>
</html>
```
