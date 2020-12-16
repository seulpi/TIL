#  instanceof 연산자
>> A instanceof B : A가 B의 인스턴스야? <br>
객체 instanceof 클래스명 → true or false로 값 반환 
### * A가 상속받는 클래스(자식클래스) instanceof 클래스명(부모클래스) 이 형태는 무조건 **true**
```java
public class RandomMain {

	public static void main(String[] args) {
		Box2 box1 = new Box2();
		PaperBox box2 = new PaperBox();
		GoldPaperBox box3 = new GoldPaperBox();
		
		wrapBox(box1);
		wrapBox(box2);
		wrapBox(box3);
	
	}
public static void wrapBox(Box2 box) { // 파라미터 타입으로 최상위 부모로 설정 
	if(box instanceof GoldPaperBox) { // instanceof는 연산자 즉, 자식들이 형변환 되는가 안되는가 판별
		((GoldPaperBox)box).goldWrap(); //wrapBox(box3); 여기서 출력 여기에서 GoldPaperBox를 포함하고 있으니까
	}
	else if (box instanceof PaperBox) {
		((PaperBox)box).paperWrap(); // Box2부모로 데이터타입을 받았는데 자식으로 PaperBox형변환 가능하냐 (PaperBox가지고 있기 때문에 가능) → box2 여기서 출력
	}
	else {
		box.simpleWrap();// wrapBox(box1); 여기서 출력 Box2본인만 가지고있으니까
		}
	}
}

class Box2 {
	public void simpleWrap() {
		System.out.println("Simple Wrapping");
	}
}

class PaperBox extends Box2 {
	public void paperWrap() {
		System.out.println("Paper Wrapping");
	}
}

class GoldPaperBox extends PaperBox { 
	public void goldWrap() {
		System.out.println("Gold Wrapping");
	}
}
```
### - 자바에서는 50% 이상 객체 아님 다형성으로 대답하면 거진 맞음
###  -클래스간에 공통으로 들어가는 건 반드시 하나로 묶어주게 되어있음 <br> → 다형성 적용해서 호출하려고

<br>

# Object클래스와 final 선언
## @ final 선언
- 클래스앞에 final = 상속하지말란뜻 (클래스상속X)
- 함수 앞에 final = 다른 클래스에서 오버라이딩 X
<br>

## @ Object클래스
### - 정의 : 모든 클래스의 최상위 클래스
- **★ 모든클래스는 Object클래스를 상속한다★**<br>  
  - 상속하는 클래스가 없다면 컴파일러에의해 java.lang.Object 클래스를 자동으로 상속하게 코드가 구성된다 / 비슷한 예로 '디폴트 생성자'
  - 단] 상속은 2개 이상 안되기 때문에 다른 클래스를 상속하면 직접적으로 Object는 상속이X, 간접적으로 상속하는 형태로 상속O 
```java
Shape circle = new Circle(10); 
System.out.println(circle);
------------------------------------
// 왜 error가 안날까? 
▶ Object는 최고의 조상 
어떠한 클래스든 다 받아내기 때문에 최고의 조상을두고 다형성을 적용한 예
```
### - Object 클래스는 함수 11개 정도로 구성되어있음 
▶ [Object 클래스 함수 링크] https://hyeonstorage.tistory.com/178
```java
// 몇개는 반드시 기억하자!

1. String toString() 

public class Main {
	public static void main(String[] args) {
		A a = new A();
		System.out.println(a);
	}
}

class A  { } == class A /*extends Object */ { }
==================================================
▶ A@28a418fc (주소값출력)

class A {
	@Override // toString은 null이 아니면 toString호출
	public String toString() {
		return "이것은 A 클래스입니다";
	}
==================================================
▶ 이것은 A 클래스입니다

public static String valueOf(Object obj) {
        return (obj == null) ? "null" : obj.toString();
    } // 고슬링 아저씨가 toString 정의해놓은 함수 
```
# [Annotation] @Override
-Annotation의 종류 https://elfinlas.github.io/2017/12/14/java-annotation/
### @Override는 개발자나 컴파일러에게 오버라이딩을 직관적으로 알려주는 것 <br>
(에러나 다른부분에 영향을 미치지않는다)
```java
class ParentAdder {
	public int add(int a, int b) {
		return a + b;
	}
}

class ChildAdder extends ParentAdder {
	@Override // 오버라이딩아니면 에러내라고 직관적으로 빨리 컴파일러에게 말해주는것
	public double add(double a, double b) { // 오버라이딩 아닌 오버로딩 상속관계에서 오버라이딩 아닌 형태 : error
		System.out.println("덧셈을 진행합니다");
		return a + b ;
	}
```
<br>

