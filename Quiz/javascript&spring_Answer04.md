# 1. 게시판 replyShape 생성시 아래의 쿼리문에서 bStep > ? 은 무슨 의미 인가?
```sql
update mvc_board set bStep = bStep + 1 where bGroup = ? and bStep > ?
```
### ▶ bStep은 계단식으로 내려가서 원본 그룹(bGroup)에서 몇번째인지를 표시하는 컬럼 <br> 따라서 기존의 bstep보다 커야 + 1을 시켜줬을때 최신순으로 정렬이 된다 (정렬을 시키기위한 조건)

# 2. sql 문제
```
-17. 부서별 급여 평균을 출력하시오
-18. 오늘은 몇요일인가? 
-10. EMP Table에서 급여가 1800 이상이면 ‘good’, 아니면 ‘poor’를 출력하시오
```
```sql
--17. 부서별 급여 평균을 출력하시오
select deptno, avg(sal) from emp group by deptno order by deptno asc;

--18. 오늘은 몇요일인가? 
select sysdate, to_char(sysdate, 'day') as day from dual; 

--10. EMP Table에서 급여가 1800 이상이면 ‘good’, 아니면 ‘poor’를 출력하시오
select ename, sal, 
case when sal>= 1800 then 'good'
     when sal <1800 then 'poor' end as good_poor from emp; 

```


# 3. 가위바위보 이미지 넣어서 짜시오
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>가위 바위 보 게임</title>
</head>
<body>
	<script type="text/javascript">
		function RandomGame() {
			var user = "";
			var gameImg = ["http://image.blog.livedoor.jp/deaiup/imgs/d/b/db18f955.png", 
				"http://isweb.joongbu.ac.kr/~solbi/images/%EB%B0%94%EC%9C%84.jpg", 
				"https://image.blog.livedoor.jp/deaiup/imgs/1/5/1506be2c.png"];
					// 가위 바위 보 이미지 링크 
		
			this.setUser = function(user) {
				this.user = user;
				
			};
			
			this.getUser = function() {
				return this.user;
			};
			
			this.userReslt = function() {
				var userImg = document.createElement("img");
				if(this.getUser() == "가위") {
					userImg.src = gameImg[0];
				}else if(this.getUser() == "바위") {
					userImg.src = gameImg[1];
				}else {
					userImg.src = gameImg[2];
				}
			
				document.body.appendChild(userImg);
			}
			
			this.comeResult = function() {
				
				var computer = Math.floor(Math.random()*3 )+ 1;
				var strCom ="";
				var comImg = document.createElement("img");
				
	  			if (computer == 1) {
	  				comImg.src = gameImg[0];
	  				strCom = "가위";
	  				
	  			} else if (computer == 2) {
	  				comImg.src = gameImg[1];
	  				strCom = "바위";
	  			} else {
	  				comImg.src = gameImg[2];
	  				strCom = "보";
	  			};
	  			
	  	  		document.body.appendChild(comImg);
			
				
	  			switch (this.getUser()) {

	  			case "가위":
	  				if (strCom== "가위") {	  					
	  					document.write("무승부입니다");
	  				} else if (strCom== "바위") {	  					
	  					document.write("컴퓨터 WIN!");
	  				} else if (strCom== "보") {
	  					document.write("유저 WIN!");
	  				}
	  				break;

	  			case "바위":
	  				if (strCom== "바위") {
	  					document.write("무승부입니다");
	  				} else if (strCom == "보") {
	  					document.write("컴퓨터 WIN!");
	  				} else if (strCom == "가위") {
	  					document.write("유저 WIN!");
	  				}
	  				break;

	  			case "보":
	  				if (strCom == "보") {
	  					document.write("무승부입니다");
	  				} else if (strCom== "가위") {
	  					document.write("컴퓨터 WIN!");
	  				} else if (strCom== "바위") {
	  					document.write("유저 WIN!");
	  				}
	  				document.write("유저가 낸 것 : " + this.getUser() + "컴퓨터가 낸 것 : " + strCom);
	  				break;
	  				
	  			default : 
	  				document.write("다시 입력하세요")
	  				break;
	  			}
	  			
	  			document.write("<br>" + "유저가 낸 것 : " + this.getUser() + " / 컴퓨터가 낸 것 : " + strCom);
	  		};	
		};
		
  		var game = new RandomGame();
  		var user = prompt("가위바위보를 입력하세요", "ex. 가위")
  		game.setUser(user);
  		game.userReslt();
  		game.comeResult();
  	
	</script>
</body>
</html>
```

### ▶ 출력
![가위바위보](https://user-images.githubusercontent.com/74290204/105336585-c6dacd80-5c1c-11eb-98a6-c114786e3058.PNG)

```jsp
//선생님 풀이 
<!DOCTYPE html>
<html>
<head>
   <meta charset="EUC-KR">
   <title>Insert title here</title>
