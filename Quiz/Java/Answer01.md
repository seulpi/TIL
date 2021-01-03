# 1. Hello World 를 출력하는 프로그램의 과정을 설명하시오
- 소스코드가 있는 자바파일을 만든다
- cmd에서 자바파일을 컴파일 시킨다
    - javac 파일명.java // JVM이 컴퓨터언어로 바꿔주는 행위가 이루어진다
- 출력을 원하는 결과값을 호출한다 
    - java 파일명 
```
cd Doucuments  
//(cd: change directory) 디렉토리를 documents로 이동한다.

dir /w  
//해당 디렉토리 안에 helloworld.java파일이 있는지 확인한다.

javac HelloWorld.java
//컴파일해서 .class 파일을 생성한다.

dir /w 
//디렉토리에 HelloWorld.class파일이 생성되어있는지 확인한다.

HelloWorld.java 와 HelloWorld.class 
//파일이 모두 있다면 성공적으로 컴파일 된 것이다.
//*해당 문법에 에러가 있으면 .class가 생성되지 않고 에러가 뜬다.

java HelloWorld
// 실행한다.
//java HelloWorld.class가 의도한 것이지만 .class생략 가능하도록 만들어준것
```
<br>

# 2. 명령어 javac / java 를 설명하시오 (기능)
- javac란 .class를 컴파일시켜주는 것 
- java는 파일을 실행하는 것

<br>

# 3. 컴파일이란 무엇인가?
- 컴파일이란 프로그래밍 언어를 CPU가 읽을 수 있는 언어 즉, 이진법으로 바꾸어주는 것

<br>

# 4. java언어를 창시한 사람은?
- 제임스 고슬링

<br>

# 5. JDK란? / 다운로드 경로 / OS별 버전이 다른이유를 설명하시오
- JDK : Java Development tool Kit 
- 다운로드는 오라클 사이트에서 가능
- 운영체제가 다른 사양 어디에서도 사용 가능(범용성이 넓음)

# 6. JVM는 무엇인지 설명하시오 
- JVM ; Java Visual Machine 
    - 자바언어를 CPU가 읽을 수 있는 언어로 바꾸어주는 통역사 같은 프로그램
