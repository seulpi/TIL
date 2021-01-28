# 이벤트 
- js가 사용하는 선택자들 다 사용이 가능! 이벤트도 마찬가지임 (따라서 크게 차이X)
>> [JS이벤트 정리] https://github.com/seulpi/TIL/blob/main/Basic/Java%20Script/JavaScript_basic07(%EC%9D%B4%EB%B2%A4%ED%8A%B8).md

## - $(선택자).이벤트이름(이벤트핸들러:리스너 { });
>> $(선택자).click(function() { console.log("Event!!!")});
```jsp
<!DOCTYPE html>
<html>

<head>
<title>Javascript</title>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
												
	<script>
		$(document).ready(function(){
			$(".logo").click(function() {
				console.log("logo lmg click!");
			});
			
		});
		</script>


</head>

<body>
      <div>
      	<img class="logo" src="img/Logo.png">
      </div>
     
</body>
</html>
```

## - 마우스 
## - form 
- event.preventDefault(); → 전송했을 때 서버로 바로 넘어가는 것을 방지함! , **다음 동작을 더이상 진행시키지 않고 싶을ㅍ때 사용**
    - form 태그의 고유동작은 submit 했을 때 action주소로 넘기는데  event.preventDefault()사용하면 **주소를 넘기지않고 중단** 시켜버림
```jsp
<!DOCTYPE html>
<html>

<head>
<title>Javascript</title>
<script
	src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>

<script>

		// 하나의 유효성 검사
		$(document).ready(function(){
			$("#loginForm").click(function(event){
				if($("#uId".val() == "")){
					alert("user id blank!");
					event.preventDefault();
				}else if($("uPw").val() == "") {
					alert("user pw blank!");
					event.preventDefault();
				}else {
					alert("login ok!");
					$("#loginForm").submit();
				}
			});
			
			$("#resBtn").click(function() {
				alert("reset ok!");
				$("#loginForm")[0].reset();
				//loginForm이 2개 이상 생길때 loginForm 전체에 대한 0번째를 reset 
			});
			
		});
		</script>


</head>

<body>
	<form id="loginForm" action="http://www.google.com">
		USER ID : <input id="uId" type="text" name="uId"><br>
		USER PW : <input id="uPw" type="password" name="upw"><br>
		<input id="sbmBtn" type="button" value="SUBMIT"> 
		<input id="resBtn" type="button" value="RESET">
	</form>
</body>
</html>
```

## - this, event
```jsp
<!DOCTYPE html>
<html>

<head>
<title>Javascript</title>
<script
	src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>

<script>

	$(document).ready(function() {
		function eventHandler(event) {
			console.log("click!!");
			console.log("this : " + this);

			for ( var key in event) {
				console.log(key + " : " + event[key]);
			}
		}
		;

		$("#objDiv").click(eventHandler);
		$("#objPar").click(eventHandler);

	});
</script>


</head>

<body>
	<div id="objDiv">Object Division</div>
	<p id="objPar">Object Paragraph</p>
</body>
</html>
```
