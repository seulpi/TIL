# 1. Maker 인터페이스에 대하여 설명하시오
: 마킹을 한다해서 마커인터페이스라고 명칭하며 인터페이스의 한 종류이다 <br> 마커인터페이스는 인터페이스와 동일하지만 사실상 어떠한 메소드도 선언하지 않은 인터페이스<br>
(비어있는 빈 깡통의 인터페이스)
- 목적: 클래스 분류를 위해 사용
- 사용: 오버라이딩 할 메소드도 없기때문에 상속을 받아 사용한다
```java
>> EX

interface Upper { }
interface Lower { }
/* 이게 바로 Marker interface 
구현할 내용은 없고 상속하면 클래스를 분류한다 */

▶ 대표적인 Marker interface 는 Serializable, Cloneable 등이 존재 
```
<br>

# 2. 추상클래스에 대하여 설명하시오
: 추상클래스는 abstract가 붙은 클래스 
- 추상클래스는 메소드가 추상메소드이면 클래스 앞에 무조건 abstract를 붙여야 하기 때문에 추상클래스가 생김 <br>
(= 추상메소드가 존재하기때문에 추상클래스가 존재)
- 추상클래스도 추상메소드와 마찬가지로 자손이 구현해라라는 뜻 내포

<br>

# 3. 추상클래스와 인터페이스의 차이는?
1. 공통점 :  구현부분이 없기 때문에 객체생성X / 참조형 변수 선언만 O
2. 차이점 : 추상클래스는 함수 구현까지 가능 
```java
// 최근에 들어서는 차이점의 경계가 애매모호해짐
why? interface에서 static 과 default 가 지원이 되면서 함수 구현도 가능해졌기 때문
```
<br>

# 4. 에러와 예외의 차이는?
- error : 코드상의 문제가 아니라 시스템적상의 문제일때 발생하는 것 <br>
    - EX) 메모리가 가득찼다, 하드디스크에 손상이 왔다 등 
- exception : 코딩상의 문제 (개발자의 로직 오류)
    - EX) 형변환, 스캐너입력시 0을 나눴다 등 
<br>

# 5. unchecked 와 cheked 예외의 차이는?
: 예외(exception) 처리는 두가지의 종류가 있는데 그게 바로 **unchecked exception, checked exception**이다 
- unchecked exception : 예외처리를 해주지 않아도 되는 에러
- checked exception : 예외처리를 **반드시** 해줘야 되는 에러 <br>
▶ 아래 6.그림 참조

<br>

