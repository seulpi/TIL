# 1. String 클래스 에서 문자열 비교시 equal을 쓰는 이유는?
: equal 함수는 Object 클래스 안에 있는 함수이다 <br>
String은 객체이고 equal함수는 객체에 대해 비교할 수 있는 함수이기 때문에 사용한다 <br> equal은 개발자가 원하는 결과로 바꿔서 사용 가능하다 (오버라이딩)


# 2. shall copy, deep copy 의 차이는?
: copy는 객체가 참조하는 주소와 값만 복사해서 사용할 수 있고 <br> 객체 안에 또다른 객체를 생성한 주소까지는 복사해주지 않는다
- shall copy는 원본에서 참조하는 주소와 값을 복사하고 <br> 그 값을 메모리에 따로 객체를 생성해서 **저장하지 않는다**  <br> 그렇기 때문에 원본과 같은 주소를 참조하는 형식 
- deep copy는 원본에서 참조하는 주소와 값을 복사하고 <br> 그 주소와 값을 메모리에 따로 객체를 생성해서 **저장한다** <br> 
따라서 deep copy는 shall copy와 다르게 원본과 다른 주소를 참조는 형식 <br> 값을 중간에 변경한다면 원본에는 전혀 영향을 주지 않고 복사해서 새로 생성한 객체의 값이 변한다

