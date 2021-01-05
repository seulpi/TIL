# 경로 : 절대 경로와 상대경로로 나뉨 

## @ 절대 경로  
- root(경로의 첫 시작점) 에서 시작하는 경로, 최상위 경로부터 모든 경로를 표시하는 것
- 표현방식 :  C:\ , http:// , /

## @ 상대 경로 : 절대 경로가 아닌 것
- 상대적인 경로는 2가지가 존재

1. 물리적인 경로(컴퓨터) 
>> C:\Users\AI\eclipse-workspace\class_ex\WebContent

2. 웹사이트에서 치고 들어올 때의 경로 (이 경로도 2가지가 존재)
>> http:// ip/context명class_ex/ ← / 는 WebContent
  1. 물리적인 경로 
  ```
  http://ip/class_ex/WebContent/request_send.jsp
  ```
  2. 유저가 치고 들어올 때 
  ```
  http://ip/class_ex/request_send.jsp 
  ```

  ```jsp
  // 원리

  WebContent안에 0104파일을 하나 생성했다
  http://ip/class_ex/0104/request_send.jsp

  /0104/request_send.jsp 는 절대경로! / 있으니까 
  상대경로는 0104/request_send.jsp

  /*- action안에는 상대 경로를 기준으로 적어주면 됨 */

  --------------------------------------------------------------------------------
  // if 0105에 있는 파일에서 0104에있는 파일로 경로를 주고 싶다

  WebContent 폴더에는 0104 폴더와 0105폴더가 있고
  0104폴더에는 request_send.jsp 0105폴더에는 requestex.html 파일이 있다고 가정
  
  즉, requestex.html -> request_send.jsp로 action 디렉토리를 향할려면 0105 폴더의 상위인 WebContent 폴더로 이동해야하므로 '../' 상위키워드를 쓴다 
  왜냐하면 request_send.jsp 폴더가 0104 폴더 안에 있기 때문 
  따라서, ../0104/request_send.jsp가 된다.  
  
  1. 0104/request_send.jsp 라고 디렉토리 경로를 주면 
  상대경로의 기준은 내가 속해있는 폴더가 기준이기 때문에 (http://ip/class_ex/0104/ 가 기준)
  http://ip/class_ex/0104/0104/request_send.jsp 가 되기 때문에 경로 틀림
  
  2. /0104/request_send.jsp 라고 디렉토리 경로를 주면 '절대경로'
  절대 경로라는것은 디폴트로 http://ip 붙여준다 (context명 안붙임)
  그래서 class_ex/0104/request_send.jsp (컨텍스트명까지 경로 지정해줘야한다 안그러면 http://ip/0104/request_send.jsp 이렇게 접속하는것과 똑같은 것)
  
  // request.getURI() : 절대경로 만들어주는것 
  따라서 <form action = "request.getURI()/0104/request_send.jsp"> 로 사용하기도 한다
  ```
