# 1. 절대경로와 상대경로에 대하여 설명하시오
## @ 절대경로 : root(경로의 첫 시작점) 에서 시작하는 경로, 최상위 경로부터 모든 경로를 표시하는 것
- 절대경로를 표기하는 종류로는 **http:// , / (웹브라우저) C:\ (컴퓨터)**

## @ 상대경로 : 절대 경로가 아닌 것, 현재 디렉토리에서 시작하는 경로
- 웹서버 프로그래밍에서 경로는 2가지의 경로를 가진다
    1. 물리적인 경로
    >> C:\Users\AI\eclipse-workspace\class_ex\WebContent
    2. 웹사이트에서 주소를 입력해서 들어오는 경로
    >>http:// ip/context명class_ex/ ← **/ = WebConetent**
      - 주소를 치고 들어올때도 경로가 나뉨
      ```
      # 유저가 치고 들어올 때 경로 http://ip/class_ex/request_send.jsp 
      # 물리적인 경로 http://ip/class_ex/WebContent/request_send.jsp
      ```
<br>

# 2. 아래의 action 속성에 대하여 아래의 3가지 케이스에 대하여 테스트 하고 결론을 내려 보세요
```jsp
/* 
- 0105/request_send.jsp
- ..0105/request_send.jsp
- /0105/request_send.jsp 
*/

<form action="0105/request_send.jsp">
당신의 나이는 : <input type="text" name="age" size="5">
		<input type="submit" value="전송">
	</form>
```
- 0105/request_send.jsp : 상대경로! 현재 페이지에 같은 선상(같은 폴더 0105)에 있는 request_send.jsp로 페이지 이동
- ..0105/request_send.jsp : 0105의 상위폴더에 있는 request_send.jsp로 페이지 이동
- /0105/request_send.jsp : / 붙었기 때문에 절대경로! 이렇게 표시해주면 http://ip 까지만 전송되기 때문에 context명까지 적어줘야한다! <br> context명이 javaex라 한다면 ▶ javaex/0105/request_send.jsp or request.getURI()/0105/request_send.jsp
>> request.getURL()은 절대경로를 만들어주는거기 때문에 사용 가능
<br>

# 3. css에서의 position 의 4가지 설명하시오
## - static
- **position의 default값** , default이기 때문에 생략가능하고 따로 positon을 지정해주지 않으면 static
- **top, left, right, bottom의 영향을 받지 않기 때문에 위치 조정이 불가능하다**

## - relative
- 상대적인 위치를 나타내는 속성으로 **주변의 위치(기존에 이미 위에서 선언된 위치)에 영향을 받아 위치한다**
- 동위선상일때는 동위선상에 있는 것에 대해서 영향을 받고 부모 자식간에는 부모에 영향을 받는다

## - absolute
-  **속성을 가지고 있지 않은 부모를 기준으로 움직인다** (if 부모중에 태그:relative, absolute, fixed가 없다면 가장 위의 태그: body를 기준으로 한다)

## - fixed : 위치 고정

# 4. float 속성에 대하여 설명하시오
- float = '띄운다'라는 의미 
- position과 마찬가지로 위치를 설정하기 위한 속성으로 **공중으로 띄워서 위치시키는 속성이다**
- float을 사용하면 띄워서 보여주는 거기 때문에 display할때 위치를 따로 지정해주지 않고 겹치게 되면 보이지 않게 된다 <br>
**단, 문자는 display할때 float영역 밖에 보이게 된다** (면적은 밑에 들어가는 지점부터 시작함) → float의 추가적인 기능 부분
<br>

# 5. 쿠키에 대하여 설명하시오
: http 프로토콜의 속성은 응답을 한번 주고 나면 서버와 유저의 연결을 끊게 되는데 연결을 어느 기간동안 유지해야되는 일들이 생김 <br> 예를 들어, 계속 로그인되어있는 상태여야 하는 것들 ! 따라서 그런 점을 보완하기 위해 나온 게 쿠키(Cookie)
- 쿠키(Cookie) : 클라이언트와 서버간의 연결을 유지시켜주기 위한 수단
- 쿠키는 모든 웹브라우저마다 존재하고 쿠키의 저장공간은 클라이언트쪽에 저장한다 
- 쿠키의 용량 : 4Kbyte , 300개까지 정보 저장이 가능 
<br>

