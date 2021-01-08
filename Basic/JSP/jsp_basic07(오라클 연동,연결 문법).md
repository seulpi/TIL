# JDBC(Java Date Base Connetion) : DB관련 라이브러리 
: java 프로그램에서 SQL문을 실행하여 데이터를 관리하기 위한 JAVA API
>> API(Application Programming Interface) : 응용 프로그램 프로그래밍 인터페이스, 
<br> 응용 프로그램에서 사용할 수 있도록 운영체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있께 만든 인터페이스
<br>*애플리케이션 소프트웨어를 구축하고 통합하기 위한 정의 및 프로토콜 세트*

- 특징 : 다양한 데이터 베이스에 대해서 별도의 프로그램을 만들 필요가 없음 <br>
        해당 DB의 JDBC를 이용하면 하나의 프로그램으로 DB관리가 가능하다(제조사별로 라이브러리 제공)
>> Oracle은 설치하면 자동으로 Oracle JDBC를 설치하고 이클립스에 해당 클래스 파일을 복사해서 사용하면 된다!


# Oracle Driver 연결 방법! (문법 암기)
## JDBC class path 복사해서 연결하는 방법 
### 1. class path 복사하는 방법

![경로](https://user-images.githubusercontent.com/74290204/103858325-754e2100-50fb-11eb-99c5-4929ab3af5e5.PNG)

![붙여넣을 경로](https://user-images.githubusercontent.com/74290204/103858332-78491180-50fb-11eb-81f5-0283942a0c8d.PNG)

### 2. 이클립스 실행한 후 연결 
```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>

<%@page import="java.sql.*"%> <!-- java.sql에 있는 모든 라이브러리 임포트 -->

<%! 
	Connection connection;
	Statement statement;
	ResultSet resultSet;
	
/* 	여기서 철자 하나 틀리면 끝장남..왠만하면 ctrl c + ctrl V 할 것 */ 
	String driver = "oracle.jdbc.driver.OracleDriver";
	String url = "jdbc:oracle:thin:@localhost:1521:xe";
	String uid = "scott";
	String upw = "tiger";
	String query = "select * from emp"; /* emp 안에 12개 들어있음 */
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
									따라서 이 문장은 oracle driver를 메모리에 올린다는 뜻 */
			connection = DriverManager.getConnection(url,uid,upw);
			statement = connection.createStatement();
			resultSet = statement.executeQuery(query);
			
			while(resultSet.next()) {
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
<br>

▶ 출력
![connection](https://user-images.githubusercontent.com/74290204/103859693-d37c0380-50fd-11eb-80a6-9bc315bb176d.PNG)
