# 커넥션풀(Connection Pool)

## @ 배경 
다수의 request → 서버 과부하 → **서버 다운** when? 오라클에 접속할 때 'connetion = DriverManager.getConnection(url, uid, upw);' <br> (요청받을때 다운되는 게 아님) → **해결방안으로 커넥션 풀 이용** 

## @ 동작 원리 
1. 서버 쪽에 Connection 객체를 **미리 여러개 만들어서** Pool이라는 저장공간에 저장한다 <br>
    -  갯수를 미리 만든다(=request에 **갯수를 제한**한다) 
2. 제한시킨 갯수를 초과하면 그 이후부터 들어오는 request는 대기조 → 웹브라우저 하나가 방을 빼면 그 공간에 대기하던 request가 입장 <br>
    - 순서대로 나가고 들어가는 원리

## @ 사용 방법 
- Servers / context.xml에 connection 정의 *(위치가 중요)*

![Connection Pool](https://user-images.githubusercontent.com/74290204/104257141-12280a00-54c0-11eb-8037-22b545551354.PNG)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
  Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
--><!-- The contents of this file will be loaded for each web application --><Context>

    <!-- Default set of monitored resources. If one of these changes, the    -->
    <!-- web application will be reloaded.                                   -->
    <WatchedResource>WEB-INF/web.xml</WatchedResource>
    <WatchedResource>WEB-INF/tomcat-web.xml</WatchedResource>
    <WatchedResource>${catalina.base}/conf/web.xml</WatchedResource>

    <!-- Uncomment this to disable session persistence across Tomcat restarts -->
    <!--
    <Manager pathname="" />
    -->
    
    <Resource 
       auth="Container" 
       driverClassName="oracle.jdbc.OracleDriver" 
       maxIdle="10" 
       maxTotal="20" 
       maxWaitMillis="-1" 
       name="jdbc/oracle" 
       password="tiger" 
       type="javax.sql.DataSource" 
       url="jdbc:oracle:thin:@127.0.0.1:1521:xe" 
       username="scott"
    />
   
   <!--
    auth : 컨테이너를 자원 관리자로 기술
    name : JDBC이름, 변경 가능
    driverClassName : JDBC 드라이버
    type : 웹에서 이 리소스를 사용할 때 DataSource로 리턴됨
    username : 접속계정
    password : 접속할 계정 비밀번호
    
    loginTimeout : 연결 끊어지는 시간
    maxActive : 최대 연결 가능한 Connection수 (기본 20개)
    maxIdle : Connection pool 유지를 위해 최대 대기 connection 숫자
    maxWait : 사용 가능한 커넥션이 없을 때 커넥션 회수를 기다리는 시간 (1000 = 1초)
    testOnBorrow : db에 test를 해볼 것인지
   -->
    
</Context>
```

---
# ★★★ MVC모델을 통한 게시판 만들기 ★★★
## @ MVC(Model View Controller)
- Model : 데이터베이스와의 관계를 담당, 데이터베이스 or 데이터를 다루는 부분 
- View : 사용자한테 보여지는 UI화면, jsp&html 같이 화면 꾸미는 것 
- Controller : 비즈니스 로직(흐름의 순서, if-els문 같은 것)

![model2](https://user-images.githubusercontent.com/74290204/104268393-24617280-54d7-11eb-8072-ef59525b9b16.PNG)

### ▶ 자세한 MVC설명 'Quiz/jsp&DB_Answer02.md' 참조 
##### https://github.com/Lee-sb92/TIL/blob/main/Quiz/jsp%26DB_Answer02.md
<br>
<br>

## @ 게시판을 만들어보자!
### 1. Data Base 생성 (글 리스트)

```sql
bid NUMBER(4), PRIMARY KEY, 전체 테이블에서 식별할 수 있는 Key(key속성을 주는거라서 중복되지 않음, 중복하면 에러남)
bName 작성자
bTitle 글제목
bContent 글내용
bDate 글작성날짜
bHit 조회수
-------------------------------------------------------
>> ★ 댓글에 관한 컬럼들
bGroup : 최초원글에 대한 그룹 , 원글 번호 
bStep : 원글에 대해 세로로 몇번째인지는 나타냄
bIndent : 가로로 몇번째인지 나타냄 (들여쓰기같은것)
```



```sql
create table mvc_board ( 
bId NUMBER(4) PRIMARY KEY,
bName VARCHAR2(20),
bTitle VARCHAR2(100),
bContent VARCHAR2(300),
bDate DATE DEFAULT SYSDATE,
bHit NUMBER(4) DEFAULT 0, 
bGroup NUMBER(4),
bStep NUMBER(4),
bIndent NUMBER(4));

create sequence mvc_board_seq; --숫자를 자동으로 한개씩 증가시켜주기 위한 쿼리문 

commit; -- commit안해주면 외부에서 반영할때 제대로 안보여짐 

--insert 
insert into mvc_board(bId, bName, bTitle, bContent, bHit, bGroup, bStep, bIndent)--해당 컬럼이름
values (mvc_board_seq.nextval, 'abcd', 'is title', 'its content', 0, mvc_board_seq.currval, 0, 0);
-- nextval: 다음번호(자동으로 한개씩 증가), currval: 현재 번호 
values (2, 'abcd', 'is title', 'its content', 0, mvc_board_seq.currval, 0, 0);
-- error why? bId는 primary Key로 지정했는데 중복되기 때문에 에러

-- insert를 또 추가 
insert into mvc_board(bId, bName, bTitle, bContent, bHit, bGroup, bStep, bIndent)--해당 컬럼이름
values (mvc_board_seq.nextval, 'abcd', 'is title', 'its content', 0, mvc_board_seq.currval, 0, 0);
>> 원글을 하나 더 생성한거기 때문에 bGroup에 번호는 2
```
![table 생성](https://user-images.githubusercontent.com/74290204/104261647-58359b80-54c9-11eb-964f-d4428989d9a2.PNG)

![table 생성2](https://user-images.githubusercontent.com/74290204/104261707-80bd9580-54c9-11eb-99b2-90d047b340bf.PNG)

![table insert](https://user-images.githubusercontent.com/74290204/104261638-54097e00-54c9-11eb-809d-90d2ad33a520.PNG)


### 2. Controller 생성 : controller는 servlet으로!

```java
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
import edu.bit.ex.command.BReplyCommand;
import edu.bit.ex.command.BReplyViewCommand;
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
	    	// System.out.println()은 콘솔 출력
	    
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
	    	}else if(com.equals("/reply_view.do")){ 
	    		command = new BReplyViewCommand(); 
	        	command.execute(request, response); 
	        	viewPage = "reply_view.jsp"; 
	    	}else if(com.equals("/reply.do")){ 
	    	// 댓글작성하는건 write와 똑같은데 답변에는 group, step, indent가 있으니 그걸 계산해서 하는게 차이(계산하는 sql문 암기)
	    		command = new BReplyCommand(); 
	        	command.execute(request, response); 
	        	viewPage = "list.do"; 
	    	}
	    
	    	RequestDispatcher dispatcher = request.getRequestDispatcher(viewPage);
	   	dispatcher.forward(request, response);
	   	/* 위 두문장이 servlet에서 forword해주는 문장! jsp에서는 액션태그를 이용해서 forward
	    	 왜 forward를 하는가? request를 살려서 view단으로 넘겨서 보여줘야 하기 때문임 (따라서 문장은 그대로 암기!!)*/
	}
	
}
```

### 3. Command 생성 
```java
// command Interface
package edu.bit.ex.command;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public interface BCommand {
	abstract void execute(HttpServletRequest request, HttpServletResponse response);
}
```

```java
//BListCommand
package edu.bit.ex.command;

