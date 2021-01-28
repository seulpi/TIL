# [게시판 풀면서 부족했던 점]
- servlet 게시판 까먹음
- 까먹으니까 쿼리문 기억 못함
- 댓글에 대한 이해도 떨어짐
## *- 수업듣고 추가&수정되야 할 부분*
- contoller단 비즈니스로직 추가된 부분(댓글 정렬하는 함수) → 서비스단으로 이동 (주석으로 내용 달았음)

## *- 혼자 구현했을때 느낀점&문제!!!!*
```
- 에러
1. Mapper도 interface! xml자식이 구현해주는거기 때문에 class가 아님
2. VO만들때 롬복이 안먹음 따라서 손으로 쳐줘야함

[에러] 프로젝트에 X 가뜬다 → Maven 업데이트 → 그래도 안된다: project-clean → 그래도 안된다: 우클릭 - property - Project Facets - Dynamic Web Module 을 4.0으로 맞추기

진짜 별별 오류가 다 생긴다ㅋㅋㅋ
오타부터 값 못받아오는 에러까지..List만 잘 구현되도 뒤에는 좀 수월할듯하다
근데 view단에서 똑같이 했는데 안되는건 왜그런거지? 기존에 view단을 복붙하면 된다!  단순에러인가..

- view단 
1. textarea로 주면 안에 value해서 내용 불러오기 안됨
따라서 밖으로 빼줘야하고 input은 안에 넣어도 value값으로 넣어주면 내용 보여줌

▶ List 구현하는게 진짜 오래걸린다(4시간걸린듯)
여기에서 진을 빼니까 뒤에는 그냥 생각안남..
*지금 안되는것들*
- 작동되는 원리
- 값을 어떤걸 넣어줘야하고 어떻게 받아서 넘기는지
- 왜 reply_view단에만 hidden을 쓰는지
- content_view단에서 reply_view 넘길때 primary key(Id) 같이 링크 걸어서 넘겨주는 것 
- 어노테이션 어디에 적절하게 사용하는지

: 사실상 그냥 다인듯 ㅋㅋㅋㅋㅋㅋㅋ(아직 못외운것,,외울수있..겠..지..)
```

# 1. BoardController [FrontController]
```java
package edu.bit.board.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;

import edu.bit.board.service.BoardService;
import edu.bit.board.vo.BoardVO;
import lombok.AllArgsConstructor;
import lombok.extern.log4j.Log4j;

@Log4j
@AllArgsConstructor// 이 어노테이션 없으면 BoardService에 @AutoWired 명시해줘야함
@Controller
public class BoardController {
	
	private BoardService boardService; // Controller에서 BoardService를 호출 
	
	
	@GetMapping("/list")
	public void list(Model model) {
		log.info("list");
		
		model.addAttribute("list", boardService.getList());
		}
		/* void를 적게 되면 함수명으로 리턴시킴 
		@GetMapping("/list")
		public String list(Model model) {
			log.info("list");
			model.addAttributr("list", boardService.getList());
			return "list"; 와 동일한 코드라고 보면됨*/
	
	@GetMapping("/write_view")
	public String write_view(Model model) {
		log.info("write_view");
		
		return "write_view";
		}
	
	@PostMapping("/write") // post로 받기 때문에 @PostMapping
	public String write(BoardVO boardVO , Model model) throws Exception { //command객체로 받는게 제일 좋음
		log.info("write()");
		boardService.writeBoard(boardVO);
		
		return "redirect:list";
		}
	
	@GetMapping("/content_view")
	public String content_view(BoardVO boardVO , Model model) throws Exception {
		log.info("content_view");
		
		model.addAttribute("content_view", boardService.getBoard(boardVO.getbId()));
		boardService.upHit(boardVO);
		
		return "content_view";
		}
	
	@GetMapping("/delete")
	public String delete(BoardVO boardVO , Model model) throws Exception {
		log.info("delete");
		boardService.deleteBoard(boardVO.getbId());
		return "redirect:list";
		}
	
	
	@PostMapping("/modify")
	public String modify(BoardVO boardVO , Model model) throws Exception {
		log.info("modify");
		
		boardService.modifyBoard(boardVO);
		return "redirect:list";
		}
	
	@GetMapping("/reply_view")
	public String reply_view(BoardVO boardVO, Model model) {
		log.info("reply_view");
		model.addAttribute("reply_view", boardService.getBoard(boardVO.getbId()));
		return "reply_view";
		}
	
	@PostMapping("/reply") 
	public String reply(BoardVO boardVO , Model model) throws Exception { 
		log.info("reply()");
		boardService.replyShape(boardVO);
		boardService.replyBoard(boardVO);
		
		/* controller에서는 함수를 한개를 호출 따라서 댓글 정렬해주고 하는 로직들은 Service단에서(비즈니스로직)
		   controller에서는 view만 결정할 수 있게 하는게 목적 (replyShape-Service단으로 옮겨야함)*/
		   
		return "redirect:list";
		}
	
}
```

