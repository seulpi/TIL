# 자바의 특성
1. 정보은닉(Hidden Information)
2. 상속(Inheritance)
3. 다형성(Polymorphism)
3. 캡슐화 (Encapsulation)

# 상속(Inheritance)
## @ 상속의 정의
- 현실세계에서 재산을 상속하듯이 자바의 세계에선 클래스가 클래스를 상속한다 <br>
- 상속관계에서 부모클래스, 자식클래스가 존재한다<br>
>> 부모클래스 = 상위클래스 = Super 클래스 <br>
자식클래스 = 하위클래스 = Sub클래스
### *상속관계 구조
![Untitled Diagram](https://user-images.githubusercontent.com/74290204/101778982-fd123100-3b37-11eb-99af-0a58fbb017f3.png)


- 자식클래스는 부모클래스의 기능과 멤버들을 자유롭게 사용이 가능하다(물려받았으니까)
- 부모클래스에서의 변수 초기화 가능하지만 초기화를 할 수 있게하면<br> 정보은닉화(private)가 깨지기 때문에 사용하지 않는것이 좋다
- 자식의 생성자 호출 → 부모의 디폴트 생성자 메모리 in → 자식의 생성자 메모리 in

## @ 상속 선언방법
- extends : 상속의 키워드
>> class 자식클래스명 **extends** 부모클래스 { } 
```java
class SuperC {
	public SuperC() { 
		System.out.println("I'm Super Class");
	}
}
class SubCLS extends SuperC { // SuperC: 부모클래스 SubCLS: 자식클래스  
	public SubCLS () {
		System.out.println("I'm Sub Class");
	}
}
```
▶ 상속에서 부모 변수를 호출하려면? <br>
상속아닌 관계에서 **this.** 사용 <br>
상속관계에서는 **super(변수명)** 사용 <br>

### [알고가자!]
- this() / this. / super() / super.
```java
1. this() : 객체 내부에서 또 다른 생성자 호출
2. this. :자기자신을 의미 
3. super() : 부모클래스 멤버들 호출 
4. super. : 부모클래스 함수 호출

(헷갈린 this)
class Friend {
	protected String name;
	protected String phone;

	public Friend(String na, String ph) {
		name = na; 
		phone = ph; // this는 생략이 가능하고 this는 생성자에서 na가 name이다를 가르킬때 사용(훨씬 직관적이겠지)
	}
```
- 자바는 only **단일상속(1개만 상속)만** 가능 
  - C언어는 다중 상속도 가능(2개 이상 상속)

```java
public class Static {
 public static void main(String[] args) {
	 SuperCLS obj1 = new SuperCLS();

	 SuperCLS obj2 = new SuperCLS();

	 SubCLS obj3 = new SubCLS();

	 obj3.showCount();
 }
}

class SuperCLS {

	static int count = 0; /* 다른 주소를 가진 객체지만 static은 공유하는 변수이기 때문에 값이 변하는 것

							자식에서 그대로 받아서 써먹을수 있고 그렇다고해서 static의 성질이 변하는게 아님

							private을 넣으면 아무리 static이어도 공유안됨*/

	public SuperCLS() { // 다른 생성자가 없는 상태에서 자식에서 부모를 호출하면 디폴트 생성자를 호출하게 됨
		count++;
	}
}

class SubCLS extends SuperCLS {
	public void showCount() { 
		System.out.println(count);
	}
}
```

## @ 상속의 특성
1. 자식클래스 = 부모의 특성 + 자식클래스의 특성 <br>
(부모클래스는 부모클래스만)
2. B is A 관계 (B는 A이다) <br> - 자매품 has A (A를 가지고 있는 포함관계)
```java
EX>>

- is A 관계 
"컴퓨터"는 "노트북"이다 

- has A 관계
"컴퓨터"안에는 "CPU"가 있다
(컴퓨터 안에는 메모리,cpu,램 등등이 존재)
```
3. 상속 START! → 메모리에 부모클래스 first 입장 → 자식클래스 뒤따라 입장 <br>
▶ 근데 반대라면? (자식오고 부모오고) 그러면 아예 입장불가! 부모가 뭘 물려줘야 받지
```java
A a = new A(); 
B b = new B();
A ab = new B(); /* 부모 변수명 = new 자식 ; 
		1.다형성[부모=자식(without 형변환:형변환을 하지않음)]: 형이 많다 상속만 받으면 부모는 여러개의 타입을 받아낼수있다 */
B ba = new B(); /* 자식 변수명 = new 부모 ; error[자식≠부모]*/
	}
}

class A { }
class B extends A { }
```
```java
>> ★ VERY VERY IMPPORTANTS ★
public class RandomMain {
	public static void main(String[] args) {
		SmartPhone ph1 = new SmartPhone("011-555-7777", "Nougat");
		MobilePhone ph2 = new SmartPhone("012-345-6789", "Nougat");

		ph1.answer(); // MobilePhone(부모)함수 호출

		ph1.playApp(); // SmartPhone(자식)함수 호출

		System.out.println();

		ph2.answer();

		ph2.playapp(); // 데이터타입이 MobilePhone이니까SmartPhone내에서 MobilePhone까지의 영역만 접근 가능

		SmartPhone ph3 = new MobilePhone("012-345-6789", "Nougat"); // 다 담아야되는데 부모만 메모리에 올라가고 자식 멤버들이 안올라가서 X
	}
}

class MobilePhone {

	protected String number;

	public MobilePhone(String num) {
		number = num;
	}

	public void answer() {
		System.out.println("Hi~ from" + number);
	}

}

class SmartPhone extends MobilePhone {

	private String androidVer;

	public SmartPhone(String num, String ver) {
		super(num);
		androidVer = ver;
	}

	public void playApp() {
		System.out.println("App is running in" + androidVer);
	}
}
```
### @ 상속의 활용 
- Q1. 다음 main 메소드 결과를 가지고 상속을 이용해 프로그래밍을 해라
```java

public static void main(String[] args) {
		ColorTV myTV = new ColorTV(32, 1024);
		myTV.printProperty();
}

>> 상속할 부모의 TV클래스는 다음과 같다

public class TV {
	private int size; 

	public TV(int size) { 
		this.size = size;
	}

	protected int getSize() {
		return size;
	}
}

// Answer

public class TV {
	private int size; // private 선언하면 상속못받아...힝..놓치지말자구ㅠㅠ
	public TV(int size) { 
		this.size = size;
	}

	protected int getSize() {
		return size;
	}
}

class ColorTV extends TV {

	int color;

	ColorTV(int size, int color) {
		super(size);
		this.color = color;
	}

	public void printProperty() {
		System.out.println(getSize() + "인치 " + color + "컬러"); // 호출방법1

		System.out.println(super.getSize() + "인치 " + color + "컬러"); //호출방법2

	}   /* 호출할때 size 가져오면 private으로 선언되서 사용X 
        따라서 getSize함수를 호출해야함*/
}
```
- Q2. 다음 main() 메소드와 실행 결과를 참고하여 ColorTV를 상속받는 IPTV 클래스를 작성하라
```java

public static void main(String[] args) {
   IPTV iptv = new IPTV("192.1.1.2", 32, 2048); 
   iptv.printProperty();
}
=============================================
나의 IPTV는 192.1.1.2 주소의 32인치 2048컬러


// Answer

public class TV {

	private int size;
	
	public TV(int size) { 
		this.size = size;
	}

	protected int getSize() {
		return size;
	}
}

class ColorTV extends TV {

	int color;

	ColorTV(int size, int color) {
		super(size);
		this.color = color;
	}
	protected int getSColor() {
		return color;
	}
	public void printProperty() {
		System.out.println(super.getSize() + "인치 " + color + "컬러"); 
}

class IPTV extends ColorTV {
	String address;

	IPTV(String address, int size, int color) {
		super(size, color);
		this.address = address;
	}

	public String getIptv() {
		return address;
	}

	public void printProperty() {
		System.out.println("나의 IPTV는 " + getIptv() + "주소의 " + super.getSize() + "인치, " + super.getSColor() +"컬러" ); //방법2

		System.out.print("나의 IPTV는 " + getIptv() + "주소의 " + " " ); // 방법2
		super.printProperty();
	}
}
```


## @ 오버라이딩 (핵심 Of 핵심)
- [!면접 필수 질문!] 오버라이딩이란? / 오버라이딩과 오버라이드의 차이점은?
>> 오버라이딩은 반드시 **상속관계**과 동반된다 
- 오버라이딩은 상속관계에서 부모와 자식의 함수 리턴타입, 함수명, 파라미터까지 **동일** <br>
**{구현하는 내용} 만 다름** 
```java

class Cake {
	public void yummy() {
		System.out.println("Yummy Cake");
	}
}
 
class CheeseCake extends Cake {
	public void yummy() {
		System.out.println("Yummy Cheese Cake");
	}
}
```
### @ 오버라이딩 중요 특성 
 - 오버라이딩은 **자식**꺼~ <br>
 한마디로 '덮어쓰는거'고 정확히 말하면 부모의 함수 주소값을 자식의 주소로 바꿔버리는 것 
 - 단! 변수는 오버라이딩의 대상이 아님!! <br>
 변수명이 같아도 메모리에 다르게 다른 공간에 들어간다 (변수이외의 멤버들은 자식으로 탈바꿈해버렷..)
 ```java
 public class RandomMain {
	public static void main(String[] args) {
		Cake c1 = new CheeseCake();
		CheeseCake c2 = new CheeseCake();
		c1.yummy(); 
		c2.yummy();
	}
}
class Cake {
	public void yummy() {
		System.out.println("Yummy Cake");
	}
}
class CheeseCake extends Cake {
	public void yummy() {
		System.out.println("Yummy Cheese Cake");
	}
}
=======================================================
▶ 
Yummy Cheese Cake
Yummy Cheese Cake

// ★오버라이딩은 '자식'꺼니까 출력하면서 덮어 쓴 결과
```

### @ 오버라이딩 된 메소드 호출 방법 
>> super.함수 (super로 내부 인스턴스에서만 호출 가능/ 외부에서는 호출할 방법이 X )

### @ 오버라이딩의 활용 
- Q1. Shape 를 함수오버라이딩 하는 Circle, Rectangle을 만드시오(원&사각형 넓이를 구해라)
```java
class Shape {
	public double getArea() {
	}
}
---------------------------------------------------------------

// Answer
public class TranningMain {

	public static void main(String[] args) {
		Circle circle = new Circle(3.0);
		System.out.println(circle.getArea());
	} // 나는 출력하는게 잘 안됨 

}

class Shape {
	public double getArea() {
		return 0.0;
	}
}

>> 위의 함수오버라이딩하는 Circle을 만드시오(원의넓이를 리턴) 

class Circle extends Shape {
	private double r;
	Circle(double r) {
		this.r = r;
	}

	@Override// 컴파일에 영향을 주지않고 에러를 잡지않지만 컴파일러와 개발자에게 '명확하게' 알려주는 역할
	public double getArea() {
		return r*r*Math.PI;
	}
}
```
- Q2. 다형성을 활용해서 Circle과 Rectangle의 면적의 합을 구하시오 
```java

public class TranningMain {
	public static void main(String[] args) {
		Shape[] shape = new Shape[2];
		shape[0] = new Circle(10);
		shape[1] = new Rectangle(10, 10);
		/* Circle과 Rectangle의 면적의 합을 구해라
		다형성이 좋은이유는 예전같으면 이거 일일히 다 더해서 구했는데 다형성이 가능해지면 간결하게 면적의 합을 구할 수 있기 때문 */

		double area = 0;
    
		for(int i = 0; i <shape.length; i++) {
			area = area + shape[i].getArea();
		}
		System.out.println(area);
	}
}

class Shape {
	public double getArea() {
		return 0.0
	}
}
class Circle extends Shape {
	private double r;

	Circle(double r) {
		this.r = r;
	}

	public double getArea() {
		return r*r*Math.PI;
	}
}

class Rectangle extends Shape {
	private double width;
	private double height;

	Rectangle(double width, double height) {
		this.width = width;
		this.height = height;
	}

	@Override
	public double getArea() {
		return width*height;
	}
}
```
