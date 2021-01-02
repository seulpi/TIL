# 1. throws 에 대하여 설명하시오
- throws는 try- catch와 같은 에러처리방식의 한 종류이다 <br>
>> **[throws 사용 방법]** <br>
함수 선언() **throws** { }

- throws는 '던진다'고 하는데 에러 처리를 어디에 던져서 처리할까의 방식으로 이해하면된다 <br>
▶ 어디로 던져서 처리할까? <br>
   - 자신을 **호출한** 함수에 던져서 에러처리하라고 요구한다 <br>
   
![throw](https://user-images.githubusercontent.com/74290204/102306314-4b468a80-3fa5-11eb-939d-7dec7d56f6d1.PNG) 

- throws는 main함수에서도 사용가능하나 main에서 잡는건 실무에서 적절한 방법이 아님!
- throws는 함수단위로 에러를 넘기는 것이기 때문에 클래스 단위로 넘겨주진 않는다 

# 2. 아래가 컴파일 에러가 나는 이유에 대하여 설명하시오
```java
try {
    int num = 6 / 0;	
} catch (Exception e) {
	e.printStackTrace();
} catch (InputMismatchException e) {
	e.printStackTrace();
}
```
☞ **Exception**은 모든 에러 객체의 최고 조상님이다 <br> 근데 Exception이 다른 오류처리 구문보다 먼저 오게 되면 Exception 혼자 다 처리 가능하기 때문에 <br> 다른 오류처리 구문이 필요없다 그래서 하위 객체의 오류처리를 타지않고 그대로 구문을 탈출! <br> 근데 오류처리를 하라고 하니 error가 발생하는 것 UnreachableException
   - 단, 에러처리는 개발자에게 '이런 오류가 날 수 있습니다'하고 명시하는 것이기 때문에 <br>
   최상위 객체로 명시하는 것보다 자세한 오류 처리를 하는 것이 GOOD!


# 3. try with resource 에 대하여 설명하시오
- try with resource는 자바 1.7ver부터 사용 가능한 문법
- 기존의 try - catch 구문은 ()없이 {}로 감싸서 오류 처리를 하고 실행한 메모리를 이제 사용하지 않고 없애도 좋다는 <br> 의미전달로(JVM에게) close를 직접 호출해줘야했었음 → 이 부분이 쌓이다 보면 out of memory → 향상된 게 try with resource 
  - try with resources는 실행이 종료될때 close()를 자동 호출해주면서 메모리를 날려줌
```java
  // ↓ 이게 resource
try(객체를 직접생성) { }

try-catch-finally는 .close() 직접 호출
try with resource는 .close() 자동 호출 
[모든걸 close해주는건 아니고 AutoCloseable을 가지고 있는 것(:implements or 상속) 에 한해서 자동 호출] 
```

# 4. equals 함수에 대하여 설명하시오
- equals는 최상위 Object에 존재하는 함수
  - 따라서 따로 오버라이딩을 해주지 않아도 사용 가능 (모든 클래스는 Object를 상속받으니까)
```java
// equals 함수의 정의
 public boolean equals(Object obj) {
        return (this == obj);
} 

▶ 이렇게 정의되어있기 때문에 호출가능 (부모가 갖고있기 때문에)
this : 비교할 자기자신이 참조하고 있는 주소값을 의미 
obj : 비교할 변수가 참조하고 있는 객체의 주소값을 의미
```

# 5. 아래를 프로그래밍하시오
```java
과일, 사과, 배, 포도를 표현한 클래스를 만들고 
이들 간의 관계를 고려하여 하나의 클래스를 추상 클래스로 만들어 메소드 print()를 구현하고
다음과 같은 소스와 결과가 나오도록 클래스를 작성하시오.

// 소스
Fruit fAry[] = {new Grape(), new Apple(), new Pear());

for(Fruit f : fAry)

f.print();

// 결과

나는 포도이다.
나는 사과이다.
나는 배이다.
```
```java
//Answer 
public class TranningMain {

	public static void main(String[] args) {
		Fruit fAry[] = {new Grape(), new Apple(), new Pear()};
		
		for(Fruit f : fAry)
			f.print();
	}
}
abstract class Fruit {
	public abstract void print();
}

class Grape extends Fruit {
	String grape = "포도";
	
	public void print() {
		System.out.println("나는 " + grape + "이다.");
	}
}
class Apple extends Fruit {
	String apple = "사과";
	
	public void print() {
		System.out.println("나는 " + apple + "이다.");	
	}
}
class Pear extends Fruit {
	String pear = "배";
	
	public void print() {
		System.out.println("나는 " + pear + "이다.");	
	}
}
```

# 6. 다음 조건을 만족하도록 클래스 Person과 Student를 작성하시오
## 소스코드 스캐너로 받아서 소스 줄여보기!
```java
- 클래스 Person
* 필드 : 이름, 나이, 주소 선언

- 클래스 Student
* 필드 : 학교명, 학과, 학번, 8개 평균평점을 저장할 배열로 선언
* 생성자 : 학교명, 학과, 학번 지정
* 메소드 average() : 8개 학기 평균평점의 평균을 반환
- 클레스 Person과 Student 프로그램 테스트 프로그램의 결과 : 8개 학기의 평균평점은 표준입력으로 받도록한다.
이름 : 김다정
나이 : 20

주소 : 서울시 관악구
학교 : 동양서울대학교
학과 : 전산정보학과
학번 : 20132222

----------------------------------------

8학기 학점을 순서대로 입력하세요

1학기 학점  → 3.37
2학기 학점  → 3.89
3학기 학점  → 4.35
4학기 학점  → 3.76
5학기 학점  → 3.89
6학기 학점  → 4.26
7학기 학점  → 4.89
8학기 학점  → 3.89 

----------------------------------------

8학기 총 평균 평점은 4.0375점입니다.
```

```java
//Answer
import java.util.Scanner;

public class TranningMain {

	public static void main(String[] args) {
		
		Student person = new Student("김다정", 20, "서울시 관악구", "동양서울대학교", "전산정보학과", 20132222);
		person.studentPrint();
		System.out.println("------------------------------");
		
		person.average();
		
	}
}

class Person {
	private int age;
	private String name, adress; 
	
	Person(String name, int age, String adress) {
		this.name = name;
		this.age = age;
		this.adress = adress;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getAdress() {
		return adress;
	}

	public void setAdress(String adress) {
		this.adress = adress;
	}
}
class Student extends Person {
	
	private String schoolN, major;
	private int studentID;
	private double[] avgArry;
	private int top;
	
	Student(String name, int age, String adress, String schoolN, String major, int studentID) {
		super(name, age, adress);
		this.schoolN = schoolN;
		this.major = major;
		this.studentID = studentID;
	}
	
	public void studentPrint() {
		System.out.println("이름: " + super.getName());
		System.out.println("나이: " + super.getAge());
		System.out.println();
		System.out.println("주소: " + super.getAdress());
		System.out.println("학교: " + schoolN);
		System.out.println("학과: " + major);
		System.out.println("학번: " + studentID);
		
	}
	
	public void average() {
		Scanner sc = new Scanner(System.in);
		double sum = 0.0;
		avgArry = new double[8];
	
		for(int i = 0; i < avgArry.length; i++) {
			System.out.print(i + 1 + "학기 학점 → ");
			avgArry[i] = sc.nextDouble();
			sum += avgArry[i];
		}
		System.out.println("-----------------------------------------------------");
		System.out.println(avgArry.length + "학기의 총 평균 평점은 " + sum/avgArry.length + "점 입니다.");
	}
}
```

# 7.아래를 프로그래밍하시오
```java
>> 다음은 도형의 구성을 묘사하는 인터페이스이다

interface Shape {
   final double PI = 3.14; // 상수
   void draw(); // 도형을 그리는 추상 메소드
   double getArea(); // 도형의 면적을 리턴하는 추상 메소드
   default public void redraw() { // 디폴트 메소드
      System.out.print("--- 다시 그립니다.");
      draw();
   }
}

>> 다음 main() 메소드와 실행 결과를 참고하여, 인터페이스 Shape을 구현한 
클래스 Circle를 작성하고 전체 프로그램을 완성하라.

public static void main(String[] args) {
   Shape donut = new Circle(10); // 반지름이 10인 원 객체
   donut.redraw();
   System.out.println("면적은 "+ donut.getArea());
}
```
```java
//Answer
public class TranningMain {

	public static void main(String[] args) {
		
		Shape donut = new Circle(10);
		donut.redraw();
		System.out.println("면적은 " + donut.getArea());

	}
}

interface Shape {
	final double PI = 3.14; // 상수

	void draw(); // 도형을 그리는 추상 메소드

	double getArea(); // 도형의 면적을 리턴하는 추상 메소드

	default public void redraw() { // 디폴트 메소드
		System.out.print("--- 다시 그립니다.");
		draw();
	}
}

class Circle implements Shape {
	final double PI = 3.14; 
	private double radius;
	
	Circle(double radius) {
		this.radius = radius;
	}
	
	public void draw() {
		if(radius == 0) {
			System.out.println("이 도형은 원이 아닙니다");
		} else {
			System.out.println("이 도형은 원이 맞습니다");
		}
	}

	public double getArea() {
		return radius*radius*PI;
	}	
}
```

# 8. 7번과 연관된 문제입니다
```java
다음 main() 메소드와 실행 결과를 참고하여, 문제 7의 Shape 인터페이스를 구현한 
클래스 Oval, Rect를 추가 작성하고 전체 프로그램을 완성하라.

public static void main(String[] args) {
   Shape[] list = new Shape[3]; // Shape을 상속받은 클래스 객체의 레퍼런스 배열
   list[0] = new Circle(10); // 반지름이 10인 원 객체
   list[1] = new Oval(20, 30); // 20x30 사각형에 내접하는 타원
   list[2] = new Rect(10, 40); // 10x40 크기의 사각형
   for(int i=0; i<list.length; i++) list[i].redraw();
   for(int i=0; i<list.length; i++) System.out.println("면적은 "+ list[i].getArea());
}
--- 다시 그립니다.반지름이 10인 원입니다.
--- 다시 그립니다.20x30에 내접하는 타원입니다.
--- 다시 그립니다.10x40크기의 사각형 입니다.
면적은 314.0
면적은 1884.0000000000002
면적은 400.0
```
```java
//Answer
public class TranningMain {

	public static void main(String[] args) {
		Shape[] list = new Shape[3]; // Shape을 상속받은 클래스 객체의 레퍼런스 배열
		list[0] = new Circle(10); // 반지름이 10인 원 객체
		list[1] = new Oval(20, 30); // 20x30 사각형에 내접하는 타원
		list[2] = new Rect(10, 40); // 10x40 크기의 사각형
		for (int i = 0; i < list.length; i++)
			list[i].redraw();
		for (int i = 0; i < list.length; i++)
			System.out.println("면적은 " + list[i].getArea());

	}
}

interface Shape {
	final double PI = 3.14; // 상수

	void draw(); // 도형을 그리는 추상 메소드

	double getArea(); // 도형의 면적을 리턴하는 추상 메소드

	default public void redraw() { // 디폴트 메소드
		System.out.print("--- 다시 그립니다.");
		draw();
	}
}

class Circle implements Shape {
	final double PI = 3.14; 
	private double radius;
	
	Circle(double radius) {
		this.radius = radius;
	}
	
	public double getCircle() {
		return radius;
	}
	
	public void draw() {
		System.out.println("반지름이 " + radius + "인 원 객체");
	}

	public double getArea() {
		return radius*radius*PI;
	}
	
}

class Oval extends Circle {
	private double num;
	
	Oval(double radius , double num) {
		super(radius);
		this.num = num;
	}
	
	public void draw() {
		System.out.println(super.getCircle()+ "x" + num + "사각형에 내접하는 타원");
	}
	
	public double getArea() {
		return super.getCircle()*num*PI;
	}
}

class Rect implements Shape {
	private double weight, height;
	
	Rect(double weight, double height) {
		this.weight = weight;
		this.height = height;
	}
	
	public void draw() {
		System.out.println(weight + " x" + height + " 크기의 사각형입니다.");
	}

	public double getArea() {
		return weight*height;
	}	
}
```
