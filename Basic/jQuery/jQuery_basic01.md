# jQuery
: javascript를 한번 더 캡슐화한 라이브러리 (JS문법을 좀 더 쉽게 사용하기 위해 만든 언어)
- 따라서, 태그라이브러리가 존재


## @ jQuery 설치 방법
### [링크 참조] https://dullyshin.github.io/2019/09/05/jQuery-download/
1. 파일을 다운받아 적용
2. CDN을 통해 < header > 태그 안에 입력 
```jsp
	<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
```

▶ 배포시에 만약 인터넷이 안되는 곳에 적용하는 것이라면 무조건 1번 방법으로 JQuery를 설치 받아야 추후에도 사용 가능, <br>
CDN을 통해 사용하는 것은 인터넷이 작동한다는 가정하에 간단하게 사용

## @ 기본 문법
### $(선택자).함수명(매개변수); = jquery(선택자).함수명(매개변수)
   - jquery도 귀찮아서 $로 많이 사용한게 된 것 
   - EL과는 완전히 다름(전혀 별개의 문법!!)
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
