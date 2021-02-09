# - xml , json + ajax → RESTful → 게시판 
---
# - xml (Extensible Markup Language)
- Markup 언어를 정의하기 위한 언어, 확장이 가능한 언어
- 배경 : 데이터들을 클라이언트에게 전달받을 때 데이터 형식이 통일되지 않아서 전달받게 됨 <br> → 한두명도 아니고 형식의 통일의 필요성 대두 → 하나의 형식을 정의해서 클라이언트가 보내게 만듦 → 그게 바로 xml 문서 
- xml의 대표적인 활용 : HTML
- 모바일은 PC기반이 아님 (PC기반에서는 데이터를 html로 주고받음) → 모바일 : json 사용 

# json 

- Java Script를 확장해서 만든 언어
- Java Script 객체 표기법을 따름 
- 프로그래밍 언어와 운영체제가 독립적임 → Java, PHP를 써도 json or xml로 변환할 수 밖에 없다는 뜻 (empVO객체 자바를 클라이언트가 읽어들일 수 없으니까)
- 장점 : 표현자체가 간단, 다른언어에 비해 문법이 많지 않음

>> [공식사이트 문법 참조] https://www.json.org/json-ko.html

- json 문자열을 전송 받은 후에 해당 문자열을 바로 파싱 → xml보다 빠른 처리 속도 (xml문서는 xml DOM을 이용해 해당문서에 접근)
```html
<dog>
	<name>식빵</name>
	<family>웰시코기</family>
	<age> 1</age>
	<weight>2.14</wheight>
</dog>
------------------------------------------------
▶ json으로

{
	"name" : "식빵";
	"family" : "웰시코기";
	"age" : 1;
	"weight" : 2.14;
} 

```
- http프로토콜에는 두개의 영역으로 나뉨 Header, Body ???왜 나온거지 이 개념은???

## @ json 활용 방법
### 1. pom.xml에서 dependency 추가
```xml
<!--pom.xml-->
 <!-- 자바객체를 xml으로 -->
      <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.dataformat/jackson-dataformat-xml -->

      <dependency>
         <groupId>com.fasterxml.jackson.dataformat</groupId>
         <artifactId>jackson-dataformat-xml</artifactId>
         <version>2.9.6</version>
      </dependency>
      
      <!-- 자바객체를 Json으로 -->
      <!-- https://mvnrepository.com/artifact/com.google.code.gson/gson -->
      <dependency>
         <groupId>com.google.code.gson</groupId>
         <artifactId>gson</artifactId>
         <version>2.8.2</version>
      </dependency>
```

### 2. RESTful + json 을 이용한 게시판
```java
//RestBoardSpring4BeforeController.java

package edu.bit.ex.board.controller;


import java.util.List;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

import edu.bit.ex.board.service.BService;
import edu.bit.ex.board.vo.BoardVO;
import lombok.AllArgsConstructor;
import lombok.extern.log4j.Log4j;

@Log4j
@AllArgsConstructor
@Controller
//Rest풀을 이용한 게시판, Spring v4.0 이전에서의 json이어서 이름을 이렇게 지음(@ResponseBody + @Controller)
public class RestBoardSpring4BeforeController { 
	
	private BService bSerivce;
	
	@ResponseBody // 지금까지의 문법은 잊어라 (다른 형식의 문법이 생기는것)
	@RequestMapping("/rest/before")   //url은 ajax에 있는 url 맵핑
	public List<BoardVO> before(Model model) { // @ResponseBody을 사용하면 자바객체를 xml로 바꿔줌(단 라이브러리 등록해줘야함)
		log.info("before()");
		
		/* model.addAttribute("list", bSerivce.getList()); */
		List<BoardVO> list = bSerivce.getList();
		return list; // 예전에는 view를 리턴했는데 문법이 달라짐!
	}
}
```
```java
//RestBoardSpring4AfterController
package edu.bit.ex.board.controller;


import java.util.List;

import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import edu.bit.ex.board.service.BService;
import edu.bit.ex.board.vo.BoardVO;
import lombok.AllArgsConstructor;
import lombok.extern.log4j.Log4j;

@Log4j
@AllArgsConstructor
@RestController // -> restful을 이용한다는 뜻이고 이 어노테이션을 사용하면 @ResponseBody 사용하지 않아도됨
//spring v4.0에서부터 @RestController라는 어노테이션을 추가해서 해당 Controller의 모든 메서드의 리턴타입을 기존과 다르게 처리한다는것을 명시
public class RestBoardSpring4AfterController { 
	
	private BService bSerivce;

	@RequestMapping("/rest/after") 
	public List<BoardVO> before(Model model) {
		log.info("before()");
		
		List<BoardVO> list = bSerivce.getList();
		return list; 
	}	
}
```
```java
//BController.java
package edu.bit.ex.board.controller;


import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;

import edu.bit.ex.board.service.BService;
import edu.bit.ex.board.service.BServiceImpl;
import edu.bit.ex.board.vo.BoardVO;
import lombok.AllArgsConstructor;
import lombok.extern.log4j.Log4j;

@Log4j
@AllArgsConstructor
@Controller
public class BController {
	
	
	  private BService bSerivce;
	  
	  @RequestMapping("/rest/list")
	  public String restList() {
		  log.info("rest/list");
			
		  return "ajaxList"; //view단 처리 (view단에서 ajax로 구현해놓음)
	  }
}
```

