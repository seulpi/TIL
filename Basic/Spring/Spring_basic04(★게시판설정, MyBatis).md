# Spring - 요청 생명주기
## ▶ 동작 원리
![Sprin_MVC](https://user-images.githubusercontent.com/74290204/105934997-8522aa80-6094-11eb-8540-aecbf8e4c0f2.png)

## ▶ 게시판 사용을 위한 설계도
![Sprin_MVC_Web](https://user-images.githubusercontent.com/74290204/105824158-57405600-6001-11eb-9a73-557db599a85d.png)
- Service : command 부분이 Service
- DAO, DTO, DB : Mapper = Repository = 영속계층(Persistence)
- HandlerMapping : Controller 결정
- HandlerAdapter : Controller 함수 실행
- ViewResolver : view에대한 기본적인 설정
    - viewResolver 객체를 통해 return할때 login.jsp 안하고 return "login" 으로 해도 가능한 것 

# Spring - 게시판을 위한 설정
## 1. pom.xml 설정
- 직접 각각의 사이트에서 다운로드 받을 필요없이 이곳에 dependency를 추가하면 필요한 파일들이 자동으로 다운로드 받아진다
   - 다운로드가 완료되면 Maven Depindencies에 파일이 추가된 것 확인 가능
- 버전을 타고, 수많은 라이브러리들이 서로서로 상당하게 얽혀있기 때문에 까다롭다 <br> 
(무료가 많아서 버그도 맞고 버전별로 안맞는 부분들도 많음)
- ※ 주의) 라이브러리 관리가 제일 중요하고 어렵기 때문에 실무에서는 최고 대빵님이 컨트롤 
    - 따라서, 필요한 라이브러리(dependency)는 대빵한테 물어보고 추가 할 것! 
    
>> [설정 build 안될 때] 일단 다운로드 받을 때 굉장히 오래걸리는데 함부로 끄거나 중단하면 오류날 가능성이 많기 때문에 일단 끝까지 기다릴 것!<br>
먼저 다른 프로젝트를 클로즈한다 → sts 종료 → C:\Users\...\.m2 경로로 들어가서 여기 repository폴더 삭제 →  sts 실행 → 다운로드 다시 자동으로 실행됨

*학원에서는 선생님 repository 압축폴더 받아서 복붙해서 사용함*
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>edu.bit</groupId>
	<artifactId>board</artifactId>
	<name>spring_board_5ver</name>
	<packaging>war</packaging>
	<version>1.0.0-BUILD-SNAPSHOT</version>

▶ 상위 이 부분들은 건드리지 말고 밑에 <properties> 부분 기존에 있는 거 지우고 코드 추가하기
```
<details><summary>pom.xml 복붙할 코드 </summary>

```xml
<properties>
      <java-version>1.8</java-version>
      <org.springframework-version>5.0.8.RELEASE</org.springframework-version>
      <org.aspectj-version>1.6.10</org.aspectj-version>
      <org.slf4j-version>1.6.6</org.slf4j-version>
      <org.security-version>5.0.6.RELEASE</org.security-version>
   </properties>
   
   <repositories>
      <repository>
          <id>oracle</id>
          <url>http://www.datanucleus.org/downloads/maven2/</url>
      </repository>
   </repositories>   
   
   <dependencies>
      <!-- 오라클 JDBC 드라이버 -->
      <dependency>
          <groupId>oracle</groupId>
          <artifactId>ojdbc6</artifactId>
          <version>11.2.0.3</version>
      </dependency>
      
      <!-- Spring -->
      <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-context</artifactId>
         <version>${org.springframework-version}</version>
         <exclusions>
            <!-- Exclude Commons Logging in favor of SLF4j -->
            <exclusion>
               <groupId>commons-logging</groupId>
               <artifactId>commons-logging</artifactId>
            </exclusion>
         </exclusions>
      </dependency>
      <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-webmvc</artifactId>
         <version>${org.springframework-version}</version>
      </dependency>

      <!-- AspectJ -->
      <dependency>
         <groupId>org.aspectj</groupId>
         <artifactId>aspectjrt</artifactId>
         <version>${org.aspectj-version}</version>
      </dependency>
      
      <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
      <dependency>
          <groupId>org.aspectj</groupId>
          <artifactId>aspectjweaver</artifactId>
          <version>${org.aspectj-version}</version>
      </dependency>

      <!-- Logging -->
      <dependency>
         <groupId>org.slf4j</groupId>
         <artifactId>slf4j-api</artifactId>
         <version>${org.slf4j-version}</version>
      </dependency>
      <dependency>
         <groupId>org.slf4j</groupId>
         <artifactId>jcl-over-slf4j</artifactId>
         <version>${org.slf4j-version}</version>
         <scope>runtime</scope>
      </dependency>
      <dependency>
         <groupId>org.slf4j</groupId>
         <artifactId>slf4j-log4j12</artifactId>
         <version>${org.slf4j-version}</version>
         <scope>runtime</scope>
      </dependency>
      <dependency>
         <groupId>log4j</groupId>
         <artifactId>log4j</artifactId>
         <version>1.2.15</version>
         <exclusions>
            <exclusion>
               <groupId>javax.mail</groupId>
               <artifactId>mail</artifactId>
            </exclusion>
            <exclusion>
               <groupId>javax.jms</groupId>
               <artifactId>jms</artifactId>
            </exclusion>
            <exclusion>
               <groupId>com.sun.jdmk</groupId>
               <artifactId>jmxtools</artifactId>
            </exclusion>
            <exclusion>
               <groupId>com.sun.jmx</groupId>
               <artifactId>jmxri</artifactId>
            </exclusion>
         </exclusions>
         <!-- <scope>runtime</scope> -->
      </dependency>

      <!-- @Inject -->
      <dependency>
         <groupId>javax.inject</groupId>
         <artifactId>javax.inject</artifactId>
         <version>1</version>
      </dependency>

      <!-- Servlet -->
      <dependency>
         <groupId>javax.servlet</groupId>
         <artifactId>javax.servlet-api</artifactId>
         <version>3.1.0</version>
         <scope>provided</scope>
      </dependency>

      <dependency>
         <groupId>javax.servlet.jsp</groupId>
         <artifactId>jsp-api</artifactId>
         <version>2.1</version>
         <scope>provided</scope>
      </dependency>
      <dependency>
         <groupId>javax.servlet</groupId>
         <artifactId>jstl</artifactId>
         <version>1.2</version>
      </dependency>

      <!-- Test -->
      <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.12</version>
         <scope>test</scope>
      </dependency>

      <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-test</artifactId>
         <version>${org.springframework-version}</version>
      </dependency>
      <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-jdbc</artifactId>
         <version>${org.springframework-version}</version>
      </dependency>
      <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-tx</artifactId>
         <version>${org.springframework-version}</version>
      </dependency>

      <dependency>
         <groupId>com.zaxxer</groupId>
         <artifactId>HikariCP</artifactId>
         <version>2.7.8</version>
      </dependency>


      <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
      <dependency>
         <groupId>org.mybatis</groupId>
         <artifactId>mybatis</artifactId>
         <version>3.4.6</version>
      </dependency>

      <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
      <dependency>
         <groupId>org.mybatis</groupId>
         <artifactId>mybatis-spring</artifactId>
         <version>1.3.2</version>
      </dependency>


      <dependency>
         <groupId>org.bgee.log4jdbc-log4j2</groupId>
         <artifactId>log4jdbc-log4j2-jdbc4</artifactId>
         <version>1.16</version>
      </dependency>

      <dependency>
         <groupId>org.projectlombok</groupId>
         <artifactId>lombok</artifactId>
         <version>1.18.0</version>
         <scope>provided</scope>
      </dependency>

      
      <dependency>
         <groupId>com.fasterxml.jackson.core</groupId>
         <artifactId>jackson-databind</artifactId>
         <version>2.9.6</version>
      </dependency>
      
      <!-- 자바객체를 xml으로 -->
      <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.dataformat/jackson-dataformat-xml -->
      <dependency>
         <groupId>com.fasterxml.jackson.dataformat</groupId>
         <artifactId>jackson-dataformat-xml</artifactId>
         <version>2.9.6</version>
      </dependency>
      
      <!-- 자바객체를 Json으로 -->
      <!-- https://mvnrepository.com/artifact/com.google.code.gson/gson -->
      <dependency>
         <groupId>com.google.code.gson</groupId>
         <artifactId>gson</artifactId>
         <version>2.8.2</version>
      </dependency>
      
      <!-- Spring Security -->
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-core</artifactId>
            <version>${org.security-version}</version>
        </dependency>
        
      <dependency>
          <groupId>org.springframework.security</groupId>
          <artifactId>spring-security-web</artifactId>
          <version>${org.security-version}</version>
      </dependency>
      
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-config</artifactId>
            <version>${org.security-version}</version>
        </dependency>
        
        <dependency>
          <groupId>org.springframework.security</groupId>
          <artifactId>spring-security-taglibs</artifactId>
          <version>${org.security-version}</version>
      </dependency>
        
   </dependencies>
   
   
   
   <build>
      <plugins>
         <plugin>
            <artifactId>maven-eclipse-plugin</artifactId>
            <version>2.9</version>
            <configuration>
               <additionalProjectnatures>
                  <projectnature>org.springframework.ide.eclipse.core.springnature</projectnature>
               </additionalProjectnatures>
               <additionalBuildcommands>
                  <buildcommand>org.springframework.ide.eclipse.core.springbuilder</buildcommand>
               </additionalBuildcommands>
               <downloadSources>true</downloadSources>
               <downloadJavadocs>true</downloadJavadocs>
            </configuration>
         </plugin>
         <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>2.5.1</version>
            <configuration>
               <source>1.8</source>
               <target>1.8</target>
               <compilerArgument>-Xlint:all</compilerArgument>
               <showWarnings>true</showWarnings>
               <showDeprecation>true</showDeprecation>
            </configuration>
         </plugin>
         <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>1.2.1</version>
            <configuration>
               <mainClass>org.test.int1.Main</mainClass>
            </configuration>
         </plugin>
      </plugins>
   </build>
</project>
```
</details>
<br>

## 2. web.xml 설정
- **ContextLoadListener**는 **root-WebApplicationContext를 생성**
- [x] web.xml에서 설정하는 종류 → dispatcher 생성, fliter처리(한글처리 등), 예외처리, session처리등
- 학원에서 한글처리 여기서 fliter로 정리했음 
- web.xml에서 한글코딩을 UTF-8로 설정했기 때문에 해당 jsp파일들은  UTF-8로 맞춰줘야 한글이 깨지지않는다 <br>
※ 설정을  UTF-8로 하고 EUC-KR로 하면 한글이 깨짐
- < /web-app> 위에 복붙(**위치가 중요!**)
    <details><summary>web.xml 복붙할 코드</summary>

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
    </details>
- web.xml 복붙 후 최종 코드
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5" xmlns="http://java.sun.com/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee https://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">

	<!-- The definition of the Root Spring Container shared by all Servlets and Filters -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/spring/root-context.xml</param-value>
	</context-param>
	
	<!-- Creates the Spring Container shared by all Servlets and Filters -->
    <!-- web.xml을 톰캣이 읽어들이는데 이<listener> 부분에서 <listner>Root WebAppilcationContext를 만들고 ContextLoaderListrner라는 객체를만듬 -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>

	<!-- Processes application requests -->
    <!-- Dispatcher 객체 생성 → dispatcher servlet-context.xml이 root-context.xml를 참고 -->
	<servlet>
		<servlet-name>appServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>  <!-- Dispatcher Servlet은 여러개 생성가능하기 때문에 Selvelt의 부팅수선서가 첫번째라는 뜻 ,
        없어도 프로그램이 잘 돌아감 : 없다는 의미는 클라이언트가 처음으로 접속했을 때 객체생성을 한다는 의미
        이 태그가 있다 : 먼저 객체 생성을 하고 클라이언트의 요청을 받는다-->
	</servlet>
		
	<servlet-mapping>
		<servlet-name>appServlet</servlet-name>
		<url-pattern>/</url-pattern>
        <!-- 클라이언트가 접속할 수 있는 유일한 방법 : URL
        DispatcherServlet가 받아낼 url 맵핑을 의미하며 '/'은 모든 url을 받겠다는 뜻 -->
	</servlet-mapping>
	
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

</web-app>
```

## 3. root-context.xml →  DB관련 설정등 처리
1. 기존에 있는 코드 지우기
2. 복붙(히카리 사용을위한 코드)
    <details><summary>Hikary 복붙 코드 </summary>

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:mybatis-spring="http://mybatis.org/schema/mybatis-spring"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:tx="http://www.springframework.org/schema/tx"
    xsi:schemaLocation="http://mybatis.org/schema/mybatis-spring http://mybatis.org/schema/mybatis-spring-1.2.xsd
        http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd
        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.3.xsd
        http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.3.xsd">
    
    <!-- Root Context: defines shared resources visible to all other web components -->
        <!-- Root Context: defines shared resources visible to all other web components -->
    
    <bean id="hikariConfig" class="com.zaxxer.hikari.HikariConfig">
        <property name="driverClassName"
            value="net.sf.log4jdbc.sql.jdbcapi.DriverSpy"></property>
        <property name="jdbcUrl"
            value="jdbc:log4jdbc:oracle:thin:@localhost:1521:XE"></property>
        <property name="username" value="scott"></property>
        <property name="password" value="tiger"></property>
    </bean>

    <!-- HikariCP configuration, Connction Pool 생성 -->
    <bean id="dataSource" class="com.zaxxer.hikari.HikariDataSource"
        destroy-method="close">
        <constructor-arg ref="hikariConfig" />
    </bean>

    <!-- 1.번방법을 위하여 mapperLocations 을 추가 함 -->
    <!-- sqlSessionFactory : 커넥션풀과 xml 위치 -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <property name="mapperLocations" value="classpath:/edu/bit/board/mapper/*.xml" />
    </bean>
    <!-- 1번 방식 사용을 위한 sqlSession -->
    <!-- sqlsession : sqlsessionTemplate(Mybatis)객체 생성  -->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
        <constructor-arg index="0" ref="sqlSessionFactory" /> <!-- sqlSessionFactory 를 index=0번째에 넣는다 -->
    </bean>
    
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
            <property name="dataSource" ref="dataSource" />
        </bean>
        
        <tx:annotation-driven transaction-manager="transactionManager" />

    <mybatis-spring:scan
        base-package="edu.bit.board.mapper" />

        <context:component-scan
        base-package="edu.bit.board.service"></context:component-scan>
        
    <!-- <aop:aspectj-autoproxy></aop:aspectj-autoproxy> -->
        
    </beans>

    ```
    </details>

3. Spring을 DB와 연동하기위해서 중간에 매개해주는 연결 매개체(Hikary등)를 사용하는것 

## 4. servlet-context.xml 
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns="http://www.springframework.org/schema/mvc"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:beans="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd
		http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

	<!-- DispatcherServlet Context: defines this servlet's request-processing infrastructure -->
	
	<!-- Enables the Spring MVC @Controller programming model -->
    <!-- 이 한줄의 의미 = Controller Annotation을 실행시키기위한 객체 
    * Controller 실행 → DispatcherServlet은 Controller를 직접 실행시키지 않고 HandlerMappin, HandlerAdapter를 통해 Controller를 실행시킴
    (DispatcherServlet이HandlerMapping, HandlerAdapter에게 위임한다) = HandlerMapping, HandlerAdapter가 Controller를 실행 
    * HandlerMapping, HandlerAdapter를 <annotation-driven/>으로 두 객체를 생성해놓은것!!!!!!!-->
	<annotation-driven />

	<!-- Handles HTTP GET requests for /resources/** by efficiently serving up static resources in the ${webappRoot}/resources directory -->
    <!--정적리소스 처리를 위한 경로설정 
    정적리소스 : .jsp파일 아닌것 .html .java .css .img .mp4 (자바프로그램 안들어가는것)  
    mapping = url / location = 물리적위치 -->
	<resources mapping="/resources/**" location="/resources/" />

	<!-- Resolves views selected for rendering by @Controllers to .jsp resources in the /WEB-INF/views directory -->
    <!-- ViewResolver설정 : view에대한 기본적인 설정 -->
	<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<beans:property name="prefix" value="/WEB-INF/views/" />
		<beans:property name="suffix" value=".jsp" />
	</beans:bean>
	
     <!-- edu.bit.board 이 패키지 밑에 있는 모든 것들에 대해서 @Component를 읽어들여라
     = @붙어있는거 전부 다 객체 생성해서 servlet으로 갖다 떄려라  -->
	<context:component-scan base-package="edu.bit.board" />
		
</beans:beans>
```
### @Component 자식들 대표 3가지 
1. @Controller
2. @Service : command객체에서 비즈니스 로직에 해당하는 클래스 
3. @Repository : DAO 부분 
```java
@Component
@Controller
public class HomeController { }


▶@Component의 역할은 HomeControlloer 객체 생성을 한다 
우리가 따로 HomeController home = new HomeController(); 안할 수 있는 이유
```

### - Context를 두개로 나눈 이유 : for 역할 분리
- Servlet WebApplicationContext : 웹관련 객체 생성 (정적인 부분을 처리)
    - Controller, View Resolvers, HandelerMapping 등 
- Root WebApplicationContext : 그 이외의 객체 생성 (자바프로그램이 들어가는 객체 담당)
    - Model, DB 등 
    
---

# Mybatis : 3종세트를 캡슐화한 것 → xml로 구현
- IBatis → MyBatis(ver3) 
- 사용하려면 또 라이브러리 다운받아야함...^____^  (이미 위에서 이거 포함해서 다운로드할 수 있게 설정한것)
    - pom.xml에서 < dependency >
    ```xml
      <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
      <!-- MyBatis 관련 (본인) -->
      <dependency>
         <groupId>org.mybatis</groupId>
         <artifactId>mybatis</artifactId>
         <version>3.4.6</version>
      </dependency>

      <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
      <!-- 스프링 연결 관련 -->
      <dependency>
         <groupId>org.mybatis</groupId>
         <artifactId>mybatis-spring</artifactId>
         <version>1.3.2</version>
      </dependency>
      ```

##  @ 사용 방법 DAO 
- src/main/resource → package → Name: edu.bit.board.mapper <br> 
→ 우클릭 →  xml 파일 생성 →  BoardMapper.xml →  기본으로 생성되는거 지우고 밑에 코드 복붙  
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="edu.bit.board.mapper.BoardMapper">
   <!-- interface BoardMapper 맵핑(연결)-->
   <select id="getList" resultType="edu.bit.board.vo.BoardVO">
 <!-- 구현해야할 함수를 id를 통해 집어넣는다, resultType에서 List(컬렉션프레임워크)맞춰줄필요없음 MyBatis는 그런거 없고 자기가 알아서 맞춰줌
따라서 리턴해줘야할 객체 타입의 경로만 정리해주면됨-->
   
<!-- sql문장을 읽어들일때  연산자는 xml로 읽어들이기 때문에 에러가남  ![CDATA 를 넣어주면 xml문장이 아니라고 표시해주는것-->
<![CDATA[ 
      select bId, bName, bTitle, bContent, bDate, bHit, bGroup, bStep, bIndent from mvc_board order by bGroup desc, bStep asc
   ]]> <!-- ';' 습관적으로 넣어주는데 있으면 에러남 절대 넣지말기-->
   </select>
</mapper>
```

### - loog4j.xml 
- 기존 코드 지우고 다시 복붙
<details><summary>복붙할 코드</summary>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE log4j:configuration SYSTEM "http://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/xml/doc-files/log4j.dtd">
<log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/">

   <!-- Appenders -->
   <appender name="console" class="org.apache.log4j.ConsoleAppender">
      <param name="Target" value="System.out" />
      <layout class="org.apache.log4j.PatternLayout">
         <param name="ConversionPattern" value="[%d{yyyy-MM-dd HH:mm:ss}] %-5p: %c - %m%n" />
      </layout>
   </appender>
   
	<!-- log안나올 때 이 부분 있는지 check -->
   <appender name="fileLogger" class="org.apache.log4j.DailyRollingFileAppender">
        <param name="file" value="d://logs//spring//spring.Log"/>
        <param name="Append" value="true"/>
        <param name="dataPattern" value=".yyyy-MM-dd"/>
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="[%d{yyyy-MM-dd HH:mm:ss}] %-5p: %F:%L - %m%n" />
        </layout>
    </appender>
   
   <!-- Application Loggers -->
   <logger name="edu.bit.board">
      <level value="info" />
   </logger>
   
   <!-- 3rdparty Loggers -->
   <logger name="org.springframework.core">
      <level value="info" />
   </logger>
   
   <logger name="org.springframework.beans">
      <level value="info" />
   </logger>
   
   <logger name="org.springframework.context">
      <level value="info" />
   </logger>

   <logger name="org.springframework.web">
      <level value="info" />
   </logger>

   <!-- Root Logger -->
   <root>
      <priority value="info" />
      <appender-ref ref="console" />
      <appender-ref ref="fileLogger"/>
   </root>
   
</log4j:configuration>
```
</details>


### - log4jdbc.log4j2.properties
- 기존 코드 지우고 다시 복붙 , Untilted Text File로 만들면됨
```text
log4jdbc.spylogdelegator.name=net.sf.log4jdbc.log.slf4j.Slf4jSpyLogDelegator
log4jdbc.dump.sql.maxlinelength=0
```
- ※ 위치 주의 : log4j.xml과 같은 위치 
![위치](https://user-images.githubusercontent.com/74290204/105832203-003f7e80-600b-11eb-8640-be80fc31e6ba.PNG)


## ▶ 설정이 다 끝나면 만들어지는 폴더들 
![mvc보드 폴더](https://user-images.githubusercontent.com/74290204/105835073-ab9e0280-600e-11eb-8a09-10b0dbeb8ee2.PNG)



##  @ Mybatis 사용 방법 (4가지 + 1)
### ▶ 어떤 방법을 사용하든 **방법을 통일** 시키는 게 중요! 

### +1 
- @Repository 는 마이바티스가 나오면서 없어진 개념인데 예전에 3종세트 DAO에서 구현할때 **DAO가 @Repository** <br>
→ Service단에서 사용이 가능 (예전에 게시판 만들때처럼)

### 1. SqlSession을 가져와 getMapper사용 

- **interface IBDao를  XML namespace에 매핑** 
```xml
<mapper namespace="edu.bit.ex.one.IBDao"> 
```

- **sqlSession.getMapper(IBDao.class)**를 이용
	
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

<mapper namespace="edu.bit.ex.board.one.IBDao"> <!-- 해당 클래스,인터페이스의 위치 -->

<select id="listDao" resultType="edu.bit.ex.board.BoardVO"> 
<![CDATA[ 
select bId, bName, bTitle, bContent, bDate, bHit, bGroup, bStep, bIndent from mvc_board order by bGroup desc, bStep asc

]]>
</select>
</mapper>
```
```xml
<!--root-context.xml-->
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

### 2. SqlSession 안에 있는 함수 이용 

- interface는 필요가 없음
- SqlSession에서 제공하는 함수(selectList, selectOne등)을 이용함
- **쿼리구현**을 위한 xml이 필요, **해당 xml의 namespace는 개발자가 지정**

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
//Board2.xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="board"> <!-- 개발자가 직접 정의 -->

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

### 3. interface를 통해서 mapper를 가져와 interface를 정의하는 방법 (우리가 쓰던 것)

- root-context.xml에서 mapperLocation이 필요 X
```xml
<property name="mapperLocations" value="classpath:/edu/bit/ex/board/mapper/*.xml" /> 
```

- **root-context.xml**에서 **scan을 사용해서 MyBatis 경로 지정**
```xml
<mybatis-spring:scan
      base-package="edu.bit.ex.board.mapper" />
```
▶ **[주의]** 여기서 base-package="edu.bit.ex"만 주면 순환 참조가 일어나서 (edu.bit.ex로 시작하는 패키지 전부 읽어들임) Service 다형성 적용이 안 일어나는 에러가 발생!

### 4. Mapper Interface → mapper.xml(자손이 구현)하는 방식 
- Mapper Interface에서 xml이 구현하지 않고 **Interface에 @어노테이션으로 사용**
- 단점: pasing같은 한 줄에 들어갈 수 없는 쿼리문은 복잡해짐(간결해지지 못함) 
	- 따라서 간단한것만 사용 (그렇기 때문에 실무에서는 사용할 일이 별로 없음)
- 이 방식을 사용하면 mapper scan의 경로는 필요 없음 

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


