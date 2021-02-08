# KAKAO LOGIN
>> [참조링크] https://daddyprogrammer.org/post/1012/springboot2-rest-api-social-login-kakao/

## ▶ 소셜로그인 원리
>> [kakaodeveloper] https://developers.kakao.com/docs/latest/ko/kakaologin/common
![소셜로그인 원리](https://user-images.githubusercontent.com/74290204/106353008-bc11ee00-632a-11eb-9ac5-d996268cc46c.png)

## 1. pom.xml (필요한 dependebcy 추가)
<details><summary>dependency 추가 </summary>
	
	```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/maven-v4_0_0.xsd">
		<modelVersion>4.0.0</modelVersion>
		<groupId>edu.bit</groupId>
		<artifactId>ex</artifactId>
		<name>social_login_test</name>
		<packaging>war</packaging>
		<version>1.0.0-BUILD-SNAPSHOT</version>

		  <properties>
	      <java-version>1.8</java-version>
	      <org.springframework-version>5.0.7.RELEASE</org.springframework-version>
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

	      <!-- https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient -->
			<dependency>
			    <groupId>org.apache.httpcomponents</groupId>
			    <artifactId>httpclient</artifactId>
			    <version>4.5.13</version>
			</dependency>

			<!-- https://mvnrepository.com/artifact/com.googlecode.json-simple/json-simple -->
			<dependency>
			    <groupId>com.googlecode.json-simple</groupId>
			    <artifactId>json-simple</artifactId>
			    <version>1.1.1</version>
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

## 2. Controller
```java
package edu.bit.ex;

import java.util.HashMap;
import java.util.Map;

import org.json.simple.JSONObject;
import org.json.simple.parser.JSONParser;
import org.json.simple.parser.ParseException;
import org.springframework.http.HttpEntity;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpMethod;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.util.LinkedMultiValueMap;
import org.springframework.util.MultiValueMap;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.client.RestTemplate;

import edu.bit.ex.util.HttpClientManager;

/**
 * Handles requests for the application home page.
 */
@Controller
public class SocialController {
	// 1. 안드로이드&IOS : 네이티브앱 → 핸드폰관련 2. REST API 키 : 내부서버에서 동작할수 있게 해주는 키
	private static final String CLIENT_ID = "YOUR_CLIENT_ID"; // REST API 키 : 내부서버에서 동작할수 있게 해주는 키
	private static final String REDIRECT_URL = "YOUR_CLIENT_REDIRECT_URL";

	@GetMapping("/login")
	public String login() {
		String requestUrl = String.format(
				"https://kauth.kakao.com/oauth/authorize?client_id=%s&redirect_uri=%s&response_type=code", CLIENT_ID,
				REDIRECT_URL);
		return "redirect:" + requestUrl;
	}

	@GetMapping(value = "/login/callback") // url 맵핑은 위에 redirect url과 주소가 같아야하고 developer에도 redirect url로 맞춰줘야함
	public String redirect(@RequestParam(name = "code", required = false) String code, Model model) {
		/*
		 * Map<String, String> params = new HashMap<String, String>(); // 문서>카카오 로그인>
		 * REST API params.put("grant_type", "authorization_code"); //grant_type은
		 * authorization_code로 설정하라고 카카오에서 정리해줘서 이렇게 설정한것 params.put("client_id",
		 * CLIENT_ID); params.put("client_secret", "YOUR_CLIENT_SECRET");
		 * Client Secret (카카오디벨롭퍼에 내 토큰) : 토큰 발급 시, 보안을 강화하기 위해 Client Secret을 사용할 수
		 * 있습니다. (REST API인 경우에 해당) 내 애플리케이션>제품 설정>카카오 로그인> 보안
		 * 
		 * params.put("code", code); params.put("redirect_uri", REDIRECT_URL);
		 */
		// https://daddyprogrammer.org/post/1012/springboot2-rest-api-social-login-kakao/
		HttpHeaders headers = new HttpHeaders();
		headers.setContentType(MediaType.APPLICATION_FORM_URLENCODED);

		MultiValueMap<String, Object> params = new LinkedMultiValueMap<>();
		params.set("grant_type", "authorization_code");
		params.set("client_id", CLIENT_ID);
		params.set("client_secret", "YOUR_CLIENT_SECRET");
		params.set("code", code);
		params.set("redirect_uri", REDIRECT_URL);

		HttpEntity<MultiValueMap<String, Object>> restRequest = new HttpEntity<>(params, headers);
		RestTemplate restTemplate = new RestTemplate();

        String result = restTemplate.postForObject("https://kauth.kakao.com/oauth/token", restRequest, String.class);
        
        JSONParser parser = new JSONParser();
        try {
            JSONObject object = (JSONObject) parser.parse(result);
            if (object.containsKey("access_token")) {
                String accessToken = (String) object.get("access_token");
                Map<String, Object> map = getProfile(accessToken);
                
                model.addAttribute("id", map.get("id"));
                model.addAttribute("nickname", map.get("nickname"));
                model.addAttribute("email", map.get("email"));
            }
        } catch (Exception e) {
            System.out.println("json parsing error");
        }

		return "info";
	}
	
	// 사용자 정보 가져오기
    private Map<String, Object> getProfile(String accessToken) {
        Map<String, Object> map = new HashMap<>();

        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.APPLICATION_FORM_URLENCODED);
        headers.set("Authorization", "Bearer " + accessToken);

        HttpEntity<String> entity = new HttpEntity<String>(headers);
        RestTemplate rest = new RestTemplate();
        ResponseEntity<String> result = rest.exchange("https://kapi.kakao.com/v2/user/me", HttpMethod.GET, entity, String.class);
        
        if (result.getStatusCode() == HttpStatus.OK) {
            result.getBody();
            JSONParser parser = new JSONParser();
            try {
                JSONObject object = (JSONObject) parser.parse(result.getBody());
                if (object.containsKey("id")) {
                    map.put("id", object.get("id"));
                }
                if (object.containsKey("kakao_account")) {
                    JSONObject accountObj = (JSONObject) object.get("kakao_account");
                    if (accountObj.containsKey("profile")) {
                        JSONObject profileObj = (JSONObject) accountObj.get("profile");
                        map.put("nickname", profileObj.get("nickname"));
                    }
                    if (accountObj.containsKey("email")) {
                        map.put("email", accountObj.get("email"));
                    }
                }
            } catch (ParseException e) {
                System.out.println("json parsing error");
            }
        }

        return map;
    }
}
```

## 3. View // 유저 정보 받아오는 값으로 view페이지 뿌리는거 해보기(**님 환영합니다)
```jsp
//home.jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%@ page session="false" %>
<html>
<head>
	<title>Home</title>
