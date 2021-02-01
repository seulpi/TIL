# ★ Pasing (게시판의 마무리)
- 페이징 처리는 두 부분으로 나뉨 
1. UI처리를 위한 설계 : 눈에 보이는 것 처리(변수 7개를 가지고 처리)
2. DB(쿼리처리)
  - → my sql(limit로처리) , 오라클11g 이하, 12이상 (오라클은 11g를 실무에서 많이 사용, 오라클12미만은 limit처리를 지원하지 않기 때문에 DB처리가 좀 까다로움)
 ## - 페이징 처리를 위해서는 페이지 번호와 한 페이지당 몇개의 데이터를 보여줄것인지 결정해줘야함
 - 무엇을 변수로 표현할것인가?가 첫번째! 
 
 ## *@rownum*
 - 진짜 컬럼이 아닌데 컬럼처럼 행동하는 요소
 - 행번호를 할당(현재행의 순서를 반환한다)
 - 오라클 전용 
 - rownum을 사용해 paging하는 것을 3중세트로 표현(3개의 문이 들어가서)
  - 3중세트말고 다른거 쓰면 중간에 에러가나기 때문에 3중세트도 에러가 나긴하지만 이게 정석이니까 이렇게 쿼리를 사용하는게 BEST!
 ```sql
select rownum rn, a.* from emp a; 
select rownum rn, * from emp a; -- error! rownum을 사용할때는 어디에서 *(all)인지 표시해줘야한다
```

### *★ rownum이 언제 적용되는가?*
- **sql 처리 순서**
1. from/where절이 먼저 처리
2. rownum이 할당되고 from/where절에서 전달되는 각각의 출력 row에 대해 증가
	- **rownum은 select가 적용되기 전에** where까지 확인을 하고 번호를 할당
3. select 적용
4. group by 조건 적용
5. having 적용
6. order by 적용

```sql
select rownum rn, bid, bname, btitle from mvc_board where rownum > 10 and rownum <= 20;
--rownum은 처음에 무조건 1 where 1 > 10 false 따라서 아예 갖고오지 못하기 때문에 select할 수 없음
select rownum rn, bid, bname, btitle from mvc_board where rownum < 10;
-- 1 < 10 은 만족하니까 true이기때문에 이 조건문은 true! 그래서 출력하는 것
select bid, bname from 
(select rownum rn, bid, bname, btitle from mvc_board where rownum <= 20)
where rn > 10; 
-- 20번 보다 작은걸 뽑아서 그 그룹안에서 10번보다 큰 것만 출력
```

