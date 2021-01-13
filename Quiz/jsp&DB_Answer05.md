# 1. 게시판 삭제와 업데이트(수정)를 구현하시오
```java
// BFrontController(servlet)

package edu.bit.ex.controller;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import edu.bit.ex.command.BCommand;
import edu.bit.ex.command.BContentCommand;
import edu.bit.ex.command.BDeleteCommand;
import edu.bit.ex.command.BListCommand;
import edu.bit.ex.command.BModifyCommand;
import edu.bit.ex.command.BWriteCommand;

/**
 * Servlet implementation class FrontController
 */
@WebServlet("*.do") //앞에 뭘 붙이던 끝에 .do로 오는 모든걸 받겠다
public class BFrontController extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public BFrontController() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("doGet");
		actionDo(request, response);
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("doPost");
		actionDo(request, response);
	}
	
	private void actionDo(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("actionDo");
		
		request.setCharacterEncoding("EUC-KR"); // 한글처리할때 에러 → throws ServletException, IOException로 처리
		
		String viewPage = null;
		BCommand command = null; 
		
		String uri = request.getRequestURI();
	    String conPath = request.getContextPath();
	    String com = uri.substring(conPath.length()); // subString은 길이를 지정해준 지점부터 문자를 읽어들여서 출력 
	      
	    System.out.println(uri); // /jsp_mvcBoard/list.do
	    System.out.println(conPath);// /jsp_mvcBoard
	    System.out.println(com);// /list.do
	    //   System.out.println()은 콘솔 출력
	    
	    if(com.equals("/list.do")){
	    	command = new BListCommand(); // 다형성적용, 부모는 자식(BCommand인터페이스로 부모, BListCommand 자식-구현)
	        command.execute(request, response); //BListCommand에 있는 함수 호출(구현한부분)
	        viewPage = "list.jsp"; // list.do가 실행되면 보여줄 viewPage
	    }else if(com.equals("/content_view.do")){ // 걸려있는 링크 처리
	    	command = new BContentCommand(); 
	        command.execute(request, response); 
	        viewPage = "content_view.jsp"; 
	    }else if(com.equals("/write_view.do")){ 
	        viewPage = "write_view.jsp"; 
	    }else if(com.equals("/write.do")){ 
	    	command = new BWriteCommand(); 
	        command.execute(request, response); 
	        viewPage = "list.do"; 
	    }else if(com.equals("/delete.do")){ 
	    	command = new BDeleteCommand(); 
	        command.execute(request, response); 
	        viewPage = "list.do"; 
	    }else if(com.equals("/modify.do")){ 
	    	command = new BModifyCommand(); 
	        command.execute(request, response); 
	        viewPage = "list.do"; 
	    }      
	    RequestDispatcher dispatcher = request.getRequestDispatcher(viewPage);
	    dispatcher.forward(request, response);
	    /* 위 두문장이 servlet에서 forword해주는 문장! jsp에서는 액션태그를 이용해서 forward
	     왜 forward를 하는가? request를 살려서 view단으로 넘겨서 보여줘야 하기 때문임 (따라서 문장은 그대로 암기!!)
	    */
	}
}
```
```java
//BDao

package edu.bit.ex.dao;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.Timestamp;
import java.util.ArrayList;

import javax.naming.Context; // naming으로 import!!!!!
import javax.naming.InitialContext;
import javax.sql.DataSource; // 항상 import할때 조심!! sql임

import edu.bit.ex.dto.BDto;

public class BDao {
	private DataSource dataSource; //Connection Pool , 커네션 풀은 DataSource다
	
	public BDao() {
		
		try {
			Context context = new InitialContext(); // Context는 해당 서버 안에 있는 context.xml을 읽어들이는 객체
			dataSource = (DataSource) context.lookup("java:comp/env/jdbc/oracle"); //lookup=찾는다, jdbc/oracle은 context.xml에 name과 반드시 맞춰줘야함
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	public ArrayList<BDto> list() { // DB에 있는것을 객체화시켜서 가져온 것(Data Transfer Object), 레코드를 하나씩 리스트로 저장한다
	    
	    ArrayList<BDto> dtos = new ArrayList<BDto>(); //BDto도 패키지달라서 임포트해줘야됨
	    Connection connection = null;
	    PreparedStatement preparedStatement = null;
	    ResultSet resultSet = null;
	    
	    try {
	       connection = dataSource.getConnection();//DriverManager가 아닌 dataSource - 커넥션풀에서 객체를 가져올거기 때문에
	       
	       String query = "select bId, bName, bTitle, bContent, bDate, bHit, bGroup, bStep, bIndent from mvc_board order by bGroup desc, bStep asc";
	       // order by bGroup desc, bStep asc 안해주면 댓글이 정렬이 안됨
	       preparedStatement = connection.prepareStatement(query);
	       resultSet = preparedStatement.executeQuery();
	       
	       while (resultSet.next()) {
	          int bId = resultSet.getInt("bId");
	          String bName = resultSet.getString("bName");
	          String bTitle = resultSet.getString("bTitle");
	          String bContent = resultSet.getString("bContent");
	          Timestamp bDate = resultSet.getTimestamp("bDate");
	          int bHit = resultSet.getInt("bHit");
	          int bGroup = resultSet.getInt("bGroup");
	          int bStep = resultSet.getInt("bStep");
	          int bIndent = resultSet.getInt("bIndent");
	          
	          BDto dto = new BDto(bId, bName, bTitle, bContent, bDate, bHit, bGroup, bStep, bIndent);
	          //DB에서 가져온 레코드들을 BDto에 값을 저장하기 위해 객체선언하고 값을 저장한다
	          dtos.add(dto);
	          // 값들을 AraayList에 하나씩 더해서 dto 출력하면 테이블 화면 구현이된다
	       }
	       
	    } catch (Exception e) {
	       e.printStackTrace();
	    } finally {
	       try {
	          if(resultSet != null) resultSet.close();
	          if(preparedStatement != null) preparedStatement.close();
	          if(connection != null) connection.close();
	       } catch (Exception e2) {
	          e2.printStackTrace();
	       }
	    }
	    return dtos;

	 }

	public BDto contentView(String bId) {
		BDto dtos = null;
		
	    Connection connection = null;
	    PreparedStatement preparedStatement = null;
	    ResultSet resultSet = null;
	    
	    try {
	       connection = dataSource.getConnection();
	       
	       String query = "select * from mvc_board where bId = ?"; // ?는 PreparedStatement의 변수처리를 위한 문법 (PreparedStatement는 자바문법)
	       
	       preparedStatement = connection.prepareStatement(query);
	       preparedStatement.setInt(1, Integer.parseInt(bId)); // 1번째 물음표에는 bId 들어오는 값 넣어라 
	       
	       resultSet = preparedStatement.executeQuery();
	       
	       while (resultSet.next()) {
	          int id = resultSet.getInt("bId");
	          String bName = resultSet.getString("bName");
	          String bTitle = resultSet.getString("bTitle");
	          String bContent = resultSet.getString("bContent");
	          Timestamp bDate = resultSet.getTimestamp("bDate");
	          int bHit = resultSet.getInt("bHit");
	          int bGroup = resultSet.getInt("bGroup");
	          int bStep = resultSet.getInt("bStep");
	          int bIndent = resultSet.getInt("bIndent");
	          
	          dtos = new BDto(id, bName, bTitle, bContent, bDate, bHit, bGroup, bStep, bIndent);
	          
	       }
	       
	    } catch (Exception e) {
	       e.printStackTrace();
	    } finally {
	       try {
	          if(resultSet != null) resultSet.close();
	          if(preparedStatement != null) preparedStatement.close();
	          if(connection != null) connection.close();
	       } catch (Exception e2) {
	          e2.printStackTrace();
	       }
	    }
	    return dtos;
	
	}

	public void write(String bName, String bTitle, String bContent) {
		
	    Connection connection = null;
	    PreparedStatement preparedStatement = null;
	  
	    
	    try {
	       connection = dataSource.getConnection();
	       
	       String query = "insert into mvc_board (bId, bName, bTitle, bContent, bHit, bGroup, bStep, bIndent) values (mvc_board_seq.nextval, ?, ?, ?, 0, mvc_board_seq.currval, 0, 0 )"; 
	       
	       preparedStatement = connection.prepareStatement(query);
	       preparedStatement.setString(1,bName);
	       preparedStatement.setString(2,bTitle);
	       preparedStatement.setString(3,bContent);
	      
	       int rn = preparedStatement.executeUpdate(); // 업데이트하는게 성공하면 rn은 0 으로 리턴 
	       System.out.println("insert 결과: " + rn); // 디버깅용(업데이트 잘됐는지 확인)
	       
	    } catch (Exception e) {
	       e.printStackTrace();
	    } finally {
	       try {
	     
	          if(preparedStatement != null) preparedStatement.close();
	          if(connection != null) connection.close();
	       } catch (Exception e2) {
	          e2.printStackTrace();
	       }
	    }

	}

	public void delete(int bId) {

	    Connection connection = null;
	    PreparedStatement preparedStatement = null;
	    
	  
	    
	    try {
	       connection = dataSource.getConnection();
	       
	       String query = "delete from mvc_board where bId=?"; 
	       preparedStatement = connection.prepareStatement(query);
	       preparedStatement.setInt(1, bId);
	       
	       int rn = preparedStatement.executeUpdate();
	       System.out.println("delete 성공! " + rn);
	       
	      
	    } catch (Exception e) {
	       e.printStackTrace();
	    } finally {
	       try {
	     
	          if(preparedStatement != null) preparedStatement.close();
	          if(connection != null) connection.close();
	       } catch (Exception e2) {
	          e2.printStackTrace();
	       }
	    }
		
	}

	public BDto modify(String bId, String bName, String bTitle, String bContent) {
		BDto dtos = null;
		
	    Connection connection = null;
	    PreparedStatement preparedStatement = null;
	    ResultSet resultSet = null;
	    
	    try {
	       connection = dataSource.getConnection();
	       
	       String query = "update mvc_board set bName=?, bTitle=?, bContent=? where bId =?"; 
	       
	       preparedStatement = connection.prepareStatement(query);
	       preparedStatement.setString(1, bName);
	       preparedStatement.setString(2, bTitle);
	       preparedStatement.setString(3, bContent);
	       preparedStatement.setString(4, bId);
	       
	       resultSet = preparedStatement.executeQuery();
	       
	       while (resultSet.next()) {
	          int id = resultSet.getInt("bId");
	          String name = resultSet.getString("bName");
	          String title = resultSet.getString("bTitle");
	          String content = resultSet.getString("bContent");
	          Timestamp bDate = resultSet.getTimestamp("bDate");
	          int bHit = resultSet.getInt("bHit");
	          int bGroup = resultSet.getInt("bGroup");
	          int bStep = resultSet.getInt("bStep");
	          int bIndent = resultSet.getInt("bIndent");
	          
	          dtos = new BDto(id, name, title, content, bDate, bHit, bGroup, bStep, bIndent);
	          
	       }
	       
	    } catch (Exception e) {
	       e.printStackTrace();
	    } finally {
	       try {
	          if(resultSet != null) resultSet.close();
	          if(preparedStatement != null) preparedStatement.close();
	          if(connection != null) connection.close();
	       } catch (Exception e2) {
	          e2.printStackTrace();
	       }
	    }
	    return dtos;
	
	}
}
```
```java
//BDeleteCommand

package edu.bit.ex.command;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import edu.bit.ex.dao.BDao;

public class BDeleteCommand implements BCommand {

	@Override
	public void execute(HttpServletRequest request, HttpServletResponse response) {
		int id = Integer.parseInt(request.getParameter("bId"));
		BDao dao = new BDao();
		dao.delete(id);

	}

}
```
```java
//BModifyCommmand

package edu.bit.ex.command;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import edu.bit.ex.dao.BDao;
import edu.bit.ex.dto.BDto;

public class BModifyCommand implements BCommand {

	@Override
	public void execute(HttpServletRequest request, HttpServletResponse response) {
		
		String bId = request.getParameter("bId");
		String bName = request.getParameter("bName");
		String bTitle = request.getParameter("bTitle");
		String bContent = request.getParameter("bContent");
		
		BDao dao = new BDao();
		BDto dto = dao.modify(bId, bName, bTitle, bContent);
		request.setAttribute("modify", dto);

	}
}
```
<br>

