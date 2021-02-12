# 1. RESTful에 대하여 설명하시오
>> [Spring_basic03 RESTful 정리 ] <br> https://github.com/seulpi/TIL/blob/main/Basic/Spring/Spring_basic03(Restful,%EC%96%B4%EB%85%B8%ED%85%8C%EC%9D%B4%EC%85%98,%20%EC%9C%A0%ED%9A%A8%EC%84%B1).md


# 2. RESTful과 ajax를 이용해 delete를 구현하시오
## ▶ *Answer(ajax작동은 잘되나 새로고침해야 delete가 업데이트됨)*
```java
//RestBoardController
package edu.bit.ex.board.controller;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.servlet.ModelAndView;

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
		
		log.info("rest_delete");
		
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

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
	<title>Insert title here</title>
	<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
	<script type="text/javascript"></script>
<!-- 	$.ajax({
		url : '서비스 주소'
			, data : '서비스 처리에 필요한 인자값'
			, type : 'HTTP방식' (POST/GET 등)
			, dataType : 'return 받을 데이터 타입' (json, text 등)
			, success : function('결과값'){
			// 서비스 성공 시 처리 할 내용
			}, error : function('결과값'){
			// 서비스 실패 시 처리 할 내용
			}
		}); -->
				
	<script>
		$(document).ready(function(){

			  $("#rest_delete").click(function(event) {
				    
				    event.preventDefault();
			
				    var id = $(this).data('id');
				    var url = "${pageContext.request.contextPath}/restful/board/" + id;
				    
				    $.ajax({ //ajax함수 안에 객체를 넣는것, 객체 형태는 key : value 형태 , google 추천검색어 자동완성 기능으로 유명세를 탐 
				          type: 'DELETE', //반드시 대문자로!
				          url: url,
				          cache : false,
				       /* dataType: 'json'
				          dataType은 xml도 있고 plain(일반 text), html도 있음 (지정안해주면 plain으로 넘어옴 text 그대로 받아들임) 
				          
				          data:{bId: "1", 
				            bContent: "내용"}
				          } -> form 태그의 name="bId" value="1"와 동일
				          data: 수정이나 insert의 데이터를 json으로 보낸다 key, value형태로 데이터 저장
				        */
				        success: function(result) {
				          // success,error 안에 콜백함수(매개변수를 함수로 사용하는것) 
				          var resultObj = $(this).parent();
				          
				          if(result.equals("SUCCESS") ) {
				            resultObj.remove(); 
				           
				          }
				        }
				    });
				});
			});
			  
 </script>

</head>
<body>
<table  width="100%" cellspacing="0" border="1">
	<tr>
		<td>번호</td>
		<td>이름</td>
		<td>제목</td>
		<td>날짜</td>
		<td>히트</td>
		<td>삭제</td>
	</tr>
	
	<c:forEach items="${list}" var="dto">

	<tr>
		<td>${dto.bId}</td>
		<td>${dto.bName}</td>
		<td><c:forEach begin="1" end="${dto.bIndent}">[re]</c:forEach>
			<a href="${pageContext.request.contextPath}/restful/board/${dto.bId}">${dto.bTitle}</a></td>
		<td>${dto.bDate}</td>
		<td>${dto.bHit}</td>
		<td id="rest_delete" data-id="${dto.bId}"><a href="${pageContext.request.contextPath}/restful/board/${dto.bId}">삭제</a></td>

	</tr>
	</c:forEach>

	<tr>
		<td colspan="5"><a href="write_view.do">글작성</a></td>
	</tr>
</table>
</body>
</html>
```

## ▶ *선생님 풀이*
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
	<title>Insert title here</title>
	<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
	<script type="text/javascript"></script>

<script>
	$(document).ready(function() {

		$(".rest_delete").click(function(event) {
		/* 질문① 왜 class로 지정해줘야하는지? : id는 한문서에 한번만 사용가능(id명은 유일무이해야함), class는 여러번 사용 가능 
			-> 근데 for문 돌리면 클릭했을때 id가 여러개 도는거라서 id를 사용하면 event가 발생하지x 
		질문② 왜 td에 널고 var resultObj = $(this).parent();로 하면 안되는지? */
					
			event.preventDefault();
			
			var resultObj = $(this).parent().parent(); //클로저

			$.ajax({
				type : 'DELETE',
				url : $(this).attr("href"),
				cache : false,

				success : function(result) {

					if (result == ("SUCCESS")) {
						$(resultObj).remove();

					}
				}
			});
		});
	});
</script>

</head>
<body>
<table  width="100%" cellspacing="0" border="1">
	<tr>
		<td>번호</td>
		<td>이름</td>
		<td>제목</td>
		<td>날짜</td>
		<td>히트</td>
		<td>삭제</td>
	</tr>
	<c:forEach items="${list}" var="dto">

	<tr>
		<td>${dto.bId}</td>
		<td>${dto.bName}</td>
		<td><c:forEach begin="1" end="${dto.bIndent}">[re]</c:forEach>
			<a href="${pageContext.request.contextPath}/restful/board/${dto.bId}">${dto.bTitle}</a></td>
		<td>${dto.bDate}</td>
		<td>${dto.bHit}</td>
		<td><a class="rest_delete" data-id="${dto.bId}" href="${pageContext.request.contextPath}/restful/board/${dto.bId}">삭제</a></td>				
	</tr>
	</c:forEach>
	
	<tr>
		<td colspan="5"><a href="write_view.do">글작성</a></td>
	</tr>
</table>
</body>
</html>
```
