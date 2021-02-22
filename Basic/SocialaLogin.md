# KAKAO LOGIN
>> [참조링크] https://daddyprogrammer.org/post/1012/springboot2-rest-api-social-login-kakao/

## ▶ 소셜로그인 원리
>> [kakaodeveloper] https://developers.kakao.com/docs/latest/ko/kakaologin/common
![소셜로그인 원리](https://user-images.githubusercontent.com/74290204/106353008-bc11ee00-632a-11eb-9ac5-d996268cc46c.png)

## 1. pom.xml (필요한 dependebcy 추가)
```xml
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
```

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



# Google LOGIN
>>[참조 링크] https://m.blog.naver.com/PostView.nhn?blogId=sam_sist&logNo=220969414214&proxyReferer=https:%2F%2Fwww.google.com%2F

## 1. pom.xml - dependency 추가
```xml
   <!-- google -->
	<dependency>
	    <groupId>org.springframework.social</groupId>
	    <artifactId>spring-social-google</artifactId>
            <version>1.0.0.RELEASE</version>
    </dependency>
```

## 2. servlet-context.xml에 url, id, secret 객체 생성
```xml
<!-- google Class Bean설정 추가 -->
	<!-- 클라이언트ID와 보안비밀 세팅 -->
	<beans:bean id="googleConnectionFactory"
		class="org.springframework.social.google.connect.GoogleConnectionFactory">
		<beans:constructor-arg 
			value="YOUR_CLIENT_ID" /> 
		<beans:constructor-arg
			value="YOUR_CLIENT_SECRET" />
	</beans:bean>
	
	<!-- 승인된 자바스크립트 원본과 승인된 리디렉션 URI -->
	<beans:bean id="googleOAuth2Parameters"
		class="org.springframework.social.oauth2.OAuth2Parameters">
		<beans:property name="scope"
			value="https://www.googleapis.com/auth/plus.login" />
		<beans:property name="redirectUri"
			value="http://localhost:8282/ex/oauth2callback" />
	</beans:bean>
```

## 3. Cotroller
```java
package edu.bit.ex.controller;

import java.io.IOException;

import javax.servlet.http.HttpSession;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.social.google.connect.GoogleConnectionFactory;
import org.springframework.social.oauth2.GrantType;
import org.springframework.social.oauth2.OAuth2Operations;
import org.springframework.social.oauth2.OAuth2Parameters;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller
public class GoogleLoginController {
	
	
	/* GoogleLogin */
	@Autowired
	private GoogleConnectionFactory googleConnectionFactory;
	
	@Autowired
	private OAuth2Parameters googleOauth2Parameters;
	
	//로그인 첫 화면 요청
	@RequestMapping("/google")
	public String login(Model model, HttpSession session) {
		
		//구글 code 발행
		OAuth2Operations oAuth2Operations = googleConnectionFactory.getOAuthOperations();
		String url = oAuth2Operations.buildAuthenticateUrl(GrantType.AUTHORIZATION_CODE, googleOauth2Parameters);
		System.out.println("구글 url:" +url);
		
		model.addAttribute("google_url", url);
		
		return "googleLogin";
	}
	
	//구글 callback 호출
	@RequestMapping("/oauth2callback")
	public String googleCallback(Model model, @RequestParam String code) throws IOException {
		System.out.println("callback 실행");
		
		return "googleSuccess";
	}
}
```

## 4. View
### -*googleLogin*
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
<title>구글 로그인</title>

<script type="text/javascript" src="https://static.nid.naver.com/js/naverLogin_implicit-1.0.2.js" charset="utf-8"></script>
  <script type="text/javascript" src="http://code.jquery.com/jquery-1.11.3.min.js"></script>
  <style type="text/css">
	  html, div, body,h3{
	  	margin: 0;
	  	padding: 0;
	  }
	  h3{
	  	display: inline-block;
	  	padding: 0.6em;
	  }
	  </style>
</head>
<body>
<div style="background-color:#15a181; width: 100%; height: 50px;text-align: center; color: white; "><h3>SIST Login</h3></div>
<br>
<!-- 구글 로그인 화면으로 이동 시키는 URL -->
<!-- 구글 로그인 화면에서 ID, PW를 올바르게 입력하면 oauth2callback 메소드 실행 요청-->
<div id="google_id_login" style="text-align:center">
<a href="${google_url}"><img width="230" src="https://img1.daumcdn.net/thumb/R1280x0.fjpg/?fname=http://t1.daumcdn.net/brunch/service/user/5rH/image/aFrEyVpANu07FvoBZQbIB4aF_uc"/></a></div>

</body>
</html>
```