# 인터페이스(Interface)
: 구현되는 것이 없는 밑그림만 가진 설계도
>> interface 클래스명 {구현X } <br>
    class 클래스명 {구현O}
    <br>
    ▷ 클래스 자리에 오는 키워드이며 클래스와 동일함
<br>

## @ 목적과 장점
### 1. 목적: 인터페이스는 **표준 규약 강제** 단어로 정의내릴수 있는데 <br> 실무에서 협업을 할 때 하나의 틀을 만듬으로써 정해진 공통된 것을 사용하고 <br> 코드를 간결하게 하기 위함
### 2. 장점: 표준화가 가능하기때문에 기본 틀을 interface로 만들고 <br> interface를 구현하여 사용한다는 것 (목적이 곧 장점)

>> **[tip1] 다형성을 적용가능하면 하는게 BEST!** <br> ▶ why? 구현해야 할 내용이 복잡해지면 다형성 적용 X → 형 변환을 계속 해줘야한다 → 으 복잡쓰.. <br>
다형성 적용 O → 형변환 필요없이 객체생성자만 처리하면 되기때문! 

>> **[tip2] 하나의 파일 안에 클래스 여러개 O , public은 only 1개!** <br>
why? 관습적으로 public은 하나의 파일안에서 진입점 역할을 하기 때문에 2개 이상이면 error가 난다
<br>

## @ 특징
1. 상수와 추상메소드 선언만 가능하다(= {구현부분}이 없다) <br>근데 1.7ver부터 static과  default를 허용
2. 객체생성을 안되고 선언만 가능하다
3. 구현이 없는 함수에는 **abstract**가 붙어야하는데 interface에서는 **생략이 가능하다** 
4. 접근제한자 public만 가능(private X) <br>
▶ 3, 4번) abstract(함수에붙는것), public, final, static을 생략하면 컴파일러가 자동으로 붙여줌
5. 클래스와 다르게 객체생성 X (= new 키워드 사용이 안된다는것) <br> 
▶ why? 객체를 생성해줘도 구현부분이 없기 때문에 써먹을 게 없다
```java
 Printable p = new Printable(); 이거 안됨!

 //단, 선언은 가능하다
 Printable p;
 ```
 - [tip] interface명 설정할때 2가지 방법을 자주 사용
   1. 'I'를 붙이거나 
   2. 끝에 'able'을 붙이거나 
   ```java
   public interface Icalculator { }
   public interface Calculatable { }
   ```
6. 인터페이스는 다중상속이 가능(둘 이상의 인터페이스 구현이 가능)
 ```java
class Robot extends Machine implements Movable, Runnable { }
```
<br>

7. interface는 interface 상속이 가능하고 static함수는 @Override로 재정의 X

7. instanceof 연산자 사용 가능
## @ 사용 (활용)
>>> interface 인터페이스명 {} <br> class 클래스명 **implements** 인터페이스명{ 오버라이딩된 함수 구현}
```java
public static void main(String[] args) {
		Printable prn = new SimplePrinter();
		prn.print("Hello");
	} // 선언은 가능하니까 부모인터페이스 변수명 = new 자손 → 다형성 적용  

interface Printable { //interface는 클래스와 똑같다 interface 클래스명 { }
	abstract public void print(String doc);
} 

class SimplePrinter implements Printable { // extends랑 비슷한 기능의 키워드

	@Override
	public void print(String doc) {
		System.out.println(doc);
	} // 인터페이스 여기서 구현 , abstract = 자손이 구현해라	
}
```
- interface안에 함수가 추가되면 자손에서 추가적으로 기능을 구현해줘야한다 <br>
→ 안해주면 error / 함수가 여러개 있는데 하나만 구현하겠다는것은 안되며 가진 함수의 기능을 다 구현해줘야한다~
  - ▶ [해결방법] 기존에 클래스를 두고 추가로 하나의 클래스를 만들어 구현한다 
  ```java
  // 인터페이스도 상속이 가능하기 때문에 extends로 표현 
  interface ColorPrint extends Printable {
  }
  ```
<br>

## @ 디폴트메소드
>> 예를들어, 프린트의 제조사가 1-2개가 아닌 200개 이상이다<br> 
200개의 제조사를 추가하려면 200개의 클래스를 추가해줘야함 <br>
이런 문제를 해결하기 위해 1.7ver부터 제공되는게 인터페이스의 '디폴트메소드'

- 디폴트 메소드도 @Override로 재정의 가능 