# 2. BoardVO [DTO]
```java
package edu.bit.board.vo;

import java.sql.Timestamp; // import 주의 sql!! 

// 롬복이 쓰면 에러남..그냥 손으로 만들기 (대소문자 섞이니까 못잡는것같다는 에러)
public class BoardVO {

	private int bId;
	private String bName;
	private String bTitle;
	private String bContent;
	private Timestamp bDate;
	private int bHit;
	private int bGroup;
	private int bStep;
	private int bIndent;

	public BoardVO() {

	}

	public BoardVO(int bId, String bName, String bTitle, String bContent, Timestamp bDate, int bHit, int bGroup,
			int bStep, int bIndent) {
		
		this.bId = bId;
		this.bName = bName;
		this.bTitle = bTitle;
		this.bContent = bContent;
		this.bDate = bDate;
		this.bHit = bHit;
		this.bGroup = bGroup;
		this.bStep = bStep;
		this.bIndent = bIndent;
	}

	public int getbId() {
		return bId;
	}

	public void setbId(int bId) {
		this.bId = bId;
	}

	public String getbName() {
		return bName;
	}

	public void setbName(String bName) {
		this.bName = bName;
	}

	public String getbTitle() {
		return bTitle;
	}

	public void setbTitle(String bTitle) {
		this.bTitle = bTitle;
	}

	public String getbContent() {
		return bContent;
	}

	public void setbContent(String bContent) {
		this.bContent = bContent;
	}

	public Timestamp getbDate() {
		return bDate;
	}

	public void setbDate(Timestamp bDate) {
		this.bDate = bDate;
	}

	public int getbHit() {
		return bHit;
	}

	public void setbHit(int bHit) {
		this.bHit = bHit;
	}

	public int getbGroup() {
		return bGroup;
	}

	public void setbGroup(int bGroup) {
		this.bGroup = bGroup;
	}

	public int getbStep() {
		return bStep;
	}

	public void setbStep(int bStep) {
		this.bStep = bStep;
	}

	public int getbIndent() {
		return bIndent;
	}

	public void setbIndent(int bIndent) {
		this.bIndent = bIndent;
	}

}
```

# 3. BoardService [BCommand]
```java
package edu.bit.board.service;

import java.util.List;

import edu.bit.board.vo.BoardVO;

public interface BoardService {
	
	public List<BoardVO> getList(); // 목록으로 가지고오기

	public void writeBoard(BoardVO boardVO); // 글추가

	public BoardVO getBoard(int getbId);  //글쓴거 보기

	public void deleteBoard(int getbId); // 삭제 

	public void modifyBoard(BoardVO boardVO); // 수정
	
	public void replyBoard(BoardVO boardVO); //답변달기

	public void replyShape(BoardVO boardVO); //답변 정렬

	public void upHit(BoardVO boardVO);
}
```

# 4. BoardServiceImpl [servlet에서 Command를 기능별로 나눈것을 한번에 정의하고 있는 클래스]
```java
package edu.bit.board.service;

import java.util.List;

import org.springframework.stereotype.Service;

import edu.bit.board.mapper.BoardMapper;
import edu.bit.board.vo.BoardVO;
import lombok.AllArgsConstructor;
import lombok.extern.log4j.Log4j;

@Log4j
@Service // Service임을 표시
@AllArgsConstructor 
public class BoardServiceImpl implements BoardService{
	
	/* 기존 전 버전에서는 @Autowired  or @Inject or new를 사용했는데
	→ 5.0 버전부터는 @Autowired, @Inject 안달아줘도 자동으로 객체 생성해서 주입해줌 */ 
	private BoardMapper mapper;
	
	@Override
	public List<BoardVO> getList() {
		return mapper.getList();
	}

	@Override
	public void writeBoard(BoardVO boardVO) {
		mapper.insert(boardVO);
	}

	@Override
	public BoardVO getBoard(int bno) {
		log.info("getBoard...");
		return mapper.read(bno);
	}

	@Override
	public void deleteBoard(int bno) {
		mapper.delete(bno);
		
	}

	@Override
	public void modifyBoard(BoardVO boardVO) {
		mapper.modify(boardVO);
		
	}

	@Override
	public void replyBoard(BoardVO boardVO) {
		mapper.reply(boardVO);
	}

	@Override
	public void replyShape(BoardVO boardVO) {
		mapper.replyShape(boardVO);
	}

	@Override
	public void upHit(BoardVO boardVO) {
		mapper.upHit(boardVO);
		// 만약에 매개변수가 두개 들어오면 xml에서 어떤 객체의 값을 가져오는지 명시를 해줘야한다!
	}	
}
```

