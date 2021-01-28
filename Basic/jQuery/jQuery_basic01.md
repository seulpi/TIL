# jQuery
: javascript를 한번 더 캡슐화한 라이브러리 (JS문법을 좀 더 쉽게 사용하기 위해 만든 언어)
- 따라서, 태그라이브러리가 존재
>>[jQuery UI] https://jqueryui.com/

# - jQuery 설치 방법
### [링크 참조] https://dullyshin.github.io/2019/09/05/jQuery-download/
1. 파일을 다운받아 적용
2. CDN을 통해 < header > 태그 안에 입력 
```jsp
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
```

▶ 배포시에 만약 인터넷이 안되는 곳에 적용하는 것이라면 무조건 1번 방법으로 JQuery를 설치 받아야 추후에도 사용 가능, <br>
CDN을 통해 사용하는 것은 인터넷이 작동한다는 가정하에 간단하게 사용

# - 선택자
## $(선택자).함수명(매개변수); = jquery(선택자).함수명(매개변수)
   - jquery도 귀찮아서 $로 많이 사용한게 된 것 
   - EL과는 완전히 다름(전혀 별개의 문법!!)
   - jQuery에서는 기존 선택자 다 사용 가능
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
		window.onload = function() {
			
			jQuery("p").css("color", "red");
			$("p").css("fontWeight", "bolder");
			
			var myJQ = jQuery("p");
			myJQ.css("textDecoration", "underLine");
			
			myJQ.css("width", "300px").css("border", "1px solid #ff0000");
			
			$("p").css("fontSize", "2em").css("height", "300px").css("lineHeight", "300px").css("textAlign", "center");
			/*
			$("p").css("height", "300px").css("lineHeight", "300px").css("textAlign", "center");
			$("p").css("lineHeight", "300px").css("textAlign", "center");
			$("p").css("textAlign", "center");
			
			이렇게 적용(결국 리턴하는건 뭐다? $("p") , $("p")안에 css적용하고 $("p")를 리턴한다는 것)
			*/
		};
	</script>
	

</head>

<body>

      <p> Hello jQuery! </p>

</body>
</html>
```

## - $(document).ready() = window.onload()
- html을 읽어들인다 = 메모리에 올린다는 뜻
- js에서 window.onload()는 **메모리에 올린 것들을 컨트롤**했는데 그와 같은 기능을 하는 $(document).ready()
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
		$(document).ready(function() {
			$("*").css("fontsize", "1.1em"); //전체 선택자
			$("li").css("color", "#0000ff"); //태그 선택자
			$("#item").css("background", "#ffff00").css("width", "300px"); //id 선택자
			$(".seoul").css("background", "#ff0000"); // class 선택자 
			$("#wrap p").css("border", "2px solid #cccccc"); //후손 선택자
			$("#wrap > p").css("border", "5px solid #cccccc"); //자손 선택자 (후손말고 자손만, 바로 다이렉트로 있는 것)
		});
		
	</script>
	

</head>

<body>
	<div id="wrap">
         <p>jQuery selector</p>
         <div id="centent">
            <h1>centent</h1>
            <p>ㄱㄴㄷㄹㅁㅂㅅㅇㅈㅊㅌ</p>
            <p>ABCDEFGHIJKLMN</p>
    </div>
         <div id="item">
            <ul>
               <li class="seoul">서울</li>
               <li class="gyeonggi_do">경기도</li>
               <li>충청도</li>
               <li>전라도</li>
               <li>경상도</li>
               <li>강원도</li>
               <li>제주도</li>
            </ul>
         </div>
      </div>
</body>
</html>
```

## - $(document).ready(functtion() { } = $(functtion() { } 
```jsp
$(function() {
			$("*").css("fontsize", "1.1em"); //전체 선택자
			$("li").css("color", "#0000ff"); //태그 선택자
			$("#item").css("background", "#ffff00").css("width", "300px"); //id 선택자
			$(".seoul").css("background", "#ff0000"); // class 선택자 
			$("#wrap p").css("border", "2px solid #cccccc"); //후손 선택자
			$("#wrap > p").css("border", "5px solid #cccccc"); //자손 선택자 (후손말고 자손만, 바로 다이렉트로 있는 것)
		});

```
▶ 출력 <br>