# 2. 아래를 sql 문으로 나타 내시오
```sql
-- 1> 부서테이블의 모든 데이터를 출력하라.
-- 2> EMP테이블에서 각 사원의 직업, 사원번호, 이름, 입사일을 출력하라.
-- 3> EMP테이블에서 직업을 출력하되, 각 항목(ROW)가 중복되지 않게 출력하라.
-- 4> 급여가 2850 이상인 사원의 이름 및 급여를 표시하는 출력하라.
-- 5> 사원번호가 7566인 사원의 이름 및 부서번호를 표시하는 출력하라.
-- 6> 급여가 1500이상 ~ 2850이하의 범위에 속하지 않는 모든 사원의 이름 및 급여를 출력하라.
/* 7> 1981년 2월 20일 ~ 1981년 5월 1일에 입사한 사원의 이름,직업 및 입사일을 출력하라. 
(입사일을 기준으로 해서 오름차순으로 정렬) */
-- 8> 10번 및 30번 부서에 속하는 모든 사원의 이름과 부서 번호를 출력하되, 이름을 알파벳순으로 정렬하여 출력하라.
/* 9> 10번 및 30번 부서에 속하는 모든 사원 중 급여가 1500을 넘는 사원의 이름 및 급여를 출력하라. 
(단 컬럼명을 각각 employee 및 Monthly Salary로 지정하시오) */
-- 10> 관리자가 없는 모든 사원의 이름 및 직위를 출력하라.
/* 11> 커미션을 받는 모든 사원의 이름, 급여 및 커미션을 출력하되, 
급여를 기준으로 내림차순으로 정렬하여 출력하라.*/
-- 12> 이름의 세 번째 문자가 A인 모든 사원의 이름을 출력하라.
-- 13> 이름에 L이 두 번 들어가며 부서 30에 속해있는 사원의 이름을 출력하라.
/* 14> 직업이 Clerk 또는 Analyst 이면서 급여가 1000,3000,5000 이 아닌 
모든 사원의 이름, 직업 및 급여를 출력하라. */
/* 15> 사원번호, 이름, 급여 그리고 15%인상된 급여를 정수로 표시하되 
컬럼명을 New Salary로 지정하여 출력하라.*/
/* 16> 15번 문제와 동일한 데이타에서 급여 인상분(새 급여에서 이전 급여를 뺀 값)을 추가해서 출력하라
(컬럼명은 Increase로 하라) */
/* 17> 모든 사원의 이름(첫 글자는 대문자로, 나머지 글자는 소문자로 표시) 및 이름 길이를 표시하는 쿼리를 작성하고 
컬럼 별칭은 적당히 넣어서 출력하라. */
-- 18> 사원의 이름과 커미션을 출력하되, 커미션이 책정되지 않은 사원의 커미션은 'no commission'으로 출력하라.
-- 19> 모든 사원의 이름,부서번호,부서이름을 표시하는 질의를 작성하라.(DECODE)
```

