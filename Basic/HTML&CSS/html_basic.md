# 들어가기 전 참조
- 팀 버너스리 : HTML 창시자 
- **https://www.w3.org/** : 월드 와이드 웹을 위한 표준을 개발하고 장려하는 조직의 사이트 팀 버너스리를 중심으로 1994년에 설립 
- W3C(영어: World Wide Web Consortium, 축약형은 영어: WWW 또는 W3)
- https://www.w3schools.com/ (여기서 모르는 코드들이나 언어들 개념들 search가능)


# - 용어 정리
- 웹 : 네트워트가 세계적으로 연결된 상태 
- HTML **(Hyper Text Markup Language)** : 웹문서를 기술하는 언어
- CSS(Cascading Style Sheets) : HTML문서를 **디자인적으로 예쁘게 만들어 정보 전달**을 하기 위해 만들어진 문서 
- 태그(Tag) : <>안에 있는것으로 태그에는 이름과 속성(property)이 있음
    - html은 태그를 사용하지만 출력되는 화면에서는 출력X (태그를 해서 속성을 적용할 뿐 → **해석해서 출력하는 주체: 톰캣X 웹브라우저O**)
>> [html_Answer01용어 참조] https://github.com/seulpi/TIL/blob/main/Quiz/JSP%26HTML(CSS)/jsp%26html_Answer01.md