import java.util.ArrayList;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import edu.bit.ex.dao.BDao;
import edu.bit.ex.dto.BDto;

public class BListCommand implements BCommand {

	@Override
	public void execute(HttpServletRequest request, HttpServletResponse response) {
		
		BDao dao = new BDao();
		
		ArrayList<BDto> dtos = dao.list(); // BDto에 는 list 함수 호출 
		request.setAttribute("list", dtos); //list란 이름으로 request 객체에 담는다
	}
}
```
```java
//BWriteCommand
package edu.bit.ex.command;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import edu.bit.ex.dao.BDao;

public class BWriteCommand implements BCommand {

	@Override
	public void execute(HttpServletRequest request, HttpServletResponse response) {
		String bName = request.getParameter("bName");
		String bTitle = request.getParameter("bTitle");
		String bContent = request.getParameter("bContent");
		
		BDao dao = new BDao();
		dao.write(bName, bTitle, bContent);
	}
}
```
```java
//BContentCommand
package edu.bit.ex.command;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import edu.bit.ex.dao.BDao;
import edu.bit.ex.dto.BDto;

public class BContentCommand implements BCommand {

	@Override
	public void execute(HttpServletRequest request, HttpServletResponse response) {
		String bId = request.getParameter("bId");

		BDao dao = new BDao();
		BDto dto = dao.contentView(bId);
		
		request.setAttribute("content_view", dto);
	}
}
```
```java
//BReplyViewCommand
package edu.bit.ex.command;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import edu.bit.ex.dao.BDao;
import edu.bit.ex.dto.BDto;

