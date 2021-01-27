# 1. 아래의 xml 에 대하여 설명하시오
```
- pom.xml
- web.xml
- *-context.xml
```

## pom.xml
: 프로젝트 전체의 관리 및 환경설정 등 프로젝트 관리자
- 프로젝트의 중요한 정보를 정의하고 정리하기 위한 곳, 개발환경 구축을 위한 툴

## web.xml
: Web Application 설정을 위한 Deployment Descriptor(환경파일 : 배포서술자, DD파일)로서 XML 형식의 파일
- 각종 설정을 위한 설정파일 
- 모든 Web application은 반드시 하나의 web.xm l파일을 가져야 함
- 위치 : WEB-INF 폴더 아래
- web.xml 파일의 설정들은 Web Application 시작시 메모리에 로딩됨 (수정을 할 경우 web application을 재시작 해야함)

### ※ 주의
- **대소문자 구분**해줘야함 
- attribute 값은 **반드시 " " or ' '로 감싸야한다**
- 태그 반드시 닫기 
    - content가 없는 태그의 경우 → < br/ >


## *-context.xml
: Spring Maven에서의 -context.xml은 두개의 context로 나뉜다<br>
▶ 나누는 이유는 두개의 context.xml의 역할을 분리시키기 위해서
- Root-WebApplicationContext : jsp파일 즉 자바프로그램이 들어가는 객체를 컨트롤한다 
    - Service(Command), Mapping(DAO), DB 
- Servlet-WebApplicationContext : jsp파일 아닌 정적파일에 대한 객체를 컨트롤한다 (웹 관련)
    - HandlerMapping, HandlerAdaptor, ViweResolver, View, Controller
<br>

# 2. 스프링에서 게시판 사용을 위한 설계도를 그리시오
![Sprin_MVC_Web](https://user-images.githubusercontent.com/74290204/105824158-57405600-6001-11eb-9a73-557db599a85d.png)
<br>

# 3. 스프링에서 한글처리는 어떻게 하는가?
- web.xml 에서 filter로 처리 (filter추가) 
- 프로젝트를 새로 만들때마다 수정해줘야하기 때문에 filter 복붙으로 처리
    - ↓ 밑에 코드를 복사 붙여넣기해서 사용
```xml
   <filter>
      <filter-name>encoding</filter-name>
      <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
      <init-param>
         <param-name>encoding</param-name>
         <param-value>UTF-8</param-value>
      </init-param>
   </filter>

   <filter-mapping>
      <filter-name>encoding</filter-name>
      <servlet-name>appServlet</servlet-name>
   </filter-mapping>
```
<br>

# 4. 마이바티스에 대하여 설명하시오
: 자바의 관계형 데이터 베이스 프로그래밍을 좀더 쉽게 할수 있게 도와주는 개발 프레임워크, JDBC를 통해 DB에 접근하는 방식을 캡슐화함
- 개발자가 작성한 SQL 명령어와 자바 객체를 매핑해서 사용이 가능하다 
- DB관련 처리를 xml파일로 처리

## @ 주요 컴포넌트 
- MyBatis 설정파일(SqlMapConfig.xml) : VO 객체의 정보를 설정
- SqlSessionFactory : MyBatis 설정파일을 바탕으로 SqlSessionFactory를 생성, Spring Bean으로 등록해야 한다
- SqlSessionTemplate : 핵심적인 역할을 하는 클래스로서 SQL 실행이나 트랜잭션 관리를 실행한다 <br> SqlSession 인터페이스를 구현하며, Spring Bean으로 등록해야 한다.
- Mapping 파일 (user.xml) : SQL문 또는 Mapping을 설정
- Spring Bean 설정 파일 (mybatisBeans.xml) : SqlSessionFactoryBean을 Bean 등록할 때 DataSource 정보와 MyBatis Config 파일정보, Mapping 파일의 정보를 함께 설정 <br> SqlSessionTemplate을 Bean으로 등록한다.

>> 참조] https://velog.io/@changyeonyoo/Mybatis%EB%9E%80-%EC%9E%A5%EC%A0%90-%ED%8A%B9%EC%A7%95-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8

# 5. 스프링에서 hello.jsp 가 유저에게 전달되기 까지의  순서와 해당 객체를 그림으로 표현하시오
![Sprin_MVC](https://user-images.githubusercontent.com/74290204/105934997-8522aa80-6094-11eb-8540-aecbf8e4c0f2.png)

