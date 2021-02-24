# 스프링부트
## @ 개념 
- 스프링과 똑같은데 스프링'부트'는 **기존에 있던 스프링의 단점을 개선**시킴 
<br> 스프링 프레임워크를 사용하는 프로젝트를 **간편하게 설정할 수 있는 스프링 프레임워크의 서브 프로젝트**
<br>

## @ 설정
-  https://start.spring.io사이트에 다운로드 받아서 관리하는 것 (예전보다는 pom.xml을 쉽게 관리가 가능한것)
![spring_boot_설정3](https://user-images.githubusercontent.com/74290204/108806921-5e4d8a80-75e6-11eb-9cd6-082feb593b55.PNG)

- 우클릭 → new → Spring Starter Project 
- Java Version : 스프링 부트 기반으로 8버전 이상 
- Packaging : jar로 해도 되지만 기본적으로 **웹에 대해서는 War로 설정**
    >> jar: java 실행 프로그램 <br> war : 톰캣같은 웹서버 위에서 돌아가는 프로젝트

- Spring Boot Version : 2버전 이상이면 ok 
    - avaliable에서 필요한 기능 검색해서 선택하면 기능 추가 가능하지만 선생님피셜로는 써보니 pom.xml에서 뜯어 고쳐야한다고함 
    - why? 버전도 잘 안맞고 해서 다시 정리하고 복붙해주는게 낫다고함 (그래도 lombok, myBatis는 잘 불러와서 잘맞는다함)
    - 나중에 dependency 추가도 가능!
    >> 우클릭 → Spring → Add starters

	![spring_boot_설정1](https://user-images.githubusercontent.com/74290204/108806617-88527d00-75e5-11eb-854d-f59874ba091d.PNG)

	![spring_boot_설정2](https://user-images.githubusercontent.com/74290204/108806620-8983aa00-75e5-11eb-9755-bf83ea2271f4.PNG)
<br>

## @ 특징
1. **단독으로 실행이 가능**한 스프링 애플리케이션을 생성
- @SpringBootApplication : 자기 설정을 읽어들이는 어노테이션 
![스프링부트3](https://user-images.githubusercontent.com/74290204/108807436-c2bd1980-75e7-11eb-8657-7fc0b0ad3f64.PNG)


2. **톰캣 내장** (새로 다운로드 받을 필요가X)
- 서버를 돌릴 때 legacy는 context명이 충돌될까봐 하나씩 돌렸는데 톰캣이 각 프로젝트마다 내장 되어있기 때문에 context명이 의미가 없어짐
    - legacy도 컨텍스트명이 다르면 몇개를 돌려도 상관없음


3. 기본 설정되어있는 starter 컴포넌트를 제공 
4. 설정을 위한 xml 코드를 요구하지X → 단, xml으로 설정했던 설정들을 자바코드로 만들어줘야함 
- web.xml도 없어짐 

![스프링부트1](https://user-images.githubusercontent.com/74290204/108807226-2abf3000-75e7-11eb-9cf2-ee89a4e04b45.PNG)

![스프링부트2](https://user-images.githubusercontent.com/74290204/108807325-7376e900-75e7-11eb-9c57-1beabebead7d.PNG)

5. 스프링부트는 jsp를 권장하지X , tymeleaf를 추천함 
- tymeleaf 사용
    - ① html 추가 
    ```xml
    <html lang="ko" xmlns:th="http://www.thymeleaf.org">은 html문서언어는 한글이며 타임리프를 사용하겠다는 말
    ```
    - ② dependency 추가 
    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>
    ``` 
    - 타임리프엔진에서는 th를 사용, th:text는 텍스트를 출력한다는 속성으로서 th:text="${변수이름}을 입력하면 해당 변수의 값이 text로 출력이 된다
    
    ```html
    <p th:text="${test}"></p>
    ```

6. 폴더 경로
- static : 이미지같은 리소스 (resources) 파일 
- templete : timeleaf(앞에 th붙은 것) 파일 
![static,templete](https://user-images.githubusercontent.com/74290204/108811027-cb195280-75ef-11eb-95f6-32364db54612.PNG)

## @ 스프링부트에서 tymeleaf말고 jsp 사용
- jsp, jstl(지원x) 사용하려면 라이브러리 다운 받아야함 / pom.xml 에 추가 
```xml
<dependency>
    <groupId>org.apache.tomcat.embed</groupId>
    <artifactId>tomcat-embed-jasper</artifactId>
</dependency>

<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>jstl</artifactId>
</dependency>
```

- application.properties 에 추가
```xml
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp
server.jsp-servlet.init-parameters.development=true
```

- controller: @RestController가 아닌 @Controller 사용
```java
@Controller
public class HomeController {
	
	@RequestMapping("/")
	@ResponseBody
	public String home() {
		return "Hello, Spring boot!";
	}
	
	@GetMapping("/board")
	public String board() {
		return "list";
	}

}
```

- list.jsp (WEP-INF / views 폴더도 직접 만들어야함)
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ page session="false"%>
<html>
<head>
<title>spring boot</title>
</head>
<body>
this is list.jsp
	
</body>
</html>
```

---

## Q. Hello World를 찍어봅시다~!
### 1. class 생성
```java
package edu.bit.ex;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController // 스프링 부트에서는 @RestController가 기본
public class HomeController {
	
	@RequestMapping("/")
	public String home() {
		return "Hello, Spring boot!"; // view 설정 X return "<h1>Hello, Spring boot!<h1>"; 이렇게도 사용 가능  
	}
}
```

### 2. application.properties에 설정
- port 번호 지정(서버 port)
```
#server port number
server.port = 8282
```
![application](https://user-images.githubusercontent.com/74290204/108807945-db79ff00-75e8-11eb-8eb5-b8b0d16e7cd5.PNG)

- DB설정도 여기서 설정
```
#datasource (oracle)
spring.datasource.driver-class-name=oracle.jdbc.driver.OracleDriver
spring.datasource.url=jdbc:oracle:thin:@localhost:1521/xe

#spring.datasource.driver-class-name=net.sf.log4jdbc.sql.jdbcapi.DriverSpy
#spring.datasource.url=jdbc:log4jdbc:oracle:thin:@localhost:1521/xe

spring.datasource.username=scott
spring.datasource.password=tiger 
```

### 3. pom.xml 설정
- pom.xml에 DB관련 repository, dependency 추가
    - ※ [위치주의] repository는 properties 밑에, 오라클 드라이버 dependency 안 맨 윗줄
```xml
<repositories>
	<repository>
		<id>oracle</id>
		<url>http://www.datanucleus.org/downloads/maven2/</url>
	</repository>
</repositories>

<!-- 오라클 JDBC 드라이버 -->
<dependency>
	<groupId>oracle</groupId>
	<artifactId>ojdbc6</artifactId>
	<version>11.2.0.3</version>
</dependency>
```

- ! 설정안하면 나는 에러

![url 오라클 설정](https://user-images.githubusercontent.com/74290204/108808262-91454d80-75e9-11eb-8863-bc03da09dc3f.PNG) 


### 4. 서버 실행
- run as → run on server가 아닌 spring boot app으로 run! 

### ▶ 출력
- 콘솔에서 이미 port번호가 사용되어있다고 떴는데 주소를 그냥 직접치니까 결과가 나옴 그리고 그 이후에는 잘 돌아감

![화면 출력](https://user-images.githubusercontent.com/74290204/108808689-7cb58500-75ea-11eb-8085-568a7956f7b9.PNG)


## @ tymeleaf 
- 템플릿 엔진으로 프레임워크의 한 종류
>> [프레임워크 종류] jsp / velocity / tymeleaf / vue.js ...etc

- 스프링 부트는 디폴트로 tymeleaf를 사용, tymeleaf의 기본 확장자는 html
```jsp
기본 형태 <html xmlns:th="http://www.thymeleaf.org">
```
- dependency 추가 
```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

### - 활용
- tymeleaf와 jsp를 혼용해서 같이 써보기 위한 설정(실무에서는 절대 이렇게 안함)
```
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp
spring.thymeleaf.view-names=thymeleaf/*
```
▶ 타임리프의 디폴트 폴더는 templates 따라서 tymeleaf라는 폴더를 만들어줘야함 <br>
![타임리프](https://user-images.githubusercontent.com/74290204/108933994-74635580-768f-11eb-93bf-48a6af3adc0c.PNG)


### - 문법
1. 변수식 : ${ }
2. 선택식 : *{ }
```java
@Controller
public class HomeController {
	
	@RequestMapping("/")
	public String home(Model model) {
		
		BoardVO board = new BoardVO();
		board.setBContent("컨텐츠");
		board.setBTitle("타이틀");
		board.setBName("홍길동");
		
		model.addAttribute("board", board);
		
		return "thymeleaf/index"; //타임리프로 넘긴다 temlates안에 tymeleaf폴더에 index파일
	}
}
```
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
<title>Title</title>
</head>
<body>
    <h1>Hello World</h1>
    <!-- $로 변수를 가져오고 *를 통해서 선택적 변수를 가져옴  -->
    <p th:object="${board}"></p> <!-- object는 태그안에 dom객체 만들때 board라는 객체를 같이 올리는것 p태그에 -->
	<p th:text="${board.bName}"></p> <!-- text는 <p>bname</p>을 가져오는 것  -->
	<p th:text="*{bContent}"></p> 
	<!-- *는 어디에서 가지고 오는 변수인지를 명시해줘야하는데 이렇게만 주면 어디에서 오는지 모르기때문에 뿌리지않음
		▶ 선택자는 부모자식관계에서 사용하거나 해당 변수명을 명시해줘야 사용가능하다-->
		
  <!-- 	해결①
  	<p th:text="*{board.bContent}"></p> or 
 	 	
 	 	해결② 자식부모관계를 만들어준것 
  	<div th:object="${board}">
  		<p th:text="${board.bName}"></p>
  		<p th:text="*{bContent}"></p> 
  	</div>  
  -->
</body>
</html>
```

![1](https://user-images.githubusercontent.com/74290204/108941483-8ac1df00-7698-11eb-9ac6-ecfcf086503b.PNG)

3. 메세지식 : #{ }
```html
<h1 th:text="#{content.id}"></h1>
<h1 th:text="#{content.name}"></h1>
```

![message](https://user-images.githubusercontent.com/74290204/108953428-830d3500-76ae-11eb-84a0-7519165183a4.PNG)
<br>

▶출력

![22222](https://user-images.githubusercontent.com/74290204/108953462-915b5100-76ae-11eb-9896-c837651cd147.PNG)


4. 링크식(경로) : @{ }
- 링크식의 디폴트는 static 폴더 따라서 위에 링크는 static/css/index.css 표현
```html
<link th:href="@{/css/index.css}" rel="stylesheet" type="text/css">
```

## @ 스프링부트 테스트 
- 통합 테스트 : 우리쪽에서는 controller test(mok을 활용한 test)
- 시스템 테스트 (부하테스트) : 실제로 서버에서 동위접속사 몇명인지 대략적으로 계산하여 동일한 환경을 만들어서 테스트 하는 것
```java
package edu.bit.ex;

import static org.assertj.core.api.Assertions.assertThat;
import static org.junit.jupiter.api.Assertions.assertEquals;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import javax.sql.DataSource;

import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit.jupiter.SpringExtension;

import edu.bit.ex.mapper.BoardMapper;
import edu.bit.ex.vo.BoardVO;
import lombok.extern.slf4j.Slf4j;


@Slf4j
@ExtendWith(SpringExtension.class)
//@RunWith(SpringRunner.class) test도 버전을 타기 때문에 @RunWith는 4ver 5ver은 @ExtendWith(SpringExtension.class) 사용해서 test
@SpringBootTest
class BoardTest {

	@Autowired
	private BoardMapper mapper;
	
	@Autowired
	private DataSource ds;
	
	@Test
	public void testDataSource() {
		System.out.println("DS=" + ds);
		
		try (Connection conn = ds.getConnection()){
			System.out.println("Cooooon=" + conn);
			
			assertThat(conn).isInstanceOf(Connection.class);
			assertEquals(100, getLong(conn, "select 100 from dual"));
		}catch (Exception e) {
			e.printStackTrace();
		}
	}

	private long getLong(Connection conn, String sql) {

		long result = 0;
		
	    try (Statement stmt = conn.createStatement()) {
	         ResultSet rs = stmt.executeQuery(sql);
	         if (rs.next()) {
	            result = rs.getLong(1);
	         }
	         rs.close();
	      } catch (Exception e) {
	         e.printStackTrace();
	      }

		return result;
	}
	
	@Test
	   public void testGetList() {

	      System.out.println(mapper);
	      System.out.println(mapper.getList().size());
	      
	      for (BoardVO vo : mapper.getList()) {
	         log.info(vo.getBName());
	         System.out.println(vo.getBName());
	      }   

	   } 
}
```