![선택자출력](https://user-images.githubusercontent.com/74290204/105927807-12133700-6088-11eb-9378-2a115f415337.PNG)

## - $(document).ready(functtion() { } → 하나의 스크립트에 여러개 올 경우 : 각각 순서대로 다 뿌린다 (onload는 마지막만 출력!!)
```jsp
<script type="text/javascript">
		$(function() {
			document.write("1번 출력");
		});
		
		$(function() {
			document.write("2번 출력");
		});
		
		$(function() {
			document.write("3번 출력");
		});
		
	</script>
=================================
▶ 1번 출력2번 출력3번 출력
```
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
		$(document).ready(function() {
			$("input[type='text']").css("color", "red"); // input text box 색상 변경
			$("input[type='password']").css("color", "blue") // input pw box 색상 변경
			$("input:submit").css("fontSize", "4em"); // input submit 글씨 크기 , fontsize로 대문자를 소문자로 쓰면 안먹음
			var chk = $("input:checked").val(); // check되어있는 안에 있는 value
			console.log("chk: " + chk); //check 뿌리기 
			
			$("li:nth-child(2n+1)").css("background", "#ffff00"); // list 자식들 중 홀수만 색상 변경
		})
		
	</script>
	

</head>

<body>
	<form>
         UserID <input type="text">
         <br />
         UserPW <input type="password">
         <br />
         Submit <input type="submit" value="submit">
         <br />
         Reset <input type="reset" value="reset">
         <br />
         Food <br />
         사과<input type="checkbox" id="apple" value="사과" />
         바나나<input type="checkbox" id="banana" value="바나나" checked />
         수박<input type="checkbox" id="watermelon" value="수박" />
         <br />
         
         <ul>
            <li>이만기</li>
            <li>이승엽</li>
            <li>이종범</li>
            <li>이재학</li>
            <li>이순철</li>
            <li>이길동</li>
            <li>이동국</li>
         </ul>
      </form>
</body>
</html>
```
▶ 출력<br>
![선택자2](https://user-images.githubusercontent.com/74290204/105927808-13446400-6088-11eb-903b-29496148808b.PNG)

## - appendTo , append() : 컨텐츠를 선택된 요소 내부의 끝 부분에서 삽입
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
		$(document).ready(function() {
			var wrapObj = $("wrap");
			
			var divObj = $("<div> Hello jQuery! </div>");
			divObj.appendTo(wrapObj); // appendTo를 사용하면 wrapObj가 부모가된다 
			divObj.css("background", "#ff0000");
		
			//= $("<div> Hello jQuery!>/div>.appendTo($("wrap")).css("background", "#ff0000"));
			
			var imgObj = $("<img src='img/logo.png' /><br>");
			imgObj.appendTo(wrapObj);
			
			var tempObj = {
				src : "img/arm_mbed.png",
				width: 297,
				height: 124,
				border: "5px"
			};
			
			var imgAObj = $("<img>", tempObj);
			imgAObj.appendTo(wrapObj);
		});
	</script>
	

</head>

<body>
	<div id="wrap">
		<div>Wrap Parents</div>
	</div>
	
</body>
</html>
```
```jsp
// append
<head>
<script type="text/javascript">
		$(document).ready(function() {
			 $("#wrap").append("<div>Hello</div>")
		});
</script>
</head>
<body>
	<div id="wrap">
		<div>contents</div>
	</div>
	
</body>	
```
▶ 출력<br>
![1](https://user-images.githubusercontent.com/74290204/105933023-1d1e9500-6091-11eb-9575-fdd386199949.PNG)

## - prependTo() , prepend() : 콘텐트를 선택한 요소 내부의 시작부분에 삽입 (부모안에 자식으로 들어가는데 그 안에서 위에 놓겠다, 앞에 추가)
```jsp
<head>
<script type="text/javascript">
		$(document).ready(function() {
			 $("#wrap").prepend("<div>Hello</div>")
		});
</script>
</head>
<body>
	<div id="wrap">
		<div>contents</div>
	</div>
	
</body>	
```
▶ 출력<br>
![prepend](https://user-images.githubusercontent.com/74290204/105934192-0d07b500-6093-11eb-85c4-b3ab21109ced.PNG)

## - after() : 선택한 요소 뒤에 컨텐츠 삽입
```jsp
<head>
<script type="text/javascript">
		$(document).ready(function() {
			 $("#wrap").after("<div>Hello</div>")
		});
</script>
</head>
<body>
	<div id="wrap">
		<div>contents</div>
	</div>
	
</body>	
```
▶ 출력<br>
![after](https://user-images.githubusercontent.com/74290204/105934445-8d2e1a80-6093-11eb-90c6-9c8b33218120.PNG)


## - before : 선택된 요소 앞에 컨텐츠 삽입
```jsp
<head>
<script type="text/javascript">
		$(document).ready(function() {
			 $("#wrap").before("<div>Hello</div>")
		});
</script>
</head>
<body>
	<div id="wrap">
		<div>contents</div>
	</div>
	
</body>	
```
▶ 출력<br>
![before](https://user-images.githubusercontent.com/74290204/105934512-afc03380-6093-11eb-9ea2-265ee5f9e5bd.PNG)
