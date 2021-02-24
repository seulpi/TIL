# CSRF (Cross Site Request Forgery) : 서버 공격 (사이트 간 위조 요청)
- CSRF는 시큐리티가 공식적으로 지원
- 해킹의 최종 목적 : admin 계정 탈취, 따라서 해당 시스템 버전 정보에 취약점을 공격한다

## @ 공격 원리 
![csrf](https://user-images.githubusercontent.com/74290204/109007943-0dc25400-76f0-11eb-91fd-e7c8a80a6241.png)

- 로그인 → 해커가 고객에게 링크가 담긴 e-mail을 보냄 → 해커가 보낸 링크 클릭하면 링크의 서버로 이동 → 다이렉트로 서버로 들어가 유저 정보 털기 
    - **해커가 아는건 parameter!** 'www.naver.com/id=해커본인꺼&pw=9080'
    - get방식으로 서버로 보내게 되면 id와 pw를 생성시킨다는 url을 아는 것 
    - admin의 id와 pw는 모름

## @ 해결 방법
### 1. CAPCHAR 
- 비밀번호 여러 번 틀렸을 때 **문자열로 본인확인**하는 방법 
![capchar](https://user-images.githubusercontent.com/74290204/108649042-e6a53000-74ff-11eb-9421-e32ccff49e7a.PNG)

>>[출처] https://developers.naver.com/products/captcha/

### 2. Security
- **서버 쪽에서 password를 한 개 더 부여**  → password를 알아야 접근 할 수 있게 한 방법(전에는 링크 클릭한번으로 털리기 때문!)
```js
name="${_csrf.parameterName}" value="${_csrf.token}" 
```
- 스프링 시큐리티가 알아서 자동으로 심어줌 → **링크 클릭했을 때 패스워드 '(_csrf.token)'를 통해 요청 사이트를 한번 더 체크함** → password 다르게 되면 서버쪽에서 요청을 받지 X → post 발생할때마다(페이지마다)  _csrf.token 발행
<br> ▶ 발행하는 password를 가지고 있지 않기 때문에(해커들은 url만 알고 있기 때문) *reject = 사이트 위조 방지* 

## @ 사용 방법
### - security 
- security-custom-context.xml
```xml
 <http>	
     	<csrf disabled="true"/> <!-- true = csrf를 적용하지 않겠다, 따라서 default=false -> 생략을 하더라도 false로 적용 --> 
 </http> 
```

- **'POST' 방식에서만 적용됨** : 코드에서 적용되는 것 form 태그의 method , ajax 
    - ① form 형식의 CSRF 적용 방법
    ```jsp
    <%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
    <!-- 이 태그를 묶게되면 내가 적어주지 않아도 넣어줌 , 안넣으면 ▼ 밑에처럼 코드 처리를 반드시 해줘야함, 따라서 이 태그 라이브러리로 묶으면 주석처리해도 되는게 자기가 알아서 넣어줌 
    (get일때는 전혀 상관이 없고 post로 보낼땐 token hidden으로 묶어서 같이 보내게 되어있음)-->

    <input type="hidden" name="${_csrf.parameterName}" value="${_csrf.token}" />
    ```

    - ② ajax일 때 CSRF 적용 방법
        - header에 실어서 보내야함 
        - beforeSend 사용
        - ajax beforeSend***(데이터를 전송하기 전)에 HTTP Header를 설정**
        ```jsp
        <!DOCTYPE html>
        <html xmlns="http://www.w3.org/1999/xhtml" xmlns:th="http://www.thymeleaf.org">
        <head>
        <meta name="_csrf" th:content="${_csrf.token}"/>
        <meta name="_csrf_header" th:content="${_csrf.headerName}"/>
        <title>CSRF TEST</title>
        </head>

        <body>
        <!--AJAX Submit-->
        <div class="panel-heading"></div>
        <input autocomplete="off" type="text" class="form-control" id="userName" name="name">
        <button class="btn btn-md btn-danger btn-block" id="ajaxCall" type="button">AJAX Call</button> 
        </body>

        <script type="text/javascript">

            $(function() {
                $("#ajaxCall").click(function(){
                    ajaxCall();
                });
            });
            
            function ajaxCall(){
                var token = $("meta[name='_csrf']").attr("content");
                var header = $("meta[name='_csrf_header']").attr("content");
                var name = $("#userName").val();
                
                var jsonData = {
                    "name" : name
                }
                
                $.ajax({
                    type: 'POST',
                    contentType: "application/json",
                    url:'/csrf/ajax',
                    data: JSON.stringify(jsonData), // String -> json 형태로 변환
                    beforeSend : function(xhr)
                    {   /*데이터를 전송하기 전에 헤더에 csrf값을 설정한다*/
                        xhr.setRequestHeader(header, token);
                    },
                    dataType: 'json', // success 시 받아올 데이터 형
                    async: true, //동기, 비동기 여부
                    cache :false, // 캐시 여부
                    success: function(data){
                        console.log(data.name);
                    },
                    error:function(xhr,status,error){
                        console.log('error:'+error);
                    }
                });
            }
        </script>
        ```

        >> [csrf_ajax 설정 참조] https://hyunsangwon93.tistory.com/28

# XSS
# 동일 출처원칙
# Sql injection($, #)

---

# 로그(log)
## @ 설정 
- 로깅 레벨 설정을 *'info'* 로 했을 경우 *'TRACE'* , *'DEBUG'* 레벨은 무시한다
```xml
<logger name="edu.bit.ex">
		<level value="info" />
	</logger>
```
1. jdbc를 통해 해당 로그 받기 
2. root-context.xml
```xml
<property name="driverClassName" value="net.sf.log4jdbc.sql.jdbcapi.DriverSpy"></property>
```
3. properties 설정 (log4jdbc.log4j2.properties)
```properties
log4jdbc.spylogdelegator.name=net.sf.log4jdbc.log.slf4j.Slf4jSpyLogDelegator
log4jdbc.dump.sql.maxlinelength=0
```
4. 로그 레벨 조절, log4j.xml (↓ 코드 Root Logger 위에 복붙)
```xml
 <!-- 예를 들어 로깅 레벨 설정을 "INFO"로 하였을 경우 "TRACE", "DEBUG" 레벨은 무시한다. -->

   <!-- 출력되는 로그의 양 순서 : ERROR < WARN < INFO < DEBUG < TRACE -->
   <!-- com.freedy.sample 하위 패키지에서 로그설정 -->
   <!-- additivity가 false인 경우 상위로거의 설정값을 상속받지 않는다. -->
   
   <!--  
    - jdbc.sqlonly : SQL문만을 로그로 남기며, PreparedStatement일 경우 관련된 argument 값으로 대체된 SQL문이 보여진다. 
    - jdbc.sqltiming : SQL문과 해당 SQL을 실행시키는데 수행된 시간 정보(milliseconds)를 포함한다. 
    - jdbc.audit : ResultSet을 제외한 모든 JDBC 호출 정보를 로그로 남긴다. 
    - jdbc.resultset : ResultSet을 포함한 모든 JDBC 호출 정보를 로그로 남긴다.
    - jdbc.resultsettable : SQL 결과 조회된 데이터의 table을 로그로 남긴다. 
    -->
   
      <!-- SQL Logger -->

   <logger name="jdbc.sqltiming" additivity="false">
      <level value="warn" />
      <appender-ref ref="console" />
   </logger>

   <logger name="jdbc.sqlonly" additivity="false">
      <level value="info" />
      <appender-ref ref="console" />
   </logger>

   <logger name="jdbc.audit" additivity="false">
      <level value="warn" />
      <appender-ref ref="console" />
   </logger>

   <logger name="jdbc.resultset" additivity="false">
      <level value="warn" />
      <appender-ref ref="console" />
   </logger>

   <logger name="jdbc.resultsettable" additivity="false">
      <level value="info" />
      <appender-ref ref="console" />
   </logger>
```
   - 로그 적용 안시키려면 <appender-ref ref="fileLogger"/> 만 주석 처리
   ```xml
   <root>
      <priority value="info" />
      <appender-ref ref="console" />
      <appender-ref ref="fileLogger"/> <!-- 주석처리-->
   </root>
    ```