```html
<!DOCTYPE html> <!-- !DOCTYPE: html ver.5이란 표시/ version-->
<html>
	<head>
	<meta charset="EUC-KR"> <!-- charset="EUC-KR" :이걸 속성이라 부름 (한국어 패치)-->
	<title>하하하</title> <!-- 링크 위에 타이틀 -->
	</head>
	<body>
		안녕하세요. 처음입니다.<br  />
		HelloWorld!!
	</body>
</html>
```
▶ 위 코드 출력
![html tag02](https://user-images.githubusercontent.com/74290204/103184518-ef142c80-48fb-11eb-90c3-6b334bd77413.PNG)
<br>

# - 태그 종류 
![html tag01](https://user-images.githubusercontent.com/74290204/103184605-3995a900-48fc-11eb-8456-7f185f775b4e.PNG)

### - 사이트 참조 
### - href : hypertext reference
<br>

## @ list : 표를 나타내는 태그
- < li > : list
- < ul > : unorder list (순서X)
- < ol > : order list (순서O)
- target="_blank" : 새 창으로 연결 
- target="_self" :  기존창에서 연결

![html tag03](https://user-images.githubusercontent.com/74290204/103184529-fb988500-48fb-11eb-84bc-ece7918492d7.PNG)

## @ table : 표를 나타내는 태그
```html
<table> : 표를 만들겠다 표시
<tr> : table row (행을 연다)
<td> : table devision (테이블을 나눈다 : 열)
<tablr border = "1"> : 경계를 1만큼의 굵기로~
<td rowspan="4">중간고사 성적</td> :이 자리에서 4개의 열을 합쳐라, 열을 잡아먹는 것(열을 붙여라)
<td colspan="2">평규ㅠㄴ</td> : 이 자리에서 2개의 행을 합쳐라 
```

**▶ table EX_출력**
![html tag04](https://user-images.githubusercontent.com/74290204/103185889-bf682300-4901-11eb-98fa-068948ef2f4e.PNG)
<br>

## @ image & audio & video
- src : 오디오파일의 경로를 명시
- controls : 오디오의 기본적인 동작을 조절할 수 있는 패널를 명시함
- autoplay : 오디오의 자동 재생 여부
- loop : 오디오의 반복 재생 여부를 명시
- preload : 오디오를 재생하기 전에 파일의 내용을 모두 불러올지를 명시함

```html 
<!--image html -->
<p><img src ="경로" alt="꿈 Story"></p>

<p><img src ="Navi_dr.png" alt="꿈 Story"></p> <!--상대경로-->
<p><img src ="http://www.sba.seoul.kr/kr/images/header/Navi_dr.png" alt="꿈 Story"></p> <!--절대경로-->
```

```html
<!--video html -->
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<video controls="controls">
		<source src="411.mp4" type="video/mp4" />
	</video>
	
	<video src="411.mp4" type="video/mp4" controls="controls">
	</video>

</body>
</html>
```
<br>

## - ★경로
1. 절대경로 : http://로 시작하는것 (루트, 처음부터 시작되는 위치가 없는것)
2. 상대경로: 파일명부터 시작, 내가 기준이 되어서 . 이거나 ./(현재폴더) 로 오는것 (위치는 아무표시가 없는 상태에서는 html에 있는 해당 폴더에서 파일을 찾는다)

## ★ @ form : 사용자의 데이터를 서버에 전송하게 해주는 태그
- 사용자가 다양한 정보를 입력하고 서버에 전달하고자 할 때 form 태그를 사용
- checkbox : ☑ 여러개 선택 가능 
- radio : ☑ 한개만 선택 가능
- checked : checked를 하면 디폴트로 체크되는것 (초기화) 
- input : 입력할 박스를 만드는 것 
- submit : 전송 (입력받은 값을 서버로 전송)
- button : 버튼은 submit과 달리 값 저장을 하지 않음, 그냥 누를 버튼만 생성
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<form action="#" method="get"> <!--action은 이 form을 보낼 주소, #은 자기페이지로 간다라는 뜻 --> 
		이름: 	<input type="text" name="uname" /><br />
		아이디: 	<input type ="text" name="uid" /><br />
		비밀번호: <input type="password" name="upw" /><br />
		연락처:	<input type ="text" name="uphone1" size="5" /> - 
				<input type ="text" name="uphone2" size="5"/> -
				<input type="text" name="uphone3" size="5"/><br />
		사진: 	<input type="file" name="upic/"><br />
		성별구분: <input type="radio" name="gender" value="m">남, 
				<input type="radio" name="gender" value="w">여 <br />
		사용언어: <input type="checkbox" name="lan" value="kor" checked="checked">
				<input type="checkbox" name="lan" value="eng" />영어,
				<input type="checkbox" name="lan" value="jap" />일어,
				<input type="checkbox" name="lan" value="chi" />중국어 <br />
		자기소개:	<textarea rows="5" cols="30">간단하게 입력하세요.</textarea><br />
		국적:	<select>
					<option>KOREA</option>
					<option>USA</option>
					<option>JAPAN</option>
					<option>CHINA</option>
				</select>
		좋아하는 음식:	<select multiple="multiple">
						<option>김치</option>
						<option>불고기</option>
						<option>파전</option>
						<option>비빔밥</option>
				</select><br />
			<input type = "submit"/>
	</form>
</body>
</html>
```
### ▶ <form action="#" method="get">  
: get 방식이니까 ?key=value형태로 넘어오게 됨
```
예를들어, 
http://localhost:8282/servlet_hello3/tag_ex.html?uname=a&uid=bcd&upw=1234&uphone1=010&uphone2=1234&uphone3=56789&upic%2F=&gender=w&lan=kor#

uname=a (uname이 key / a(:user가 입력한) 가 vaule 
&로 그 다음줄 받음 uid(key)=bcd(value)
```
<br>

## @ 레이아웃 구성 태그(div / span)
### - block태그 : 개행이 일어남 (div)
### - nonblock태그(inline) : 개행이 일어나지 않음 (span)
>> div 는 자동으로 개행이 일어나고(display하고 싶은 것을 개행만해줄 뿐)<br> span은 자동으로 개행이 일어나지X
- block안에 inline요소를 넣어 속성을 넣을 수 있음 

```html
<p>동해물과 <span type="color:blue font="bold"> 백두산이 마르고 <span/> 닳도록 </p>
```

<details>
<summary> ▶ 태그 종류 참조 </summary>
- https://calmdawnstudio.tistory.com/51
- https://potionstory.tistory.com/11
- https://stajun.tistory.com/entry/HTML-%ED%83%9C%EA%B7%B8-%EC%A0%95%EB%A6%AC
</details>

## @ 시멘틱(semantic)을 이용한 레이아웃 : div + 의미 
- 문서의 구조와 의미를 브라우저와 개발자 모두에게 명확하게 설명하는 태그 
- ex) < form >, < table >, < Header >등
	
>> [자세한 설명 - Quiz 참고] https://github.com/seulpi/TIL/blob/main/Quiz/JSP&HTML(CSS)/jsp&html_Answer03.md
```html
<!--시멘틱 이전 코드-->
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<div>
		<h1>MY HOMEPAGE</h1>
		<hr/>
	</div>
	
	<div>
		<ul>
			<li>HTML5</li>
			<li>CSS3</li>
			<li>JAVASCRIPT</li>
			<li>JQUERY</li>
		</ul>
		<hr/>
	</div>
	
	<div>
		<h1>What is HTML5?</h1>
		<p> HTML5 is gooooooooooood </p>
	</div>
	<div>
		<p>xxx주식회사 서울시 oo구oo동</p>
	</div>
</body>
</html>
```
▶ 출력
![시멘틱01](https://user-images.githubusercontent.com/74290204/103254117-6cf13a00-49c7-11eb-9bda-f273c22d9a8e.PNG)

```html
<!--시멘틱 코드-->
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<header>
		<h1>MY HOMEPAGE</h1>
		<hr/>
	</header>
	
	<nav>
		<ul>
			<li>HTML5</li>
			<li>CSS3</li>
			<li>JAVASCRIPT</li>
			<li>JQUERY</li>
		</ul>
		<hr/>
	</nav>
	
	<section>
		<h1>What is HTML5?</h1>
		<p> HTML5 is gooooooooooood </p>
	</section>
	
	<footer>
		<p>xxx주식회사 서울시 oo구oo동</p>
	</footer>
</body>
</html>
```
▶ 출력
![시멘틱02](https://user-images.githubusercontent.com/74290204/103254118-6e226700-49c7-11eb-83ff-5da0760f8e5e.PNG)

>> (폰트사이즈 크기의 차이는 좀 있음)
