# 1. Object 클래스에 대하여 설명하시오
- 정의: Object클래스란 자바에서 기본으로 제공하는 최상위 클래스(최고조상님) <br> 상속관계가 없는 **모든 클래스는 Object 클래스를 상속한다**
- 단, 상속은 2개 이상 할 수 없기 때문에 간접적으로 상속하게 됨
- Object 클래스에는 총 11개의 함수가 정의되어있고 그 함수는 아래 링크 확인 <br>
▶ [Object 클래스 함수 링크] https://hyeonstorage.tistory.com/178 <br>
★ int hashCode / Strind toString / boolean equals 는 꼭꼭 암기 ★

<br>

# 2. 아래와 같이 출력되는 이유를 하시오
```java
class A {
	
	 @Override
	 public String toString() {
		
		 return "이것은 A 클래스 입니다.";
	 }	
}

public class TestMain {
	public static void main(String[] args) {
		A a  = new A();
		System.out.println(a);
		
	   }		
}
================================================
이것은 A 클래스 입니다
```
```java
Object클래스안에는 toString이라는 함수가 정의되어있는데 
이 함수는 현재 파라미터로 받는 객체의 값을 문자열로 반환해주는 함수이다

public static String valueOf(Object obj) {
        return (obj == null) ? "null" : obj.toString();
    } // 고슬링 아저씨가 toString 정의해놓은 함수

▶ 이게 바로println의 정의인데 여기서 만약 객체가 null이면 "null"을 반환 
null이 아니면 toString함수 호출 함수안에 오버라이딩 안되어있다면 부모안에 함수 호출
문자열이 정의되어있지않다면 주소값은 '@~~~'라고 출력하게 정의되어있기 때문
→ 자손안에 toSting으로 오버라이딩해서 문자열 출력결과 넣어줘야한다(아님 주소값 뿌림)
따라서 위 예시는 이러한 일련의 과정들이 적용된 케이스

println자체가 toString을 호출하는 함수로 정의되어있기때문이다 
```

<br>

# 3. class 이름 및 함수 에서 final 의 의미는?
: class 이름이나 함수에 final이 붙는다는건 이 클래스나 함수가 인터페이스가 적용되고 있음을 의미 <br>
interface안에서 final 사용은 함수에서는 오버라이딩X , 클래스는 상속이 X 

