# STS3 (이클립스 + 스프링 기능을 최적화 한 TOOL)

### [다운로드 링크] https://github.com/spring-projects/toolsuite-distribution/wiki/Spring-Tool-Suite-3\

![1](https://user-images.githubusercontent.com/74290204/104861323-b99ab600-5972-11eb-8c4e-d81e01122d70.PNG)

![2](https://user-images.githubusercontent.com/74290204/104861326-bb647980-5972-11eb-819c-84ce0130aa11.PNG)

![3](https://user-images.githubusercontent.com/74290204/104861328-bbfd1000-5972-11eb-977c-ef4b9a76715c.PNG)

## @ 자동 완성 기능 안될때
### [자동완성하는 방법 링크] https://blog.naver.com/njw1204/221654918683
- install new software → 
http://oss.opensagres.fr/tern.repository/1.2.0/를 복사 붙여넣기
>> 혹시, 중간에 Security Warning이 뜨면 Install anyway를 클릭해주면된다

- ▶ 적용할 프로젝트 우클릭 → Configure → Convert to tern Project
 → Browser체크, CKEditor체크, jQuery 체크 (프로젝트마다 해줘야함) : 내 노트북에서 설정 ok

- 톰캣 설정 다시 잡아줘야함 : 내 노트북에서 설정 ok
<br>

## @ import 하는 방법 ▶ eclips와 동일
![import](https://user-images.githubusercontent.com/74290204/104990858-d31a2b80-5a60-11eb-8e05-5dc5ed087573.PNG)

## @ 환경설정 
- new → Spring Legacy Project → Simple Spring Maven 
- new → Spring Legacy Project → Simple Java (위로 하니 spring 폴더로 안생겨서 학원에서 이렇게 설정함)
   - 우클릭 → config → converr to maven project (maven project로 바꿈)
   - 우클릭  → Spring  → Add Spring Project Nature (Spring으로 S자 생김)
```
[참고]
폴더에 S자붙으면 Spring이란 뜻
폴더에 M, S 붙은거 Maven Project란 뜻 
```
- **Maven**은 Spring과 아무런 연관이 없지만 **라이브러리를 자동으로 받아줄수있게 해줌** 따라서 Maven을 build tool(라이브러리 다운로드~ 배포까지)이라 부름 
   - Maven을 관리하는 프로젝트 = pom.xml
   - pom에 태그를 붙여줘야 Maven이 자동으로 라이브러리를 다운받을 수 있음, 태그 복붙은 ↓ 아래와 같음 
   ```xml
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0       https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>spring_student</groupId>
    <artifactId>spring_student</artifactId>
    <version>0.0.1-SNAPSHOT</version>
  
    <properties>
     
     <!-- Generic properties -->
      <java.version>1.6</java.version>
      <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
      <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>

      <!-- Spring -->
      <spring-framework.version>3.2.3.RELEASE</spring-framework.version>

      <!-- Hibernate / JPA -->
      <hibernate.version>4.2.1.Final</hibernate.version>

      <!-- Logging -->
      <logback.version>1.0.13</logback.version>
      <slf4j.version>1.7.5</slf4j.version>

      <!-- Test -->
      <junit.version>4.11</junit.version>

   </properties>
   
   <dependencies> <!-- dependency는 다 라이브러리 이름 -->
      <!-- Spring and Transactions -->
      <dependency>
         <groupId>org.springframework</groupId> <!-- org.springfanmework에서 라이브러리를 다운로드 자동으로 받아줌 -->
         <artifactId>spring-context</artifactId>
         <version>${spring-framework.version}</version>
      </dependency>
      <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-tx</artifactId>
         <version>${spring-framework.version}</version>
      </dependency>

      <!-- Logging with SLF4J & LogBack -->
      <dependency>
         <groupId>org.slf4j</groupId>
         <artifactId>slf4j-api</artifactId>
         <version>${slf4j.version}</version>
         <scope>compile</scope>
      </dependency>
      <dependency>
         <groupId>ch.qos.logback</groupId>
         <artifactId>logback-classic</artifactId>
         <version>${logback.version}</version>
         <scope>runtime</scope>
      </dependency>

      <!-- Hibernate -->
      <dependency>
         <groupId>org.hibernate</groupId>
         <artifactId>hibernate-entitymanager</artifactId>
         <version>${hibernate.version}</version>
      </dependency>

      
      <!-- Test Artifacts -->
      <dependency> 
         <groupId>org.springframework</groupId>
         <artifactId>spring-test</artifactId>
         <version>${spring-framework.version}</version>
         <scope>test</scope>
      </dependency>
      <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>${junit.version}</version>
         <scope>test</scope>
      </dependency>

   </dependencies>   
  

  
  <build>
    <sourceDirectory>src</sourceDirectory>
    <plugins>
      <plugin>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.8.1</version>
        <configuration>
          <release>15</release>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>
