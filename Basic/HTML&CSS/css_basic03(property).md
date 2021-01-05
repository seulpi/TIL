# - margin & padding 
## @ margin : 외부 간격(외부 공백) 정의
1. margin-top : 위 
2. margin-bottonm : 아래
3. margin-right : 오른쪽
4. margin-left : 왼쪽 

## @ padding : 내부 간격(내부 공백) 정의 (padding도 top.botton,right,left 다 설정 가능)

![Padding](https://user-images.githubusercontent.com/74290204/103602568-b0135600-4f4f-11eb-9e75-855a5f74c805.png)

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
	<style>
		body {
		margin: 0;
		}
		
		div {
			width: 200px; 
			height: 200px;
			color: #ffffff;
			text-align: center;
		}
		
		#content1 {
			background-color: red;
		}
		
		#content2 {
			background-color: green;
			margin: 10px 10px 10px 10px;
		}
		
		#content3 {
			background-color:blue;
			padding: 10px 10px 10px 10px;
		}
		
		#content4 {
			background-color:yellow;
			margin: 10px 10px 10px 10px;
			padding: 10px 10px 10px 10px;
		}
		
	</style>
</head>
<body>
	<div id="content1"> 200*200</div>
	<div id="content2"> 200*200<br>margin</div>
	<div id="content3"> 200*200<br>padding</div>
	<div id="content4"> 200*200<br>margin<br>padding</div>
		
</body>
</html>
```
▶출력

![margin](https://user-images.githubusercontent.com/74290204/103493068-8df7d600-4e72-11eb-98f1-5297ca5f6709.PNG)

# - box-sizing
## @ 정의 : padding과 margin, border등 속성을 사용할 때 기존 width와 height를 어떻게 계산하는지를 정의하는 속성 <br> (포함해서 할지 안할지))
### 1. box-sizing : border-box → 기존 크기 내부에서 해결
### 2. box-sizing : content-box → default / 기존 크기를 건드리지 않고 외부에서 내용들이 더해짐 (더해질수록 크기가 커짐)

# - border 
▶ border-style 종류 참조 https://www.w3schools.com/css/css_border.asp

# - background-image
1. background-size
2. background-repeat : 배경 이미지 반복 여부
3. background-attachment : 배경 이미지의 스크롤 여부 
 >> background-attachment : scroll (해당영역의 이미지 고정, 스크롤해도 배경 고정) <br> background-attachment : fixed (스크롤 움직이지 않음)

```html

<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
	<style>
		div:first-child {
			width:80%;
			margin:0 auto;
			height:400px;
			background-image: url('http://www.sba.seoul.kr/kr/images/index/visual5.jpg');
			background-size: 100%;
		}
		
		div:last-child {
			width:80%;
			margin:0 auto;
			height:400px;
			background-image: url('http://www.sba.seoul.kr/kr/images/index/visual5.jpg');
			background-size: 50%; <!-- url의 이미지 사이즈를 반으로 해서 2개로 copy해서 출력-->
		}
	</style>
</head>
<body>
	<div></div>
	<div></div>