## @ Pasing 처리
### [게시판 코드] https://github.com/seulpi/TIL/blob/main/Quiz/javascript%26spring_Answer08(spring%EA%B2%8C%EC%8B%9C%ED%8C%90%E2%98%85).md
```java
//Criteria.java
package edu.bit.board.page;

import lombok.Getter;
import lombok.Setter;
import lombok.ToString;

@ToString
@Getter
@Setter
public class Criteria {
	// 페이징 처리를 위해서는 페이지 번호와 한 페이지당 몇개의 데이터를 보여줄것인지 결정해줘야함
	private int pageNum; //page번호
	private int amount; // 한 페이지 당 몇개의 데이터를 보여줄것인가?
	
	public Criteria() {
		this(1, 10); // 생성자 호출 , 기본값 1페이지를 10개로 지정
	}

	public Criteria(int pageNum, int amount) { 
		this.pageNum = pageNum;
		this.amount = amount;
	
	}
}
```
```java
//PageVO.java
package edu.bit.board.page;

import org.springframework.web.util.UriComponents;
import org.springframework.web.util.UriComponentsBuilder;

import lombok.Getter;
import lombok.ToString;


@Getter
@ToString
public class PageVO {
	// 페이징 처리할때 필요한 정보들, 초기화 단계
	private int startPage; // 화면에 보여질 시작번호 1~10 11~20 ...이라면 1, 11
	private int endPage; // 화면에 보여질 끝번호 10, 20 
	private boolean prev, next; // 이전<< >>다음으로 이동 가능한 링크 표시 
	
	private int total; // 전체 글 갯수(외부에서 받아와야함)
	private Criteria cri; // 구조적으로 Criteria, PageVO 나눠서 표현해주는게 좋음
	
	public PageVO(Criteria cri, int total) {
		this.cri = cri;
		this.total = total;
		
		//현재 페이지가 13이면 13/10 = 1.3 → 2 , 끝페이지는 2*10 = 20
		this.endPage = (int)(Math.ceil(cri.getPageNum() /10.0)) * 10; // 10개씩 끊어져서 나옴 1-10, 11-20, 21-30...
		this.startPage = this.endPage - 9;
		
		// total을 통한 endPage의 재계산 
		//10개씩 보여주는 경우 전체 데이터 수가 80개라고 가정하면 끝번호는 10이 아닌 8이됨 
		int realEnd = (int)(Math.ceil((total*1.0)/cri.getAmount())); 
		
		if(realEnd <= this.endPage) {
			this.endPage = realEnd; // realEnd가 8이면 8을 endPage에 넣어준다 
		}
		
		//시작번호가 1보다 큰 경우 
		this.prev = this.startPage > 1;
		//realEnd가 끝번호(endPage)보다 큰 경우 
		this.next = this.endPage < realEnd;
	}
	
	public String makeQuery(int page) {
	      UriComponents uriComponentsBuilder = UriComponentsBuilder.newInstance().queryParam("pageNum", page) // pageNum = 3
	              .queryParam("amount", cri.getAmount()) // pageNum=3&amount=10
	              .build(); // ?pageNum=3&amount=10
	        return uriComponentsBuilder.toUriString(); // ?pageNum=3&amount=10 리턴 
	}
}
```
```java
//Controller.java
package edu.bit.board.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;

import edu.bit.board.page.Criteria;
import edu.bit.board.page.PageVO;
import edu.bit.board.service.BoardService;
import edu.bit.board.vo.BoardVO;
import lombok.AllArgsConstructor;
import lombok.extern.log4j.Log4j;

@Log4j
@AllArgsConstructor// 이 어노테이션 없으면 BoardService에 @AutoWired 명시해줘야함
@Controller
public class BoardController {
	
	private BoardService boardService; // Controller에서 BoardService를 호출 

	@GetMapping("/list2")
	public void list2(Criteria cri, Model model) {
		log.info("list2()");
		log.info(cri); //PageVO에서 toString을 해놨기 때문에 toString 되는것들이 콘솔에 뿌려진다
		
		model.addAttribute("list2", boardService.getList2(cri));
		int total = boardService.getTotal(cri);
		
		log.info("total" + total);
		model.addAttribute("pageMaker", new PageVO(cri, total));
	
		}
}
```
```java
//Service
package edu.bit.board.service;

import java.util.List;

import edu.bit.board.page.Criteria;
import edu.bit.board.vo.BoardVO;

public interface BoardService {

	public  List<BoardVO> getList2(Criteria criteria);

	public int getTotal(Criteria cri);
}
```
```java
//ServiceImpl
package edu.bit.board.service;

import java.util.List;

import org.springframework.stereotype.Service;

import edu.bit.board.mapper.BoardMapper;
import edu.bit.board.page.Criteria;
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
	public List<BoardVO> getList2(Criteria criteria) {
		
		return mapper.getListWithPaging(criteria);
	}

	@Override
	public int getTotal(Criteria cri) {
		
		return mapper.getTotalCount(cri);
	}
}
```
```java
//Mapper
package edu.bit.board.mapper;

import java.util.List;

import edu.bit.board.page.Criteria;
import edu.bit.board.vo.BoardVO;

public interface BoardMapper {

	public List<BoardVO> getListWithPaging(Criteria criteria);

	public int getTotalCount(Criteria cri);
	
}
```
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  
<mapper namespace="edu.bit.board.mapper.BoardMapper">
	<!-- interface BoardMapper 맵핑(연결) -->
	<select id="getListWithPaging" resultType="edu.bit.board.vo.BoardVO">
	<![CDATA[ 
     select * from (
   		 select rownum as rnum, a.* 
         	from ( 
                select * from mvc_board order by bGroup desc, bStep asc
            ) a where rownum <= #{pageNum} * #{amount}
   	)where rnum > (#{pageNum}-1) * #{amount} 
   	]]>
	</select>
	
	<select id="getTotalCount" resultType="int">
	<![CDATA[ 
      select count(*) from mvc_board
   	]]>
	</select>
	
</mapper>
```
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
	<!-- 리스트 처리 부분 -->
	<table width="500" cellpadding="0" cellspacing="0" border="1">
		<tr>
			<td>번호</td>
			<td>이름</td>
			<td>제목</td>
			<td>날짜</td>
			<td>히트</td>
		</tr>
		<c:forEach items="${list2}" var="dto">
			<tr>
				<td>${dto.bId}</td>
				<td>${dto.bName}</td>
				<td><c:forEach begin="1" end="${dto.bIndent}">-</c:forEach> <a
					href="content_view?bId=${dto.bId}">${dto.bTitle}</a></td>
				<td>${dto.bDate}</td>
				<td>${dto.bHit}</td>
			</tr>
		</c:forEach>
		<tr>
			<td colspan="5"><a href="write_view">글작성</a></td>
		</tr>
	</table>

	<!-- 여기서부터 페이징 처리부분 -->
	<c:if test="${pageMaker.prev}">
		<a href="list2${pageMaker.makeQuery(pageMaker.startPage - 1) }">«</a>
	</c:if>

	<c:forEach begin="${pageMaker.startPage }" end="${pageMaker.endPage }"
		var="idx">
		<c:out value="${pageMaker.cri.pageNum == idx?'':''}" />
		<a href="list2${pageMaker.makeQuery(idx)}">${idx}</a>
	</c:forEach>

	<c:if test="${pageMaker.next && pageMaker.endPage > 0}">
		<a href="list2${pageMaker.makeQuery(pageMaker.endPage +1) }"> » </a>
	</c:if>
	<br>

	<button type="submit">글쓰기</button>
</body>
</html>
```
### ▶ 출력
![pasing](https://user-images.githubusercontent.com/74290204/106412739-c3044200-648b-11eb-862c-717f9b913608.PNG)