# Ajax (Asynchoronous Javascript and XML) = 비동기통신 
- 클라이언트와 서버간 데이터를 주고받는 기술
- **$.ajax{ }** → jQuery안에 ajax함수 → ajax함수 안에 **객체를 만들어서 넘겨주는 것**
```javascript
$.ajax({
	url : '서비스 주소'
		, data : '서비스 처리에 필요한 인자값'
		, type : 'HTTP방식' (POST/GET 등)
		, dataType : 'return 받을 데이터 타입' (json, text 등)
		, success : function('결과값'){
		// 서비스 성공 시 처리 할 내용
		}, error : function('결과값'){
		// 서비스 실패 시 처리 할 내용
		}
}); 
```

## - 통신
1. 비동기 통신: 친구를 기다리지 X , **동기통신보다 효율적**
- **응답과 상관없이 일을 처리** → 기다리지 않고 자신의 코드를 처리하면서 서버로부터 응답받았을 때 응답 받은 내용들을 처리한다
2. 동기 통신 : 친구를 기다림 
- 서버로부터 응답받았을때 다음 일을 처리 (그전에는 그냥 **응답받을때까지 마냥 기다림**)

### ▶ Ajax(비동기 통신)를 사용하면 예를 들어 삭제 누르면 서버에 요청을 하고 다시 연산시킬 필요가 X <br> → why? ajax는 비동기 통신이기 때문 
- ajax를 사용하지 않았을때는 새로고침하고 리스트를 다시 불러온다
   - "데이터를 다시 불러온다" = 화면이 깜박거리는것 

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
	<title>Insert title here</title>
	<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
	<script type="text/javascript">

		function getList() {
			var url = "${pageContext.request.contextPath}/rest/after.json";

			$.ajax({
	            type: 'GET',
	            url: url,
	            cache : false, // 이걸 안쓰거나 true하면 수정해도 값반영이 잘안댐
	            dataType: 'json',// 데이터 타입을 제이슨 꼭해야함, 다른방법도 2가지있음
		        success: function(result) { //json 객체를 다 담아두는 함수의 변수 꼭 result라고 쓰지 않아도되고 변수 한개가 들어오면 이러한 용도로 사용된다

					var htmls="";
					
		        	$("#list-table").html("");	

					$("<tr>" , {
						html : "<td>" + "번호" + "</td>"+  // 컬럼명들
								"<td>" + "이름" + "</td>"+
								"<td>" + "제목" + "</td>"+
								"<td>" + "날짜" + "</td>"+				
								"<td>" + "히트" + "</td>"
					}).appendTo("#list-table") // 이것을 테이블에붙임

					if(result.length < 1){
						htmls.push("등록된 댓글이 없습니다.");
					} else {

		                    $(result).each(function(){			                    			                    
			                    htmls += '<tr>';
			                    htmls += '<td>'+ this.bId + '</td>';
			                    htmls += '<td>'+ this.bName + '</td>';
			                    htmls += '<td>'
			         			for(var i=0; i < this.bIndent; i++) { //for 문은 시작하는 숫자와 종료되는 숫자를 적고 증가되는 값을 적어요. i++ 은 1씩 증가 i+2 는 2씩 증가^^
			         				htmls += '-'	
			        			}
			                    htmls += '<a href="${pageContext.request.contextPath}/content_view?bId=' + this.bId + '">' + this.bTitle + '</a></td>';
 			                    htmls += '<td>'+ this.bDate + '</td>'; 
			                    htmls += '<td>'+ this.bHit + '</td>';	
			                    htmls += '</tr>';			                    		                   
		                	});	//each end

		                	htmls+='<tr>';
		                	htmls+='<td colspan="5"> <a href="${pageContext.request.contextPath}/write_view">글작성</a> </td>';		                	
		                	htmls+='</tr>';
		                	
					}

					$("#list-table").append(htmls);
					
		        }

			});	// Ajax end
		
		}//end	getList()	
	</script>
	
	<script>
		$(document).ready(function(){
			getList();
		});
	</script>

