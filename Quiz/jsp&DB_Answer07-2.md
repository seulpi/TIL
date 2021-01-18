# 1. 아래를 try catch 로 묶지 않으면 에러가 나는 이유를 설명하시오 <br>(설명필요, 다시 복습하기!!!!! 동영상으로..ㅎ..까먹었어...)
```java
	try {
			connection = dataSource.getConnection();
			String query = "insert into mvc_board (bId, bName, bTitle, bContent, bHit, bGroup, bStep, bIndent) values (mvc_board_seq.nextval, ?, ?, ?, 0, mvc_board_seq.currval, 0, 0 )";
			preparedStatement = connection.prepareStatement(query);
			preparedStatement.setString(1, bName);
			preparedStatement.setString(2, bTitle);
			preparedStatement.setString(3, bContent);
			int rn = preparedStatement.executeUpdate();
		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
		}
```
- 자바에서는 **close()가 필요한 객체들이 존재**하는데 자바어플리케이션 내에서만 사용되는 게 아니라 <br> 운영체제의 자원들을 사용하는 것들 **(File, Network, DB등)** 이 해당된다
- Exception 중 RuntimeException 이외의 모든것들은 예외처리를 해줘야한다. 따라서 SQL 관련된 SQLException도 예외처리 해줘야함
   - 예외처리 방법 1. therows 2. try-catch 
   ### - **basic / java / basic15 참고!! https://github.com/seulpi/TIL/blob/main/Basic/Java/basic15(%EC%98%88%EC%99%B8%EC%B2%98%EB%A6%AC).md**
- *에러처리를 try-catch로만 하고 finally 블럭안에 close()를 호출해 연결 종료를 해주지 않을 경우* 한정된 시스템 자원을 낭비하게 된다 <br> 자원은 close() 메소드를 호출할 때 까지는 반환되지 않기 때문에 중간에 예외가 발생하여 close() 메소드를 호출할 수 없는 경우 <br> 그 Connection 객체는 계속해서 데이터베이스 연결된 상태로 남아 있게 되며 데이터베이스 연결은 쓸데없이 시스템 자원만을 차지하게 된다 <br> **결국 어플리케이션이 사용할 수 있는 자원이 모자란 상황이 발생하게 된다** <br>
▶ 자원 부족 현상이 발생하지 않도록 하기 위해서는 예외 발생 여부와 상관없이 <br> 사용한 모든 자원(Connection, Statement, PreparedStatement, ResultSet)의 close() 메소드를 호출할 수 있도록 해야 한다<br> 방법으로는 try .. catch .. finally 블럭을 사용하면 된다
  - **finally 블럭은 try { .. } catch 블럭에서 예외가 발생했는지의 여부에 상관없이 항상 실행된다** <br> 따라서, finally 블럭은 사용한 모든 자원을 반납(close)하기에 가장 알맞은 곳 (안전하게 자원을 반납한다=어플리케이션이 좀 더 안정적으로 작동한다)

# 2. 데이터 무결성을 위한 제약조건 4가지는?
## @ 정의 : 데이터 무결성이란 데이터베이스에 저장된 데이터 값과 실제 표햔되는 값이 일치하는 정확성을 의미<br>데이터베이스에 저장된 데이터의 일관성과 정확성을 지키는 것

>> 무결성 제약조건 = DB에 저장되어있는 데이터의 정확성(일관성)을 위해 <br> 자료가 DB내에 저장되는 것을 방지하기 위한 제약조건

## @ 종류
1. Not Null : Null을 허용하지 않는다

	- Null값 입력금지, 해당 컬럼의 값 입력 필수 
2. Unique : 중복된 값을 허용하지 않고 **항상 유일한 값을 갖도록함** 
	- Map의 key같은 존재 

3. Primary Key : Null을 허용하지 않고 중복된 값도 허용하지 않음
	- **Not Null + Unique**
	- 테이블에 저장된 record 데이터를 고유하게 식별하기 위한 기본키
	- 하나의 테이블에 하나의 기본키만 제약 가능 

