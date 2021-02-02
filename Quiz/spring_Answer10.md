# 1. emp 에서 더미데이터 2000 개 넣어서 emp 테스트 하기
```java
package edu.bit.ex.page;

import lombok.Getter;
import lombok.Setter;
import lombok.ToString;

@Getter
@Setter
@ToString
public class Criteria {
	
	private int pageNum; // 이 변수들과 list에 사용하는 변수명이 같아야 getPageNum을 알아서 찾아올 수 가 있음!!! (주의!!!)
	private int amount;
	
	public Criteria() {
		this(1, 10); 
		
	}
	
	public Criteria(int pageNum, int amount) {
		this.pageNum = pageNum;
		this.amount = amount;
	}
}
```
```java
package edu.bit.ex.page;

import org.springframework.web.util.UriComponents;
import org.springframework.web.util.UriComponentsBuilder;

import lombok.Getter;
import lombok.ToString;

@Getter
@ToString
public class PageVO {
	
	// 필요한 변수 페이지 시작점, 끝점, 이전, 다음, 총 페이지 수, 페이지&양 만들어놓은 객체
	private int startPage;
	private int endPage;
	private boolean next, previus;
	
	private int total;
	private Criteria cri;
	
	public PageVO(Criteria cri, int total) {
		this.cri = cri;
		this.total = total;
		
		// 이 생성자에서 strtPage와 endPage 등 변수에 대한 초기화, 정의
		this.endPage = (int)(Math.ceil(cri.getPageNum()/10.0)) * 10; // endPage는 10개로 보여줄거니까 들어온 페이지 값을 10개로 나눈 것에 대해 *10 
		this.startPage = this.endPage - 9;
		
		int realEnd = (int)(Math.ceil((total*1.0)/cri.getAmount())); // 1.0을 해주는 이유? ceil함수를 사용하기 위해 소수점으로 바꿔주려고, 근데 이 부분이 잘 이해가 가지않음
		
		if(realEnd <= this.endPage) {
			this.endPage = realEnd;
		}
		
		this.previus = this.startPage > 1; // 1보다 커야하는 이유는 1보다 커야 이전으로 그 전 목록으로 넘어갈수 있기 때문에 1이 페이징의 첫 시작임
		this.next = this.endPage < realEnd; // 그렇다면 endPage는 realPage보다 작아야 다음으로 갈 목록이 있는 것임
	}
	
	public String makeQuery(int page) { //1page, 2page,.. 값만으로 query문 계산해서 넘기기때문에 필요한 요소는 page값뿐임(rownum사용할거니까)
		UriComponents uricomp = UriComponentsBuilder.newInstance().queryParam("pageNum", page).queryParam("amount", cri.getAmount()).build();
		/* UriComponents: 문자열 URI를 만들어야 할때, URI를 동적으로 만들어주는 클래스
		 * UriComponentsBuilder : 여러 개의 파라미터들을 연결하여 URL 형태로 만들어 주는 기능, 
		 * 즉 Controller단에서 addAttribute로 하나 하나 속성을 지정해주지 않아도 이 class를 이용하면 손쉽고 간단하게 파라미터 전달 가능*/
		return uricomp.toUriString();
	}
}
```
```java
package edu.bit.ex.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import edu.bit.ex.page.Criteria;
import edu.bit.ex.page.PageVO;
import edu.bit.ex.service.EmpService;
import edu.bit.ex.vo.EmpVO;
import lombok.AllArgsConstructor;
import lombok.extern.log4j.Log4j;

@Log4j
@AllArgsConstructor
@Controller
public class EmpController {
	
	private EmpService empService;
	
	@RequestMapping("/list")
	public String empList(Criteria cri, Model model) {
		log.info("empList()실행");
		log.info("cri 실행" + cri);
		
		//model.addAttribute("empList", empService.empList()); 
		model.addAttribute("empList", empService.empPage(cri));
        // 이 함수 자체가 리스트를 10개씩만 해서 보일수있도록 이미 설정을 다했기 때문에 위에 List 전체를 다 뿌리고 또 함수사용하면 X
		
		for (EmpVO vo :  empService.empPage(cri)) {
			System.out.println("번호" + vo.getEmpno());
			
		}
		
		int total = empService.getTotal(cri);
		
		log.info("total 실행" + total);
		model.addAttribute("pageMaker", new PageVO(cri, total));
		
		System.out.println();
		return "list";
		
	}
	
}
```
```java
package edu.bit.ex.service;

import java.util.List;

import edu.bit.ex.page.Criteria;
import edu.bit.ex.vo.EmpVO;

public interface EmpService {

	public List<EmpVO> empPage(Criteria cri);

	public int getTotal(Criteria cri);
}
```
```java
package edu.bit.ex.service;

import java.util.List;

import org.springframework.stereotype.Service;

import edu.bit.ex.mapper.EmpMapper;
import edu.bit.ex.page.Criteria;
import edu.bit.ex.vo.EmpVO;
import lombok.AllArgsConstructor;
import lombok.extern.log4j.Log4j;

@Log4j
@AllArgsConstructor
@Service
public class EmpServiceImpl implements EmpService {

	@Override
	public List<EmpVO> empPage(Criteria cri) {
		
		return empMapper.empListPage(cri);
	}

	@Override
	public int getTotal(Criteria cri) {
		
		return empMapper.getTotal(cri);
	}

}
```
```java
package edu.bit.ex.mapper;

import java.util.List;

import org.apache.ibatis.annotations.Mapper;

import edu.bit.ex.page.Criteria;
import edu.bit.ex.vo.EmpVO;

@Mapper
public interface EmpMapper {

	public List<EmpVO> empListPage(Criteria cri);

	public int getTotal(Criteria cri);

}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="edu.bit.ex.mapper.EmpMapper">

<select id="empListPage" resultType="edu.bit.ex.vo.EmpVO"> 
<![CDATA[ 
select * from (
	select rownum as rnum, a.* 
		from ( 
			select * from emp2
		) a where rownum <= #{pageNum} * #{amount}
	)where rnum > (#{pageNum} -1 * #{amount}) 
]]> 
</select> 
<!-- rownum이 두 번 생성되는거고 rownum은 다시 생성될 때 1부터 할당되기 때문에 
별칭을 줘서 만들어진 rnum에 대해서 비교가 들어가야 where 조건절이 성립되는 것 1 > 10 클 수 가 없기 때문에 조건 성립이 안되서 출력 X  -->

<select id="getTotal" resultType="int"> 
<![CDATA[ 
select count(*) from emp2
]]>
</select>
</mapper>
```
```jsp
<table width="100%" cellspacing="0">
    <tr>
        <td>사원번호</td>
		<td>사원이름</td>
		<td>사원직급</td>
		<td>매니저</td>
		<td>입사일</td>
		<td>급여</td>
		<td>커미션</td>
		<td>부서번호</td>
	</tr>

    <c:forEach items="${empList}" var="dao">

	<tr>
		<td>${dao.empno}</td>
		<td>${dao.ename}</td>
		<td>${dao.job}</td>
		<td>${dao.mgr}</td>
		<td>${dao.hiredate}</td>
		<td>${dao.sal}</td>
		<td>${dao.comm}</td>
		<td>${dao.deptno}</td>
	</tr>
	</c:forEach>

    <tr class="addCenter">
		<td colspan="8" style=""><a href="EmpAdd_View">사원 추가</a></td>
	</tr>
</table>						

<c:if test="${pageMaker.previus}">
	<a href = "list${pageMaker.makeQuery(pageMaker.startPage - 1)}"> 이전 </a>
</c:if>
								
<c:forEach begin="${pageMaker.startPage}" end="${pageMaker.endPage}" var="idx">
	<a href = "list${pageMaker.makeQuery(idx)}">${idx}</a>
</c:forEach>
								
<c:if test= "${pageMaker.next && pageMaker.endPage > 0}">
	<a href ="list${pageMaker.makeQuery(pageMaker.endPage +1)}"> 다음 </a>
</c:if>
<br>
```
<br>