public class BReplyViewCommand implements BCommand {

	@Override
	public void execute(HttpServletRequest request, HttpServletResponse response) {
		String bId = request.getParameter("bId");
		BDao dao = new BDao();
		BDto dto = dao.reply_view(bId);
		
		request.setAttribute("reply_view", dto);			
	}
}
```
```java
//BReplyCommand
package edu.bit.ex.command;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import edu.bit.ex.dao.BDao;

public class BReplyCommand implements BCommand {

	@Override
	public void execute(HttpServletRequest request, HttpServletResponse response) {
		String bId = request.getParameter("bId");
	      	String bName = request.getParameter("bName");
	     	String bTitle = request.getParameter("bTitle");
	      	String bContent = request.getParameter("bContent");
	      	// 위에는 원글에 대한 정보 (write할때는 저것만 있어도됨)
	     	String bGroup = request.getParameter("bGroup");
	      	String bStep = request.getParameter("bStep");
	     	String bIndent = request.getParameter("bIndent");
	      	//댓글에 필요한 요소(hidden으로 숨겨진 요소들을 받아야하기때문에 선언)
	      
	     	BDao dao = new BDao();
	      	dao.reply(bId, bName, bTitle, bContent, bGroup, bStep, bIndent);
	}
}
```
```java
//BModifyCommand
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
		dao.modify(bId, bName, bTitle, bContent);
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

### 4. DAO(Data Access Object) 생성 
```java
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
		upHit(bId);
		
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
	       
	       String query = "insert into mvc_board(bId, bName, bTitle, bContent, bDate, bHit, bGroup, bStep, bIndent) values (mvc_board_seq.nextval, ?, ?, ?, sysdate, 0, mvc_board_seq.currval, 0, 0 )"; 
	       
	       preparedStatement = connection.prepareStatement(query);
	       preparedStatement.setString(1,bName);
	       preparedStatement.setString(2,bTitle);
	       preparedStatement.setString(3,bContent);
	      
	       int rn = preparedStatement.executeUpdate(); // 업데이트하는게 성공하면 처리된 레코드 수를 반환
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
	       // 위에 String으로 받아도됨 단, 여기서 Integer로 바꿔줘야함
	       
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

	public void modify(String bId, String bName, String bTitle, String bContent) {
		// write랑 똑같은것! 수정만 하는거기때문에
	    Connection connection = null;
	    PreparedStatement preparedStatement = null;
	    
	    try {
	       connection = dataSource.getConnection();
	       
	       String query = "update mvc_board set bName=?, bTitle=?, bContent=? where bId =?"; 
	       
	       preparedStatement = connection.prepareStatement(query);
	       preparedStatement.setString(1, bName);
	       preparedStatement.setString(2, bTitle);
	       preparedStatement.setString(3, bContent);
	       preparedStatement.setString(4, bId);
	       
	       int rn = preparedStatement.executeUpdate();
	       
	    }catch (Exception e) {
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

	public BDto reply_view(String bId) {
		
		BDto dtos = null;
		
	    Connection connection = null;
	    PreparedStatement preparedStatement = null;
	    ResultSet resultSet = null;
	    
	    try {
	       connection = dataSource.getConnection();
	       
	       String query = "select * from mvc_board where bId = ?"; 
	       
	       preparedStatement = connection.prepareStatement(query);
	       preparedStatement.setInt(1, Integer.parseInt(bId)); 
	       
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

	public void reply(String bId, String bName, String bTitle, String bContent, String bGroup, String bStep,
			String bIndent) {
	//reply는 insert로 처리한 후 보여줄 필요 없으니까 insert만 처리해주면됨!	
	    replyShape(bGroup, bStep); // 다른 댓글이 최신으로 달리면 달려있던 댓글 밀어내는 함수
	    Connection connection = null;
	    PreparedStatement preparedStatement = null;
	    
	    try {
	       connection = dataSource.getConnection();
	       
	       String query = "insert into mvc_board (bId, bName, bTitle, bContent, bGroup, bStep, bIndent) values (mvc_board_seq.nextval, ?, ?, ?, ?, ?, ?)";
	       preparedStatement = connection.prepareStatement(query);
	       
           preparedStatement.setString(1, bName);
           preparedStatement.setString(2, bTitle);
           preparedStatement.setString(3, bContent);
           preparedStatement.setInt(4, Integer.parseInt(bGroup));
           preparedStatement.setInt(5, Integer.parseInt(bStep) + 1); // 밀어낸 후 본인 번호 매기는것
           preparedStatement.setInt(6, Integer.parseInt(bIndent) + 1);
           
           int rn = preparedStatement.executeUpdate();
           /* execute : 실행한다의 뜻이니까 이걸해야 DB가 실행되는것!!!
           int rn으로 변수 선언한게 디버깅용 rn 값에 따라서 잘들어갔는지를 확인하는 용 */
           System.out.println(rn);
	       
	
	    } catch (Exception e) {
	       e.printStackTrace();
	    } 
	}
	
	private void replyShape(String strGroup, String strStep) {
		// 답변 정렬을 위한 함수 
		Connection connection = null;
		PreparedStatement preparedStatement = null;
		      
		try {
			connection = dataSource.getConnection();
			String query = "update mvc_board set bStep = bStep + 1 where bGroup = ? and bStep > ?";
			/* 원본글이 1인 상태에서 step들이 다 1이면 정렬이 안되기 때문에(:시간순으로 정렬해줘야함)
			+1을 해줌으로써 정렬을 시킨다 (개념은 다 1이 맞음) 따라서 step은 세로로 계단순서대로 정렬해준다라고 생각해도 됨*/
			
			preparedStatement = connection.prepareStatement(query);
			preparedStatement.setInt(1, Integer.parseInt(strGroup));
			preparedStatement.setInt(2, Integer.parseInt(strStep));
		         
			int rn = preparedStatement.executeUpdate();
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

	public void upHit(String bId) {
		Connection connection = null;
		PreparedStatement preparedStatement = null;
		
		try {
			connection = dataSource.getConnection();
			String query = "update mvc_board set bHit = bHit+1 where bId=?";
			
			preparedStatement = connection.prepareStatement(query);
			preparedStatement.setString(1, bId);
			
			int rs = preparedStatement.executeUpdate();
			
		} catch(Exception e) {
			e.printStackTrace();
		} finally {
			try {
				if(connection != null) {
					connection.close();
				}if(preparedStatement != null) {
					preparedStatement.close();
				}
			} catch(Exception e) {
				e.printStackTrace();
			}
		}
		
	}
}

```