</head>
<body>
	<table id="list-table" width="500" cellpadding="0" cellspacing="0" border="1">
	</table>
</body>
</html>
```

--- 

# RESTful 
- 실무에서ㅓ의 **RESTful은 URI 설계**
```js
/board/board.jpg → resource 위치 (url, location) 
/board/1000 → board의 1000번째 글 (uri , idenfication)
▶ 1000번째 + Select인지 , Update인지 Delete인지 Insert인지

▶ REST API : /board/1000 
```
- Create Read Udatate Delete (RESEful, 설계) 
- Spring을 위한 개념이 아니라 하나의 통합적인 개념 
- 주소 **경로**는 왠만하면 **다 소문자로~**

![Rest API](https://user-images.githubusercontent.com/74290204/107378347-3d3d6200-6b2f-11eb-99e4-5ca2554ecfc1.png)

```java
//RestBoardController.java
package edu.bit.ex.board.controller;

import javax.servlet.jsp.PageContext;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.servlet.ModelAndView;

import edu.bit.ex.board.service.BService;
import edu.bit.ex.board.service.BServiceImpl;
import edu.bit.ex.board.vo.BoardVO;
import lombok.AllArgsConstructor;
import lombok.extern.log4j.Log4j;



//REST : Representational State Transfer
//하나의 URI가 하나의 고유한 리소스를 대표하도록 설계된 개념

//http://localhost/spring02/list?bno=1 ==> url+파라미터
//http://localhost/spring02/list/1 ==> url
//RestController은 스프링 4.0부터 지원
//@Controller, @RestController 차이점은 메서드가 종료되면 화면전환의 유무

@Log4j
@AllArgsConstructor
@RestController
@RequestMapping("/restful/*")
public class RestBoardController {

	private BServiceImpl bSerivce;
	
	//기존 @Controller 방식으로 넘기려면 ModelAndView를 사용해 데이터와 view를 넘겨줘야함
	// 1. list(처음 진입 화면이므로 화면이 깜박여도 상관없기 때문에 @Controller방식으로 접근 -> 꼭 json으로 넘겨야 된다 그런거아님
	@GetMapping("/board")
	public ModelAndView list(ModelAndView mav) {
		mav.setViewName("rest_list");
		mav.addObject("list", bSerivce.getList());
		
		return mav;
		
	}
	
	@GetMapping("/board/{bId}") 
	//bId 처리 방식 -> 1. @PathVariable로 처리 @PathVariable("bId") String bId, Model model 2. Command객체로 처리
	public String rest_content_view(BoardVO boardVO, Model model) {
		
		log.info("rest_content_view");
		model.addAttribute("content_view", bSerivce.getBoard(boardVO.getbId()));
		
		return "content_view";
	}

	@DeleteMapping("/board/{bId}") 
	//delete는 기본적으로 form 태그에서 지원하지 x(get, post만 지원) -> list에서 클릭을 하게되면 js&jquery-ajax로 처리 (submit이런거말고)
	// 어떨 때 사용하느냐? 댓글 삭제 -> ajax
	public ResponseEntity<String> rest_delete(BoardVO boardVO, Model model) {
		
		ResponseEntity<String> entity = null;
		
		log.info("rest_delet");
		
		try {
			bSerivce.remove(boardVO.getbId());
			//삭제가 성공하면 성공 상태 메세지 저장
			entity = new ResponseEntity<String>("SUCCESS", HttpStatus.OK);
											//json (X), 그냥 SUCCESS라는 일반 문자열
		}catch (Exception e) {
			e.printStackTrace();
			//삭제가 실패하면 실패 상태 메세지 저장
			entity = new ResponseEntity<String>(e.getMessage(), HttpStatus.BAD_REQUEST);
		}
		//삭제처리 http 상태 메세지 리턴
		return entity;
	}
}
```