```java
interface Printable {
	void print(String doc);
}
▲ 변경 전
===============================================
▼ 변경 후

interface Printable {
	void print(String doc);
	default void printCMYK(String doc) {
		
	}
}

// 기존에 있던 interface 오버라이딩도 가능 
class SPrinterDriver implements Printable {
	public void print(String doc) {
		System.out.println(doc);
	} /* default는 자손이 구현하든 안하던 상관없다
 		그래서 기존꺼+ 새로운게 적용 가능해짐 */
}
```
- interface는 static로 붙일 수 있는데 static과 default 붙이면 구현내용이 들어갈 수 O
```java
interface Printable {
	 static void print(String doc) {
		 System.out.println(doc);
	 }
	 
	 default void print2(String doc) {
		printLine(doc); // default는 printLine으로
	 }
}
```
<br>

## @ Marker 인터페이스
1. **정의** 
- 마킹을 한다해서 Marker 인터페이스
- Marker 인터페이스는 인터페이스의 한 종류이며 사실상 아무 메소드도 선언하지 않은 인터페이스 <br>
(비어있는 빈 깡통같은 interface) 
- 단지 자신을 구현하는 클래스가 특정속성을 가짐을 표시해주는 것 
2. **목적 :** 클래스 분류를 위해 사용 <br>
ex) 해양생물 100개 육지동물 100개 있는데 종류안의 동물,해산물 호출할때마다 함수를 매번 만들어줘야함 <br>
번거롭고 언제 100개 만들어서 추가해..그래서 marker인터페이스로 interface 해양생물{ } interface 육지동물{ } 만들어서 implments로 받아서 호출하면 간-편 <br>
(해양생물인지 육지동물인지 구분 or 분류해주는것)

3. **대표적인 EX :** Serializable, Cloneable
```java
interface Upper { }
interface Lower { }

/* interface Upper 는 toUpperCase( ) 호출 : 다 대문자로 바꾸는 함수 
   interface Lower 는 toLowerCase( ) 호출 : 다 소문자로 바꾸는 함수
```
```java
public class Main {
	
	public static void main(String[] args) {
		Printer prn = new Printer();
		Report doc = new Report("Simple Funny News~");
		prn.printContents(doc);
	}
}
interface Upper {
	
}
interface Lower {
	
}

interface Printable {
	String getContents();
}

class Report implements Printable, Upper { // Printable과 Upper를 동시에 상속
	String cons;

	Report(String cons) { // interface안에 있는게 아니니까 객체생성가능 interface가 객체생성이 X 
		this.cons = cons;
	}
	
	public String getContents() {
		return cons;
	}
}

class Printer {
	public void printContents(Printable doc) {
		
		if(doc instanceof Upper) {
			System.out.println(doc.getContents().toUpperCase());
		}
		else if(doc instanceof Lower) {
			System.out.println(doc.getContents().toLowerCase());
		}
		else {
			System.out.println(doc.getContents());
		}
	}
}
```
# 추상클래스 
## abstract class : 얘도 추상메소드와 마찬가지로 자손이 구현해라
- 정의 : 클래스안에 추상메소드가 한개라도 존재하면 클래스에 **abstract** <br> (= 추상메소드가 있기 때문에 추상클래스가 존재하는 것) 
	- abstract 붙은 함수가 있는데 클래스에 abstract 없으면 error
- 인터페이스처럼 구현부분이 없기 때문에 객체생성X , 참조변수 선언O
- 일반클래스의 특성 + abstract함수 포함 
### Q. interface와 추상클래스의 차이점은?
>>> interface에 **default 나 static**이 지원이 안됐을때는 명확했으나 <br> 지금은 **default 나 static**를 interface에 사용할 수 있게 되면서 차이가 애매모호해짐 
: 무튼, 차이는 함수를 구현할 수 있느냐 없느냐의 차이 + 클래스냐 인터페이스냐의 차이 
```java
public class Main {
	
	public static void main(String[] args) {
		// Car car = new Car(); 안됨! 구현할게 없으니까 객체생성X
		Car car = new Genesis();
		car.run(); // 자손이 구현한 함수 호출됨
	}
}
abstract class Car {
	public abstract void start();
	public abstract void stop();
	
	final public void run() { // final은 상속하지말라고~
		start(); // 아직 구현되지 않은 함수를 호출가능(문법적으로 허용)
		stop();
	}
}// 차이! 인터페이스는 추상메소드만 가능했는데 추상클래스는 함수구현까지 가능 (근데 실무에서는 추상클래스 잘 안씀)

class Genesis extends Car { // 얘는 implement아님 인터페이스 관계가 아니니까 상속받아서 써먹어야해서 extends
	
	@Override
	public void start() {
		System.out.println("시동걸기");
	}
	
	@Override
	public void stop() {
		System.out.println("제동걸기");
	}
}
```
