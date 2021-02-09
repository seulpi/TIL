# ★ Transaction : DB와 관련된 용어 (프로그래밍과 관련 X)
- DB 업데이트 과정에서 에러 등 서버가 중단됐을 때 commit, rollback을 위한 처리
>> - commit : 영속적인 저장 (commit하지 않으면 저장X)<br> - rollback : commit 이전으로 상태를 되돌리는 것
- **트랜잭션 대상** : (select를 제외한) **Insert, Update, Delete**
- spring에서 트랜잭션 처리 :  @(Annotation) 사용
```java
@Transactional
```

- **하나의 코드 이상** 사용할때는 **반드시 트랙잭션을 사용**해야함 
    - [대표적인 예] 게시판 댓글(최신순으로 정렬, step정렬)
    ```java
    @Transactional
    @Override
    public void writeReply(BoardVO boardVO) {
        mapper.updateShape(boardVO);
        mapper.insertReply(boardVO);
            
    }
    ```
- 트랙잭션의 우선순위 : 메소드 > 클래스 > 인터페이스 메소드> 인터페이스 순 
- 최대한 선언적인 트랜잭션 방식으로 프로그램을 설계해야함
- ★ **checked와 unchecked Exception에서의 rollback 차이**
    - checked Exception은 throws , try-catch로 반드시 체크해줘야하는 에러! <br> → 따라서 checked Exception은 프로그램이 관여하지 않기 때문에 롤백 x (개발자가 알아서 처리해주는 에러, @Transactional이 적용되지X) → 개발자가 rollback 따로 지정해줘야함
- ★ **Transaction의 위치 = Controller는 안됨!** <br> → why? 설계를 할 때 Controller는 기본적으로 view만 결정해주는 용도이기 때문에 로직, 구현해야 할 내용은 Service단에서 처리

## @ 설정
### - root-context.xml → 추가 (버전마다 다름) 
- DB관련이기 때문에 root-context.xml 
- transactionManager 이름도 바뀌면 안됨 그대로 복사!

```xml
<!--  @Transactional 사용을 위한 객체 생성 -->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource" />
    </bean>
    
    <tx:annotation-driven transaction-manager="transactionManager" />
```