```sql
-- 1> 부서테이블의 모든 데이터를 출력하라.
select emp.*, dept.dname, dept.loc from emp, dept where emp.deptno = dept.deptno;

-- 2> EMP테이블에서 각 사원의 직업, 사원번호, 이름, 입사일을 출력하라.
select job, empno, ename, hiredate from emp;

-- 3> EMP테이블에서 직업을 출력하되, 각 항목(ROW)가 중복되지 않게 출력하라.
select distinct(job) from emp;
select job from emp group by job;

-- 4> 급여가 2850 이상인 사원의 이름 및 급여를 표시하는 출력하라.
select ename, sal from emp where sal>=2850;

-- 5> 사원번호가 7566인 사원의 이름 및 부서번호를 표시하는 출력하라.
select ename, empno from emp where empno=7566;

-- 6> 급여가 1500이상 ~ 2850이하의 범위에 속하지 않는 모든 사원의 이름 및 급여를 출력하라.
select ename, sal from emp where sal between 1500 and 2850;

/* 7> 1981년 2월 20일 ~ 1981년 5월 1일에 입사한 사원의 이름,직업 및 입사일을 출력하라. 
(입사일을 기준으로 해서 오름차순으로 정렬) */
select ename, job, hiredate from emp where hiredate between '1981/02/20' and '1981/05/01' order by hiredate asc;

-- 8> 10번 및 30번 부서에 속하는 모든 사원의 이름과 부서 번호를 출력하되, 이름을 알파벳순으로 정렬하여 출력하라.
select ename, empno from emp where deptno=10 or deptno=30 order by ename asc;

/* 9> 10번 및 30번 부서에 속하는 모든 사원 중 급여가 1500을 넘는 사원의 이름 및 급여를 출력하라. 
(단 컬럼명을 각각 employee 및 Monthly Salary로 지정하시오) */
select ename as "employee", sal as "Monthly Salary" from emp where deptno in(10,30) and sal>1500;

-- 10> 관리자가 없는 모든 사원의 이름 및 직위를 출력하라.
select ename, job from emp where mgr is null; 

/* 11> 커미션을 받는 모든 사원의 이름, 급여 및 커미션을 출력하되, 
급여를 기준으로 내림차순으로 정렬하여 출력하라.*/
select ename, sal, comm from emp where comm is not null order by sal desc;

-- 12> 이름의 세 번째 문자가 A인 모든 사원의 이름을 출력하라.
select ename from emp where ename like '__A%';

-- 13> 이름에 L이 두 번 들어가며 부서 30에 속해있는 사원의 이름을 출력하라.
select ename from emp where ename like '%L%L%' and deptno=30;

/* 14> 직업이 Clerk 또는 Analyst 이면서 급여가 1000,3000,5000 이 아닌 
모든 사원의 이름, 직업 및 급여를 출력하라. */
select ename, job, sal from emp where job in('Clerk', 'Analyst') and sal not in(1000, 3000, 5000);

/* 15> 사원번호, 이름, 급여 그리고 15%인상된 급여를 정수로 표시하되 
컬럼명을 New Salary로 지정하여 출력하라.*/
select empno, ename, sal, floor(sal + sal*0.15) as "New Salary" from emp;
select empno, ename, sal, round(sal + sal*0.15) as "New Salary" from emp;
→ floor는 소수점 버리기 / round는 반올림 

/* 16> 15번 문제와 동일한 데이타에서 급여 인상분(새 급여에서 이전 급여를 뺀 값)을 추가해서 출력하라
(컬럼명은 Increase로 하라) */
select empno, ename, sal, floor(sal + sal*0.15) as "New Salary", floor((sal+sal*0.15)-sal) as "Increase" from emp;

/* 17> 모든 사원의 이름(첫 글자는 대문자로, 나머지 글자는 소문자로 표시) 및 이름 길이를 표시하는 쿼리를 작성하고 
컬럼 별칭은 적당히 넣어서 출력하라. */
select initcap(ename) as "첫글자만 대문자", length(ename) as "이름의 길이" from emp;

-- 18> 사원의 이름과 커미션을 출력하되, 커미션이 책정되지 않은 사원의 커미션은 'no commission'으로 출력하라. 
select ename, nvl(to_char(comm), 'no commission')from emp ;

-- 19> 모든 사원의 이름,부서번호,부서이름을 표시하는 질의를 작성하라.(DECODE) 
문제 이해 못함
```