# 6. 액션 태그에 대하여 설명하시오
: 액션태그란 어떤 동작을 하도록 지시하는 태그 / < jsp:action / >
- forward : 현재페이지에서 다른 페이지로 이동하기 위한 태그 <br>
**서버내에서 페이지를 넘겨주는거기 때문에 주소가 바뀌지 X** (redirect는 주소 바뀜 :클라이언트에게 다시 접근하게 하기 때문)
- include : 현재 페이지에서 다른 페이지를 가져오는 태그 (삽입하는 태그)
- param : forward 태그를 사용할때 값을 같이 전달하기 위한 태그 


---
# Quiz

## 1. 아래를 프로그래밍하시오
![q1](https://user-images.githubusercontent.com/74290204/103618380-37be8c00-4f73-11eb-9676-52e1eca798a8.PNG)

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
	
	<style>
		div {
			width: 600px;
			height: 50px;
		
		}
	
		#header {
			background-color: green;
		}
		
		#nav div:nth-child(1) {
			width: 200px;
			height: 300px;
			float: left;
			background-color: yellow;
		}
		
		#nav div:nth-child(2) {
			width: 200px;
			height: 300px;
			position : 200px;
			float: left;
			background-color: red;
		}
		
		#nav div:nth-child(3) {
			width: 200px;
			height: 300px;
			position : 400px;
			float: left;
			background-color: gray;
		}
		
		#footer {
			position: fixed; 
			/* footer는 nav와 동일선상에 있기 때문에 nav에 영향을 받음 
			만약 어떤 태그도 주지 않으면 footer에 부모인 <body>에 속성을 받게 되고 static이기 때문에 position의 영향을 받지않음
			(순서대로 쌓이는데 nav의 영향으로 위치가 변화)
			따라서 위치를 고정시키면서 position의 변화를 주기 위해 설정했음
			relative를 주면 nav를 기준으로 포지션이 움직임 */ 
			top: 350px;
			background-color: blue;
		}
	</style>
	
</head>
<body>
	<div id="header">헤더</div>
	<div id="nav">
		<div>컨텐츠 LEFT</div>
		<div>컨텐츠 CENTER</div>
		<div>컨텐츠 RIGHT</div>
	</div>
	<div id="footer">푸터<</div>

