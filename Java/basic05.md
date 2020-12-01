# 메소드(=함수 =function)
## 정의 : 함수를 만드는 것 / call = invoke : 함수 호출
### public static void main(String[ ] args) { }

* main ; 함수명
* void ; 리턴타입 존재하지않음(값을 반환하지않는다)
* void main(String[ ] args) { } ; 함수범위
* ( ) ; 대괄호 있으면 함수
* { } ; 함수 만드는 부분에는 중괄호 존재하고 실행(구현)되는 부분있음 / 호출 하는 부분에서는 값만 적어주고 구현하는 부분에서는 변수 선언한다(메모리할당)
* String[] args ; 매개변수 = 파라미터 = 인자 (여러개여도 상관없고 ★ 매개변수 자리에는 변수선언/초기화X★)
* 함수 안에 함수 존재(X) , class안에 존재 , 파라미터 안에 바로 변수 선언하면서 값 할당(X)
```java
public static void main(String[] args) {  // main메소드는 프로그램의 시작이자 끝
	System.out.println("프로그램의 시작");
	hiEveryone(12); // 함수 호출 , 함수 실행 , '12'라는 값(value)할당
	hiEveryone(13);
	System.out.println("프로그램의 끝");
	}
	
public static void hiEveryone(int age) {  // 매개변수 자리에 int age 변수선언 , 함수만든것
	System.out.println("좋은 아침입니다");
	System.out.println("제 나이는 " + age + "세 입니다");
}

/* 값들이 한번 실행되고(호출한번하고) 나면 방 안에 값 사라짐 
그래서 호출할때 값 여러개(hiEveryone(12); hiEveryone(13);)를 호출가능!!	
======================================================================
▶ 
프로그램의 시작
좋은 아침입니다
제 나이는 12세 입니다
좋은 아침입니다
제 나이는 13세 입니다
프로그램의 끝
```
* 메소드는 입력만 있을수도 있고 없을수도 있음
```java
pubilc static void byEveryone( ) {
	System.out.println("다음에 뵙겠습니다.");
}	// 이게 입력 비워두는것! ( ) 안에 비워두면 입력이 없는것 
```
* return : 값의 반환(메소드 호출문을 그 결과로 대체한다)과 동시에 종료
```java
public static void main(String[] args) {
	square(2.5, 3.5);	
}
	
public static double square(double width, double height) {
		
	double squareArea = width*height;
	System.out.println(squareArea);
	return squareArea; // return값은 함수의 데이터타입과 일치해야함
```
		

## 함수를 써야하는 이유와 언제 쓰는지에 대한 궁금증

* 소스의 길이가 엄청 길어짐 (같은 코드를 반복) → 똑같은 소스 코드 2번 이상 들어갈때 함수 사용(;똑같은 기능이 2번 이상 중복될때)
* 하나의 함수는 한개의 기능을 수행해야함  

<br>


# 재귀함수
* 함수안에서 자신을 호출하는 것
```java
public static int factorial(int n) {
	if(n ==1)
		return 1;   //반드시 종료조건 있어야함! 조건 없으면 무한루프 돎
	else
		return n * factorial(n-1);
```
* ex_재귀함수로 2ⁿ 구하기
```java
public static int powerOfTwo(int n) {
	if(n == 0) { 
    return 1;
  }
	
	else {
    return 2 * powerOfTwo(n-1);
  }
  ```
		