4. ★Foreign Key : 참조되는 테이블 컬럼의 값이 존재하면 허용
	- 테이블 관계를 정의 (key를 복사하게되면 부모와 자식간의 관계가 형성된다)

5. Check : 저장 가능한 데이터 값의 범위나 조건을 지정하여 설정한 값만을 허용
	- 입력할 수 있는 값의 범위를 제한

# 3. foreign key 에 대하여 설명하시오
- 다른쪽 테이블에 있는 컬럼을 갖고 오는 것(각각 다른 테이블을 서로 연결하는 데 사용되는 키)
- Key를 갖고오게 되면 테이블이 관계를 갖게됨(=Relation이 생기면 부모자식간의 관계가 형성) 
>> 자식: 부모에 있는 Key를 가지고 오는 쪽 <br>
부모: Key를 갖고있는 쪽, 어디서 Key를 가지고 오지 않는다
```sql
-- 관계를 갖게되면 관계가 맺어지는 유형 3가지
* 1은 무조건 부모쪽
1. 1 : 1 
	▶ 1개에 대해서 1개
2. 1 : N
	▶ 1개에 대해서 여러개를 사용
	DEPT DEPTNO ='10' EMP DEPTNO='10' emp에서 여러명에 부서 번호 할당 (n은 세발낙지 모양기호)
	ex) 철수가 여러 수업을 수강한다
3. N : N 
	▶ 1개에 대해서 2개 테이블 이상인 여러개를 사용 
	(각각의 테이블이 1개의 테이블을 중점으로 각각 1 : N 관계를 형성)
	ex) 학원에서 여러개의 수업을 제공하고 학생은 여러개의 수업을 들을 수 있다
```

![forignKey](https://user-images.githubusercontent.com/74290204/104687827-4828f080-5743-11eb-93ea-6c3aa4dbd453.PNG)


# 4. 아래 쿼리문을 프로그래밍하시오
```sql
--47> 직업이 Clerk 인 사원들보다 더 많은 급여를 받는 사원의 사원번호, 이름, 급여를 출력하되 결과를 급여가 높은 순으로 정렬하라.
--48> 이름에 A가 들어가는 사원과 같은 직업을 가진 사원의 이름과 월급, 부서번호를 출력하라.
--49> New  York 에서 근무하는 사원과 급여 및 커미션이 같은 사원의 사원이름과 부서명을 출력하라.
```
```sql
--47> 직업이 Clerk 인 사원들보다 더 많은 급여를 받는 사원의 사원번호, 이름, 급여를 출력하되 결과를 급여가 높은 순으로 정렬하라.
select empno, ename, sal from emp where sal > all(select sal from emp where job = 'CLERK') order by sal desc; 
→ cleck부서의 급여들을 전체적으로 다 제외하고 뽑은 결과 (따라서 clecrk의 급여중 최대급여를 받는 사람을 기준으로 비교)
select empno, ename, sal from emp where sal > all(select sal from emp where job = 'CLERK') order by sal desc;
→ cleck 부서의 최저 급여를 받는 사람을 기준으로 쿼리문 출력
* 비교의 대상이 한개가 아니기때문에(급여가 사람마다 다르기때문) 정확한 비교기준이 필요

--48> 이름에 A가 들어가는 사원과 같은 직업을 가진 사원의 이름과 월급, 부서번호를 출력하라.
select ename, sal, deptno from emp where job in (select job from emp where ename like '%A%');
* like 대신 자꾸 '='를 사용 (주의할것!!)

--49> New  York 에서 근무하는 사원과 급여 및 커미션이 같은 사원의 사원이름과 부서명을 출력하라.
select ename, dname from emp, dept where emp.deptno = dept.deptno and sal in
(select sal from dept, emp where dept.deptno = emp.deptno AND  loc ='NEW YORK') 
and nvl(emp.comm,0) in (select nvl(comm, 0) from emp,dept where emp.deptno=dept.deptno and dept.loc = 'NEW YORK');

* 설명필요
```