</body>
</html>
```

▶출력

![2](https://user-images.githubusercontent.com/74290204/103494100-728fc980-4e78-11eb-865b-88fb9e0dfc81.PNG)

# - font 
1. font-familiy : 폰트 여러개 가능 (찾는 폰트가 없다면 다음폰트 찾음 / 기재된 폰트가 다 없다면 웹이 갖고 있는 기본 디폴트 폰트를 뿌린다)
2. line-height : 줄 간격 
>>[tip] height와 line-height 값을 동일하게 주면 **가운데 정렬**

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
	<style>
		#nav {
			width: 700px;
			margin: 0 auto;
			border: 1px solid gray;
			overflow: hidden;
		}
		
		#nav div {
			width : 150px;
			height : 100px;
			line-height: 100px; 
			text-align: center;
			font-size : 18px;
			border-top: 1px solid black;
			border-bottom: 1px solod black;
			margin: 5px;
			float : left;
		}
		
		a {
			text-decoration: none; /* 링크 달면 밑줄 생기는거 없애는 방법1 */
		}
		
	</style>
</head>
<body>
	<div id = "nav">
		<div><a href="#">menu1</a></div>
		<div><a href="#">menu2</a></div>
		<div><a href="#">menu3</a></div>
		<div><a href="#">menu4</a></div>
	</div>
</body>
</html>
```
# - position : 위치를 나타내는 속성 (defalut=static)
1. 정적 위치 **(static position)** : 기본위치이기 때문에 생략이 가능하지만 top,left,right,bottom에 영향을 받지 않는다
>> top,left,right,bottom로 위치 조정이 불가능 
2. 상대 위치 **(relative position)** : 기존에 있는 위치에 대해서 영향을 받아 상대적으로 위치한다 
>> 동일 선상일때는 동일 선상에 있는것에 대해서 영향을 받고 부모 자식간에는 부모에 영향을 받는다
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
	<style>
		#red {
			width: 100px;
			height: 100px;
			background-color: red;  
		}
		
		#yellow {
			width: 400px; 
			height: 400px;
			background-color: yellow;
			position: relative;
			top: 100px; 
			left: 100px;
		}
		
		#gereen {
			width: 100px;
			height: 100px;
			background-color: green;
			position: relative;
			top: 0;
			left: 100px;
		}
		
		#blue {
			width: 100px;
			height: 100px;
			background-color: blue;
			top: 100px;
			left: 100px;
		}
	</style>
</head>
<body>
    <div id="red"></div>
    <div id="yellow">
        <div id="green"></div>
        <div id="blue"></div>
    </div>
</body>
</html>
```

▶출력 
![position](https://user-images.githubusercontent.com/74290204/103594757-f1e6d100-4f3c-11eb-95df-07cac749b70b.PNG)

3. **absolute position :** 속성을 가지고 있지 않은 부모를 기준으로 움직인다 
>> if, 부모중에 position태그가 없다면 가장 위의 태그인 body를 기준으로 한다
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
	<style>
		div {
			width: 100px; 
			height: 100px;
			opacity: 0.7;
		}
		
		div:nth-child(1) {
			background-color: #ff0000;
			position: fixed;
			top: 0;
			left: 0;
		}
		
		div:nth-child(2) {
			background-color: #00ff00;
			position: absolute;
			top: 50px;
			left: 50px;
		}
		
		div:nth-child(3) {
			background-color: #0000ff;
			position: absolute;
			top: 100px;
			left: 100px;
	
		}
		
		#wrap {
			width: 300px;
			height: 300px;
			position: fixed;
			top: 300px;
			left: 300px;
			background-color: yellow;
			opacity: 1.0;
		}
		
		#wrap .content {
			width: 100px;
			height: 100px;
			position: absolute;
			top:100px; 
			left:100px;
			background-color: red;
			opacity: 1.0;
		}
	</style>
</head>
<body>
    <div></div>
    <div></div>
    <div></div>

    <div id="wrap">
        <div class="content"></div>
    </div>
</body>
</html>
```

▶출력 
![absolute](https://user-images.githubusercontent.com/74290204/103595359-90276680-4f3e-11eb-9028-4627ce963c07.PNG)

### - viewport : 스마트 기기상에서 최초에 페이지를 로딩할 때 확대정도, 최대 확대비율, 최소 확대비율등을 다루는 meta data에 속하는 속성 
>> viewport는 스마트기기 화면에서 실제 내용이 표시되는 영역을 의미 <br> 따라서 웹사이트를 제작할때 viewport에 대한 설정을 해주어야 반응형 웹사이트를 제대로 동작시킬 수 있음

- [viewport 참고 링크] https://penguingoon.tistory.com/129

# - float : position과 마찬가지로 위치를 설정하기 위한 속성
>> float : 띄운다의 의미
```html
<style>
	float : left
</style>

▶ 프로그래밍 내부적으로는 왼쪽으로가서 공중으로 띄운다는 뜻 
```
- [float 원리] float을 하면 다음에 올 영역이 바로 밑으로 들어가게되서 display되지 않음(띄워져있어서 가리니까)
- [에외!] float을 하면 당연히 영역이 float한 영역 밑으로 쌓여야하는데 **글자는 예외!** <br>
why? css의 목적은 사용을 쉽게하기 위함이기 때문에 기능적으로 따로 추가된 부분 
```
원리는 글자의 컨텐의 면적은 float 영역 밑에서부터 시작되지만 display는 float영역 밖에 보이게 된다
```

