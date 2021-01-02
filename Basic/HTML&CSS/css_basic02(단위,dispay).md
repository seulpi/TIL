# css 단위 
1. em : 기본크기의 *(곱하기) 해주는 단위 
    - ex) 2em : 기본크기 * 2
    - 태그의 기본 px 크기 : 16px = 1em
2. pt
3. px
4. % 
등등 

# 속성
## ★ @ Display
- margin : 0 auto → 중앙 정렬  / margin : 외부에 공백을 주는 것 
: 모든 태그는 기본적으로 **3가지 속성을 가진다(block, inline, inline-block)**
1. display : block → block으로 보여주기 (block 자동개행이 일어남)
    - 의미 : 개행을 줬다는 의미는 **전체 행에 대해서 내 구역이다**라는 의미(크기 설정하는 건 크기만큼 내가 구역을 다 가지겠다는 의미)
2. display : inline
    - 크기(width, height)를 설정해도 inline으로 display를 준 순간 크기가 적용되지 않음
    - block태그여도 개행 X 
3. display : none → 안 보여주기 
4. ★ **display : inline-block → inline요소 + block요소**
    - 크기 설정이 가능하면서 inline요소도 갖고 있기 때문에 block과 달리 영역 허용 가능 <br> ex) < button > < select >
    >>[tip] < li >는 원래 아래로 리스트업되서 출력 but, display:inline 옆으로 리스트 출력 가능
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
	<style>
		div:nth-child(1) {
			width:100px;
			height:100px;
			background-color: #ff0000;
			color: #ffffff;
			font-weight: bold;
			text-align: center;	
		}
		div:nth-child(2) {
			width:100px;
			height:100px;
			background-color: #00ff00;
			color: #ffffff;
			font-weight: bold;
			text-align: center;	
		}
		div:nth-child(3) {
			width:100px;
			height:100px;
			background-color: #0000ff;
			color: #ffffff;
			font-weight: bold;
			text-align: center;	
		}
		div:nth-child(4) {
			width:100px;
			height:100px;
			background-color: #ff0000;
			color: #ffffff;
			font-weight: bold;
			text-align: center;	
		}
		div:nth-child(5) {
			width:100px;
			height:100px;
			background-color: #00ff00;
			color: #ffffff;
			font-weight: bold;
			text-align: center;	
			display: inline;
			margin: 10px 10px 10px 10px;
		}
		div:nth-child(6) {
			width:100px;
			height:100px;
			background-color: #0000ff;
			color: #ffffff;
			font-weight: bold;
			text-align: center;	
			display: inline;
		}
		div:nth-child(7) {
			width:100px;
			height:100px;
			background-color: #ff0000;
			color: #ffffff;
			font-weight: bold;
			text-align: center;	
			display: none;
		}
		div:nth-child(8) {
			width:100px;
			height:100px;
			background-color: #00ff00;
			color: #ffffff;
			font-weight: bold;
			text-align: center;	
			display: inline;
		}
		div:nth-child(9) {
			width:100px;
			height:100px;
			background-color: #0000ff;
			color: #ffffff;
			font-weight: bold;
			text-align: center;	
			display: inline-block;
		}
	</style>
</head>
<body>
	<div>content1</div>
	<div>content2</div>
	<div>content3</div>
	<div>content4</div>
	<div>content5</div>
	<div>content6</div>
	<div>content7</div>
	<div>content8</div>
	<div>content9</div>
</body>
</html>
```

<details><summary>▶ 출력</summary>

![2](https://user-images.githubusercontent.com/74290204/103393320-67c9f180-4b65-11eb-8af2-3cbd64227a5f.PNG)
</details>

## @ Visibility 
- display : none → 아예 출력하지마(영역 없음)
- visibility : hidden → 출력은 하는데 빈 공간처럼 보여줘(해당 영역은 차지하고 있음)

## @ Opacity 
: opacity의 허용되는 **값의 범위는 0 ~ 1**