# 3. 금일 배운 Rectangle의 shall copy 와 deep copy 일때의 그림을 그리시오
![clone](https://user-images.githubusercontent.com/74290204/102446980-5b786b80-4072-11eb-83a9-409af15b55fc.png)

# 4. 다음 main()이 실행되면 아래 예시와 같이 출력되도록 MyPoint 클래스를 작성하라 
```java
public static void main(String [] args) {
	MyPoint p = new MyPoint(3, 50);
	MyPoint q = new MyPoint(4, 50);
	System.out.println(p);
	if(p.equals(q)) {
        System.out.println("같은 점");
    }
	else System.out.println("다른 점");			
}
======================================================
▶
Point(3,50)
다른점
```
```java
// Answer
class MyPoint {
	int x, y;
	
	MyPoint(int x, int y) {
		this.x = x;
		this.y = y;
	}
	
	public String toString() {
		return "Point(" + x + "," + y + ")";
	}	
}
```

# 5. 아래를 프로그래밍 하시오
```java
중심을 나타내는 정수 x, y와 반지름 radius 필드를 가지는 Circle 클래스를 작성하고자 한다. 
생성자는 3개의 인자(x, y, raidus)를 받아 해당 필드를 초기화하고, 
equals() 메소드는 두 개의 Circle 객체의 중심이 같으면 같은 것으로 판별하도록 한다.

public static void main(String[] args) {
	Circle a = new Circle(2, 3, 5); // 중심 (2, 3)에 반지름 5인 원
	Circle b = new Circle(2, 3, 30); // 중심 (2, 3)에 반지름 30인 원
	System.out.println("원 a : " + a);
	System.out.println("원 b : " + b);
	if(a.equals(b))
		System.out.println("같은 원");
	else 
		System.out.println("서로 다른 원");
}

=====================================================================
▶
원 a : Circle(2,3)반지름5
원 b : Circle(2,3)반지름30
같은 원
```
```java
class Circle {
	private int x, y;
	private double radius;
	
	Circle(int x, int y, double radius) {
		this.x = x;
		this.y = y;
		this.radius = radius;
	}
	
	@Override 
	public boolean equals(Object obj) {
		if(this.x == ((Circle)obj).x || this.y == ((Circle)obj).y) {
			return true;
		}else {
			return false;
		}
	}
	
	public String toString() {
		return "Circle(" + x + "," + y +")반지름 " + radius;
	}
}
```

# 6. 문자열을 입력받아 한 글자씩 회전시켜 모두 출력하는 프로그램을 작성하라<br> (클래스로 작성할 필요없이 메인에서 직접 할것)
## (하는중)
```java
문자열을 입력하세요. 빈칸이나 있어도 되고 영어 한글 모두 됩니다.
I Love you
 Love youI
Love youI 
ove youI L
ve youI Lo
e youI Lov
 youI Love
youI Love 
ouI Love y
uI Love yo
I Love you

[Hint] Scanner.nextLine()을 이용하면 빈칸을 포함하여 한 번에 한 줄을 읽을 수 있다
```
```java
import java.util.Scanner;

public class Answer19_06 {

	public static void main(String[] args) {
		
		Scanner sc = new Scanner(System.in);
		String text;
		System.out.println("문장을 입력하세요.");
		text = sc.nextLine();
		
		
		String[] textArr;
		String[] temp = new String[0];
		
		
		textArr = text.split(""); // 문자열입력하면 다 들어가있는 상태 
		
		// a p p l e 5 0~4 j = 1~5 2~6
		for(int i = 0; i <textArr.length; i++) {
			
			
			for(int j = i; j <textArr.length + i; j++) {
				if(j > textArr.length) {   
					j = textArr.length - i +1; 
				}
				temp[j+1] = textArr[j];
			}
			
			for(int k = 0; k < temp.length; k++) {
				System.out.println(temp[k]);
			}
		}
	}
}
```

# 7. 래퍼 클래스란 무엇인가?
: 무언가를 감싸는 함수 <br>
자바는 기본적으로 객체지향언어이기 때문에 사실상 우리가 쓰는 기본타입에 대해서도 객체를 만들어 줄 때가 종종있다 <br>
그러한 이유 때문에 기본데이터 형을 감싸기 위한 목적을 가진 것이 바로 래퍼 클래스이다 <br>
래퍼 클래스 안 내부에는 기본 타입에 대한 함수 선언들과 기본타입에 대한 값을 반환하는 내용이 들어있다
- 래퍼클래스의 최상위 클래스는 Number class!
   - Number class의 정의된 함수는 abstract로 정의되어있고  <br> 이 함수들은 자손이 구현할 수 있어서 형 변환 없이 사용이 가능하다
```java
Integer num1 = new Integer(29);
	System.out.println(num1.intValue());
	System.out.println(num1.doubleValue());
		
Double num2 = new Double(3.14);
	System.out.println(num2.intValue());
    System.out.println(num2.doubleValue());
```

# 8. auto unboxing 이란?
: wrapper class를 정의하는 용어로는 '포장클래스'라고도 하는데 여기서 파생되는 단어가 '박싱' '언박싱'이다 <br>
- 박싱(Boxing): 기본형 → 객체로 바꾸는 것 
- 언박싱(UnBoxing) : 객체 → 기본형으로 바꾸는 것 (함수를 풀어헤친다라고 이해해도 좋다) 

>> 언박싱을 하게 되면 .을 찍고 함수를 호출해줘야 하는데 함수 호출 부분을 생략하면 <br> 컴파일러가 자동으로 호출해주는 것을 말한다
```java
// unboxing
int num1 = integer.intValue();
double num2 = double1.doubleValue(); 

// auto unboxing
int num1 = integer; 
double num2 = double1;

** 연산도 다이렉트로 가능하다! **
```

# 9. 다음 조건을 만족하는 클래스 Person을 구현하여 테스트하는 프로그램을 작성하시오 * 必 *
```java
- 클래스 Person은 이름을 저장하는 필드 구성

- 클래스 Person은 상위 클래스 Object의 메소드 equals()를 오버라이딩하여 이름이 같으면 true를 반환하는 메소드 구현

- 다음과 같은 소스로 클래스 Person을 점검

Person p1 = new Person("홍길동");
System.out.println(p1.equals(new Person("홍길동")));
System.out.println(p1.equals(new Person("최명태")));
```
```java
//Answer
class Person {
	private String name;
	
	Person(String name) {
		this.name = name;
	}
	
	@Override
	public boolean equals(Object obj) {
		if(this.name == ((Person)obj).name) {
			return true;
		}
		return false;
	}
}
```

# 10. 다음 조건을 만족하는 클래스 String의 객체 이용 프로그램을 작성하여 <br> 메소드 equals()와 연산자 == 의 차이를 비교 설명하시오 * 必 * 
```java
- 메소드 equals()와 비교 연산자 ==의 차이를 다음 소스로 점검

String s1 = new String("java");
String s2 = new String("java");
String s3 = s2;

System.out.println(s1 == s2);
System.out.println(s1.equals(s2));
System.out.println(s2 == s3);
System.out.println(s2.equals(s3));
```

>> 연산자 '==' 는 객체가 참조하는 주소값이 같은지에 대한 물음 <br>
equals()는 객체 안에 내용이 같은지에 대한 물음 (즉, equals는 String객체가 갖고있는 문자열에 대한 비교이다)


# 11. 다음 조건을 만족하도록 오늘의 정보를 출력하는 프로그램을 작성하시오
## (하는중)
```java

- 클래스 Calendar의 객체의 다음 메소드를 사용하며

 * get(Calendar.DAY_OF_WEEK_IN_MONTH) : 달에서 요일의 횟수 반환
 * get(Calendar.DAY_OF_WEEK) : 요일을 반환, 1이 일요일
 * get(Calendar.WEEK_OF_MONTH) : 월의 주 횟수를 반환
 * get(Calendar.DAY_OF_YEAR) : 해의 날짜를 반환
 * get(Calendar.WEEK_OF_YEAR) : 해의 주 횟수를 반환

- 다음과 같이 출력되도록 한다.

오늘은 2012년 6월 17일 일요일입니다.
이 달의 3번째 일요일입니다.
이 달의 4번째 주입니다.
이 해의 169일입니다.
이 해의 25번째 주입니다. 
```
```java
import java.util.Scanner;

public class Tutorial {

	public static void main(String[] args) {
		Calender calender = new Calender();
		calender.input();
		calender.DAY_OF_WEEK_IN_MONTH();
		calender.DAY_OF_WEEK();
		calender.WEEK_OF_MONTH();
		calender.DAY_OF_YEAR();
		calender.WEEK_OF_MONTH();
	
	}
}

class Calender {
	
	final int totalDay = 365;
	final int based_month = 12;
	final int based_day = 31;
	final int based_week = based_day/7;
	int based_year = 1;
	int sum = 0;
	int monthP;
	
	
	int year,month, day;
	String dayOfWeek;
	
	public void input() {
	Scanner sc = new Scanner(System.in);
	System.out.println("오늘 날짜의 정보를 입력하세요");
	year = sc.nextInt();
	month = sc.nextInt();
	day = sc.nextInt();
	dayOfWeek = sc.next();
	}
	
	final public void DAY_OF_WEEK_IN_MONTH() {
		System.out.println("오늘은 " + year + "년 " + month + "월 " + day + "일 " + dayOfWeek + "입니다." );
	}
	
	final public void DAY_OF_WEEK() { 
		for(int i = 0; i < based_week; i++) {
			if(i == day/4) {
				System.out.println("이달의 " + i + "번째 " + dayOfWeek + "입니다.");
			}
		}
	}
	
	final public void WEEK_OF_MONTH() {
		for(int i = 0; i < based_week; i++) {
			if(i == day/4 + 1) {
				System.out.println("이달의 " + i + "번째 " + "주 입니다.");
			}
		}
	}
	
	final public void DAY_OF_YEAR() {
		sum = (based_day* month) + day;
		System.out.println("이 해의 " + sum + "일 입니다.");
	}
	
	final public void WEEK_OF_YEAR() { 
		monthP = (based_week*month) +1;
		System.out.println("이 해의 " + monthP + "주 입니다.");
	}
}
```
