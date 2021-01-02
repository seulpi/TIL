# CSS
- 목적 : html상에 style을 주는 것 (꾸밈), 디자인 요소
▶ css properties 링크 https://www.w3schools.com/cssref/ 

- html 태그상으로는 < style > → < style >을 사용하면 문법이 달라짐 (< head >< /head >안에 넣어서 하위코드들의 적용)
>> 선택자 { }

- ★ 선택자 : html안에 있는 요소(body안에 태그 요소)들을 선택하는 것

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="EUC-KR">
	<title>Insert title here</title>
	
	<style>
        /* div가 선택자 */
		div { 
			background:#ffd800;
		}
	</style>
</head>
<body>
	
	<h1>제목 입니다.</h1>
	<p>본문 입니다.</p>
	
	<div>
		<h1>제목 입니다.</h1>
		<p>본문 입니다.</p>
	</div>
</body>
</html>
```
<details><summary>▶ 출력</summary>
	
![css01](https://user-images.githubusercontent.com/74290204/103255065-2dc4e800-49cb-11eb-94e4-c43c4873fd0c.PNG)
</details>

# - 선택자 종류 
## @ * 선택자
- * : 전체를 뜻함(ALL), 하위 태그들을 전체적으로 선택해서 style을 적용
- 활용: 전체와 특정태그의 혼합 가능 ; 전체적으로 속성을 설정한 다음 특정 태그의 속성을 변경
>> 중복으로 적용하면 더 밑에 있는 코드가 적용됨!
<br>

## ★ @ id(#) & claa(.) 
- body 안에서 **id를 주게되면 선택자에서는 #선택자로 사용, class는 .선택자**
    - id나 class에 한해서 속성을 줄 수 있음

- #id 와 .class의 사용은?

## ★ @ overflow & float
: 어려운 부분(안되면 암기, 자바에서 지네릭과 같은 존재)
- float를 통해 위치 조정이 가능

## @ 속성 선택자
: input box에서 속성을 적용시킬 수 있다 
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
	<style>
		input {
			color: green;
			font-size: 30px;
			font-weight: bold;
		}
		
		input[type=text] {
			color: orange;
			font-size: 50px;
			width: 200px;
		}
		
		input[type=tel] {
		color: red;
		}
		
		img[src] {
			border: 5px solid green;
		}
	
	</style>

</head>
<body>
	<form>
		이름 : <input type = "text"/><br>
		아이디 : <input type = "text"/><br>
		비밀번호 : <input type = "password"/><br>
		전화번호 : <input type = "tel"/><br> 
	</form>
	
	<img src = "http://www.sba.seoul.kr/kr/images/header/Navi_dr.png"/> <br>
	
</body>
</html>
```

<details><summary>▶ 출력</summary>
	