</head>
<body>
<h1>
	Hello world!  
</h1>

<P>  The time on the server is ${serverTime}. </P>
<a href="/ex/login">KAKAO LOGIN</a>
</body>
</html>
```
```jsp
//info.jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%@ page session="false" %>
<html>
<head>
	<title>Home</title>
</head>
<body>
<h1>
	Hello world!  
</h1>

<P>  success </P>
</body>
</html>
```

## ▶ 출력 
![1](https://user-images.githubusercontent.com/74290204/106352964-6d645400-632a-11eb-9ebe-99b697529ec5.PNG)

![2](https://user-images.githubusercontent.com/74290204/106352968-6e958100-632a-11eb-92f5-ed7d393f33cc.PNG)

![3](https://user-images.githubusercontent.com/74290204/106352970-6f2e1780-632a-11eb-812c-ad5c2ff1c476.PNG)


# NAVER LOGIN
>> [참조링크] https://yoo-hyeok.tistory.com/88

## 1. naverLogin.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@page import="java.net.URLEncoder" %>
<%@page import="java.security.SecureRandom" %>
<%@page import="java.math.BigInteger" %>
<%@page contentType="text/html; charset=UTF-8" language="java" %>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>네이버 로그인</title>


</head>

<%
String clientId = "YOUR_CLIENT_ID";
String redirectURI =URLEncoder.encode("YOUR_CLIENT_CALLBACK_URL", "UTF-8");

SecureRandom random = new SecureRandom();

String state = new BigInteger(130, random).toString();

String apiURL = "https://nid.naver.com/oauth2.0/authorize?response_type=code";
apiURL += "&client_id=" + clientId;
apiURL += "&redirect_uri=" + redirectURI;
apiURL += "&state=" + state;

session.setAttribute("state", state);
%>
<a href="<%=apiURL%>"><img height ="50" src="http://static.nid.naver.com/oauth/small_g_in.PNG"/></a>
<body>

</body>
</html>
```