## @ 사용
```java
//TracnsactionService.java
package edu.bit.ex.board.service;

import java.sql.SQLException;

import javax.management.RuntimeErrorException;

import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import edu.bit.ex.board.mapper.Bmapper;
import edu.bit.ex.board.vo.BoardVO;
import lombok.AllArgsConstructor;
import lombok.extern.log4j.Log4j;

@Log4j
@Service 
@AllArgsConstructor 
public class TransactionService  { //test여서 다이렉트로함 원래는 interface만들어서 폴리몰티즘 적용하는게 좋음!
	
	private Bmapper mapper;

	public void transactionTest1() {

		log.info("transionTest1()테스트 ");

		BoardVO boardVO = new BoardVO();

		boardVO.setbContent("트랜잭션1");
		boardVO.setbName("트랜잭션1");
		boardVO.setbTitle("트랜잭션1");

		mapper.writeBoard(boardVO); // insert기능 -> 있는거 사용하려고 따로 안만들어서 함수명이 그런것

		boardVO.setbContent("트랜잭션2");
		boardVO.setbName("트랜잭션2");
		boardVO.setbTitle("트랜잭션2");

		mapper.writeBoard(boardVO);

	}
	
	public void transactionTest2() {

		log.info("transionTest2()테스트 ");

		BoardVO boardVO = new BoardVO();

		boardVO.setbContent("트랜잭션1");
		boardVO.setbName("트랜잭션1");
		boardVO.setbTitle("트랜잭션1");

		mapper.writeBoard(boardVO); 

		boardVO.setbContent("트랜잭션2");
		boardVO.setbName("트랜잭션2");
		boardVO.setbTitle("트랜잭션2");
		
		// 일부러 에러를 내기 위한 null 처리
		boardVO = null;
		mapper.writeBoard(boardVO);

	}
	
	@Transactional //@Transactional의 의미 : 함수내에서 에러가 나게되면 해당 DB를 함수이전으로 되돌리는 것 (밑에 null에러가 나면 그 전에 코드가 맞다고해도 같이 DB에 반영하지 않음)
	public void transactionTest3() {

		log.info("transionTest3()테스트 ");

		BoardVO boardVO = new BoardVO(); 

		boardVO.setbContent("트랜잭션1");
		boardVO.setbName("트랜잭션1");
		boardVO.setbTitle("트랜잭션1");

		mapper.writeBoard(boardVO); 

		boardVO.setbContent("트랜잭션2");
		boardVO.setbName("트랜잭션2");
		boardVO.setbTitle("트랜잭션2");
		
		// 일부러 에러를 내기 위한 null 처리
		boardVO = null;
		mapper.writeBoard(boardVO);

	}
	
	@Transactional //unchekedException -> 롤백함
	public void transactionTest4() {

		log.info("transionTest4()테스트 ");

		BoardVO boardVO = new BoardVO();

		boardVO.setbContent("트랜잭션4");
		boardVO.setbName("트랜잭션4");
		boardVO.setbTitle("트랜잭션4");

		mapper.writeBoard(boardVO); 
		
		throw new RuntimeException("RuntimeException for rollback");

	}
	
	@Transactional //CheckedException -> 롤백안함(@Transactional먹지x)
	public void transactionTest5() throws SQLException { 
		/*
		 * throw를 안던지면 try-catch -> 반드시 예외처리를 해줘야함 -> 개발자로 하여금 에러처리를 맡긴다!
		 * 그래서 @Transactional해도 기본적으로 롤백처리를 해주지 X
		 */

		log.info("transionTest5()테스트 ");

		BoardVO boardVO = new BoardVO();

		boardVO.setbContent("트랜잭션5");
		boardVO.setbName("트랜잭션5");
		boardVO.setbTitle("트랜잭션5");

		mapper.writeBoard(boardVO); 
		
		throw new SQLException("SQLExcetion for rollback"); 

	}
	   //@Transactional의 rollbackFor 옵션을 이용하면 Rollback이 되는 클래스를 지정 가능함.
	   // Exception예외로 롤백을 하려면 다음과 같이 지정하면됩니다. 
	   //@Transactional(rollbackFor = Exception.class) 
	   // 여러개의 예외를 지정할 수도 있습니다. @Transactional(rollbackFor = {RuntimeException.class, Exception.class})
	   //
	@Transactional(rollbackFor = Exception.class) //모든 예외에 대한 처리에 대해서 rollback하기 위한 코드(checked Exception은 롤백안해주니까 따로 롤백을 위해 지정해줘야함)
	public void transactionTest6() throws SQLException { 
		
		log.info("transionTest6()테스트 ");

		BoardVO boardVO = new BoardVO();

		boardVO.setbContent("트랜잭션6");
		boardVO.setbName("트랜잭션6");
		boardVO.setbTitle("트랜잭션6");

		mapper.writeBoard(boardVO); 
		
		throw new SQLException("SQLExcetion for rollback"); 

	}
	
	@Transactional(rollbackFor = SQLException.class) //SQLException 예외에 대한 처리
	public void transactionTest7() throws SQLException { 
		
		log.info("transionTest7()테스트 ");

		BoardVO boardVO = new BoardVO();

		boardVO.setbContent("트랜잭션7");
		boardVO.setbName("트랜잭션7");
		boardVO.setbTitle("트랜잭션7");

		mapper.writeBoard(boardVO); 
		
		throw new SQLException("SQLExcetion for rollback"); 

	}
}
```
```java
//TransactionController.java

package edu.bit.ex.board.controller;

import java.sql.SQLException;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import edu.bit.ex.board.service.TransactionService;
import lombok.AllArgsConstructor;
import lombok.extern.log4j.Log4j;

@Log4j
@AllArgsConstructor
@Controller
public class TransactionController {
	
	private TransactionService tranService;
	
	@GetMapping("/tran/{num}")
    public void transaction(@PathVariable("num") int num) throws SQLException {
		/*
		 * transaction처리는 일반적인 에러처리가 아닌 DB처리 http://localhost:8282/ex/tran/1 치면 웹은 당연히
		 * 404에러가 뜸 view가 없으니까 단 DB에는 transactionTest1()을 호출하니까 DB insert가 잘됨
		 */
         if(num == 1){
            log.info("transionTest1");
            tranService.transactionTest1();
         }else if(num == 2) {
        	 log.info("transionTest2");
             tranService.transactionTest2();
         }else if(num == 3) {
        	 log.info("transionTest3");
             tranService.transactionTest3();
         }else if(num == 4) {
        	 log.info("transionTest4");
             tranService.transactionTest4();
         }else if(num == 5) {
        	 log.info("transionTest5");
             tranService.transactionTest5();
         }else if(num == 6) {
        	 log.info("transionTest6");
             tranService.transactionTest6();
         }else if(num == 7) {
        	 log.info("transionTest7");
             tranService.transactionTest7();
         }
    }
}
```

![tran](https://user-images.githubusercontent.com/74290204/107323432-98e4fc80-6ae9-11eb-8338-de3cb4e6c6a5.PNG)

![tran2](https://user-images.githubusercontent.com/74290204/107323435-9a162980-6ae9-11eb-9bf8-661829635a82.PNG)

