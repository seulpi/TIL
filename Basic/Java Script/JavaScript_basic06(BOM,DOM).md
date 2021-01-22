# ★ BOM(Browser Object Model) 객체 : 브라우저와 관련된 객체 
- 뒤로가기, 주소창등을 JS로 컨트롤 하기 위해 제공되는 객체 

## @ 종류 (5가지)
### 1. Window : window 전체를 컨트롤 하는 객체로 window를 생략하고 사용이 가능하다 
- window객체 안에 나머지 4가지 객체(location, history, navigator, screen)가 포함되는 형태
```javascript
alert("window");
	window.alert("window");
	
	window.open("http://www.google.com", "openWindow", "width=500, height=300");
	// 새로운 창 open 
	

	var newWindow = window.open("http://www.google.com", "google", "width=500, height=600");
	
	//위치 조정
	newWindow.moveTo(50, 50);
	for(var i =0; i<1000; i++) {
		newWindow.moveBy(1, 1);
	}
	
	//사이즈조정
	newWindow.resizeTo(500, 600);
	for(var i = 0; i <200; i++) {
		newWindow.resizeBy(-1, -1);
	}
```
- **★ onload()** : 호출된 웹문서가 **모두 완료된 후 자동으로 실행**하는 함수
    - 브라우저가 웹문서< html >를 끝까지 다 읽어들인 후 메모리에 올리고 나서 호출된다 
    - **제일 마지막에 실행되는 함수**
    - onload의 위치는 어디에 있어도 상관 없지만 관습적으로 < head>< /head>에 써준다
```javascript
window.onload = function() {
    console.log("First!");
};
		
console.log("Second!");
console.log("Third!")
	
--------------------------------------------------------
▶
Second!
Third!
First!
```
```javascript
window.onload = function() {
	console.log(1);
};
		
window.onload = function() {
	console.log(2);
};
		
window.onload = function() {
	console.log(3);
};
--------------------------------------------------------
▶ 3 
```
#### ※ [주의] onnload는 최종적으로 onload된것만 출력된다 
- < script>로 묶어서 쓰면 모두 다 출력할수 있음 

### 2. Screen : 화면과 연결된 객체 
- screen.width, screen.height : 화면상의 해상도 (1920*1080)
```javascript
 function openWin(url, width, height) {
         
    console.log("screen.width : " + screen.width);
    console.log("screen.height : " + screen.height);
            
    var left = screen.width/2 - width/2;
    var top = screen.height/2 - height/2;
    // left , top을 이렇게 지정해주면 '중앙정렬'
            
    var opt = "width = " + width + ", height = " + height + ", left = " + left + ", top = " + top;
            
    open(url, "newWin", opt); 
    // window.open에서 window를 생략 위치값에는 스트링으로 받아도 가능(변수로 초기화해서 넣어도 상관없음)
         
    }
         
openWin("https://yahoo.com", 800, 500);
```

### 3. Location 
- 이 객체를 이용해서 윈도우의 문서 URL을 변경할 수 있고, 문서의 위치와 관련해서 다양한 정보를 얻을 수 있다 
- href, replace, assign 태그 이용 
- 3개의 태그의 역할을 동일하나(해당url로 이동) history에 남냐 안남냐의 차이가 존재한다
>> href : history에 남지 않기때문에 뒤로가기 X <br> replace : history가 남기 때문에 뒤로가기 O 
```javascript
window.onload = function() {
    location.href = "http://www.yahoo.com";// 할당이 아니라 바로 실행
}
      	
window.onload = function() {
    location.replace("http://www.yahoo.com");
}

window.onload = function() {
    location.assign("http://www.yahoo.com");
}
```

### 4. History 객체 : 브라우저의 세션기록(현제 페이지를 불러온 탭 ro 방문했던 페이지) 객체 
- 보안상의 문제로 History객체는 세션 기록 안에 다른 페이지의 URL정보는 알 수 없으나 세션기록 탐색은 가능하다  
- back() : 뒤로가기 버튼 
```javascript
history.back();
```
- forward() : 앞으로 가기
```javascript
history.forward();
```
- go() : 지정한 위치로 가기
```javascript
history.go(-1); = 뒤로가기 history.back();
```

### 5. Navigator 객체 : 브라우저 공급자 및 버전 정보 등을 포함한 브라우저에 대한 다양한 정보를 저장하는 객체
<br>

# ★ DOM(Document Object Model) : 웹문서(태그)와 관련된 객체
## → 메모리에 올린 태그들을 트리형태로 관리하는 것 자체를 말한다
- 태그 : Elemnet 
- 속성 : Attribute
- 태그 안에 내용 : Text 