# 4. 연습문제 7-22 번을 푸시오
![4  연습문제 7-22 번](https://user-images.githubusercontent.com/74290204/102061274-99894b80-3e36-11eb-9633-1ccfb47ec6f1.PNG)
<br>
![4  연습문제 7-22 번 (추가)](https://user-images.githubusercontent.com/74290204/102061281-9b530f00-3e36-11eb-98cb-aa7f80c2086d.PNG)

```java

public class TranningMain {

	public static void main(String[] args) {
		Circle circle = new Circle();
		System.out.println(circle.calcArea(3.0));
		
		Rectangle rectangle = new Rectangle();
		System.out.println(rectangle.calcArea(5.0, 2.0));
		System.out.println(rectangle.isSquare());
	}
}

class Circle extends Shape {
	double radius;
	
	Circle() {
	}
	
	Circle(double radius) {
		this.radius = radius;
	}
	
	double calcArea() {
		return 0;
	}
	
	double calcArea(double radius) {
		return radius*radius*Math.PI;
	}
}

class Rectangle extends Shape {
	double width, height;
	
	Rectangle() {
	}
	
	Rectangle(double width, double height) {
		this.width = width;
		this.height = height;
	}
	
	double calcArea() {
		return 0;
	}
	
	double calcArea (double width, double height) {
		return width*height;
	}
	
	public boolean isSquare() {
		String trueOrFalse = null;
		if(trueOrFalse.equals(Circle.class)) {
			return false;
		}else {
			return true;
		}
	}
}
```
<br>

# 5. 연습문제 7-23 번을 푸시오
![5  연습문제 7-23 번 re](https://user-images.githubusercontent.com/74290204/102061812-4368d800-3e37-11eb-876e-18150897e59a.PNG)

```java
// 면적의합 0나옴
public class TranningMain {

	public static void main(String[] args) {
		Shape[] arr = { new Circle(5.0), new Rectangle(3, 4), new Circle(1) };

		System.out.println("면적의 합: "+ sumArea(arr));
	}

	private static double sumArea(Shape[] arr) {
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
	double radius;
	
	Circle() {
		
	}
	
	Circle(double radius) {
		this.radius = radius;
	}
	
	double calcArea() {
		return 0;
	}
	
	double calcArea(double radius) {
		return radius*radius*Math.PI;
	}
}

class Rectangle extends Shape {
	double width, height;
	
	Rectangle() {
		
	}
	
	Rectangle(double width, double height) {
		this.width = width;
		this.height = height;
	}
	
	double calcArea() {
		return 0;
	}
	
	double calcArea (double width, double height) {
		return width*height;
	}
	
	public boolean isSquare() {
		String trueOrFalse = null;
		if(trueOrFalse.equals(Circle.class)) {
			return false;
		}else {
			return true;
		}
	}
}
```
<br>

# 6. interface 와 class 의 차이는?
- interface는 상수만 가능하고 함수의 {구현부분} 이 없다 
- class에서는 객체 생성이 가능O, interface는 객체생성X 

<br>

# 7. 다음을 프로그램 하시오 * 必 *
```java
interface Printable { // MS가 정의하고 제공한 인터페이스
   public void print(String doc);
}

 SPrinterDriver 와 LPrinterDriver를 만드시오
=============================================

public static void main(String[] args) {
   String myDoc = "This is a report about...";
   
   // 삼성 프린터로 출력
   Printable prn = new SPrinterDriver();
   prn.print(myDoc);
   System.out.println();

   // LG 프린터로 출력
   prn = new LPrinterDriver();
   prn.print(myDoc);
}
==============================================
출력:
From Samsung printer
This is a report about ...

From LG printer
This is a report about ...
```
```java
//Answer
public class TranningMain {

	public static void main(String[] args) {
		String myDoc = "This is a report about...";

		// 삼성 프린터로 출력
		Printable prn = new SPrinterDriver();
		prn.print(myDoc);
		System.out.println();

		// LG 프린터로 출력
		prn = new LPrinterDriver();
		prn.print(myDoc);
	}
}

interface Printable { // MS가 정의하고 제공한 인터페이스
	   public void print(String doc);
	}

class SPrinterDriver implements Printable {
	 public void print(String doc) {
		 System.out.println("From Samsung printer");
		 System.out.println(doc);
	 }
}

class LPrinterDriver implements Printable {
	public void print(String doc) {
		System.out.println("From LG printer");
		System.out.println(doc);
	}
}
```
<br>

# 8.@Override 에 대하여 설명하시오
: 오버라이드는 Annotation의 한 종류로 에러나 다른 결과에 영향을 미치지않고 <br> 
오버라이딩 됐다는것을 컴파일러나 개발자에게 직관적으로 보여주는 것이다
- 오버라이딩이 아닌 오버로딩이 적용된다면 error를 보여줌

<br>

# 9. interface 에 대하여 설명하시오
: interface는 클래스와 거의 동일하지만 클래스와 다르게 구현부분이 존재하지않는다 
>> interface 특징
1. 상수, 추상메소드만 가능하고 {구현부분}이 X → 구현부분은 자손이 실행
   - 상속에서 오버라이딩은 자손꺼
2. 구현부분이 존재하지 않는 함수는 abstract를 입력해야하는데 interface는 생략이 가능하고 <br>
interface의 제한자는 public만 가능 
   - 생력가능한 이유는 붙이지 않더라도 컴파일러가 자동으로 생성해주기 때문!
3. interface는 클래스와 달리 객체생성이 되지 않지만 선언은 가능하다 
```java
Printable p = new Printable(); // 객체생성X
Printable p; // 이런 선언은 가능! 
```
4. interface의 선언은 class 클래스명 **implements** interface명 
   - 상속에서 extends와 비슷하게 기능한다
5. interface는 상속과 달리 다중상속이 가능하다 

>> tip] interface명을 지을때 1. I를 붙이거나 (public interface ICalculator) <br> 2. 이름 끝에 able을 붙인다(public interface Caculatable)

<br>

# 10. interface에 올 수 있는 두가지는?
## abstract , public 
>> tip] public은 클래스내에서 한개만 존재가능 <br> 관습적으로 public은 하나의 파일 안에서 진입점 같은 역활이기 때문에 <br> 두개 이상 존재하면 자동으로 error발생

<br>

# 11. abstract 키워드에 대하여 설명하시오
- abstract : '추상의'란 뜻 → 자손이 구현해라! 
- 정의 : 추상클래스란 하나 이상의 추상 메서드를 포함하고 있는 클래스 <br> 추상 클래스는 인터페이스와 마찬가지로 선언만 있고 구현부분이 존재하지X 
- abstract 함수가 포함되어있다면 클래스도 abstract키워드를 붙여줘여함
-용도 : 실체 클래스들의 공통된 필드와 메소드의 이름을 통일할 목적
>> 실무에서 협업을 할 때 통일된 양식이 필요하기 때문에 추상클래스, 인터페이스를 사용한다!

<br>

# 12. 아래의 출력 결과가 아래와 같이 나오도록 프로그래밍 하시오
```java
Object obj = new Circle(10);
System.out.println(obj);
=============================
출력: 넓이는 100 입니다.
```
```java
// Answer

/*  나는 Object클래스를 만들려고함 만들지 않아도되는데 
Object클래스는 이미 자바에서 제공하고 있고 최상위 클래스임!!(모든 클래스가 상속하는)
따라서 만들지않아도 이미 상속*/ 
class Circle {
	double radius;
	
	Circle(double radius) {
		this.radius = radius;
	}
	
	public double getArea() {
		return radius*radius*Math.PI;
	}
	
	public String toString() {
		return "넓이는 " + getArea() + "입니다.";
	}
}
```

<br>

# 13. ★ 아래의 메모리를 그리시오 ★
```java
class MobilePhone {
    protected String number;
    
    public MobilePhone(String num) {
        number = num;
    }    
    public void answer() {
        System.out.println("Hi~ from " + number);
    }
}

class SmartPhone extends MobilePhone { 
    private String androidVer;
    
    public SmartPhone(String num, String ver) {
        super(num);
        androidVer = ver;
    }    
    public void playApp() {
        System.out.println("App is running in " + androidVer);
    }
}
=======================================
	MobilePhone phone = new SmartPhone("010-555-777", "Nougat");
    	phone.answer();    	
    	SmartPhone s = (SmartPhone)phone;    	
    	s.playApp();
```
![메모리 구조](https://user-images.githubusercontent.com/74290204/102077596-e7a94980-3e4c-11eb-9863-a613f10048b7.png)