### 5. DTO(Data Transfer Object) 생성 
```java
package edu.bit.ex.dto;

import java.sql.Timestamp;

/*bId NUMBER(4) PRIMARY KEY,
bName VARCHAR2(20),
bTitle VARCHAR2(100),
bContent VARCHAR2(300),
bDate DATE DEFAULT SYSDATE,
bHit NUMBER(4) DEFAULT 0, 
bGroup NUMBER(4),
bStep NUMBER(4),
bIndent NUMBER(4));
*/

public class BDto {

	   int bId;
	   String bName;
	   String bTitle;
	   String bContent;
	   Timestamp bDate;
	   int bHit;
	   int bGroup;
	   int bStep;
	   int bIndent;
	   
	   public BDto() {
	    
	   }
	   
	   public BDto(int bId, String bName, String bTitle, String bContent, Timestamp bDate, int bHit, int bGroup, int bStep, int bIndent) {
	      
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

### 6. View 생성 
```jsp
<!--list.jsp-->

<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
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
      <!-- .setAtribute()해서 forward한거에 대해서는 request.해서 함수 호출할 필요가X 
      why? 메모리에 이미 있기 때문에 .setAtribute()는 BListCommand에서 선언함
      
      * jsp는 html을 동적으로 만듬(프로그램요소[:전체가 java문법] 를 만든다) 
      → 웹브라우저는 java언어를 해석하지 못하기 때문에 응답받을때는 웹브라우저가 알아듣는 언어로 만들어줘야함(html, 자바스크립트)
      → el,jstl도 자바문법이기 때문에 웹브라우저에 보내기전에 톰캣에서 자동으로 변환시켜줌-->
      <tr>
         <td>${dto.bId}</td> <!-- bID호출 -->
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
//content_view.jsp

<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
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
         <tr >
            <td colspan="2"> <input type="submit" value="수정"> &nbsp;&nbsp; <a href="list.do">목록보기</a> &nbsp;&nbsp; <a href="delete.do?bId=${content_view.bId}">삭제</a> &nbsp;&nbsp; <a href="reply_view.do?bId=${content_view.bId}">답변</a></td>
         </tr>
      </form>
   </table>
   
</body>
</html>
```
```jsp
//reply_view.jsp

<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=EUC-KR">
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
```jsp
//write_view.jsp

<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=EUC-KR">
<title>Insert title here</title>
</head>
<body>

   <table width="500" cellpadding="0" cellspacing="0" border="1">
      <form action="write.do" method="post">
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
