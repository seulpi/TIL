# 1. 아래를 SQL문으로 표기 하시오
```sql
-- 부서 번호(DEPTNO)가 20인 사원에 관한 정보만 출력
--이름(ENAME)이 FORD인 사람의 사번(empno), 이름(ename), 급여(SAL)를 출력하는 쿼리문
--1982년 1월 1일 이후에 입사한 사원을 출력하는 쿼리문
--job 이 manage 이고 10번 부서인 사원
--job 이 manage 이거나 10번 부서인 사원
--10번 부서가 아닌 사원
--급여가 2000~3000 사이의 사원을 검색하는 쿼리문
--급여가 2000 미만이거나 3000 초과인 사원을 검색하는 쿼리문
--1987년에 입사한 사원을 출력하는 쿼리문
--커미션(COMM)이 300 혹은 500 혹은 1400인 사원이 있는지 검색하는 쿼리문
--커미션(COMM)이 300 혹은 500 혹은 1400이 아닌 사원이 있는지 검색하는 쿼리문
--이 F로 시작하는 사람을 찾는 쿼리문
--위치 상관 없이 이름 중에 A가 들어있는 사람을 찾는 쿼리문
--이름이 N으로 끝나는 사람을 찾는 쿼리문
--이름의 두 번째 글자가 A인 사원을 찾는 쿼리문
--이름의 세 번째 글자가 A인 사원을 찾는 쿼리문
--이름에 A를 포함하지 않는 사람만 검색하는 쿼리문

--사원들의 급여를 오름차순으로 정렬하는 쿼리문
--가장 최근에 입사한 사원부터 출력하는 쿼리문
--사원들이 소속되어 있는 부서의 번호를 출력하는 쿼리문

--아래와 같이 출력하시오.
SMITH is a CLERK
ALLEN is a SALESMAN
WARD is a SALESMAN
JONES is a MANAGER
MARTIN is a SALESMAN
BLAKE is a MANAGER
CLARK is a MANAGER
KING is a PRESIDENT
TURNER is a SALESMAN
JAMES is a CLERK
FORD is a ANALYST
MILLER is a CLERK
```

<details><summary> ANSWER! </summary>
	```sql
	-- 부서 번호(DEPTNO)가 20인 사원에 관한 정보만 출력
	select * from emp where deptno= 20;

	--이름(ENAME)이 FORD인 사람의 사번(empno), 이름(ename), 급여(SAL)를 출력하는 쿼리문
	select empno, ename, sal from emp where ename='FORD';

	--1982년 1월 1일 이후에 입사한 사원을 출력하는 쿼리문
	select * from emp where hiredate > '1982/01/01';

	--job 이 manage 이고 10번 부서인 사원
	select * from emp where job = 'MANAGER' and deptno=10;

	--job 이 manage 이거나 10번 부서인 사원
	select * from emp where job = 'MANAGER' or deptno=10;

	--10번 부서가 아닌 사원
	select * from emp where not deptno = 10;
	select * from emp where deptno <> 10;

	--급여가 2000~3000 사이의 사원을 검색하는 쿼리문
	select * from emp where sal >= 2000 and sal <= 3000;
	select * from emp where sal BETWEEN 2000 AND 3000;

	--급여가 2000 미만이거나 3000 초과인 사원을 검색하는 쿼리문
	select * from emp where sal < 2000 and sal > 3000;

	--1987년에 입사한 사원을 출력하는 쿼리문
	select * from emp where hiredate BETWEEN'1987/01/01' and '1987/01/01';

	--커미션(COMM)이 300 혹은 500 혹은 1400인 사원이 있는지 검색하는 쿼리문
	select * from emp where comm=300 or comm=500 or comm=1400;
	select * from emp where comm in(300, 500, 1400);

	--커미션(COMM)이 300 혹은 500 혹은 1400이 아닌 사원이 있는지 검색하는 쿼리문
	select * from emp where not comm in(300, 500, 1400);
	select * from emp where comm not in(300, 500, 1400);

	--이름이 F로 시작하는 사람을 찾는 쿼리문
	select * from emp where ename like 'F%';

	--위치 상관 없이 이름 중에 A가 들어있는 사람을 찾는 쿼리문
	select * from emp where ename like '%A%';

	--이름이 N으로 끝나는 사람을 찾는 쿼리문
	select * from emp where ename like '%N';

	--이름의 두 번째 글자가 A인 사원을 찾는 쿼리문
	select * from emp where ename like '_A%';

	--이름의 세 번째 글자가 A인 사원을 찾는 쿼리문
	select * from emp where ename like '__A%';

	--이름에 A를 포함하지 않는 사람만 검색하는 쿼리문
	select * from emp where ename not like '%A%';
	select * from emp where not ename like '%A%';

	--사원들의 급여를 오름차순으로 정렬하는 쿼리문
	select * from emp order by sal asc ;

	--가장 최근에 입사한 사원부터 출력하는 쿼리문
	select * from emp order by hiredate desc;

	--사원들이 소속되어 있는 부서의 번호를 출력하는 쿼리문
	select distinct deptno from emp;

	-- 아래와같이 출력하라(연결문)
	select ename || 'is a' || job from emp;
	```
