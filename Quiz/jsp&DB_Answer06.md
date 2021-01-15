# 1. 게시판 설계도를 그리시오(Model 2,MVC)
![model2](https://user-images.githubusercontent.com/74290204/104569545-52d87c80-5694-11eb-85ba-fca25ff62ce1.PNG)
<br>

# 2. DB 관련하여 아래를 정리하시오 ★★★★★게시판 DB 설계(특히 댓글관련 컬럼)★★★★★
## - list 뽑아내는 sql
```sql
select bId, bName, bTitle, bContent, bDate, bHit, bGroup, bStep, bIndent from mvc_board from mvc_board order by bGroup desc, bStep asc;
```

## - 댓글 달기 위한 sql
```sql
1. 댓글 insert 
insert into mvc_board(bId, bName, bTiltle, bContent, bGroup, bStep, bIndent) values(mvc_board_seq.nextval, ?, ?, ?, ?, ?, ? );

2. 댓글 정렬 
update mvc_board set bStep = bStep + 1 where bGroup = ? and bStep > ?
```

## - 수정 sql
```sql
update mvc_board set bNam= ?, bTitle=?, bContent=? where bId=?
```

## - 게시판 글 insert sql
```sql
insert into mvc_board(bId, bName, bTitle, bContent, bHit, bGroup, bStep, bIndent) 
values (mvc_board_seq.nextval, ?, ?, ?, 0, mvc_board_seq.currval, 0, 0);
```
<br>

# 3. Servlet 에서 forward 방법은?
```java
RequestDispatcher dispatcher = request.getDispatcher(viewpage); //viewpage = forward할 페이지
dispatcher.forward();
```
<br>

# 4. 게시판을 한시간 정도로 짤수 있도록 노력합시다.
<br>

# 5. 아래 쿼리를 푸시오 (맞는지 모르겠음)
## -  자신의 급여가 평균 급여보다 많고 이름에 T가 들어가는 사원과 <br> 동일한 부서에 근무하는 모든 사원의 사원 번호, 이름 및 급여를 출력하라
## -  DALLAS에서 근무하는 사원과 직업이 일치하는 사원의 이름,부서이름, 및 급여를 출력하시오

```sql
--43> 자신의 급여가 평균 급여보다 많고 이름에 T가 들어가는 사원과
 동일한 부서에 근무하는 모든 사원의 사원 번호, 이름 및 급여를 출력하라.
select empno, ename, sal from emp where 
sal > (select avg(sal) from emp where deptno in (select deptno from emp where ename like'%T%'));

--45> DALLAS에서 근무하는 사원과 직업이 일치하는 사원의 이름,부서이름, 및 급여를 출력하시오
select e.ename, d.dname, e.sal from emp e, dept d where ename = (select ename from dept where loc= 'DALLAS');
```