</body>
</html>
```
<details><summary> 출력</summary>

![a1](https://user-images.githubusercontent.com/74290204/103618486-63417680-4f73-11eb-9376-e2b3a682ea9c.PNG)
</details>

## 2. 아래를 프로그래밍하시오
![q2](https://user-images.githubusercontent.com/74290204/103618563-82d89f00-4f73-11eb-8e8a-c12abe23d445.PNG)

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>

	<style>
		#box { 
			width : 700px;
			height : 700px;
			border: 1px solid black;
			padding: 10px;
			margin :5px;
			text-align: center;
			border-collapse: collapse;
	
		}
		#box div {
			border: 1px solid black;
			border-collapse: collapse;
			padding: 10px;
			margin :0 auto;
			
		}
		
		#header {
			width: 650px;
			height : 100px;
			text-align: center;
			padding: 5px;

		}
		
		#nav {
			width: 650px;
			height : 100px;
			text-align: center;
			padding-left: 20px;
			
		}
		
		#nav p {
			width: 80px;
			height : 30px;
			line-height: 30px;
			border: 1px solid black;
			float: left;
			position : relative;
			left: 120px;
			text-align: center;
			
			
		}
		
		#nav div:nth-child(1) {
			border: 0;
			text-align: center;
		}
		
		#content {
			width: 650px;
			height : 300px;
			text-align: center;
			
		}
		
		#content div:nth-child(1) {
			width: 300px;
			height: 270px;
			float: left;
			margin-right: 2px; 
		}
		
		#content div:nth-child(2) {
			width: 300px;
			height: 270px;
			float: left;
			
		}
		
		#footer {
			width: 650px;
			height : 100px;
			text-align: center;
		}
		
	</style>

</body>
	<div id ="box">
	
		<div id="header">HEADER</div>
		<div id="nav">
			<div>NAVIGATION</div>
			<p>menu1</p>
			<p>menu2</p>
			<p>menu3</p>
			<p>menu4</p>
			<p>menu5</p>
		</div>
		<div id="content">
			<div>CONTENT</div>
			<div>BANNER</div>
		</div>
		<div id="footer">FOOTER</div>
		
	</div>
</html>
```html
<!-- answer.me-->
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>

	<style>
		#box { 
			width : 700px;
			height : 750px;
			border: 1px solid black;
			padding: 10px;
			text-align: center;
			border-collapse: collapse;
	
		}
		#box div {
			border: 1px solid black;
			border-collapse: collapse;
			padding: 10px;
			margin : 10px;
			
		}
		
		#header {
			width: 650px;
			height : 100px;
			text-align: center;
			padding: 5px;

		}
		
		#nav {
			width: 650px;
			height : 100px;
			text-align: center;
			padding-left: 20px;
			
		}
		
		#nav p {
			width: 80px;
			height : 30px;
			line-height: 30px;
			border: 1px solid black;
			float: left;
			position : relative;
			left: 120px;
			top : -15px;
			text-align: center;
			
		}
		
		#nav div:nth-child(1) {
			border: 0;
			text-align: center;
		}
		
		#content {
			width: 650px;
			height : 300px;
			text-align: center;
			
		}
		
		#content div:nth-child(1) {
			width: 280px;
			height: 270px;
			float: left;
			margin-right: 2px; 
		}
		
		#content div:nth-child(2) {
			width: 280px;
			height: 270px;
			float: left;
			
		}
		
		#footer {
			width: 650px;
			height : 100px;
			text-align: center;
		}
		
	</style>

</body>
	<div id ="box">
	
		<div id="header">HEADER</div>
		<div id="nav">
			<div>NAVIGATION</div>
			<p>menu1</p>
			<p>menu2</p>
			<p>menu3</p>
			<p>menu4</p>
			<p>menu5</p>
		</div>
		<div id="content">
			<div>CONTENT</div>
			<div>BANNER</div>
		</div>
		<div id="footer">FOOTER</div>
		
	</div>
</html>
```
<details><summary>출력</summary>

![a2](https://user-images.githubusercontent.com/74290204/103637340-b2e16b80-4f8e-11eb-9d63-6d9fcfd0ef5d.PNG)
</details>

```html
<!-- 출제자 정답-->
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
	<style>
	
        div {
            border:1px solid #cccccc;
            padding:5px; 
            margin:5px;
            text-align:center;
        }

        #con {
            width:800px;
            margin:0 auto;
            overflow:hidden;
        }

        #header {
            width:780px; 
            height:100px;
            line-height:100px;
        }

        #nav {
            width:780px; height:100px;
        }

        #nav ul { 
        	overflow:hidden; 
        }

        #nav ul li {
            width:138px; height:40px;
            line-height:40px;
            text-align:center;
            list-style:none;
            float:left;
            border:1px solid #dddddd;
        }

        #wrap {
            width:780px; 
            overflow:hidden;
        }

        #content {
            width:600px; height:300px;
            float:left;
        }

        #banner {
            width:135px; height:300px;
            float:left;
        }

        #footer {
            width:780px; height:100px;
            line-height:100px;
        }
    
	</style>
</head>
<body>
	<div id="con">
		<div id="header">HEADER</div>
		<div id="nav">
			<p>NAVIGATION</p>
			<ul>
				<li>menu1</li>
				<li>menu2</li>
				<li>menu3</li>
				<li>menu4</li>
				<li>menu5</li>	
			</ul>
		</div>
		
		<div id ="wrap">
			<div id="content">CONTENT</div>
			<div id="banner">BANNER</div>
		</div>
		
		<div id = "footer">FOOTER</div>
		
	</div>

</body>
</html>
```
