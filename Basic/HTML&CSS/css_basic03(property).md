# - margin & padding 
## @ margin : 외부 간격(외부 공백) 정의
1. margin-top : 위 
2. margin-bottonm : 아래
3. margin-right : 오른쪽
4. margin-left : 왼쪽 

## @ padding : 내부 간격(내부 공백) 정의 (padding도 top.botton,right,left 다 설정 가능)

![Padding](https://user-images.githubusercontent.com/74290204/103523587-6a0eb180-4ebf-11eb-9e90-e00bcebc5816.png)

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
