# 문법 3종 세트  Conncion / Statement / ResultSet
- 제조사마다 구현(java는 interface만 제공) <br>
*→ why? DB는 제조사마다 다르기 때문에 제조사마다 구현해야하는 건 너무나 당연한 것*

>> SQL(Structed Query Language) : RDBMS의 데이터를 관리하기 위해 설계된 특수 목적의 프로그래밍 언어 

## 1. Connection 
#### : DriverManager클래스의 getConnection()메소드를 실행함으로써 정의되며 DB와 연결된 session역할을 한다 
- 이 session을 이용하여 DB의 SQL을 전송하고 그 결과인 ResultSet 객체를 얻는다

## 2. Statement 
### : Statement의 객체는 Connection 인터페이스와 createStatement()메소드를 이용하여 생성되며 DB에 SQL문을 보내기 위한 작업과 <br> 실제 SQL을 실행하여 결과 값을 반환하는 기능을 제공한다 
- Connction 객체의 연결 정보를 가져오기 때문에 Connection객체가 먼전 존재해야함 
- SQL문을 전송하고 실행할 수 있는 객체를 생성한다 , 삽입&수정&삭제&검색을 처리하는 DML문을 사용할 때 이 인터페이스를 사용
>> DML(Data Manipulation Language) : 정의된 DB에 입력된 레코드를 조회 or 수정, 삭제하는 등의 역할을 하는 언어
##### - DML 종류
- exexcuteUpdate(): 테이블 내용 변경
- insert : 추가(삽입)
- delete : 삭제
- update : 수정
- select : 조회 

#### - JDBC 드라이버가 쿼리를 실행할 수 있도록 제공하는 함수
>> query란 DB에 정보를 요청하는 것 

1.  execute() : 모든 유형의 SQL문과 함께 사용할 수 있음(select, insert, update, delete, ddl문 모두 실행 가능) 
- boolen 값을 반환하다 <br>
▶ *반환값이 true* : getResultSet()메소드를 사용해 결과의 집합을 얻을 수 있음! <br>
▶ *반환값 false* : 업데이트 개수 or 결과가 없는 경우

2.  executeUpdate () : DB에 데이터를 select문 제외한 추가, 삭제, 수정하는 SQL문을 실행하는 함수
- 메서드의 반환 값은 해당 SQL문 실행에 영향을 받는 행 수를 반환 


3. executeQuery() : DB에 데이터를 가져와서 결과 집합을 반환하는 함수, DB에 명령
- ResultSet 객체에 결과값을 담을 수 있음
- **select문에서만 실행할 수 있음** 

## 3. ResultSet
### : Statement 객체로 select문을 사용하요 얻어온 레코드 값들을 테이블의 형태로 갖게 되는 객체 
- select문을 통해 데이터를 가져온다면 ResultSet 객체에 그 데이터를 저장해야함
- next() : 다음 행으로 커서를 이동한다 (다음 행이 없으면 false 반환)

```jsp
while(rs.next())
BOF의 위치에 커서가 있음 
true가되면 첫번째 row로 이동

EOF의 위치 : 마지막에 값이 없으면 커서가 EOF로 이동
```

#### - Oracle 이클립스와 연결 문법과 원리
```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>

<%@page import="java.sql.*"%> <!-- java.sql에 있는 모든 라이브러리 임포트 -->

<%! 
	Connection connection;
	Statement statement; // Statement = Query용 객체
	ResultSet resultSet; // ResultSet =  table의 결과 값 저장하는 객체

	String driver = "oracle.jdbc.driver.OracleDriver";
	String url = "jdbc:oracle:thin:@localhost:1521:xe"; 
    // //"jdbc:oracle:thin(15버전에서 제공하는 기본문구, 버전마다 다름):@IP:오라클port번호:xe"; 
	String uid = "scott";
	String upw = "tiger";
	String query = "select * from emp";  // quety에 table에서 가져오는 값을 대입
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<%
		/* try catch 문은 무조건 써야함(문법임) */
		try {
			Class.forName(driver); /* Class명이 Class이고 static , driver 는 위에 oracle 드라이버 
									따라서 이 문장은 oracle driver를 메모리에 올린다는 뜻 / OracleDriver 객체 생성, 로딩한다고 표현(sqldeveloper-lib에 있음)
                                    이 문구를 올려야지 3종세트에 대한 자식들의 구현이 실행된다 
                                    
                                    왜 DriverManager를 메모리에 올리는가? 
                                    3종세트에 대한 내용이 DriverManager에 들어있기 때문에 
                                    메모리에 올려야 3종세트에 대한 사용이 가능하다 */
			connection = DriverManager.getConnection(url,uid,upw); // 내가 가지고 있는 DB와 커넥션하는 문장, 채팅처럼 연결되어있는 상태
			statement = connection.createStatement(); /* Statement객체의 정보를 Connection에서 가져오는것
			                                                Query란 DB에 정보를 요청하는 것 
                                                            쿼리는 웹 서버에 특정한 정보를 보여달라는 웹 클라이언트 요청(주로 문자열을 기반으로 한 요청이다)에 의한 처리
                                                            쿼리는 대개 데이터베이스로부터 특정한 주제어나 어귀를 찾기 위해 사용된다 */

			resultSet = statement.executeQuery(query);
			
			while(resultSet.next()) { //정보가 있으면 while문 타고 없으면 빠져나와라

				String name = resultSet.getString("ename");
				
				out.println("이름: " + name + " 월급: " + salary + "<br>");
			}
			
		}catch(Exception e) {
			
		}finally { // try 문 안에 위에 실행이 다 끝나면 닫아주는것(close = 메모리 해제해라) → 안하면 에러날수있음
			try {
				if(resultSet != null) 
					resultSet.close();
				
				if(statement != null) 
					statement.close();
				if(connection != null)
					connection.close();
			} catch(Exception e) {
				
			}
		}
	
	%>
</body>
</html> 
```

