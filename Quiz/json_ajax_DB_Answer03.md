# 1. 아래에 대하여 설명하시오
```
- 트랜잭션 이란?
- rollback 과 commit 이란?
```

### - **트랜잭션** : 프로그래밍 error와 상관없이 **DB와 관련된 용어**, 에러가 나서 **중단되는 경우 rollback과 commit을 위해 사용**한다
- 트랜잭션 **대상** : (select를 제외한) **update, delete, insert** 
- 하나의 코드 이상을 사용할때는 반드시 트랜잭션을 사용해야한다
### ※ 주의 ※ 
- checked와 unchecked Exception에서의 rollback 차이 

    - **checked는 개발자가 직접** throws, try-catch 를 사용해 **예외처리를 해주기 때문에 rollback이 이루어지지않음** (개발자소관) <br> → 따라서 rollback할 함수 처리를 해주어야한다

    ```java
    @Transactional //CheckedException -> 롤백안함(@Transactional 적용X)
	public void transactionTest5() throws SQLException { 

		log.info("transionTest5()테스트 ");

		BoardVO boardVO = new BoardVO();

		boardVO.setbContent("트랜잭션5");
		boardVO.setbName("트랜잭션5");
		boardVO.setbTitle("트랜잭션5");

		mapper.writeBoard(boardVO); 
		
		throw new SQLException("SQLExcetion for rollback"); 

	}
	  
	@Transactional(rollbackFor = Exception.class) 
	public void transactionTest6() throws SQLException { 
		
		log.info("transionTest6()테스트 ");

		BoardVO boardVO = new BoardVO();

		boardVO.setbContent("트랜잭션6");
		boardVO.setbName("트랜잭션6");
		boardVO.setbTitle("트랜잭션6");

		mapper.writeBoard(boardVO); 
		
		throw new SQLException("SQLExcetion for rollback"); 
	}
    ```
<br>

# 2. 스프링에서의 트랜잭션 처리 방법은?
- 트랜잭션 Spring에서의 처리 방법 → **@(Annotation)을 사용**한다 , 사용하는 방법은 더 있지만 이게 실무에서 가장 많이 쓰이는 방법!
```java
@Transactional
```

# 3. ajax를 활용해 게시판 update를 구현하시오
- 기존 게시판의 query문과 service단 함수, view단을 가져와 사용하였음
- [의문점①] 화면이 한번 깜박이고 다시 수정해야 수정이 된다
- [의문점②] event.preventDefault(); 사용해야 submit기능을 막고 click이벤트를 발생하는데 event.preventDefault(); 추가하면 console 400에러 존재 
- [의문점③] id값을 url에 어떻게 넘겨줄지? $(this).attr('input') 아닌 
	- < a class="rest_delete" data-id="${dto.bId}" href="${pageContext.request.contextPath}/restful/board/${dto.bId}" > 로 넘겨서 <br>  
	var id = $(this).data('id'); <br>
	var url = "${pageContext.request.contextPath}/restful/board/" + id; 로 처리하면 되는지?
	
## ▶ *Answer*
```java
package edu.bit.ex.board.controller;

@Log4j
@AllArgsConstructor
@RestController
@RequestMapping("/restful/*")
public class RestBoardController {

	private BServiceImpl bSerivce;
	
	@GetMapping("/board")
	public ModelAndView list(ModelAndView mav) {
		mav.setViewName("rest_list");
		mav.addObject("list", bSerivce.getList());
		
		return mav;
		
	}
	
	@GetMapping("/board/{bId}") 
	public ModelAndView rest_content_view(BoardVO boardVO, ModelAndView mav) {
		
		log.info("rest_content_view");
		mav.setViewName("content_view"); //view 물리적인 경로
		mav.addObject("content_view", bSerivce.getBoard(boardVO.getbId()));
		
		return mav;
	}

	@PutMapping("/board/{bId}")
	public ResponseEntity<String> rest_update(BoardVO boardVO, Model model) {

		ResponseEntity<String> entity = null;

		log.info("rest_update");

		try {
			bSerivce.modify(boardVO);
			entity = new ResponseEntity<String>("SUCCESS", HttpStatus.OK);
		
		} catch (Exception e) {
			e.printStackTrace();
			entity = new ResponseEntity<String>(e.getMessage(), HttpStatus.BAD_REQUEST);
		}
		return entity;
	}
}
```
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>

<html>
<head>
<title>List</title>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script type="text/javascript">


	$(document).ready(function(){
		
		$(".submitDirect").click(function(event) {
			var contentModify = $("#contentModify").serialize(); //serialize()는 data를 한번에 전송하게 해주는 함수
			/* event.preventDefault(); */
			
			$.ajax({
				type:'PUT',
				url : $(this).attr('input'),
				cache :  false,
				data : contentModify,
				success : function(result) {
					
					if (result == ("SUCCESS")) {
						location.href = '${pageContext.request.contextPath}/restful/board'
	
					}
				}
			});
		});
	});
</script>

</head>
<body>
  
   <table width="500" cellpadding="0" cellspacing="0" border="1">
   
   <form id="contentModify">
   <input type="hidden" name="bId" value="${content_view.bId}">
      <tr>
         <td>번호</td>
         <td>${content_view.bId}</td>
      </tr>
      
        <tr>
         <td>히트</td>
         <td>${content_view.bHit}</td>
      </tr>
      
      <tr>
         <td>이름</td>
         <td><input type="text" name="bName" value="${content_view.bName}"></td>
      </tr>
      
      <tr>
         <td>제목</td>
         <td><input type="text" name="bTitle" value="${content_view.bTitle}"></td>
      </tr>
      
      <tr>
         <td>내용</td>
         <td><textarea rows="10" name="bContent">${content_view.bContent}</textarea></td>
      </tr>
  
      <tr>
         <td colspan="2"><input class="submitDirect" type="submit" value="수정">&nbsp; &nbsp; <a href="list">목록보기</a>
         &nbsp; &nbsp; <a href="delete">삭제</a>&nbsp; &nbsp; <a href="reply_view?bId=${content_view.bId}">답변</a></td>
      </tr>
		
      </form>
   </table>

</body>
</html>
```