</details>

---
# Quiz

## 1. 가위바위보 게임을 useBean을 활용하여(객체로 만들어서) 만드시오
<details><summary> ANSWER! </summary>
	
	```jsp
	//gameInput.jsp
	<%@ page language="java" contentType="text/html; charset=EUC-KR"
	    pageEncoding="EUC-KR"%>
	<!DOCTYPE html>
	<html>
	<head>
	<meta charset="EUC-KR">
	<title>Insert title here</title>
	</head>
	<body>
		<h1>가위바위보 게임</h1>
		<img src ="http://res.heraldm.com/content/image/2016/06/17/20160617000234_0.jpg" width="30%">
		<form action="gameObj.jsp">
			<select name ="user">
				<option value="1">가위</option>
				<option value="2">바위</option>
				<option value="3">보</option>
			</select>
			<input type="submit" value="제출">
		</form>
	</body>
	</html>
	```
	```jsp
	//gameObj.jsp
	<%@ page language="java" contentType="text/html; charset=EUC-KR"
	    pageEncoding="EUC-KR"%>
	<jsp:useBean id="gamerun" class="answer2.GameRun"/>
	<!DOCTYPE html>
	<html>
	<head>
	<meta charset="EUC-KR">
	<title>Insert title here</title>
	</head>
	<body>

		<%
			gamerun.setMyNum(Integer.parseInt(request.getParameter("user")));

			out.println("<h1>당신이 낸 것</h1>");

			if(gamerun.getMyNum() == 1) {
				out.println("<img src='https://apktada.com/storage/images/appinventor/ai_bbestest2/RSP_sjy_06/appinventor.ai_bbestest2.RSP_sjy_06_1.png'>");

			} else if(gamerun.getMyNum() == 2) {
				out.println("<img src='https://blog.kakaocdn.net/dn/dWscPZ/btqFnPyjcoJ/EkFxXpYN58Xaf0HuAMWpH0/img.png'>");
			} else {
				out.println("<img src='https://mnmsoft.co.kr/aivs/images/3.png'>");
			} 

			out.println("<br><h1>컴퓨터가 낸 것</h1>");

			if(gamerun.getComNum() == 1) {
				out.println("<br><img src='https://apktada.com/storage/images/appinventor/ai_bbestest2/RSP_sjy_06/appinventor.ai_bbestest2.RSP_sjy_06_1.png'>");
			} else if(gamerun.getComNum() == 2) {
				out.println("<br><img src='https://blog.kakaocdn.net/dn/dWscPZ/btqFnPyjcoJ/EkFxXpYN58Xaf0HuAMWpH0/img.png'>");
			} else {
				out.println("<br><img src='https://mnmsoft.co.kr/aivs/images/3.png'>");
			}

			if(gamerun.getComNum() == gamerun.getMyNum()) {
				out.println("<br><h1>무승부</h1>");
			} else if((gamerun.getComNum() == 1)&&(gamerun.getMyNum()==3)) { //comNum=가위 userNum=보 
				out.println("<br><h1>컴퓨터 승리</h1>");
			} else if((gamerun.getComNum() == 2)&&(gamerun.getMyNum()==1)) { // comNum=주먹 userNum=가위
				out.println("<br><h1>컴퓨터 승리</h1>");
			} else if((gamerun.getComNum() == 3)&&(gamerun.getMyNum()==2)) { //comNum=보 userNum=바위
				out.println("<br><h1>컴퓨터 승리</h1>");
			} else {
				out.println("<br><h1>유저 승리</h1>");
			}

		%>
		<a href="gameInput.jsp">다시 하기</a>
	</body>
	</html>
	```
	
	```java
	//GameRun.java
	package answer2;

	public class GameRun {

		int myNum;
		int comNum = (int)(Math.random()*3)+1 ;
		public int getMyNum() {
			return myNum;
		}

		public void setMyNum(int myNum) {
			this.myNum = myNum;
		}

		public int getComNum() {
			return comNum;
		}

		public void setComNum(int comNum) {
			this.comNum = comNum;
		}

	}
	```
