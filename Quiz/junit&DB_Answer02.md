# 1. 마이바티스 사용 4가지 방법에 대하여 설명하시오.
## 1. SqlSession을 가져와 getMapper사용 
- [x] interface IBDao를  XML namespace에 매핑 <mapper namespace="edu.bit.ex.one.IBDao"> 
- [x] sqlSession.getMapper(IBDao.class)를 이용.

```java
//IBDao.java
package edu.bit.ex.board.one;

import java.util.List;

import edu.bit.ex.board.vo.BoardVO;

public interface IBDao { //Interface Board Dao 
	public List<BoardVO> listDao();
}
```
```xml
<!--Board1.xml-->
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="edu.bit.ex.board.one.IBDao"><!-- 해당 클래스,인터페이스의 위치 -->

<select id="listDao" resultType="edu.bit.ex.board.BoardVO"> 
<![CDATA[ 
select bId, bName, bTitle, bContent, bDate, bHit, bGroup, bStep, bIndent from mvc_board order by bGroup desc, bStep asc

]]>
</select>

</mapper>
```
<br>

- root-context.xml
```xml
 <!-- 1.번 방법을 위하여 mapperLocations 을 추가 함 (객체 생성 반드시 필요) -->
   <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
      <property name="dataSource" ref="dataSource"/>
   <property name="mapperLocations" value="classpath:/edu/bit/ex/board/mapper/*.xml" /> 
   </bean>
   
   <!-- 1번 방식 사용을 위한 sqlSession -->
   <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
      <constructor-arg index="0" ref="sqlSessionFactory" />
   </bean>
```
```java
// Bservice1.java
package edu.bit.ex.board.one;

import java.util.List;

import org.apache.ibatis.session.SqlSession;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import edu.bit.ex.board.vo.BoardVO;

@Service
public class Bservice1 { 
	// 1번 방식을 사용하는 방법
	
	@Autowired
	SqlSession sqlSession; //root-context.xml에 있는 객체를 가져와 사용 
	
	public List<BoardVO> selectBoardList() throws Exception {
		IBDao dao = sqlSession.getMapper(IBDao.class);
		return dao.listDao();
	}
}
```
```java
//BController1.java
package edu.bit.ex.board.controller;


import javax.inject.Inject;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;

import edu.bit.ex.board.one.Bservice1;
import edu.bit.ex.board.service.BService;
import edu.bit.ex.board.vo.BoardVO;
import lombok.AllArgsConstructor;
import lombok.extern.log4j.Log4j;

@Controller
public class BController1 {
	
	@Inject
	private Bservice1 bSerivce;
	
	@RequestMapping("/list1") 
	public String list(Model model) throws Exception {
		System.out.println("log()");
		
		model.addAttribute("list", bSerivce.selectBoardList());
		return "list1";
	}
}
```

## 2. SqlSesson 안에 있는 함수 이용
- [x] interface는 필요가 없음
- [x] sqlSession에서 제공하는 함수(selectList, selectOne)을 이용함 
- [x] 쿼리구현을 위한 xml이 필요, 해당 XML의 namespace는  개발자가가 정함 

```java
//Bservice2.java
package edu.bit.ex.board.two;

import java.util.List;

import org.apache.ibatis.session.SqlSession;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import edu.bit.ex.board.vo.BoardVO;

@Service
public class Bservice2 { 
	// 2번 방식을 사용하는 방법
	
	@Autowired
	SqlSession sqlSession; //root-context.xml에 있는 객체를 가져와 사용 
	
	public List<BoardVO> selectBoardList() throws Exception {
		return sqlSession.selectList("board.selectBoardList"); 
		/*board.selectBoardList mapper의 경로와 id가 된다
		 *<mapper namespace="board">
		 *<select id="selectBoardList" resultType="edu.bit.ex.board.BoardVO">*/
	}

}
```
```xml
<!--Board2.xml-->
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="board"><!-- 개발자가 직접 정의 -->

<select id="selectBoardList" resultType="edu.bit.ex.board.BoardVO"> 
<![CDATA[ 
select bId, bName, bTitle, bContent, bDate, bHit, bGroup, bStep, bIndent from mvc_board order by bGroup desc, bStep asc

]]>
</select>

</mapper>
```
```java
//BController2.java
package edu.bit.ex.board.two;

import javax.inject.Inject;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;

import edu.bit.ex.board.one.Bservice1;
import edu.bit.ex.board.service.BService;
import edu.bit.ex.board.vo.BoardVO;
import lombok.AllArgsConstructor;
import lombok.extern.log4j.Log4j;

@Controller
public class BController2 {

	@Inject
	private Bservice2 bSerivce;
	
	@RequestMapping("/list2") 
	public String list(Model model) throws Exception {
		System.out.println("list2()");
		
		model.addAttribute("list", bSerivce.selectBoardList());
		return "list2";
	}
	
}
```

