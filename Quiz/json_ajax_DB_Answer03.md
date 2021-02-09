# 1. 아래에 대하여 설명하시오
```
- 트랜잭션 이란?
- rollback 과 commit 이란?
```

### - **트랜잭션** : 프로그래밍 error와 상관없이 **DB와 관련된 용어**, 에러가 나서 **중단되는 경우 rollback과 commit을 위해 사용**한다
- 트랜잭션 **대상** : (select를 제외한) **update, delete, insert** 
- 하나의 코드 이상을 사용할때는 반드시 트랜잭션을 사용해야한다
### ※ 주의 ※ 
- checked와 unchecked Exception에서의 rollback 차이 

    - **checked는 개발자가 직접** throws, try-catch 를 사용해 **예외처리를 해주기 때문에 rollback이 이루어지지않음** (개발자소관) <br> → 따라서 rollback할 함수 처리를 해주어야한다

    ```java
    @Transactional //CheckedException -> 롤백안함(@Transactional 적용X)
	public void transactionTest5() throws SQLException { 

		log.info("transionTest5()테스트 ");

		BoardVO boardVO = new BoardVO();

		boardVO.setbContent("트랜잭션5");
		boardVO.setbName("트랜잭션5");
		boardVO.setbTitle("트랜잭션5");

		mapper.writeBoard(boardVO); 
		
		throw new SQLException("SQLExcetion for rollback"); 

	}
	  
	@Transactional(rollbackFor = Exception.class) 
	public void transactionTest6() throws SQLException { 
		
		log.info("transionTest6()테스트 ");

		BoardVO boardVO = new BoardVO();

		boardVO.setbContent("트랜잭션6");
		boardVO.setbName("트랜잭션6");
		boardVO.setbTitle("트랜잭션6");

		mapper.writeBoard(boardVO); 
		
		throw new SQLException("SQLExcetion for rollback"); 
	}
    ```
<br>

# 2. 스프링에서의 트랜잭션 처리 방법은?
- 트랜잭션 Spring에서의 처리 방법 → **@(Annotation)을 사용**한다 , 사용하는 방법은 더 있지만 이게 실무에서 가장 많이 쓰이는 방법!
```java
@Transactional
```