# 5. BoardMapper [DAO]
```java
package edu.bit.board.mapper;

import java.util.List;

import edu.bit.board.vo.BoardVO;

public interface BoardMapper {
	
	public List<BoardVO> getList();
	//자손을 xml로 구현 

	public void insert(BoardVO boardVO);

	public BoardVO read(int bno);

	public void delete(int bno);

	public void modify(BoardVO boardVO);

	public void reply(BoardVO boardVO);

	public void replyShape(BoardVO boardVO);

	public void upHit(BoardVO boardVO);	
}
```

# 6. BoardMapper.xml [sql정의]
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  
<mapper namespace="edu.bit.board.mapper.BoardMapper">
	<!-- interface BoardMapper 맵핑(연결) -->
	
	<select id="getList" resultType="edu.bit.board.vo.BoardVO">
	<!-- getList= 함수명 /  resultType = return할 객체 BoardVO(BoardVO가 위치한 경로 다 적어줘야함)-->
	<![CDATA[ 
      select bId, bName, bTitle, bContent, bDate, bHit, bGroup, bStep, bIndent from mvc_board order by bGroup desc, bStep asc
   	]]> <!-- 쿼리문에 ';' 붙이지않음!!  -->
	</select>
	
	
	<insert id="insert"> <!-- insert는 resultType이 없고 select로해도되지만 남이 봤을때도 insert인걸 알려주기 위해 insert로 사용 -->
	<![CDATA[
      insert into mvc_board (bId, bName, bTitle, bContent, bHit, bGroup, bStep, bIndent) values (mvc_board_seq.nextval, #{bName}, #{bTitle}, #{bContent}, 0, mvc_board_seq.currval, 0, 0)
   	]]> <!-- 객체로 넘어올때는 #을 붙여주는데 #의 의미는 변수로 넘어오는거에 대한 표시 : #{bName} → #{bName.getbName()}임 -->
	</insert>
	
	<select id="read" resultType="edu.bit.board.vo.BoardVO">
	<![CDATA[ 
      select * from mvc_board where bId = #{bno}
   	]]>
	</select>
	
	<delete id="delete">
	<![CDATA[ 
      delete from mvc_board where bId = #{bno}
   	]]>
	</delete>
	
	<update id="modify">
	<![CDATA[
      update mvc_board set bName = #{bName}, bTitle = #{bTitle}, bContent = #{bContent} where bId=#{bId}
   	]]> 
	</update>
	
	<insert id="reply"> 
	<![CDATA[
      insert into mvc_board (bId, bName, bTitle, bContent, bHit, bGroup, bStep, bIndent) values (mvc_board_seq.nextval, #{bName}, #{bTitle}, #{bContent}, 0, #{bGroup}, #{bStep}+1, #{bIndent}+1)
   	]]> 
	</insert>
	
	<update id="replyShape">
	<![CDATA[
      update mvc_board set bStep = #{bStep}+1 where bGroup = #{bGroup} and bStep > #{bStep}
   	]]> 
	</update>
	
	<update id="upHit">
	<![CDATA[
      update mvc_board set bHit = bHit+1 where bId = #{bId}
   	]]> 
	</update>
	
</mapper>
```

# 7. View [servlet과 동일]
```jsp
// list
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=EUC-KR">
<title>Insert title here</title>
</head>
<body>
   
   <table width="500" cellpadding="0" cellspacing="0" border="1">
      <tr>
         <td>번호</td>
         <td>이름</td>
         <td>제목</td>
         <td>날짜</td>
         <td>히트</td>
      </tr>
      <c:forEach items="${list}" var="dto"> 
      
      <tr>
         <td>${dto.bId}</td> 
         <td>${dto.bName}</td>
         <td>
            <c:forEach begin="1" end="${dto.bIndent}">[re]</c:forEach>
            <a href="content_view.do?bId=${dto.bId}">${dto.bTitle}</a></td>
         <td>${dto.bDate}</td>
         <td>${dto.bHit}</td>
      </tr>
      </c:forEach>
      <tr>
         <td colspan="5"> <a href="write_view.do">글작성</a> </td>
      </tr>
   </table>
   
</body>
</html>
```
```jsp
// write_view
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
   <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
   <title>Home</title>
</head>
<body>
<table width="500" cellpadding="0" cellspacing="0" border="1">
      <form action="write" method="post">
         <tr>
            <td> 이름 </td>
            <td> <input type="text" name="bName" size = "50"> </td>
         </tr>
         <tr>
            <td> 제목 </td>
            <td> <input type="text" name="bTitle" size = "50"> </td>
         </tr>
         <tr>
            <td> 내용 </td>
            <td> <textarea name="bContent" rows="10" ></textarea> </td>
         </tr>
         <tr >
            <td colspan="2"> <input type="submit" value="입력"> &nbsp;&nbsp; <a href="list.do">목록보기</a></td>
         </tr>
      </form>
   </table>
   
</body>
</html>
```
```jsp
//content_view
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=EUC-KR">
<title>Insert title here</title>
</head>
<body>

   <table width="500" cellpadding="0" cellspacing="0" border="1">
      <form action="modify.do" method="post">
         <input type="hidden" name="bId" value="${content_view.bId}">
         <tr>
            <td> 번호 </td>
            <td> ${content_view.bId} </td>
         </tr>
         <tr>
            <td> 히트 </td>
            <td> ${content_view.bHit} </td>
         </tr>
         <tr>
            <td> 이름 </td>
            <td> <input type="text" name="bName" value="${content_view.bName}"></td>
         </tr>
         <tr>
            <td> 제목 </td>
            <td> <input type="text" name="bTitle" value="${content_view.bTitle}"></td>
         </tr>
         <tr>
            <td> 내용 </td>
            <td> <textarea rows="10" name="bContent" >${content_view.bContent}</textarea></td>
         </tr>
         <tr>
            <td colspan="2"> <input type="submit" value="수정"> &nbsp;&nbsp; <a href="list.do">목록보기</a> &nbsp;&nbsp; <a href="delete.do?bId=${content_view.bId}">삭제</a> &nbsp;&nbsp; <a href="reply_view.do?bId=${content_view.bId}">답변</a></td>
         </tr>
      </form>
   </table>
   
</body>
</html>
```
```jsp
//reply_view
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>

   <table width="500" cellpadding="0" cellspacing="0" border="1">
      <form action="reply.do" method="post">
         <input type="hidden" name="bId" value="${reply_view.bId}"> 
         <!-- hidden은 user한테 보여주지않는 값들 왜 숨겨놓는가? 왜냐면 원본글에 대한 step, indent를 계산해야하기 때문에(유저한테 보여줄 필요가 없음, 프로그래밍 적으로 필요)-->
         <input type="hidden" name="bGroup" value="${reply_view.bGroup}">
         <input type="hidden" name="bStep" value="${reply_view.bStep}">
         <input type="hidden" name="bIndent" value="${reply_view.bIndent}">
         <tr>
            <td> 번호 </td>
            <td> ${reply_view.bId} </td>
         </tr>
         <tr>
            <td> 히트 </td>
            <td> ${reply_view.bHit} </td>
         </tr>
         <tr>
            <td> 이름 </td>
            <td> <input type="text" name="bName" value="${reply_view.bName}"></td>
         </tr>
         <tr>
            <td> 제목 </td>
            <td> <input type="text" name="bTitle" value="${reply_view.bTitle}"></td>
         </tr>
         <tr>
            <td> 내용 </td>
            <td> <textarea rows="10"  name="bContent">${reply_view.bContent}</textarea></td>
         </tr>
         <tr >
            <td colspan="2"><input type="submit" value="답변"> <a href="list.do" >목록</a></td>
         </tr>
      </form>
   </table>
   
</body>
</html>
```
