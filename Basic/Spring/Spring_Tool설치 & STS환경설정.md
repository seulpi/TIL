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
   - pom.xml 은 개발환경 구축을 위한 툴 /web.xml 부터가 스프링개발
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

### & Maven 다운로드 받아오는 곳 mavenrepository
- https://mvnrepository.com/

![다운1](https://user-images.githubusercontent.com/74290204/105314945-f4664d80-5c01-11eb-8b8e-51582245388b.PNG)

![다운2](https://user-images.githubusercontent.com/74290204/105314949-f5977a80-5c01-11eb-9fc7-9640c14192cd.PNG)

![다운3](https://user-images.githubusercontent.com/74290204/105314951-f6301100-5c01-11eb-90c9-ee6dab1311c5.PNG)

![다운4](https://user-images.githubusercontent.com/74290204/105314953-f6301100-5c01-11eb-9821-8aeb03309be6.PNG)

## @ STS에서 Spring MCV 생성하는 방법
### new → Spring Legacy Project → Spring MVC Project

![캡처](https://user-images.githubusercontent.com/74290204/105311590-cfbda600-5c00-11eb-855a-9118f3e84069.PNG)

![1](https://user-images.githubusercontent.com/74290204/105305122-48236780-5bff-11eb-975e-73e7e585234f.PNG)

![2](https://user-images.githubusercontent.com/74290204/105305141-48bbfe00-5bff-11eb-83f6-c39e835c5894.PNG)

![3](https://user-images.githubusercontent.com/74290204/105305149-49549480-5bff-11eb-8994-564df127fe9d.PNG)

## @ 롬복 설치 : getter&setter함수, 생성자등을 자동으로 생성해주는 어플리케이션

- 설치하는 방법
https://medium.com/@dlaudtjr07/spring-boot-lombok-%EA%B0%9C%EB%85%90-%EB%B0%8F-%EC%84%A4%EC%B9%98-71f9dbbc2f42

- 설치링크 
https://projectlombok.org/download

1. 다운로드 후  cmd
```
cd C:\Users\AI\Downloads (다운로드 받은 곳) 엔터 
C: C:\Users\AI\Downloads> java -jar lombok.jar
```

2. location 못 잡으면 이클립스, sts 바로가기 우클릭 눌러서 속성 - 대상(T) 에 경로 복사 붙여넣기 

3. 다시 재부팅

4. pom.xml에 dependency 추가 

	- 버전을 맞춰줌(다운로드한 버전) 1.18.16인데 다운로드 받는 곳에 올라와있는지 확신할 수 없어서 1.18.0으로 일단 맞춰줘도됨(학원ver)
 - <dependencies> : 안에 넣어야 됨
  ```xml
  <dependency>
   <groupId>org.projectlombok</groupId>
   <artifactId>lombok</artifactId>
   <version>1.18.0</version>
   <scope>provided</scope>
  </dependency>
      ```
5, @Data로 annotation을 붙여주면 변수에 대한 getter&setter, 생성자, equals함수, hashcode까지 만들어줌 <br>
→ 좋은 반면에 종종 에러가 남

![롬복](https://user-images.githubusercontent.com/74290204/105699071-26511a00-5f4a-11eb-8915-bf3124423acf.PNG)


