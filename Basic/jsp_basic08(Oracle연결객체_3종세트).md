# 문법 3종 세트  Conncion / Statement / ResultSet
- 제조사마다 구현(java는 interface만 제공) <br>
*→ why? DB는 제조사마다 다르기 때문에 제조사마다 구현해야하는 건 너무나 당연한 것*

## 1. Connection 
#### : DriverManager클래스의 getConnection()메소드를 실행함으로써 정의되면 DB와 연결된 session역할을 한다 
- 이 session을 이용하여 DB의 SQL을 전송하고 그 결과인 ResultSet 객체를 얻는다

## 2. Statement 
- exexcuteUpdate(): 테이블 내용 변경
- insert : 추가 
- delete 
- update

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

- #####
while(rs.next())
BOF의 위치에 커서가 있음 
true가되면 첫번째 row로 이동

EOF의 위치 : 마지막에 값이 없으면 커서가 EOF로 이동