▶ 출력 

![1111](https://user-images.githubusercontent.com/74290204/106584796-f7f6be80-6589-11eb-8078-560893ad8a3f.PNG)


# 2. Juit를 통해서 랜덤으로 emp 더미 데이터에서 deptno 업데이트 하시오

# 3. emp01테이블을 emp 에서 복사하여, pk ,fk 툴을 사용해도 좋고, alter 문으로 설정하여, emp01로 페이징 테스트 하기
```sql

COPY FROM scott/tiger CREATE emp2 USING select * FROM emp;
--emp에 있는 데이터들을 다 복사해주지만 pk, fk는 따로 설정해줘야함

DECLARE
      v_cnt NUMBER := 7935; -- max(empno)가 안먹어서 max값 찾아서 다이렉트로 넣어줌

begin
  for i in 1..1000 loop
   insert into emp2 (empno, ename, job, mgr, hiredate,sal,comm,deptno) 
   values (v_cnt, 'test' , 'job' , 0, sysdate, 0, 0, 0);

   v_cnt := v_cnt+1;

  end loop;
end; 

commit;
```
### - emp 복사 후 pk, fk 설정 
![pk설정](https://user-images.githubusercontent.com/74290204/106585419-ac90e000-658a-11eb-9f80-5e81a814568c.PNG)

![fk](https://user-images.githubusercontent.com/74290204/106585686-ef52b800-658a-11eb-9a47-fa04da6e7c50.PNG)

![fk2](https://user-images.githubusercontent.com/74290204/106585693-f11c7b80-658a-11eb-9544-30c8adcc28d3.PNG)

# 4.아래의 리스트 페이지 에서 Jquery 로 makeList() 함수를 완성하여, 페이지를 뿌리도록 하시오 (수정중)
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
	function makeList() {
						
	        	
	} //end	
	</script>
	
	<script>
		$(document).ready(function(){
			makeList();
		});
	</script>

</head>
<body>
	<table id="list-table" width="500" cellpadding="0" cellspacing="0" border="1">
	</table>
</body>
</html>
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
	
	<script type="text/javascript">

	function makeList() {
		
		$("#list-tr").appendTo("<td class='empno'>사원번호</td>");
		$(".empno").append("<td class='ename'>사원이름</td>");
		$(".ename").append("<td class='job'>사원직급</td>");
		$(".job").append("<td class='hiredate'>입사일</td>");
		$(".hiredate").append("<td class='mgr'>매니저</td>");
		$(".mgr").append("<td class='sal'>급여</td>");
		$(".sal").append("<td class='comm'>커미션</td>");
		$(".comm").append("<td class='deptno'>부서번호</td>");
						

	} //end	
	</script>
	
	<script>
		$(document).ready(function(){
			makeList();
		});
		
	</script>

</head>
<body>
	<table id="list-table" width="500" cellpadding="0" cellspacing="0" border="1">
		<tr id="list-tr"></tr>
	<c:forEach items="${empList}" var="dao">
		<tr>
			<td>${dao.empno}</td>
			<td>${dao.ename}</td>
			<td>${dao.job}</td>
			<td>${dao.mgr}</td>
			<td>${dao.hiredate}</td>
			<td>${dao.sal}</td>
			<td>${dao.comm}</td>
			<td>${dao.deptno}</td>
		</tr>
	</c:forEach>
	
	</table>

</body>
</html>
```
