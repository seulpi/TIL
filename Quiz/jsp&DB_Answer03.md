# 1. el 이란 무엇인가?
### @ 정의
- EL(Expression Language) : 액션태그나 자바스트립을 사용하지 않고 좀 더 간편하게 사용하기 위한 언어<br>
    - 다양한 위치에 있는 데이터에 접근하기 위한 언어로 JSP의 기본 문법을 보완하는 역활을 한다(주로 html에서 jsp문법을 없애고자 할 때)
- EL 문법 
>> ${ }

### @ 특징 
- JSP의 Scope에 맞는 속성을 사용할 수 있음
- 연산자 제공
- Java 클래스 메소드 호출 가능
- EL만의 기본 객체 제공 
<br>
▶ EL의 기본 객체 <br>

 ![EL기본객체](https://user-images.githubusercontent.com/74290204/104171681-5bd40e80-5446-11eb-8631-db3992637166.PNG)
<br>

# 2. jstl 문법에 대하여 설명하시오
: jstl은 태그 라이브러리이기 때문에 el과 달리 라이브러리를 다운받아서 이클립스에 복사해줘야 사용이 가능하다
>> 기본 문법 < c: >

## @ jstl 태그 

![jspl](https://user-images.githubusercontent.com/74290204/104172149-21b73c80-5447-11eb-8409-253fadeb98b8.PNG)
<br>

### 1. set, remove 
- set : 지정한 영역에 변수를 설정
- remove : 변수 제거
```jsp
<c:set var="varName" value="varValue"/>
// var=variable 변수명 , var varName = varValue; / 값 세팅

<c:remove var="varName"/> 
// 메모리에 올린 값 삭제 
```

### 2. if 
- if문과 유사하지만 else영역을 하는 기능이 없다
```jsp
<c:if test="조건">
// 조건 내용이 true면 처리
</c:if>
```

### 3. choose, when, otherwise
- switch case문과 유사 , if else의 기능
```jsp
<c:choose>
    <c:when test='조건1'>
    </c:when>
    <c:when test='조건2'>
    </c:when>
    <c:otherwise>
    </c:otherwise>
</c:choose>
```

### 4. forEach
- 배열 및 Collection에 저장된 요소들을 차례대로 처리한다

```jsp
<c:forEach var="변수" items="아이템" [begin="시작번호"] [end="끝번호"]>
/* var - EL에서 사용될 변수명, items - 배열, Collection
begin - items에서 지정한 목록해서 값을 읽어올 인덱스의 시작 값, end- items에서 지정한 목록에서 값을 읽어올 인덱스의 마지막 값*/
${변수}

</c:forEach>
```

- items이 Map인 경우 변수에서 저장되는 객체는 Map,Entry<br> 따라서 변수값을 사용할때는 $(변수, key) 와 $(변수, value)를 사용해 Map에 저장된 항목의(key, value) 접근 할 수 있다

### 5. import
- 지정한 URL에 연결하여 결과를 지정한 변수에 저장한다
```jsp
<c:import url="URL" charEncoding="캐릭터인코딩" var="변수명" scope="범위">
/* url - 결과를 읽어올 URL , charEncoding - 읽어 온 결과를 저장할때 사용할 캐릭터 인코딩 
var - 읽어올 결과를 저장할 변수명 , scope - 변수를 저장할 영역*/
    <c:param name="파라미터 이름" value="파라미터 값"/>
    // c:param - url 속성에 지정한 사이트에 연결할 때 전송할 파라미터 입력
</c:import>
```

### 6. resirect
- 지정한 페이지로 redirect, response.sendRedirect()와 비슷
```jsp
<c:redirect url="리다이렉트할 URL">
    <c:param name="파라미터 이름" value="파라미터값"/>
</c:redirect>
```

### 7. 예외처리 
```jsp
<c:catch va="error">
	<%=2/0 %>
</c:catch>
<c:out value="${error}"/>
```


# 3. scope 에 대하여 설명하시오
- scope = **영역**을 말하는것으로 요청이 왔을 때 메모리에서 사라지는 시점에서 영역이 결정된다
- 종류 : page, session, request, application
>> 넓은 범위 순서 : application > session > request > page

```
- application : application이 종료되는 시점까지
- session : 지정한 시간이 만료되는 시점까지 
- page : 해당 페이지에서만 영역 설정
- request : 요청이 왔을 때 응답하기 전까지

★ 페이지 안에서 forword를 시키면 request는 클라이언트에게 응답하기 전이므로 출력된다 
단! redirect라면 클라이언트에게 다시 접근하라고 요청하는 순간 request는 메모리에서 사라진다
``` 


# 4. join의 종류에 대하여 설명하시오 
- cartesian prosuct : 모든 경우의 수를 계산해서 join 
- equl join : **동등 join**이라고 하며 join 대상이되는 2개 이상의 테이블에서 <br> **공통적으로 존재하는 컬럼의 값이 일치되는 행을 연결**하여 결과값을 출력 
- non equi join(비등가조인) : **공통적으로 존재하는 컬럼이 없는** 테이블에 대한 연결, <br> '='가 아닌 다른 조건으로 연산자를 수행한다
- self join : 자기자신에 대해 연결을 할때 사용(동일 테이블 사이의 연결)

# 5. 아래를 sql 문으로 처리 하시오
```sql
-- EMP테이블을 EMPLOTEE와 MANAGER로 별칭을 지정한 후 특정 사원의 매니저가 누구인지 알아내는 쿼리문

-- 사원 정보를 출력할 때, 각 사원이 소속된 부서의 상세 정보를 출력하기 위해 두 개의 테이블을 조인하는 쿼리문

--부서의 최대값과 최소값을 구하되, 최대 급여가 2900 이상인 부서만 출력하는 쿼리문

-- 부서별 사원의 수와 커미션을 받는 사원의 수를 계산하는 쿼리문

--소속 부서별 급여 총액과 평균 급여를 구하는 쿼리문
```

```sql
-- EMP테이블을 EMPLOTEE와 MANAGER로 별칭을 지정한 후 특정 사원의 매니저가 누구인지 알아내는 쿼리문
select EMPLOTEE.ename, MANAGER.ename from emp EMPLOTEE, emp MANAGER where EMPLOTEE.mgr = MANAGER.empno;

-- 사원 정보를 출력할 때, 각 사원이 소속된 부서의 상세 정보를 출력하기 위해 두 개의 테이블을 조인하는 쿼리문
select * from emp, dept where emp.deptno = dept.deptno;


--부서의 최대값과 최소값을 구하되, 최대 급여가 2900 이상인 부서만 출력하는 쿼리문
select deptno, min(sal), max(sal) from emp group by deptno having max(sal) >= 2900;

-- 부서별 사원의 수와 커미션을 받는 사원의 수를 계산하는 쿼리문 (다시)
select count(*), count(comm) from emp group by deptno;

--소속 부서별 급여 총액과 평균 급여를 구하는 쿼리문
select deptno, sum(sal), avg(sal) from emp group by deptno;
```

---
# Quiz 

## 1. 아래를 프로그래밍하시오 
### - 객체생성할것 
### - 조건 객체 밑 EL 또는 JSTL을 사용해 볼 것
<img src = "![Q1](https://user-images.githubusercontent.com/74290204/104168127-e31e8380-5440-11eb-9a54-a15e153575e5.PNG)" width="40%">

<img src = "![Q2222](https://user-images.githubusercontent.com/74290204/104168130-e44fb080-5440-11eb-8f38-444d57b20f88.PNG)" width="40%">

```jsp
<!--gradeInput.jsp-->

<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<h1>성적입력</h1>
	<form action="gradeOutput.jsp" method="get">
		<table border="1">
			<tr>
				<td colspan="2">
					학번
				</td>
				<td>
					<input type="text" name="sNum" size="10">
				</td>
	
			</tr>
				
			<tr>
				<td rowspan="4">과목</td>
			</tr>
			
			<tr>
				<td>JAVA</td>
				<td><input type="text" name="Java" size="10"></td>
			</tr>
			
			<tr>
				<td>Database</td> 
				<td><input type="text" name="Database" size="10"></td>
			</tr>
			<tr>
				<td>JSP</td> 
				<td><input type="text" name="JSP" size="10"></td>
			</tr>
			
			<tr>
				<td>
					<input type="submit" value="전송" >
				</td>
				
			</tr>
		</table>
	</form>
</body>
</html>
```
```jsp
<!-- gradeOutput-->

<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>

<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	
	<table border="1">
			<tr>
				<td colspan="2">
					학번
				</td>
				<td>
					${param.sNum}
				</td>
			</tr>
			
			<tr>
				<td rowspan="4">과목</td>
			</tr>
			
			<tr>
				<td>Java</td>
				 <td>${param.Java}</td>
			</tr>
			
			<tr>
				<td>Database</td>
				 <td>${param.Database}</td>
			</tr>
			
			<tr>
				<td>JSP</td>
				 <td>${param.JSP}</td>
			</tr>

			<tr>
				<td colspan="2">
					평균점수
				</td>
				<td>
					${(param.Java + param.Database + param.JSP) / 3.0}
				</td>
			</tr>

			<tr>
				<td>
					 <a href=gradeInput.jsp><input type="submit" value="입력화면" ></a>
				</td>
			</tr>
		</table>

</body>
</html>
```

## ▶ <font color="red">부족했던 부분!</color> <br>
<font color="white">

- 테이블 만들때 크기 조절해서 나눠주는것(row, colspan)
- EL에서 기본으로 제공하는 객체에는 request, param등이 있음 <br> 따라서 reqeust.Parameter를 따로 담아서 객체를 만들지 않아도 Parameter에 자동으로 담겨 사용 가능)

## 2. 아래를 프로그래밍하시오
<img src = "![q444](https://user-images.githubusercontent.com/74290204/104170003-c9327000-5443-11eb-88ce-f4e57d8b2810.PNG)" width="40%">

<img src="![q3](https://user-images.githubusercontent.com/74290204/104170006-cafc3380-5443-11eb-819c-1f602343d5af.PNG)" width="40%">