![속성 선택자01](https://user-images.githubusercontent.com/74290204/103323241-472a6a80-4a85-11eb-9581-6ee1c2e54839.PNG)
</details>

## @ 후손 및 자손 선택자
: 하위 폴더를 선택해서 속성을 적용하는 것
- 트리모양으로 구조가 형성되어 있음 
```
div li { } : div 안에 모든 li를 선택해 속성 적용(후손들 싹 다)
div p { } : div 안에 모든 p를 선택해 속성 적용
div > h1 : 큰 쪽이 부모 div 바로 밑에 h1을 말한다 (자식만가능)
```
```html
!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
	<style>
		#header, #wrap {
			border: 1px solid #cccccc;
			width: 500px;
		}
		
		div li {
			background: red;
		}
		
		div p {
			font-size: 25px;
		}
		
		div > h1 {
			font-weight: bold;
			color: yellow;
		}
	</style>

</head>
<body>
	<div id = "header">
		<h1>LOGO</h1>
		<h2>global company</h2>
	</div>
	
	<div id="wrap">
		<p>서울시 8대 신성장동력의 하나인 녹색사업을~~~</p>
		<ul>
			<li>서울애니메이션센터</li>
			<li>서울시녹색산업지원센터</li>
			<li>서울게임콘텐츠센터</li>
		</ul>
	</div>
</body>
</html>
```

<details><summary>▶ 출력</summary>
	
![속성 선택자02](https://user-images.githubusercontent.com/74290204/103324235-f9fcc780-4a89-11eb-88c9-de972fcb599b.PNG)
</details>

## @ 동위선택자
: 같은 레벨에 있는 선택자 **(부모밑에 있는 게 아님!!)**
```
h3~div : h3에서 같은 레벨에 div까지 속성 적용
+ :  바로 옆에 있는 것 까지 속성 적용
```
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
	<style>
		h3~div {
			font-size: 10px;
			color: oreange;
		}
		
		h3+div {
			font-size: 20px;
			font-weight: bold;
			color: blue;
		}
		
		#title~div {
			width: 300px;
			background-color: yellow;
		}
		
	</style>
</head>
<body>
	<h3 id = "title">동위선택자(+,~)</h3>
	<div>div_01</div>
	<div>div_02</div>
	<div>div_03</div>
	<div>div_04</div>
	<div>div_05</div>
</body>
</html>
```

<details><summary>▶ 출력</summary>
	
![동위선택자](https://user-images.githubusercontent.com/74290204/103324259-1bf64a00-4a8a-11eb-86d2-503e072efabd.PNG)
</details>

## @ 반응 선택자
: 마우스의 반응에 따른 속성을 설정하는 선택자
- hover : 마우스
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
	<style>
		ul > li {
			font-size: 20px;
			font-weight: bold;
			color: orange;
		}
		
		li:hover {
			color: blue;
		}
	</style>
	
</head>
<body>
	<div>
		<ul>
			<li>menu1</li>
			<li>menu2</li>
			<li>menu3</li>
			<li>menu4</li>
			<li>menu5</li>
		</ul>
	</div>
</body>
</html>
```

<details><summary>▶ 출력</summary>
	
![반응 선택자](https://user-images.githubusercontent.com/74290204/103325079-7ba22480-4a8d-11eb-9f97-90abe4099d12.PNG)
</details>

## 상태 선택자
```
focus :
enabled
disabled : 못건드림(편집X)
```
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
	<style>
		input:focus {
			border: 2px solid #ff0000;
			padding: 10px;
		}
		
		input:enabled {
			color: #00ff00;
			font-weight: bold;
		}		
		
		input:disabled {
			color:#ff0000;
		}
	</style>
	
</head>
<body>
	<div>
		<form>
			이름: <input type = "text" name = "uname"/><br>
			아이디: <input type = "text" name = "uid""/><br>
			비밀번호: <input type = "password" name = "upw" value="12345" disabled = "disabled"/><br>
		</form>
	</div>
</body>
</html>
```

<details><summary>▶ 출력</summary>
	
![상태선택자1](https://user-images.githubusercontent.com/74290204/103325505-7514ac80-4a8f-11eb-87b6-a6fbb464fee8.PNG)
</details>

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
	<style>
		#content {
			width: 300px;
		}
		
		ul li {
			list-style: none; /* list style을 결정하는데 none 안해주면 디폴트로 '·' 생성  */
			border: 1px solid #cccccc;
			padding: 10px;
			background-color: #efefef;
			font-weight: bold;
			font-size: 20px;
		}
		
		ul li a {
			color: #000000;
		}
		
		ul li:nth-child(2n+1) {  /* 2n+1 : 홀수 , 2n : 짝수 */
			background-color: #de9393;
		}
		
		ul li:first-child, ul li:last-child {
			background-color: yellow;
		}
		
		ul li:first-child {
			border-radius: 10px 10px 0 0; /* 테두리 값 설정! 순서가 중요 1-2-4-3  */
		}
		
		ul li:last-child {
			border-radius: 0 0 10px 10px;
		}
	</style>
	
</head>
<body>
	<div id ="content">
		  <ul>
            <li><a href="#">menu1</a></li>
            <li><a href="#">menu2</a></li>
            <li><a href="#">menu3</a></li>
            <li><a href="#">menu4</a></li>
            <li><a href="#">menu5</a></li>
            <li><a href="#">menu6</a></li>
            <li><a href="#">menu7</a></li>
            <li><a href="#">menu8</a></li>
        </ul>
	</div>
</body>
</html>
```

<details><summary>▶ 출력</summary>
	
![선택자](https://user-images.githubusercontent.com/74290204/103326238-b8244f00-4a92-11eb-90a6-2a337eacdd15.PNG)
</details>

## @ 문자 선택자
: 특정 문자 or 문자열을 선택해서 속성 적용
```
em : 디폴트16px*em / ex) 2em = 16px*2em : 32px
first-line : 첫번째 줄 전체 (line은 줄 전체를 말함)
first-letter: 첫 글자
```
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
	<style>
	
		#history2:first-letter, #history1:first-letter {
			font-size: 2em;	
		}
		
		#history2:first-line, #history1:first-line {
			font-weight: bold;
			color:#2160d1;
		}
	</style>
</head>
<body>
	<div id ="wrap">
		<div id="herder">
			<h1>설립개요</h1>
		</div>
		
		<div id="content">
			<p> 중소기업 지원업무의 전문성과 효율성을 확보하고 중소기업에 대한 기술,경영, 인력등의 종합지원</p>
			<p id="history2">
				2014년 04월 09일 <br/> 서울산업진흥원 사명변경 6월30일 서울산업진흥원 상암DMC 이전
			</p>
			<p id="history1">
				2013년 01월 12일<br/> 다누리 시민청 개관 05월 09일 꿈꾸는청년가게 명동점 	
			</p>
			</div>
	</div>
</body>
</html>
```
<details><summary>▶ 출력</summary>
	
![1](https://user-images.githubusercontent.com/74290204/103388689-ce431580-4b4d-11eb-935b-908166d590ee.PNG)
</details>

### - 마우스로 드래그 했을 때 속성 변경
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
	<style>
		#content p:first-child::selection {
			background-color: blue;
			color: yellow;
		}
	</style>
</head>
<body>
	<div id ="wrap">
		<div id="herder">
			<h1>설립개요</h1>
		</div>
		
		<div id="content">
			<p> 중소기업 지원업무의 전문성과 효율성을 확보하고 중소기업에 대한 기술,경영, 인력등의 종합지원</p>
			<p id="history2">
				2014년 04월 09일 <br/> 서울산업진흥원 사명변경 6월30일 서울산업진흥원 상암DMC 이전
			</p>
			<p id="history1">
				2013년 01월 12일<br/> 다누리 시민청 개관 05월 09일 꿈꾸는청년가게 명동점 	
			</p>
			</div>
	</div>
</body>
</html>
```

## @ 링크 선택자
>> #선택자명 a::after { content: '-' after(href); } : a태그 안에 있는 것들을 content내용을 출력 후 뒤에 하이픈(-)을 붙이고 링크를 추가해라 
```
a::after / a::before
after: 해당내용의 뒤에
before: 해당내용의 앞에 
```

## @ 부정 선택자
>> #content li:not(.fa) { background-color:#ffd800 ] : id content로 묶여있는 태그안에 li를 찾아가서 class="fa"로 선언되지 않은것들에 대해서 속성을 적용해라
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
	<style>
		#content li:not(.fa) {
			background-color: #ffd800;
		}
	</style>
</head>
<body>
	<div id ="wrap">
	<ui>
		<li><a href="http://www.sba.seoul.kr" target="_blank">서울산업진흥원</a></li>
		<li class="fa"><a href="http://wiz.center" target="_balbk">위즈센터</a></li>
		<li><a href="http://www.yahoo.com" target="_blank">야후</a></li>
		<li class="fa"><a href="http://www.google.com" target="_balbk">구글</a></li>
	</div>
</body>
</html>
```
