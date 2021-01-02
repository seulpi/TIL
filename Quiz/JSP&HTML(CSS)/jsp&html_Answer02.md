# 1. html 이란 무엇인가?
: HTML(Hyper Text Markup Language)은 **웹문서를 기술하는 언어**를 말한다 
```
HT(Hyper Text) : 문서와 문서가 링크로 연결
M(Markup) : Mark(태그)로 이루어져있다(웹문서를 표현하기 위한 태그들로 구성)
L(Language)
```

# 2. css 란 무엇인가?
- CSS(Cascading Style Sheets)란 html문서를 디자인적으로 예쁘게 만들어 정보 전달을 하기 위해 만들어진 문서 
- style 태그로 들어가는 것들 (HTML은 정보 / CSS는 화면상의 레이아웃등을 컨트롤한다)

# 3. 아래의 태그에 대하여 설명하시오
## < del > / < ins > / < span >
- **< del > : 취소선** <br>
>> <del> HelloWorld
- **< ins > : 밑줄** <br>
>> <ins> HelloWorld
- **< span > :  웹 페이지의 영역을 설정 할 때 사용하는 태그** , 주로 < div > 와 < p > 태그와 함께 일부분에 스타일을 적용시키기 위해 사용되는 태그 <br> 예전에는 table을 사용해 전체적인 레이아웃을 구성했다면 요새는 주로 div와 span을 사용한다
```
* <span> 과 <div> 의 차이점
1. 줄바꿈
<span> 은 자동으로 줄바꿈이 되지않아 <br/> 태그와 함께 사용해야하지만 <div>는 자동으로 줄바꿈이 허용됨
2. 영역의 차이
<span>은 줄단위의 영역 <div>는 박스형태로 영역이 잡힌다
```
 
 ☞ < span > 태그 사용법 아래 링크 참조 <br>
 https://coding-factory.tistory.com/189
<br>

# 4. block 태그와 non block 태그에 대하여 설명하시오
: 태그는 inline요소와 block요소로 나눌 수 있음 

- block : 좀 더 넓은 범위를 지정할때 사용하는 태그, block요소를 사용할때는 자동으로 앞뒤 줄바꿈이 일어남 <br> 
>> 종류: < p >, < h1 >, < div >, < ol >, < table > 등 

-  non block(liLine): block을 지정하지않는다(줄 속에 넣는 요소를 말함), 특정 문자열을 선택할 때 사용되며 자동으로 줄바꿈이 일어나지 않음
 >> 종류 : < b >, < span >, < a > 등 

▶ 태그종류 링크 참조
https://calmdawnstudio.tistory.com/51
https://potionstory.tistory.com/11
https://stajun.tistory.com/entry/HTML-%ED%83%9C%EA%B7%B8-%EC%A0%95%EB%A6%AC
<br>


# ★ 5. get 방식과 post 방식에 대하여 설명하시오 (필수)
: client가 WAS에게 requst하는 방식은 크게 2가지로 나누는 데(정보를 넘기는 방식) 그게 바로 get, post방식이다
- get : 주소창 안에 **'? key value**를 달아서 전송하는 방식
```
http:// locallhost:8080/boardList?name=제목&contents=내용
```

- post : post는 HEAD 영역과 BODY영역이 존재, get방식과 달리 URL에 데이터가 노출되지 않고 BODY영역에 숨겨서 전송 된다 (주소 전송 X)

▶ 다이렉트로 URL을 치면 doget()호출이 되는 이유? <br>
post로 따로 지정을 하지 않으면 디폴트로 get이 설정되어 넘어오기 때문!
<br>

# 6. 컨텍스트 패스란 무엇인가?
: 웹 어플리케이션을 구분하기 위한 path
- context : 하나의 프로젝트를 의미 
- 이클립스의 프로젝트를 생성하면 자동으로 server.xml에 추가된다
```java
<Context docBase="servlet_hello3" path="/servlet_hello3" reloadable="true" source="org.eclipse.jst.jee.server:servlet_hello3"/></Host>

→ 현재 돌리고 있는 서버를 표시한다 


http://192.168.0.96:8282/servlet_hello3
                         ↑ 프로젝트명(실무에서는 context명이라고 부른다)
```
<br>

# 7. 아래의 객체에 대하여 설명하시오
## HttpServletRequest / HttpServletReponse
: Client가 requset한 것에 대해 response해주려면 일단 Client의 주소와 정보를 알아야하는데 그에 대한 정보와 주소는 <br> 웹브라우저가 자동으로 프로토콜을 만들어 전달 <br> 
Client의 IP주소와 정보를 전달받기 위해서는 웹서버가 정보를 저장할 내용을 갖고있어야 하는데 그게 바로 <br> **HttpServletRequest / HttpServletReponse** 이다 (HttpServletRequest : 객체를 생성하여 request를 저장 / HttpServletReponse : 객체를 생성하여 respons를 저장해 전달 )

---
# **[ Quiz ]**
## 1. 
![01](https://user-images.githubusercontent.com/74290204/103205879-ea706800-493d-11eb-8161-b33b445ecea0.PNG)


## 2. 
![02](https://user-images.githubusercontent.com/74290204/103205892-f5c39380-493d-11eb-8ea7-bcbe5af43097.PNG)


## 3. 
![03](https://user-images.githubusercontent.com/74290204/103205910-ff4cfb80-493d-11eb-8126-eb4afa0e7f59.PNG)


## 4. 
![04](https://user-images.githubusercontent.com/74290204/103205917-0247ec00-493e-11eb-95b6-afb332640c09.PNG)


## 5.
![05](https://user-images.githubusercontent.com/74290204/103205919-02e08280-493e-11eb-9d38-a601439e6a62.PNG)


```html 
<!-- //teacher -->

<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>

	<style type="text/css">
		.td_blank {
			width:50px;
		}
	</style>
 
</head>
<body>
	<table border ="1">
		<tr>
			<td>이름</td>
			<td class="td_blank" ></td> <!-- &nbsp; 공백 -->
			<td>연락처</td>
			<td class="td_blank"></td>
		</tr>
		
		<tr>
			<td>주소</td>
			<td colspan = "3" > </td>
		</tr>
		
		
		<tr>
			<td>자기소개</td>
			<td colspan ="3"></td>
	
	</table>


</body>
</html>
```