# 6. 예외처리 UML를 그리시오
![예외처리UML](https://user-images.githubusercontent.com/74290204/102187064-da489980-3ef6-11eb-9ebd-92b2deb58829.png)

<br>

# 7. 사칙연산 계산기를 아래의 조건으로 짜시오
## -interface 를 활용할것 <br> -예외처리 메커니즘을 적용할것
### * 이 문제 스캐너 입력하는 거 추가해서 풀어볼것 * (9번과같은문제)
```java

public class TranningMain {
	public static void main(String[] args) {
	// 사칙연산 계산기 interface 활용 예외처리 메커니즘
		
		ICalculator calculator = new Calculator();
		
		try {
		calculator.add(5, 7);
		calculator.min(10, 2);
		calculator.mul(8, 4);
		calculator.div(4, 0);
		} 
		catch(ArithmeticException e) {
			e.printStackTrace();
			System.out.println(e.getMessage());
		}
	}
}

interface ICalculator {
	public void add(int x, int y);
	public void min(int x, int y);
	public void mul(int x, int y);
	public void div(int x, int y);
}

class Calculator implements ICalculator {
	int x;
	int y;
	
	@Override
	public void add(int x, int y) {
		System.out.println("두 수의 덧은: " + (x + y));
	}
	@Override
	public void min(int x, int y) {
		System.out.println("두 수의 덧셈: " + (x - y));
	}
	@Override
	public void mul(int x, int y) {
		System.out.println("두 수의 곱셈: " + (x * y));
	}
	@Override
	public void div(int x, int y) {
		System.out.println("두 수의 나눗셈: " + (x / y));
	}
}
```
<br>

# 8. 다음 Stack 인터페이스를 상속받아 실수를 저장하는 <br>StringStack 클래스를 구현하라<br> (구현할수 있도록 할 것)   * 수정중 *
```java
interface Stack {
   int length(); // 현재 스택에 저장된 개수 리턴
   int capacity(); // 스택의 전체 저장 가능한 개수 리턴
   String pop(); // 스택의 톱(top)에 실수 저장
   boolean push(String val); // 스택의 톱(top)에 저장된 실수 리턴
}
그리고 다음 실행 사례와 같이 작동하도록 StackApp 클래스에 main() 메소드를 작성하라.

총 스택 저장 공간의 크기 입력 >> 3
문자열 입력 >> hello
문자열 입력 >> sunny
문자열 입력 >> smile
문자열 입력 >> happy
스택이 꽉 차서 푸시 불가!
문자열 입력 >> 그만
스택에 저장된 모든 문자열 팝 : smile sunny hello 
```
```java
//Answer [me]
import java.util.Scanner;

public class TranningMain {

	public static void main(String[] args) {
		
		Scanner sc = new Scanner(System.in);
		System.out.print("총 Stack 저장 공간의 크기 입력 >> ");
		
		StackApp app = new StackApp();
		
		int arRoomCount;
		arRoomCount = app.length();
		System.out.println(arRoomCount);
		
		while(true) {

			System.out.println("문자열 입력 >> ");
			
			String str1, str2, str3, str4, str;
			str1 = sc.nextLine();
			System.out.println("문자열 입력 >> ");
			str2 = sc.nextLine();
			System.out.println("문자열 입력 >> ");
			str3 = sc.nextLine();
			System.out.println("문자열 입력 >> ");
			str4 = sc.nextLine();
			
			app.capacity();
			
			str = sc.nextLine();
			
			if(str.equals("그만")) {
				break;
			}
			
			System.out.print(app.pop());
			app.push(str1);
			app.push(str2);
			app.push(str3);
			
		}
		sc.close();
	}
}

interface Stack {
	int length();
	int capacity();
	String pop();
	boolean push(String val);
}

class StackApp implements Stack {
	String[] arr = new String[3];
	int index = -1;
	
	
	public int length() {
		return arr.length;
	}
	
	public int capacity() {
		for(int i = 0; i <arr.length; i++) {
			if(index >= arr.length) {
				System.out.println("스택이 꽉 차서 푸시 불가!");
			}
		}return index = arr.length - index;
	}
	
	public String pop() {
		for(int i = arr.length; i < 0; i--) {
			System.out.println(arr[i]);
		}
		return "스택에 저장된 모든 문자열 팝: " ;
	}
	
	public boolean push(String val) {
		for(int i = 0; i <arr.length; i++) {
			if(val == arr[i]) {
				index++;
				System.out.print(arr[i] + " ");
			}
		}return true;
	}
}
```
```java

// Answet [teacher]
import java.util.Scanner;

public class TranningMain {

	public static void main(String[] args) {
		
		Scanner sc = new Scanner(System.in);
		System.out.print("총 Stack 저장 공간의 크기 입력 >> ");
		int num = sc.nextInt();
		
		StringStack stack = new StringStack(num);
		
		while(true) {

			System.out.println("문자열 입력 >> ");
			String word = sc.next();
			if(word.equals("그만")) {
				break;
			} 
			
			if(!stack.push(word)) {
				System.out.println("스택이 꽉 차서 푸시 불가!");
				break;
			}
		}
		
		System.out.println("스택에 저장된 모든 문자열 팝: ");
		int len = stack.length();
		for(int i = 0; i <len; i++) {
			String s = stack.pop();
			System.out.println(s + " ");
		}
			
		sc.close();
		
	}
}


interface Stack {
	int length();
	//int capacity();
	String pop();
	boolean push(String val);
}

class StringStack implements Stack {
	
	private String stack[];
	private int top;
	
	StringStack(int length) {
		stack = new String[length]; 
		top = -1; // 0으로 둬도 상관없으나 기본적으로 -1로 설정 , 인덱스 번호 가리키는 변수 
	}
	
	
	public int length() {
		return stack.length;
	}
	
	
	public boolean push(String val) {
		
		if(top == stack.length -1) // 길이만큼이니까 꽉 차면 false를 반환 top은 -1로 맞추고 length는 배열 
			return false;
		
		stack[++top] = val; //top에 먼저 숫자하나를 증가시키고 val을 넣는다 
		
		return true;
	}
	
	public String pop() {
		if(top == -1)
			return "스택이 비어있습니다.";
		return stack[top--]; // 꺼내고 나서 후위증가 , 한개꺼냈으니까 줄어야지
	}
	
}
```
# 9. 철수 학생은 다음 3개의 필드와 메소드를 가진 4개의 클래스 Add, Sub, Mul, Div를 작성하려고 한다 
### * 문제 이해가 처음에 잘안됐음 →  모르는 용어 : 피연산자, 필드  <br> 문제 7번과 같은 문제 같음 문제내는 방식만 다르다
```java
- int 타입의 a, b 필드: 2개의 피연산자
- void setValue(int a, int b): 피연산자 값을 객체 내에 저장한다.
- int calculate(): 클래스의 목적에 맞는 연산을 실행하고 결과를 리턴한다.
곰곰 생각해보니, Add, Sub, Mul, Div 클래스에 공통된 필드와 메소드가 존재하므로 
새로운 추상 클래스 Calc를 작성하고 Calc를 상속받아 만들면 되겠다고 생각했다. 
그리고 main() 메소드에서 다음 실행 사례와 같이 2개의 정수와 연산자를 입력받은 후,
 Add, Sub, Mul, Div 중에서 이 연산을 처리할 수 있는 객체를 생성하고 
 setValue() 와 calculate()를 호출하여 그 결과 값을 화면에 출력하면 된다고 생각하였다.
  철수처럼 프로그램을 작성하라.

두 정수와 연산자를 입력하시오 >> 5 7 +
12
```
```java
// Answer [Me]
import java.util.Scanner;
public class ExMain {
	public static void main(String[] args) {
	
		Scanner sc = new Scanner(System.in);
		System.out.println("두 정수와 연산자를 입력하시오 >> ");
		
		int a, b;
		char cal;
		a = sc.nextInt();
		b = sc.nextInt();
		cal = sc.next().charAt(0);
		
		Add add = new Add();
		
		if(cal == '+') {
			add.setValue(a, b);
			System.out.println(add.calculate());
		} 
	}
}
--------------------------------------------------------------
abstract class Calc  {
	int a, b;
	public void setValue(int a, int b) {
		this.a = a;
		this.b = b;
	}
	
	public int getA() {
		return a;
	}
	
	public int getB() {
		return b;
	}
	
	abstract int calculate();
}	

class Add extends Calc {
	
	public int calculate() {
		return getA() + getB();
	}
	
}

class Sub extends Calc {
	
	public int calculate() {
		return getA() - getB();
	}
}

class Mul extends Calc {
	
	public int calculate() {
		return getA() * getB();
	}
}

class Div extends Calc {
	
	public int calculate() {
		return getA() / getB();
	}
}
```
```java
//Answer [teacher]

import java.util.Scanner;
public class CalMain {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		System.out.println("두 정수와 연산자를 입력하시오 >> ");
		
		try {
		int num1 = sc.nextInt();
		int num2 = sc.nextInt();
		char op = sc.next().charAt(0);
		
		Calc cal = null;
		switch (op) {
		case '+':
			cal = new Add();
			//cal.setValue(num1, num2); 여기에도 가능
			break;
		case '-':
			cal = new Sub();
			//cal.setValue(num1, num2); 여기에도 가능
			break;
		case '*':
			cal = new Mul();
			//cal.setValue(num1, num2); 여기에도 가능
			break;
		case '/':
			cal = new Div();
			//cal.setValue(num1, num2); 여기에도 가능
			break;
		default:
			System.out.println("잘못된 연산입니다");
		}
		cal.setValue(num1, num2); // 여기에 넣은 이유는 +,-,*,/ 다 넣어줘야되는데 여기에 넣으면 한번만 넣어도되서
		System.out.println(cal.calculate());
		}
		catch (Exception e) {
			System.out.println("비정상 종료입니다");
			
		}
		sc.close();
	}
}

abstract class Calc {
	protected int num1;
	protected int num2;
	
	public void setValue(int num1, int num2) {
		this.num1 = num1;
		this.num2 = num2;
	}
	abstract public int calculate();
}

class Add extends Calc {

	@Override
	public int calculate() {
		
		return num1 + num2;
	}
}

class Sub extends Calc {

	@Override
	public int calculate() {
		
		return num1 - num2;
	}
}

class Mul extends Calc {

	@Override
	public int calculate() {
		
		return num1 * num2;
	}
}

class Div extends Calc {

	@Override
	public int calculate() {
		
		return num1 / num2;
	}
}
```

# 10. 연습문제 7-22 번을 푸시오(JavaQuiz16] 4번문제)
### Point 클래스는 어디에 어떻게 활용? 나중에 배울 문제인데 신경안써도됨
```java

public class ExMain {

	public static void main(String[] args) {
		Circle circle = new Circle(3.5, 3);
		System.out.println("원 넓이의 면적: " + circle.calcArea());
		
		Rectangle rectangle = new Rectangle(4.5, 2.5, 4);
		System.out.println("사각형 넓이의 면적: " + rectangle.calcArea());
		System.out.println(rectangle.isSquare());
	}
}

abstract class Shape {
	Point p;
	
	Shape() {
		this(new Point(0,0));
	}
	
	Shape(Point p) {
		this.p = p;
	}
	
	abstract double calcArea();
	
	Point getPosition() {
		return p;
	}
	
	void setPosition(Point p) {
		this.p = p;
	}
}

class Point {
	int x;
	int y;
	
	Point() {
		this(0,0);
	}
	
	Point(int x, int y) {
		this.x = x;
		this.y = y;
	}
	
	public String toString() {
		return "[" + x + "," + y + "]";
	}
}

class Circle extends Shape {
	private double radius;
	
	Circle(double radius , int p) {
		super.getPosition();
		this.radius = radius;	
	}
	
	public Circle() {
	
	}

	@Override
	public double calcArea() {
		return radius*radius*Math.PI;
	}
}

class Rectangle extends Shape {
	private double weight;
	private double height;
	
	Rectangle(double weight, double height, int p) {
		super.getPosition();
		this.weight = weight;
		this.height = height;
	}
	
	@Override
	public double calcArea() {
		return weight*height;
	}
	
	public boolean isSquare() {
		return (weight == height) ? true : false;
	}
}
```
<br>

# 11. 연습문제 7-23 번을 푸시오(JavaQuiz16] 5번문제)
```java
public class Main {

	public static void main(String[] args) {
		Shape[] arr = { new Circle(5.0), new Rectangle(3, 4), new Circle(1) };
		System.out.println("면적의 합: "+ sumArea(arr));
	}
	
	public static double sumArea(Shape[]arr) {
		double sum = 0.0;
		
		for (int i = 0; i < arr.length; i++) {
			sum = sum + arr[i].calcArea();
		}
		return sum;
	}
}

abstract class Shape {
	Point p;
	
	Shape() {
		this(new Point(0,0));
	}
	
	Shape(Point p) {
		this.p = p;
	}
	
	abstract double calcArea();
	
	Point getPosition() {
		return p;
	}
	
	void setPosition(Point p) {
		this.p = p;
	}
}

class Point {
	int x;
	int y;
	
	Point() {
		this(0,0);
	}
	
	Point(int x, int y) {
		this.x = x;
		this.y = y;
	}
	
	public String toString() {
		return "[" + x + "," + y + "]";
	}
}

class Circle extends Shape {
	private double radius;
	
	Circle(double radius) {
		this.radius = radius;	
	}
	
	public Circle() {
	
	}

	@Override
	public double calcArea() {
		return radius*radius*Math.PI;
	}
}

class Rectangle extends Shape {
	private double weight;
	private double height;
	
	Rectangle(double weight, double height) {
		this.weight = weight;
		this.height = height;
	}
	
	@Override
	public double calcArea() {
		return weight*height;
	}
	
	public boolean isSquare() {
		return (weight == height) ? true : false;
	}
}
```