</head>
<script type="text/javascript">
 window.onload = function(){
   
      function RPSPlayer(your) {
         var rps = ['가위','바위','보'];
         var rcpImg = [
            "https://lh3.googleusercontent.com/proxy/sKYu5a59Vo-J1kf95vaT6qNeNlJ3-P8UmPpOXYIb-qNtOboDS0kC8-uyj9_6fZaVzgABV6VqLBPXujScvQyolLNTJWyr6A",
            "https://lh3.googleusercontent.com/proxy/I8igzN6mkoV4n55CnIZGxd1s2F1tCeP21iv95qOisNQyO2RdhagyV9uncNbvSWiHftI-JkuDTXK93_e1gtKSrWoQaVvP8g",
            "http://isweb.joongbu.ac.kr/~jgm/photo/paper.png"
         ];
         
         var selfNum = 0;
         var yourRPS = your;
         
         this.setSelfNum = function() {
            selfNum = Math.floor( Math.random() * 3 );
         };

         this.result = function() {

            this.setSelfNum();
            var yourImg = document.getElementById("user");
            var comImg = document.getElementById("computer");
            var resultText = document.getElementById("result");
   

            switch (yourRPS) {

               case "가위":
                  if (  rps[selfNum] == "가위") {
                     yourImg.setAttribute("src",rcpImg[rps.indexOf(yourRPS)]);
                     comImg.setAttribute("src",rcpImg[selfNum]);
                     resultText.innerHTML = "무승부입니다";

                  } else if (rps[selfNum]== "바위") {
                     yourImg.setAttribute("src",rcpImg[rps.indexOf(yourRPS)]);
                     comImg.setAttribute("src",rcpImg[selfNum]);
                     resultText.innerHTML = "컴퓨터 WIN!";

                  } else if(rps[selfNum] == "보") {
                     yourImg.setAttribute("src",rcpImg[rps.indexOf(yourRPS)]);
                     comImg.setAttribute("src",rcpImg[selfNum]);
                     resultText.innerHTML = "유저 WIN!";
                  } 
                  break;
   
               case "바위":
                  if (rps[selfNum]== "바위") {
                     yourImg.setAttribute("src",rcpImg[rps.indexOf(yourRPS)]);
                     comImg.setAttribute("src",rcpImg[selfNum]);
                     resultText.innerHTML = "무승부입니다";
                  } else if (rps[selfNum] == "보") {
                     yourImg.setAttribute("src",rcpImg[rps.indexOf(yourRPS)]);
                     comImg.setAttribute("src",rcpImg[selfNum]);
                     resultText.innerHTML = "컴퓨터 WIN!";
                  } else if (rps[selfNum] == "가위") {
                     yourImg.setAttribute("src",rcpImg[rps.indexOf(yourRPS)]);
                     comImg.setAttribute("src",rcpImg[selfNum]);
                     resultText.innerHTML = "유저 WIN!";
                  } 
                  break;
   
               case "보":
                  if (rps[selfNum] == "보") {
                     yourImg.setAttribute("src",rcpImg[rps.indexOf(yourRPS)]);
                     comImg.setAttribute("src",rcpImg[selfNum]);
                     resultText.innerHTML = "무승부입니다";
                  } else if (rps[selfNum] == "가위") {
                     yourImg.setAttribute("src",rcpImg[rps.indexOf(yourRPS)]);
                     comImg.setAttribute("src",rcpImg[selfNum]);
                     resultText.innerHTML = "컴퓨터 WIN!";
                  } else if (rps[selfNum] == "바위") {
                     yourImg.setAttribute("src",rcpImg[rps.indexOf(yourRPS)]);
                     comImg.setAttribute("src",rcpImg[selfNum]);
                     resultText.innerHTML = "유저 WIN!";
                  }
                  break;
   
               default:
                  document.write(yourRPS + " 잘못된 입력 입니다 .다시입력하세요");

            }   
         
            
         };         
         
      }
      
      var rps = prompt("(가위, 바위, 보)를 입력하세요");
      //var player = new RPSPlayer(rps);
      //player.result();
      new RPSPlayer(rps).result() ; 
      
   } 
   </script>
<body>
   <table border='1'>
      <tr>
         <td>유저</td>
         <td>컴퓨터</td>
      </tr>
      <tr>
         <td><img id='user'></td>
         <td><img id='computer'></td>
      </tr>
      <tr>
         <td id="result" colspan='2'></td>
      </tr>
   </table>
</body>
</html>
```

# 4. Bom , 과 Dom 이란?

## @ Bom(Browser Object Model) : 웹브라우저와 관련된 객체 
→ 뒤로가기, 주소창등을 자바스크립트로 컨트롤 하기 위해 제공되는 객체
- 객체의 종류는 5가지로 window안에 4가지 객체가 포함되어 있는 형태 

### 1. Window 객체 : window 전체를 컨트롤 하는 객체로 윈도우는 생략이 가능하다 
>> window 객체는 Screen, Location, History, Navigator 객체 포함
```javascript
window.alert("window); = alert("window");  
```

- ★ onload() : 호출된 웹문서가 모두 완료된 후 자동으로 실행된다 
   - onload의 위치는 제일 마지막에 호출되기 때문에 어디에 위치하던 상관이 없으나 < head >에 넣어주는 게 관습적이다 
   - **여러개의 onload를 호출했을 때는 제일 마지막에 호출된 onload만 호출한다**

### 2. Screen 객체 : 화면과 연결된 객체 
### 3. Location 객체 : 문서와 주소 관련된 객체
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

## @ Dom(Document Object Model) : 웹문서(태그)와 관련된 객체
→ 메모리에 올린 태그들을 트리형태로 관리하는 것 자체를 말한다
- 태그 : Elemnet 
- 속성 : Attribute
- 태그 안에 내용 : Text 

>>  Dom안에 모든 태그(Element)들은 객체로 정의된다 

 ![DOM](https://user-images.githubusercontent.com/74290204/105273806-56618b80-5bdf-11eb-9e47-9f967337a63d.PNG)

### - 함수
1. Text노드 이용
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

2. img노드 이용 
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

3. innerHTML : html태그를 바로 넣는 요소 
```javascript
window.onload = function() {
      		var str = "";
      		str += "<p> javascript</p>";
      		str += "<img src = './img/logo.png',";
      		str += "width = '170', height = '67', temDate='logoImg'>";
      		
      		document.body.innerHTML = str;
      	
      	};

※ 주의) body 태그안에 글 적어놔도 innerHTML을 사용하게 되면 body안에 태그들이 무시되고 innerHTML에 들어간 태그들만 출력한다 
```

4. id를 통한 객체 선택 
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