## 3. interface를 통해서 mapper를 가져와 interface를 정의하는 방법 (우리가 쓰던 것)
- [x] root-context.xml 에서 mapperLocation을 필요없음
```xml
<property name="mapperLocations" value="classpath:/edu/bit/ex/board/mapper/*.xml" /> 
```
- [x] root-context.xml 에서 이 scan을 사용해서 mybatis 경로 이용
```xml
<mybatis-spring:scan base-package="edu.bit.ex.board.mapper" />
```

## 4. Mapper Interface -> mapper.xml(자손이 구현) 하는 방식 -> Mapper Interface에서 xml이 구현하지 않고 Interface에 @어노테이션으로 사용
- 단점: paging같은 한줄에 들어갈 수 없는 쿼리문은 복잡해짐(간결해지지못함) -> 따라서 간단한것만 사용 (그렇기 때문에 실무에서는 사용할 일이 별로 없음)
- 이 방식을 사용하면 mapper scan의 경로는 필요없음 

```java
package edu.bit.ex.board.mapper;

import java.util.List;

import org.apache.ibatis.annotations.Select;

import edu.bit.ex.board.vo.BoardVO;

public interface BMapper {
	
	@Select("select * from mvc_board")
	public List<BoardVO> list();

	public BoardVO contenetView(int getbId);

	public void writeBoard(BoardVO boardVo);

	public void modify(BoardVO boardVo);

	public void reply(BoardVO boardVo);

	public void replySort(BoardVO boardVo);
}
```
# 2. ajax+json으로 list를 뿌리시오

# 3. 아래를 설계하시오 
```
1.  DVD를 대여하기 위해서는 회원 가입을 해야 한다. 회원 가입 시에는 회원에 대한 이름, 주
   민번호, 전화번호, 핸드폰번호, 이메일, 우편번호, 주소, 등록일 등을 기록해 둔다. 가입자는 
   관심 장르를 설정하여 신 프로 입고 시 이메일을 발송한다.
2.  DVD에 대한 정보는 영화제목, 제작사, 주연배우, 감독, 영화출시일 등의 상세 정보를 관리
   해야 하며 각 DVD에 대한 파손 여부와 대여 여부 등의 상태정보도 관리되어야 한다.
3.  DVD는 장르로 구분해서 체계적으로 관리된다.
4.  대여는 신 프로의 경우에는 대여일이 기본 1일이며 구 프로인 경우에는 2일이 기본 대여일
    이며 대여료는 신 프로의 경우에는 2000원이며 구 프로인 경우에는 대여료가 1000원이다.
5.  미납회원 관리와 함께 연체되었을 경우 하루에 500원 연체료가 누적된다. DVD가 파손되
    거나 망실된 경우 해당 회원은 DVD 가격을 변상해야 한다. 
6.  회원이 DVD를 대여할 경우 대여 1회당 1점씩의 포인트 점수를 부여하고 포인트 점수가 
    10점이 되면 무료로 DVD 하나를 대여할 수 있도록 포인트제를 운영하고자 한다.
7.  DVD 대여점 운영자는 금전상의 관리를 위해서 일별, 월별 매출액과 DVD가 대여된 횟수를 
    알고 싶어 한다.
```

## ▶ *Answer* 
### 초기설계
![KakaoTalk_20210204_162017809](https://user-images.githubusercontent.com/74290204/106882567-40e17b00-6722-11eb-9b08-2c4e808ae298.jpg)

## ▶ *수업 후 추가* 
### - Entity , 속성
- 회원 : 회원번호, 이름, 주민번호, 전화번호, 핸드폰번호, 이메일, 우편번호, 주소, 등록일, 관심장르, 포인트
- DVD : DVD코드, 상태, 영화코드 **(DVD는 각 DVD니까 영화랑 따로분류해서 만들어줘야함)**
- 영화 : 영화코드, 영화제목, 제작사, 주연배우, 감독, 영화출시일, 신구구분, 장르코드 
- 장르 : 장르코드, 장르명
- 대여기준 : 신구프로구분, 대여일, 대여료
- 매출통계 : 신구프로구분, 장르, 날짜, 매출액, 대여횟수 **(통계, 매출은 관계에서 테이블 따로 빼줘야함)**
- 대여 : 대여번호, 회원번호, DVD코드 