## 2. callback.jsp
```jsp
<%@page import="org.apache.ibatis.reflection.SystemMetaObject"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@page import="java.net.URLEncoder" %>
<%@page import="java.net.URL" %>
<%@page import="java.net.HttpURLConnection" %>
<%@page import="java.io.BufferedReader" %>
<%@page import="java.io.InputStreamReader" %>
<%@page contentType="text/html; charset=UTF-8" language="java" %>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>네이버 로그인</title>
</head>

<%

	String clientdId = "YOUR_CLIENT_ID"; 
	String clientSecret = "YOUR_CLIENT_SECRET";
	String code = request.getParameter("code");
	String state = request.getParameter("state");
	String redirectURI = URLEncoder.encode("YOUR_CLIENT_CALLBACK_URL", "UTF-8");
	
	String apiURL;
	
	apiURL = "https://nid.naver.com/oauth2.0/token?grant_type=authorization_code&";
	apiURL += "client_id=" + clientdId;
	apiURL += "&client_secret=" + clientSecret;
	apiURL += "&redirect_uri=" + redirectURI;
	apiURL += "&code=" + code;
	apiURL += "&state=" + state;
	
	String access_token = "";
	String refresh_token ="";
	System.out.println("apiURL=" + apiURL);
	
	try {
		URL url = new URL(apiURL);
		HttpURLConnection con = (HttpURLConnection)url.openConnection();
		con.setRequestMethod("GET");
		
		int responseCode = con.getResponseCode();
		
		BufferedReader br;
		System.out.println("responseCode=" + responseCode);
		
		if(responseCode == 200) {
			br = new BufferedReader(new InputStreamReader(con.getInputStream()));
		} else {
			br = new BufferedReader(new InputStreamReader(con.getErrorStream()));
		}
		
		String inputLine;
		StringBuffer res = new StringBuffer();
		
		while((inputLine = br.readLine()) != null) {
			res.append(inputLine);
		}
		br.close();
		
		if(responseCode == 200) {
			out.print(res.toString());
		}
		
	}catch (Exception e) {
		System.out.println(e);
}	
%>


<body>

</body>
</html>
```

## 3. LoginController.java
```java
package edu.bit.ex.controller;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpSession;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import lombok.AllArgsConstructor;
import lombok.extern.log4j.Log4j;

@Controller
@AllArgsConstructor
@Log4j
public class LoginController {
	
	@RequestMapping(value="/testLogin")
	public String isComplete(HttpSession session) {
		return "naverLogin";
	}
	
	@RequestMapping(value="/callback")
	public String naverLogin(HttpServletRequest request) throws Exception {
		return "callback";
	}
	
	@RequestMapping(value="/pesonalInfo")
	public void personalInfo(HttpServletRequest request) throws Exception {
		String token = "YOUR_CLIENT_TOKEN";
		String header = "Bearer " + token;
		
		try {
			String apiURL ="https://openapi.naver.com/v1/nid/me";
			
			URL url = new URL(apiURL);
			HttpURLConnection con = (HttpURLConnection)url.openConnection();
			con.setRequestMethod("GET");
			con.setRequestProperty("Authorization", header);
			
			int responseCode = con.getResponseCode();
			
			BufferedReader br;
			
			if(responseCode == 200) {
				br = new BufferedReader(new InputStreamReader(con.getInputStream()));
			}else {
				br = new BufferedReader(new InputStreamReader(con.getErrorStream()));
			}
			
			String inputLine;
			StringBuffer response = new StringBuffer();
			
			while((inputLine = br.readLine()) != null) {
				response.append(inputLine);
			}
			
			br.close();
			System.out.println(response.toString());
		} catch (Exception e) {
			System.out.println(e);
		}
	}
}
```

## ▶ 출력 
![11111111111](https://user-images.githubusercontent.com/74290204/106881792-50ac8f80-6721-11eb-9e57-9b4c5a6f625d.PNG)
