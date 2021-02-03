# JUit이란
## Java Testing Framework : 자바프로그래밍 언어용 유닛 테스트 프레임워크
- 창시자 : Erich Gamma (eclips 창시자)
>> [공식 사이트] https://junit.org/junit5/

# TDD (Test-Driven Development) 
## : 테스트 주도, 테스트를 바로 하는 것
![TDD](https://user-images.githubusercontent.com/74290204/106723634-a82bfc00-664a-11eb-8196-48c789a4ea5f.png)

### - REFACTORING : 에러나면 다시 수정하는 것
<br>

>>[선생님 책 추천]
![책추천](https://user-images.githubusercontent.com/74290204/106425146-c527ca00-64a6-11eb-9931-476c9ddeaad9.PNG)

# - 단위테스트 : 함수 or 클래스 테스트 
## @ pom.xml 설정 추가
```xml
<!-- Test -->
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.12</version>
			<scope>test</scope>
		</dependency>
```

## @ 테스트 방법 
### 1. 테스트 할 함수를 위한 junit 생성 
![testunit](https://user-images.githubusercontent.com/74290204/106546018-ca8c1f80-654d-11eb-99be-3b659b4757d5.PNG)
<br>

### 2. 테스트 코드 넣고 단위테스트 진행 
![testunit2](https://user-images.githubusercontent.com/74290204/106546023-ccee7980-654d-11eb-875a-6f2fafaaabd0.PNG)

```java
//Calculatror.java 
package edu.bit.ex.calculator;

public class Calculator {
	/* TDD
	 * 1. 함수를 만든다
	2. 함수에 대해 바로 test */
	public int sum(int num1, int num2) {
		return num1+num2;
	}
	
	public int sub(int num1, int num2) {
		return num1-num2;
	}
}
```
```java
// CalculatorTest.java
package edu.bit.ex.calculator;

import static org.junit.Assert.*;

import org.junit.Test;
import org.springframework.beans.factory.annotation.Autowired;

public class CalculatorTest {
	
	Calculator cal;
	
	@Test
	public void testCalculator() { 
		assertNotNull(cal); // 참조형의 default는 null이기 때문에 현재 null상태
	}

	// test안에 test할 함수는 여러개 가능 but, 하나라도 false면 빨간색뜸
	@Test
	public void testSum() { //sum함수에 대한 test
		Calculator cal = new Calculator();
		int result = cal.sum(10, 20);
		assertEquals(30, result, 10); // 30: 예상값, result : 실제값  , 10: 오차범위
		System.out.println(result); // syso으로 콘솔로 결과값 확인도 가능
	}
	
	@Test
	public void testSub() { 
		Calculator cal = new Calculator();
		int result = cal.sub(10, 20);
		assertEquals(-10, result); 
	}
}
```

## @ Spring에서의 JUnit
### 1. Connection Pool Test 
- 플젝할때 PPT에 테스트 진행 **반드시** 있어야 하는 것 : **DataSource, VO, Service, Mapper** 
- **Spring**은 반드시 **IOC컨테이너 기반에서 테스트가 진행**되어야한다

>> [IOC컨테이너를 만들어주는 Annotation] @RunWith(SpringRunner.class)

```java
package edu.bit.ex.database;

import static org.junit.Assert.*;

import javax.inject.Inject;
import javax.sql.DataSource;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringRunner;

import lombok.extern.log4j.Log4j;

@RunWith(SpringRunner.class) //1. IOC 컨테이너 만들어주기
@ContextConfiguration("file:src/main/webapp/WEB-INF/spring/root-context.xml") // 2. 커넥션풀 연결해주는 경로 명시
@Log4j // 3. src/test/resource 여기에 log4j.xml지우고 src/main/resource에 있는 log4j.xml과 log~properties 복붙 (테스트환경과 진짜 환경을 맞추는것)
public class ConnectionTest {
	
	@Inject  // 4. 주입 @Aurowired써도 됨 
	DataSource dataSource;
	
	@Test
	public void testDataSource() {
		log.info("나와라 뽕" + dataSource);
		assertNotNull(dataSource);
	}
}
```
### ▶ test돌렸을때 초록불 나와야 환경설정이 잘된 것
![111](https://user-images.githubusercontent.com/74290204/106547474-9e25d280-6550-11eb-86e0-35cf0f053ffc.PNG)

### 2. Mapper Test
```java
package edu.bit.ex.mapper;

import static org.junit.Assert.*;

import java.util.List;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringRunner;

import edu.bit.ex.board.mapper.BMapper;
import edu.bit.ex.board.vo.BoardVO;
import lombok.Setter;
import lombok.extern.log4j.Log4j;

@RunWith(SpringRunner.class)
@ContextConfiguration("file:src/main/webapp/WEB-INF/spring/root-context.xml")
@Log4j
public class BoardMapperTest {
	
	@Setter(onMethod_ = @Autowired) //setter함수만들어서 @Autowired(주입)해라
	private BMapper boardMapper;

	@Test
	public void testBoardMapper() {
		assertNotNull(boardMapper);
		
	}
	
	@Test
	public void testList() {
		List<BoardVO> list = boardMapper.list();
		log.info(boardMapper);
		
		for(BoardVO boardVO : list) {
			log.info(boardVO.getbContent());
		}
	}

}
```

### 3. Service Test
```java
package edu.bit.ex.board.service;

import static org.junit.Assert.*;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.context.web.WebAppConfiguration;

import edu.bit.ex.board.service.BService;
import lombok.extern.log4j.Log4j;

@RunWith(SpringRunner.class)
@WebAppConfiguration
@ContextConfiguration("file:src/main/webapp/WEB-INF/spring/root-context.xml")
@Log4j
public class BoardServiceTest {

	@Autowired
	private BService boardService;

	@Test
	public void testBService() {
		assertNotNull(boardService);
	}
}
```

### 4. ★ Controller Test (이게 제일 중요!)
```java
package edu.bit.ex.board.contrroller;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.forwardedUrl;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import org.springframework.test.context.web.WebAppConfiguration;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;
import org.springframework.web.context.WebApplicationContext;

import lombok.Setter;
import lombok.extern.log4j.Log4j;

@RunWith(SpringJUnit4ClassRunner.class)
@WebAppConfiguration // @RunWith, @WebAppConfiguration -> webCotext.xml만드는 어노테이션
// WebApplicationContext 때문에 있어야됨 (WebApplicationContext받아오려고 @WebAppConfiguration 사용)
@ContextConfiguration({"file:src/main/webapp/WEB-INF/spring/root-context.xml",
		"file:src/main/webapp/WEB-INF/spring/appServlet/servlet-context.xml" })
@Log4j
public class ControllerTest {

	@Setter(onMethod_ = { @Autowired })
	private WebApplicationContext ctx; 

	private MockMvc mockMvc;
	/*
	 * MockMvc란? mvc를 테스트하기 위해 만든 객체 , 실제 객체와 비슷하지만 테스트에 필요한 기능만 가지는 가짜 객체를 만들어서
	 * 애플리케이션 서버에 배포하지 않고도 스프링 MVC 동작을 재현할 수 있는 클래스를 의미
	 */

	@Before // 테스트 초기화
	public void setup() { // MockMvc객체 생성
		this.mockMvc = MockMvcBuilders.webAppContextSetup(ctx).build();
	}

	@Test
	public void testList() throws Exception {
		mockMvc.perform(get("/board/list")).andExpect(status().isOk())// 응답 검증, 경로 잘 맞춰줘야함
				.andDo(print()).andExpect(forwardedUrl("/WEB-INF/views/list.jsp")); // view 경로
	}

}
```

