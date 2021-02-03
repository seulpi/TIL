# 1. emp 에서 더미데이터 2000 개 넣어서 emp 테스트 하기
## [더미데이터 3번 참조]

▶ 출력 

![1111](https://user-images.githubusercontent.com/74290204/106584796-f7f6be80-6589-11eb-8078-560893ad8a3f.PNG)


# 2. Juit를 통해서 랜덤으로 emp 더미 데이터에서 deptno 업데이트 하시오(다시)
```java
public interface EmpMapper {
    public void update();    
}
```
```xml
//empMapper.xml

<update id="update" >
    <![CDATA[
        update emp set deptno = (ROUND(DBMS_RANDOM.VALUE(1, 4),0)*10)
    ]]>
</update>  
```
```java
//empMapperTest

@RunWith(SpringRunner.class) //ioc 컨테이너를 생성
@ContextConfiguration("file:src/main/webapp/WEB-INF/spring/root-context.xml")  
//dataSource에 대한 정보는 root-context.xml에 존재하기 때문에 표기 필수
@Log4j
public class EmpMapperTest {
    @Setter(onMethod_ = @Autowired)
    private EmpMapper mapper;
    @Test
    public void testupdate() {  //mapper.xml id와 동일한 명으로 사용하기!
        mapper.update();        //update 후에 값 가져오기, 대신 return 타입은 void!
        List<EmpVO> list = mapper.getlist();
        log.info(mapper);
        
        for(EmpVO empVO : list) {
            log.info(empVO.getDeptno());
        }
    }
}
```

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

![fk](https://user-images.githubusercontent.com/74290204/106693150-2e2f4f00-6619-11eb-8547-f97d7ddcafef.PNG)


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