>>  Dom안에 모든 태그(Element)들은 객체로 정의된다 

![DOM TREE](https://user-images.githubusercontent.com/74290204/105366895-3499f000-5c43-11eb-954e-23472b9b62b6.png)

## - 함수
### **1. Text노드 이용**
```javascript
window.onload = function() {
    var elementNode = document.createElement("p");
    var textNode = document.createTextNode("hello JS");
      		
    elementNode.appendChild(textNode);
    document.body.appendChild(elementNode);
      		
    /*  =
      	<body>
      		<p>hello JS</p>
      	</body>
    */
}
```

### **2. img노드 이용** 
```javascript
window.onload = function() {
    var imgNode = document.createElement("img");
    imgNode.src = "./img/logo.png"; // 이미지 경로
    // https://imgnews.pstatic.net/image/025/2021/01/21/0003071521_001_20210121105305536.jpg?type=w647 링크도 가능
    imgNode.width = "150";
    imgNode.height = "50";
      		
    document.body.appendChild(imgNode);
      	
};

-----------------------------------------------------------------
window.onload = function() {
    var imgNode = document.createElement("img");
    imgNode.setAttribute("src", "./img/logo.png");
    imgNode.setAttribute("width", 150);
    imgNode.setAttribute("height", 50);
    imgNode.setAttribute("tempData", "logoImg");
     /* TempData 속성은 값을 저장한 후 바로 다음에 TempData의 값을 요청하면 값이 사용되자마자 값이 사라지는 형태의 데이터를 보관하는 데 사용
    tempData ??*/
    document.body.appendChild(imgNode);
};
```

### **3. innerHTML : html태그를 바로 넣는 요소**
```javascript
window.onload = function() {
      		var str = "";
      		str += "<p> javascript</p>";
      		str += "<img src = './img/logo.png',";
      		str += "width = '170', height = '67', temDate='logoImg'>";
      		
      		document.body.innerHTML = str;
      	
      	};
```
### ※ 주의) innerHTML을 사용하게 되면 태그안에 내용들이 무시되고 innerHTML으로 정의해준 내용만 출력한다 

### **4. id를 통한 객체 선택**
```javascript
window.onload = function() {
      		var str = "";
      		str += "<p id = 'jsTitle'> javascript </p>";
      		str += "<img id ='logoImg', src=''./img/logo.png',";
      		str += "width='150', height='50', tempData='logoImg'>";
      		
      		document.body.innerHTML = str;
      		
      		var titleNode = document.getElementById("jsTitle");
      		titleNode.innerHTML = "JS & node";
      		
      		var logoNode = document.getElementById("logoImg");
      		logoNode.setAttribute("src", "./img/arm_mbed.png");
      		logoNode.setAttribute("width", 300);
      		logoNode.setAttribute("height", 150);	
      	
      	};
```

### **5. 부모&자식 객체**  
- 부모와 자식관계로 컨트롤 
```jsp
   <head>
      <title>Javascript</title>
      <script>
      
      	window.onload = function() {
      		
      		var str = "";
      		str += "<p id='jsTitle'> javascript </p>";
      		str += "<img id='logoImg', src ='/img/logo.png',";
      		str += "width = '170', height='67', tempData='logoImg'>";
      		
      		document.body.innerHTML = str;
			//body에 str을 다이렉트로 넣겠다
      		
      		var titleNode = document.getElementById("jsTitle");
      		titleNode.parentNode.removeChild(titleNode);
      		// 부모로 접근을해서 해당객체를 삭제, 현재 <p> 태그의 부모 body
      		var logoNode = document.getElementById("logoImg");
      		logoNode.parentNode.removeChild(logoNode);
      		// 부모로 접근을해서 해당객체를 삭제, 현재 <img> 태그의 부모 body
      		// 두개 다 삭제했으니 아무것도 출력이 안됨
      	};
         
      </script>
   </head>
```

### **6. style속성을 이용한 css적용**
```jsp
 <head>
      <title>Javascript</title>
      <script>
      
      	window.onload = function() {
      		
      		var str = "";
      		str += "<p id='jsTitle'> javascript </p>";
      		str += "<img id='logoImg', src ='/img/logo.png'>";
      		
      		
      		document.body.innerHTML = str;
      		
      		var titleNode = document.getElementById("jsTitle");
      		titleNode.style.fontSize = "1.2em";
      		titleNode.style.border ="1px solid #ff0000";
      		
      		var logoNode = document.getElementById("logoImg");
      		logoNode.style.width = "170px";
      		logoNode.style.height = "67px";
      	};
         
      </script>
   </head>
```
