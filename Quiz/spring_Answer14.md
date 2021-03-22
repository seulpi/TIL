# 1. spring boot를 활용해 list 를 작성하시오
- jsp로 view페이지 작성


# *Answer*

## 1. application.properties 설정
```
server.port=8282

spring.datasource.driver-class-name=oracle.jdbc.driver.OracleDriver
spring.datasource.url=jdbc:oracle:thin:@localhost:1521/xe
spring.datasource.username=scott
spring.datasource.password=tiger

spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp
server.jsp-servlet.init-parameters.development=true
```

## 2. Controller
```java
package edu.bit.ex;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;

import edu.bit.ex.service.BoardService;
import lombok.AllArgsConstructor;


@AllArgsConstructor
@Controller
public class HomeController {
	
	private BoardService service;
	
	@RequestMapping("/")
	public String home() {
		return "home";
	}
	
	@GetMapping("/list")
	public String list(Model model) {
		
		model.addAttribute("list", service.getList());
		
		return "list";
	}
}
```

## 3. VO / Service / Mapper 
- 기존에 spring에서 작성했던 것과 동일
- 단! service에서 mapper @Autowired사용해 주입해줘야함! 

<details><summary>VO</summary>

```java
package edu.bit.ex.vo;

import java.sql.Date;

import lombok.Data;

@Data
public class BoardVO {
	
	private int bId;
	private String bName;
	private String bTitle;
	private String bContent;
	private Date bDate;
	private int bHit;
	private int bGroup;
	private int bStep;
	private int bIndent;
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
	public Date getbDate() {
		return bDate;
	}
	public void setbDate(Date bDate) {
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
	public BoardVO(int bId, String bName, String bTitle, String bContent, Date bDate, int bHit, int bGroup, int bStep,
			int bIndent) {
		super();
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
	public BoardVO() {
		super();
	}
}
```
</details>



<details><summary>Service, ServiceImpl</summary>

```java
package edu.bit.ex.service;

import java.util.List;

import edu.bit.ex.vo.BoardVO;

public interface BoardService {

	public List<BoardVO> getList();
	
}
```
```java
package edu.bit.ex.service;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import edu.bit.ex.mapper.BoardMapper;
import edu.bit.ex.vo.BoardVO;
import lombok.AllArgsConstructor;

@Service
@AllArgsConstructor
public class BoardServiceImpl implements BoardService {
	
	@Autowired
	private BoardMapper mapper;

	@Override
	public List<BoardVO> getList() {
		
		return mapper.getList();
	}

}
```
</details>

<details><summary>Mapper</summary>

```java
package edu.bit.ex.mapper;

import java.util.List;

import org.apache.ibatis.annotations.Mapper;

import edu.bit.ex.vo.BoardVO;

@Mapper
public interface BoardMapper {

	List<BoardVO> getList();
	
}
```
</details>

### - mapper를 scan해주는 방법 
① mapper에 @Mapper 어노테이션 사용 <br>
② SpringBootBoardListApplication 클래스에 @MapperScan 어노테이션 사용
```java
@MapperScan("edu.bit.ex.mapper")
```
③ application.properties에 mapper-location 추가
- mapper.xml 파일의 경로를 설정
```
mybatis.mapper-locations = classpath:mapper/**/*.xml
→  mapper폴더 만들고 mapper안에있는 모든xml (mapper폴더에 위치는 src/main/resources)
```

<details><summary>Mapper.xml</summary>
	
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="edu.bit.ex.mapper.BoardMapper">

<select id="getList" resultType="edu.bit.ex.vo.BoardVO">
<![CDATA[
select * from mvc_board order by bGroup desc, bStep asc
]]>

</select>

</mapper>
```
</details>
<br>

## 4. View

- 부트스트랩 table 사용 
<details><summary>list.jsp</summary>

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<%@ page session="false"%>
<html>
<head>

<title>List</title>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.16.0/umd/popper.min.js"></script>
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>

</head>

<body >

<div class="container">
  <h2>List Table</h2>

  <table class="table">
    <thead>
      <tr>
        <th>번호</th>
        <th>이름</th>
        <th>제목</th>
        <th>내용</th>
        <th>히트</th>
        <th>날짜</th>
      </tr>
    </thead>
    <c:forEach items="${list}" var="dao">
    <tbody>
      <tr>
        <td>${dao.bId}</td>
        <td>${dao.bName}</td>
        <td>${dao.bTitle}</td>
        <td>${dao.bContent}</td>
        <td>${dao.bHit}</td>
        <td>${dao.bDate}</td>
      </tr>
     
    </tbody>
    </c:forEach>
  </table>
</div>
	
</body>
</html>
```
</details>
<br>

☞ 화면 출력 

![캡처](https://user-images.githubusercontent.com/74290204/108827871-7a165800-7609-11eb-8147-304bc5bba621.PNG)

## 6. timeleaf 사용해보기 
### ① pom.xml에 dependency 추가
```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```
### ② application.properties 설정
```
#JSP와 같이 사용할 경우 뷰 구분을 위해 컨트롤러가 뷰 이름을 반환할때 thymeleaf/ 로 시작하면 타임리프로 처리하도록 view-names 지정 
spring.thymeleaf.view-names=thymeleaf/* -> 없어도됨 jsp랑 같이 돌릴때만 지정
spring.thymeleaf.prefix=classpath:/templates/ 
spring.thymeleaf.suffix=.html 

#thymeleaf를 사용하다 수정 사항이 생길 때 수정을 하면 재시작을 해줘야 한다. 
이를 무시하고 브라우저 새로고침시 수정사항 반영을 취해 cache=false 설정(운영시는 true)

spring.thymeleaf.cache=false 
spring.thymeleaf.check-template-location=true
```

### ③ view 
- for문 돌릴때 따로 < th:each= "dao : ${list}" > 이렇게 사용했더니 아예 에러페이지가 떴다
    - 해결: < tr th:each= "dao : ${list}">로 넣어버림 
    >> [for문 참고 사이트] https://seollica.tistory.com/118 
- ※ timeleaf쓸 때 주의할점 
    - controller에서 return할때 /list이렇게 절대 경로 쓰면 못 찾음 
    - pom.xml에 jsp를 위한 설정 dependency : -jasper , jstl 설정 dependency 주석 처리해야 사용 가능!
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>

<title>List</title>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css"></link>
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.16.0/umd/popper.min.js"></script>
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>

</head>

<body >

<div class="container">
  <h2>List Table</h2>

  <table class="table">
    <thead>
      <tr>
        <th>번호</th>
        <th>이름</th>
        <th>제목</th>
        <th>내용</th>
        <th>히트</th>
        <th>날짜</th>
      </tr>
    </thead>
    
    <tbody>
      <tr th:each= "dao : ${list}">
        <td th:text="${dao.bId}"></td>
        <td th:text="${dao.bName}"></td>
        <td th:text="${dao.bTitle}"></td>
        <td th:text="${dao.bContent}"></td>
        <td th:text="${dao.bHit}"></td>
        <td th:text="${dao.bDate}"></td>
      </tr>
     
    </tbody>
  
  </table>
</div>
	
</body>
</html>
```
