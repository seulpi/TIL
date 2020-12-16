# 예외처리 (Exception Handlimg) <br> → OOP의 특징 (★ 암기 ★)
(고수들은 이거 기가막히게 써먹음8ㅅ8)
: 자바에서는 예외고 뭐고 간에 다 클래스로 표현하게 되어있음 <br>
따라서, 예외도 클래스가 있고 클래스의 구조는 아래와 같다 ▼
![예외처리UML](https://user-images.githubusercontent.com/74290204/102187064-da489980-3ef6-11eb-9ebd-92b2deb58829.png)
## @ Error 와 Exception의 차이
1. error : 시스템상의 오류 <br>
ex] 아 사장님 메모리가 꽉 찼어요~ / 디스크가 맛이 가서 다운됐어요~
2. exception : 코딩상의 오류 (개발자의 로직 오류) <br>
▶ exception과 error를 처리하는 클래스가 따로 존재
```java
exception에는 checked exception / unchecked exception 존재 

1. Checked Exception : 예외처리 안해주면 우럭광광우럭(JVM이 삐져서 컴파일도 안해줘ㅠ)
 - 문법적 오류가 아닌 프로그램 실행과정에서 발생하는 오류(예외적오류)
 - Unchecked Exception - Runtime Exception을 제외한 Exception 
 - exception 처리 코드를 컴파일러가 check → check 없으면 오류 발생 → ★ 반드시 예외처리 해줘야함!! 
 - EX]  1) 존재하지 않는 파일 처리 (FileNotException)
        2) 클래스의 이름 잘못 표기 (ClassNotFoundException)
        3) 입력한 데이터 형식의 오류 (DataFormatException)

2. Unchecked Exception : 예외처리 안해줘도 OK OK
 - 발생 가능할 것 같은 예외를 처리해주지 않아서 발생하는 에러 (프로그래머 실수)
 - Code를 잘못 만들어서 생기는 문제
 - 컴파일 하는데는 문제 X / 실행하면 문제가 발생 (Runtime Exception)

 ▶ 따라서, Unchecked Exception은 try - catch를 사용한다기보다는 코드를 바꿔서 에러 수정하는 더 적절!


→ 어떤 종류가 있는지 어떤 구조인지 위 그림 참고
```
<br>

## @예외 처리 방법 1.
: JAVA에서는 예외처리에 대한 매커니즘을 제공하는데 그게 바로 **Try - Catch - Finally 코드**
>> 에러 객체 띄우는 주체 = JVM <br>
에러 출몰 → JVM이 관련 에러에 대해서 뿌리면서 프로그램 die <br> 
→ 이때! 예외처리 해주면 프로그램 live  <br>
JVM曰 얘들아 프로그램 죽이지말고 나한테 넘겨줄래^^? : try / oo알았어.. ▶ JVM이 catch에서 받아서 에러처리함
- Try : 에러가 생길만한 곳에 배치
- Catch : 에러 처리 (=JVM이 처리)
```java
import java.util.Scanner;
public class Main {
	public static void main(String[] args) {
		Scanner kb = new Scanner(System.in);
	
		try { // try{안에가} 예외나는 부분
			System.out.println("a/b...a? ");
			int n1 = kb.nextInt();  
            ▶ 여기서 error나면 (try안에)밑에 처리안하고  catch부분으로 바로 감 

			System.out.println("a/b...b? ");
			int n2 = kb.nextInt();

			System.out.printf("%d / %d = %d \n", n1, n2, n1 / n2);
		}  // n1/n2에서 만약 0이 들어가면 error나니까 예외처리해줘야함
     // catch방법1    
		catch (ArrayIndexOutOfBoundsException e) { // 에러나는걸 JVM이 받아서 처리해주는 부분
			System.out.println(e.getMessage());  
		} // 0넣어서 나눴을때 오류 처리
		catch (InputMismatchException e) { 
			System.out.println(e.getMessage());  
		} // 문자넣었을때 오류 처리
		System.out.println("Good Bye~!");
	}or

    // catch방법2
		catch (ArrayIndexOutOfBoundsException | InputMismatchException e) {  // | 는 or 
			System.out.println(e.getMessage());  
		}

	// catch방법3
		catch (Exception e) {  // Exception이 최고클래스이기 때문에 다형성이 적용되서 가능한것 
			e.printStackTrace();   
		}  
		System.out.println("Good Bye~!");
	}
}
===================
a/b...a? 
a
java.util.InputMismatchException
Good Bye~!
	at java.base/java.util.Scanner.throwFor(Scanner.java:939)
	at java.base/java.util.Scanner.next(Scanner.java:1594)
	at java.base/java.util.Scanner.nextInt(Scanner.java:2258)
	at java.base/java.util.Scanner.nextInt(Scanner.java:2212)
	at Main.main(Main.java:11)  
    /* 에러아님 catch에서 뿌린거 
    e.printStackTrace();  → 개발자들이 자주 사용, 고객한테 줄때는 이거 없애고 출력("에러가 없습니다")해줘야함 고객이뭘알앜ㅋㅋㅋㅋ(개발자확인용이라그럼)
    이 에러처리 함수를 사용해야 위에 catch가 뿌린거를 통해 더 자세하게 실행내용을 살펴볼 수 있기 때문! */
```
### Q. try는 어디에 생성해야할까?
A. try{ }의 영역은 개발자의 역량이고 어디에 두는게 맞다고 논하기 어렵다 <br> 다만 출력안하고 뿌리는 부분이 있기 때문에 입력되는게 얼마나 중요한지에 따라 케바케 <br>
>> 실질적으로는 try- catch 남발하게 되면 성능이 떨어지기 때문에 <br> 영역을 나눠서 묶는것보단 한번에 묶는게 goodjob!