</details>

## 2. 아래의 프로그램을 Employee 객체를 생성하여 아래를 만드시오
![q2](https://user-images.githubusercontent.com/74290204/103884434-f15b5f80-5121-11eb-9012-911261b7afde.PNG)

<details><summary> ANSWER! </summary>
	```java

	// Employee.java
	package answer2;

	public class Employee {

		private String empno, name, job, management, date, salary, comm, deptno;

		public Employee() {

		}

		public String getEmpno() {
			return empno;
		}

		public void setEmpno(String empno) {
			this.empno = empno;
		}

		public String getName() {
			return name;
		}

		public void setName(String name) {
			this.name = name;
		}

		public String getJob() {
			return job;
		}

		public void setJob(String job) {
			this.job = job;
		}

		public String getManagement() {
			return management;
		}

		public void setManagement(String management) {
			this.management = management;
		}

		public String getDate() {
			return date;
		}

		public void setDate(String date) {
			this.date = date;
		}

		public String getSalary() {
			return salary;
		}

		public void setSalary(String salary) {
			this.salary = salary;
		}

		public String getComm() {
			return comm;
		}

		public void setComm(String comm) {
			this.comm = comm;
		}

		public String getDeptno() {
			return deptno;
		}

		public void setDeptno(String deptno) {
			this.deptno = deptno;
		}

	}
	```
	```jsp
	//employee.jsp
	<%@ page language="java" contentType="text/html; charset=EUC-KR"
	    pageEncoding="EUC-KR"%>
	<%@ page import="java.sql.*" %>
	<jsp:useBean id="employee" class="answer2.Employee" />
	<!DOCTYPE html>
	<html>
	<head>
	<meta charset="EUC-KR">
	<title>Insert title here</title>
	</head>
	<body>

		<%! 
			Connection connection;
			Statement statement;
			ResultSet resultSet;

			String driver = "oracle.jdbc.driver.OracleDriver";
			String url = "jdbc:oracle:thin:@localhost:1521:xe";
			String uid = "scott";
			String upw = "tiger";
			String query = "select * from emp";
		%>

		<% 
			try {
				Class.forName(driver);
				connection = DriverManager.getConnection(url, uid, upw);
				statement = connection.createStatement();
				resultSet = statement.executeQuery(query);

				out.println("<table border='1'><tr>");
				out.println("<td>EMPNO</td>");
				out.println("<td>ENAME</td>");
				out.println("<td>JOB</td>");
				out.println("<td>MGR</td>");
				out.println("<td>HIREDATE</td>");
				out.println("<td>SAL</td>");
				out.println("<td>COMM</td>");
				out.println("<td>DEPTNO</td></tr>");

				while(resultSet.next()) {
					employee.setEmpno(resultSet.getString("EMPNO"));// set은 값을 변경하는거니까 true, get은 값 넣어져있는걸 가져오니까 false(넣은값이 없음))
					employee.setName(resultSet.getString("ENAME"));
					employee.setJob(resultSet.getString("JOB"));
					employee.setManagement(resultSet.getString("MGR"));
					employee.setDate(resultSet.getString("HIREDATE"));
					employee.setSalary(resultSet.getString("SAL"));
					employee.setComm(resultSet.getString("COMM"));
					employee.setDeptno(resultSet.getString("DEPTNO"));

					out.println("<tr>");

					if(employee.getEmpno()==null) {
						out.println("<td>"+ " " +"</td>");
					} else {
						out.println("<td>"+ employee.getEmpno()+"</td>");
					}

					if(employee.getName()==null) {
						out.println("<td>"+ " " +"</td>");
					} else {
						out.println("<td>"+employee.getName()+"</td>");
					}

					if(employee.getJob()==null) {
						out.println("<td>"+ " " +"</td>");
					} else {
						out.println("<td>"+employee.getJob()+"</td>");
					}

					if(employee.getManagement()==null) {
						out.println("<td>"+ " " +"</td>");
					} else {
						out.println("<td>"+employee.getManagement()+"</td>");
					}

					if(employee.getDate()==null) {
						out.println("<td>"+ " " +"</td>");
					} else {
						out.println("<td>"+employee.getDate()+"</td>");
					}

					if(employee.getSalary()==null) {
						out.println("<td>"+ " " +"</td>");
					} else {
						out.println("<td>"+employee.getSalary()+"</td>");
					}

					if(employee.getComm()==null) {
						out.println("<td>"+ " " +"</td>");
					} else {
						out.println("<td>"+employee.getComm()+"</td>");
					}

					if(employee.getDeptno()==null) {
						out.println("<td>"+ " " +"</td>");
					} else {
						out.println("<td>"+employee.getDeptno()+"</td>");
					}

					out.println("</tr>");

				}
				out.println("</table>");

			} catch(Exception e) {

			} finally {
				try {
					if(connection != null)
						connection.close();
					if(statement != null)
						statement.close();
					if(resultSet != null) 
						resultSet.close();
				} catch(Exception e) {
				}
			}	

		%>
	</body>
	</html>
	```
</details>

<details><summary> 모범 답안 click! </summary>
	
	```java
	//EmpVO class

	package edu.bit.ex.vo; //vo= value object의 약자

	import java.sql.Timestamp;

	public class EmpVO {

		private String ename;
		private int empno;
		private String job;
		private Timestamp date; // Timestamp 시간에 대한 타입

		public EmpVO() {

		}

		public EmpVO(String name, int empno, String job) {
			this.ename = name;
			this.empno = empno;
			this.job = job;
		}
		public String getEname() {
			return ename;
		}
		public void setEname(String ename) {
			this.ename = ename;
		}
		public int getEmpno() {
			return empno;
		}
		public void setEmpno(int empno) {
			this.empno = empno;
		}
		public String getJob() {
			return job;
		}
		public void setJob(String job) {
			this.job = job;
		}
		public Timestamp getDate() {
			return date;
		}
		public void setDate(Timestamp date) {
			this.date = date;
		}
	}
	```
	```java
	//EmpDVO.class

	package edu.bit.ex.dao; //dao : data access object의 약자

	import java.sql.Connection;
	import java.sql.DriverManager;
	import java.sql.ResultSet;
	import java.sql.Statement;
	import java.util.ArrayList;

	import edu.bit.ex.vo.EmpVO;

	public class EmpDAO { 
		private String url = "jdbc:oracle:thin:@localhost:1521:xe";
		private String uid = "scott";
		private String upw = "tiger";

		public EmpDAO() {
			try {
				Class.forName("oracle.jdbc.driver.OracleDriver");
			}catch(Exception e) {

			}
		}

		public ArrayList<EmpVO> empSelect() {
			ArrayList<EmpVO> dots = new ArrayList<EmpVO>(); // 가져오는 값을 list로 관리하겠다(row 하나 당 하나의 리스트에 담는것)

			Connection con = null;
			Statement stmt = null;
			ResultSet rs = null;
			String sql = "select * from emp order by ename"; //sql에는 가져오는 값을 오름차순으로 정렬해서 대입

			try {

				con = DriverManager.getConnection(url,uid,upw);
				stmt = con.createStatement();
				rs = stmt.executeQuery(sql);

				while(rs.next()) {
					String name = rs.getString("ename");
					int empno = rs.getInt("empno");
					String job = rs.getString("job");

					EmpVO eDto = new EmpVO(name, empno, job);
					dots.add(eDto); // 리스트를 참조하고 있는 객체에 들어오는 값을 저장

				}

			}catch(Exception e) {
				e.printStackTrace();
			}finally {
				  try {

				    if (rs != null) rs.close();
				    if(stmt != null) stmt.close();
				    if (con != null) con.close(); 

				 } catch (Exception e2) {
				    e2.printStackTrace();
				 }

			      }

			      return dots;
			}
		}
	```
	```jsp
	//empList.jsp

	<%@ page language="java" contentType="text/html; charset=EUC-KR"
	    pageEncoding="EUC-KR"%>
	<%@page import="edu.bit.ex.dao.EmpDAO"%>
	<%@page import="java.util.ArrayList"%>
	<%@page import="edu.bit.ex.vo.EmpVO"%>
	<%@page import="java.sql.*"%>
	<jsp:useBean id="eDao" class="edu.bit.ex.dao.EmpDAO" scope="page" />

	<!DOCTYPE html>
	<html>
	<head>
	<meta charset="EUC-KR">
	<title>Insert title here</title>
	</head>
	<body>
	<%
	   //EmpDAO eDao = new EmpDAO();   

	   ArrayList<EmpVO> dtos = eDao.empSelect(); // 객체 생성하고 empSelect함수 호출해서 dtos에 대입(값 불러온거 저장되어있는 것을 다시 불러오는것)
	   for (int i = 0; i < dtos.size() ; i++){
	      EmpVO vo = dtos.get(i); 

	      out.println("이름: "+vo.getEname()+", 번호: "+vo.getEmpno()+"<br/>");
	   }

	%>
	</body>
	</html>
	```
</details>
