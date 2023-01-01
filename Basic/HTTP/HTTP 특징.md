# HTTP 특징
## 1. 클라이언트 서버 구조 
  - Request, Response 구조
  - 독립적인 구조
  - 클라이언트 : UI, UX를 어떻게 그릴지 | 서버 : 로직을 어떻게 할지 분리 
## 2. 무상태 프로토콜 (Stateless) : ***최대한 무상태인 상태로 설계하는게 중요***
    >> Statefule : 상태 유지 (서버가 클라이언트 이전 상태를 보존O) <br>
    >> 노트북 구매시 점원이 구매자의 구매내역을 알고있다 -> **상태유지** <br>
    >> 기존의 점원이 변경되어도 구매자의 구매내역을 상세히 데이터를 넘겨주기 때문에 점원이 변경되어도 문제가 없다<br>(
    >> '노트북 2개를 신용카드로 구매하겠습니다.') -> **무상태**
  - 서버가 클라이언트 상태를 보존하지 X
  - 장점: 서버 확장성 ↑ | 단점: 클라이언트가 추가 데이터 전송
  ```TEXT
    [쓰임새 EX]
    *** 무상태 -> 로그인이 필요없는 단순한 서비스 소개화면 (이벤트 등)
    *** 상태유지 -> 로그인 
    - 로그인한 사용자의 경우 '로그인했다'는 상태를 서버에 유지
    - 일반적으로 쿠키와 세션등을 사용해서 상태 유지
      * 참고 : jwt, oauth
    - 상태유지는 최소한만 사용해야함 -> WHY? 비용이 많이듦
  ```
## 3. 비연결성 (Connections)
  - TCP는 기본적으로 연결을 유지하는 모델
  ```
    # 연결 유지 O 모델 EX)
    클라이언트1 -> 서버요청 (연결O) : 클라이언트1 연결 상태
    클라이언트2 -> 서버요청 (연결O) : 클라이언트1, 클라이언트2 연결 상태
    클라이언트3 -> 서버요청 (연결O) : 클라이언트1, 클라이언트2, 클라이언트3 연결 상태
    => 연결 유지하는 자원은 계속 소모되는 중 
  ```
  - 연결 유지 X 모델(요청을 주고받을 때만 연결 후 연결 종료) : 최소한의 자원만 유지됨 
    >> [단점] <br> 1. TCP/IP연결을 새로 맺어야함 -> 3 way handshake 시간 추가 <br>
                   2. HTML + js, css, img 등 수 많은 자원이 함께 다운로드 -> 자원이 연결될때마다 다운로드하는것은 비효율적임 -> HTTP 지속연결(Persistent Connections)로 문제를 해결<br> (HTTP/2, HTTP/3에서 더 최적화가 됨)
  - **HTTP 는 기본적으로 연결 유지X 모델** 
## 4. HTTP 메세지
  ```text
  [공식 스펙]
  
  HTTP-message =  start-line                      : 시작라인
                  * (header-field CRLF)           : 여러개의 헤더필드들
                  CRLF                            : Enter
                  [ message-body ]      
  ```
  - start-line (시작라인) : request-line(요청메세지 시작라인) / status-line(응답메세지 시작라인)
    >> request-line = method SP request-tartget SP HTTP-version CRLF <br>
    >> status-line = HTTP-version SP status-code SP reason-phrase CRLF <br> * *SP:공백 , CRLF:엔터 <br> 200:성공, 400:클라이언트 오류, 500:서버 내부 오류* 
  - header : HTTP 전송에 필요한 모든 부가정보
    >> header-field = field-name":" OWS field-value OWS <br> * *OWS:띄어쓰기 허용, field-name은 대소문자 구분X* 
  - message body : 실제 전송할 데이터 , byte로 표현할 수 있는 모든 데이터 전송 가능(JSON, 영상 , 문서 etc)
  ```http
    -- HTTP 요청 메세지
    GET/search?q=hello&hl=ko HTTP/1.1               : start-line 시작라인
    Host:www.google.com                             : header 헤더
                                                    : empty line 공백라인(CRLF)
    
    -- HTTP 응답 메세지
    HTTP/1.1 200 OK                                 : start-line 시작라인
    Content-Type: text/html;charset=UTF-8           : header 헤더
    Content-Length: 3423                            : header 헤더    
                                                    : empty line 공백라인(CRLF)
    <html>                                          : message body
      <body>...</body>                              : message body
    </html>                                         : message body
  ```
   - 공백라인은 무조건 있어야한다
   