<br>

▶ **헷갈린부분!**
![KakaoTalk_20201215_151241051](https://user-images.githubusercontent.com/74290204/102216550-a7b29700-3f1e-11eb-9cbb-32b25c203ede.png)

```java
/* 에러가 나는 부분에 감싸야한다고 생각했는데 메인에서 감싸야 
여러번 try 하는 대신 한 번만 하면 되기 때문에 메인 함수 호출할때 감싸줌 */
```
### - try catch는 여러개 사용가능! 단 순서가 존재
: catch()에서 에러를 잡을 때 부모 즉 **Excetion은 가장 아래에 존재해야함** 
▶ why? 예외의 최고 조상님 Excetion이 맨 위에 오게 되면 <br> Excetion은 모든 예외의 함수들을 다 포함하고 있기 때문에 <br>
맨 위에 올 경우 **하위클래스의 예외처리구문을 타지 않고 그대로 빠져나온다** <br>
그래서 **Unreachable error** 발생 
```java
public static void main(String[] args) {
		
		try {
			int num = 6 / 0;
		}
		catch (Exception e ) {
			e.printStackTrace();
		}catch (InputMismatchException e ) {
			e.printStackTrace();
		} // error ! 
	}
error 수정 전 ▲
------------------------------------------------------
error 수정 후 ▼

public class Main {
	public static void main(String[] args) {
		
		try {
			int num = 6 / 0;
		}
		catch (InputMismatchException e ) {
			e.printStackTrace();
		} 
		catch (Exception e ) {
			e.printStackTrace();
		}
	}
}
```
### - Finally는 에러와 관계없이 Finally 구문을 무조건 타서 실행시킴 (중간에 탈출X)
- ★ 따라서 **반드시! 무조건!** 실행해야되는 구문은 finally안에 위치
>> try{} : 에러X <br>
catch{} : try안에 에러없다면 탈 필요X<br>
finally{} : 에러 유무와 상관없이 탄다

   - 객체에대한.close(); 도 에러발생가능할 확률이 있기 때문에 finally 안에서 try - close로 묶어줘야함


## @예외 처리 방법 2. try-with- resources (자바1.7부터 제공) 
```java
	// ↓ resources
try(객체 직접 생성) { } 
→ 끝날때는(실행한 후) 내부(try-check문)에서 객체에 대한 
.close()를 자동으로 호출해주고 메모리를 날린다

▶ [close()]
객체에 대한 .close() : 더 이상안쓸거니까 JVM아 메모리 날려됨 ㅇㅇ
```

## @예외 처리 방법 3. throws
: 예외처리 방법 중 하나
>> 함수선언( ) throws 예외객체 { 바디 실행 }
```java
throws IOException 
```
- throws는 에러를 던진다 who? → 자기자신을 호출한 함수한테
- throws 는 함수단위로 호출을 넘기는거기 때문에 class 단위로 넘기는 문법은 아님

![throw](https://user-images.githubusercontent.com/74290204/102306314-4b468a80-3fa5-11eb-939d-7dec7d56f6d1.PNG)

### - throws main() 사용 가능?
▶ main도 함수이기 때문에 가능은 하다 
![throw_main_re](https://user-images.githubusercontent.com/74290204/102306286-3cf86e80-3fa5-11eb-8d9d-b95f30dd9ad4.PNG)

>> 추가설명) throws ','로 여러개 명시 가능 <br>
Exception은 상위클래스이기 때문에 하위 에러들의 케이스를 다 받아낼 수 있다 <br>
(Object가 그랬던 것처럼 )

# Stack 
: 양동이 같은 개념 (양동이 안에 물건 넣을때 1 - 2 -3 순이지만 꺼낼때는 3 - 2 -1 ) <br>
마찬가지로 자바에서 Stack은 함수가 쌓이는 순과 달리 제일 마지막에 호출된 순으로 call
