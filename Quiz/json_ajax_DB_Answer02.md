# 1. RESTful에 대하여 설명하시오

# 2. RESTful과 ajax를 이용해 delete를 구현하시오
## ▶ *Answer(ajax작동은 잘되나 새로고침해야 delete가 업데이트됨)*
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
