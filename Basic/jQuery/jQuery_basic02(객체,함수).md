# - 객체 
## 1. 삽입
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
			  var wrapObj = $("#wrap");
	            
	            /*
	               객체 생성
	            */
	            var divObj = $("<div> Hello jQuery! </div>");
	            
	            /*
	               객체 삽입
	            */
	            
	            divObj.appendTo(wrapObj);      //.appendTo()
	            //wrapObj.append(divObj);      //.append()
	            //divObj.prependTo(wrapObj);   //.prependTo()
	            //wrapObj.prepend(divObj);      //.prepend()
	            
	            //divObj.insertAfter(wrapObj);   //.insertAfter()
	            //wrapObj.after(divObj);      //.after()
	            //divObj.insertBefore(wrapObj);   //.insertBefore()
	            //wrapObj.before(divObj);      //.before()
	     		});
	</script>
	

</head>

<body>
	<div id="wrap">
		<div>contents</div>
	</div>
	
</body>
</html>
```

## 2. 이동
```jsp
	<script>
		$(document).ready(function() {
			$("#wrap > .contents1 > .p");
		});
	</script>
```
▶ 출력<br>
![객체이동1](https://user-images.githubusercontent.com/74290204/105933913-8e127c80-6092-11eb-9c94-a14d05aa2dc4.PNG)

```jsp
<!DOCTYPE html>
<html>

<head>
<title>Javascript</title>
	<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>	
	<script>
		$(document).ready(function() {
			$("#wrap > .contents1 > .p").appendTo($("#wrap > .contents2"));
		});
	</script>
	
	<style>
		#wrap .contents1 .p {
			background-color: yellow;
		}
		
		#wrap .contents2 .p {
			background-color: red;
		}
	</style>

</head>

<body>
      <div id="wrap">
         <div class="contents1">
            <p class="p">contents1</p>
         </div>
         
         <div class="contents2">
            <p class="p">contents2</p>
         </div>
      </div>

</body>

</html>
```
▶ 출력<br>
![객체이동2](https://user-images.githubusercontent.com/74290204/105933909-8ce14f80-6092-11eb-94c6-cfdd81cffa14.PNG)

## 3. 복사: clone
```jsp
<!DOCTYPE html>
<html>

<head>
<title>Javascript</title>
	<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>	
	<script>
		$(document).ready(function() {
			($("#wrap > .contents1 > .p").clone()).appendTo($("#wrap > .contents1 > .p"));
		});
	</script>
	
	<style>
		#wrap .contents1 .p {
			background-color: yellow;
		}
	</style>

</head>

<body>
      <div id="wrap">
         <div class="contents1">
            <p class="p">contents1</p>
         </div>
</body>

</html>
```
▶ 출력<br>
![clone](https://user-images.githubusercontent.com/74290204/105934730-15142480-6094-11eb-8481-4d90f2a4c6d4.PNG)


# - 함수
## 1. ★ each() : 배열
>> each(function(index, item)) { }
- index : 순서, 인덱스 값
- item : 객체
```jsp
<!DOCTYPE html>
<html>

<head>
<title>Javascript</title>
	<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>	
	<script>
		$(document).ready(function(){
			
			$("p").each(function(index, item) {
				console.log($(item).text());
				// console.lod($(thid).text()); → item = this
				
				if(index % 2 == 0) {
					$(item).css("background", "red");
					// $(this).css("background", "dfdfdf"); → item = this
				}else {
					$(item).css("background", "yellow");
				}
				
			})
		});
	</script>
	
	
</head>

<body>
     <p>hello</p>
     <p>jQuery</p>
     <p>so tired</p>
</body>
</html>
```

## 2. ★ html(), text() : 내용 읽고 / 쓰기 
>> $(선택자).html();
- 선택자 하위에 있는 자식을 태그나 문자열 따지지않고 전부 다 select
```jsp
html("<p>text</p>") : 태그나 문자열을 나눠서 인식 
→ <p>태그를 태그로 인식 
```

>>$(선택자).tect();
- 선택자 하위에 있는 자식 태그들의 문자열만 select
```jsp
text("<p>text</p>") : 태그까지 문자열로 인식 
→ <p>text</p> 출력 
```

>> $(선택자).val();
- input 태그에 정의된 value 속성의 값을 확인하고싶을 때  사용 
```jsp

<!DOCTYPE html>
<html>

<head>
<title>Javascript</title>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
												
	<script>
		$(document).ready(function(){
			
			console.log("htmlMethod : " + $("#htmlMethod").html());
			$("#htmlMethod").html("<strong>new</strong>" + $("#textMethod").html());
			console.log("htmlMethod : " + $("#htmlMethod").html());
			//<strong> : bold처리 , html() : html 함수안에 내용을 넣으면 기존의 내용을 다 지워버림
			
			console.log("textMethod : " + $("#textMethod").text());
			$("#textMethod").html("new" + $("#textMethod").html());
			// text() : 안에 내용만 바꿈 
			
		});
		</script>


</head>

<body>
	<div id="htmlMethod">html function</div> 
	<div id="textMethod">text function</div>  
</body>
</html>

==============================================================================
▶
htmlMethod : html function
htmlMethod : <strong>new</strong>text function
textMethod : text function
```

## 3. addClass : class 추가 
## 4. removeClass : class 삭제
- remove : **객체 제거**
- empty : 객체 **내부 제거**
## 5. attr : attribute, 속성

```jsp
<!DOCTYPE html>
<html>

<head>
<title>Javascript</title>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
												
	<script>
		$(document).ready(function(){
			$("#addClass").addClass("addCla"); //id addClass에 class addCla 추가
			$(".addCla").css("background", "#ff0000");
			
			$(".remCla").css("background", "#00ff00");
			$(".remCla").css("background", "#0000ff");
			
			$("#attrMethod img").attr("src", "img/logo.png");
			console.log("#attrMethod img src : " + $("#attrMethod img").attr("src"));
			
			$("#attrMethod img").removeAttr("src");
			console.log("#attrMethod img src : " + $("#attrMethod img").attr("src"));
			
		});
		</script>


</head>

<body>
      <div id="addClass"> addClass() 메서드 </div>
      <div id="removeClass" class="remCla"> removeClass() 메서드 </div>
      
      <div id="attrMethod">
         <img>
         <br>
         <img>
      </div>
</body>
</html>
```
