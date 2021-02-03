# 1. 아래 요구사항 분석을 하고 설계 하시오
![개념절설계문제](https://user-images.githubusercontent.com/74290204/106716557-ef61bf00-6641-11eb-8b1d-8a6007d084bd.PNG)

## ▶ *Answer* 
### 초기 설계 ①
![개념적설계 초기 설계1](https://user-images.githubusercontent.com/74290204/106716695-17512280-6642-11eb-8489-71c42316b832.jpg)

### 초기 설계 ②
![한빛마트 설계 사진1](https://user-images.githubusercontent.com/74290204/106716805-38b20e80-6642-11eb-995b-aab2b44c4e5c.jpg)

# 2. DB 설계의 순서는?
### ① 요구 사항 및 분석 
### ② 개념적 설계
- Entity 추출 : 요구사항을 기준으로 테이블이 될만한 것 추출 (명사가 될만한 것들로 추출)
- Entity 간의 관계 설정 : 동사를 포함하는 문장 속에서 관계를 설정
- Attribute 추출 : 유형 정리 (pk, fk, 전체포함 등 설정)
- ER 다이어그램 작성 

### ③ 논리적 설계
### ④ 물리적 설계
<br>

# 3. list 및 content_view함수의 mock 테스트를 하시오

## ※주의] MockMvc객체 사용할때는 경로를 잘 확인 해야함!
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
@WebAppConfiguration 
@ContextConfiguration({"file:src/main/webapp/WEB-INF/spring/root-context.xml",
		"file:src/main/webapp/WEB-INF/spring/appServlet/servlet-context.xml" })
@Log4j
public class ControllerTest {

	@Setter(onMethod_ = { @Autowired })
	private WebApplicationContext ctx; 

	private MockMvc mockMvc;

	@Before
	public void setup() {
		this.mockMvc = MockMvcBuilders.webAppContextSetup(ctx).build();
	}

	@Test
	public void testList() throws Exception {
		mockMvc.perform(get("/board/list")).andExpect(status().isOk())// 응답 검증, 경로 잘 맞춰줘야함
				.andDo(print()).andExpect(forwardedUrl("/WEB-INF/views/list.jsp")); // view 경로
	}
	
	@Test
	public void testContentView() throws Exception {
		mockMvc.perform(get("/board/content_view")).andExpect(status().isOk()).andDo(print()).andExpect(forwardedUrl("/WEB-INF/views/content_view.jsp"));
	}
}
```

## ▶ 출력
![출력](https://user-images.githubusercontent.com/74290204/106719284-76646680-6645-11eb-8904-0507360e5baf.PNG)
