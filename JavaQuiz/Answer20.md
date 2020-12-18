# 1. BigInteger 클래스에 대하여 설명하시오
: long 타입으로도 담을 수 없는 큰 범위의 숫자를 담기 위한 클래스이다 <br>
BigInteger는 사칙연산을 다이렉트로 할 수 없기 때문에 사칙연산 함수를 이용해 사용해야한다

# 2. 아래의 결과 값은 false 출력이 된다.true 가 되도록 INum을짜시오 
```java
INum[] ar1 = new INum[3];
INum[] ar2 = new INum[3];
ar1[0] = new INum(1); ar2[0] = new INum(1);
ar1[1] = new INum(2); ar2[1] = new INum(2);
ar1[2] = new INum(3); ar2[2] = new INum(3);
System.out.println(Arrays.equals(ar1, ar2));
```
```java
// Answer
class INum {
	private int num;
	
	public INum(int num) {
		this.num = num;
	}
	
	@Override
	public boolean equals(Object obj) {
		if(this.num == ((INum)obj).num) {
			return true;
		}else {
			return false;
		}
	}
}
===================================================
▶ fasle이 나오는 이유는 equals가 주소값을 비교하는 함수이기때문에
eqauls를 오버라이딩해서 재정의해줘서 true가 올 수 있게 만들어줬다(문자열 비교할 수 있게)
```

# 3. 아래에서 정렬이 이름순으로 되게끔 하시오.Person 객체를 만드시오
```java
class ArrayObjSearch {
    public static void main(String[] args) {
        Person[] ar = new Person[3];

        ar[0] = new Person("Lee", 29);
        ar[1] = new Person("Goo", 15);
        ar[2] = new Person("Soo", 37);

        Arrays.sort(ar);
 ```
 ```java
class Person implements Comparable {
	private String name;
	private int age;

	Person(String name, int age) {
		this.name = name;
		this.age = age;
	}
	
	public int compareTo(Object o) {  // !!함수명에 CompareTo로 넣어서 오버라이딩안됨 함수명은 소문자로시작,,ㅎㅎ
	  Person p = (Person)o; 
	  return this.name.compareTo(p.name); 
	  /* compareTo가 사전적으로 비교하는건데 같으면 
	   * 0, > 1, < -1 (값은 이게 맞는지는 모르겠으나 정수 반환한다고 했는데 정확히는 모름)
	   * compareTo가 Sting 클래스 안에 있는 함수
	   */  
	}
	
	public String toString() {
		return name + ": " + age;
	}
 }
```

# 4. 위의 문제에서 사람의 이름 글자수가 많은순으로 정렬 되게끔 person 객체를 만드시오
```java
class Person implements Comparable {
	private String name;
	private int age;

	Person(String name, int age) {
		this.name = name;
		this.age = age;
	}
	
	public int compareTo(Object o) {  
	  Person p = (Person)o; 
	  return p.name.length() - this.name.length();
			  
	}
	
	public String toString() {
		return name + ": " + age;
	}
 }
 ```

 # 5. 경과시간을 맞추는 게임을 작성하라 
 ## *(????)*
 ```java
 다음 예시를 참고하면, <Enter> 키를 입력하면 현재 초 시간을 보여주고 여기서 10초에 더 근접하도록
  다음 <Enter> 키를 입력한 사람이 이기는 게임이다

10초에 가까운 사람이 이기는 게임입니다.
황기태 시작 키  >>
	현재 초 시간 = 42
10초 예상 후 키  >>
	현재 초 시간 = 50
이재문 시작 키  >>
	현재 초 시간 = 51
10초 예상 후 키  >>
	현재 초 시간 = 4
황기태의 결과 8, 이재문의 결과 13, 승자는 황기태
```

# 6. 지네릭이란?
: 실시간 에러나는 것을 컴파일 에러로 받아주기 위한 코드 <br> 최상위 Object 클래스로 상속받은 클래스를 여러개 만들다 보니 당연히 구현되지 말아야될 코드들이 <br>
에러를 던지지 않고 구현되면서 많은 문제들이 발생시킴 이러한 문제를 보완하기 위한 하나의 문법으로 제네릭이 생겼다
>> [사용 방법] <br> class 클래스명 < T > {  }
- 클래스 안에 참조형 타입들을 T로 대체 하겠다는 뜻 (Object로 받던 자리들을 T로 대체) <br>
: 'T' 는 타입 매개변수 (타입을 결정한다)
```java
Box<Apple> aBox = new Box<Apple>();

// Box<Apple> 까지가 aBox의 타입
```
- 제네릭은 다형성 적용 X <br> (다이렉트로 타입을 지정해서 넣어줬기 때문에 애초에 형변환을 할 수 없고 필요도X)

# 7. 아래를 프로그래밍 하시오
```java
Rectangle r1 = new Rectangle(5,6);
Rectangle r2 = new Rectangle(7,9);

Rectangle r3 = Rectangle.compareRect(r1,r2);

System.out.println(r3.getHeight() + " : " + r3.getWidth()  + "입니다.");
===========================================================
출력 : 9 : 7 입니다.
```
```java
//Answer
class Rectangle {
	private int width;
	private int height;
	
	Rectangle(int width, int height) {
		this.width = width;
		this.height = height;
	}
	
	public int getWidth() {
		return width;
	}

	public void setWidth(int width) {
		this.width = width;
	}

	public int getHeight() {
		return height;
	}

	public void setHeight(int height) {
		this.height = height;
	}
	
	public int recArea() {
		return width*height;
	}
	
	public static Rectangle compareRect(Rectangle o1, Rectangle o2) {
		if(o1.recArea() > o2.recArea()) {
			return o1;
		}else {
			return o2;
		}
	}
}
```

# 8. 아래를 프로그래밍 하시오
## *(하는중 : 내림차순 구현...못하는중,,)*
```java
- Rectangle 배열 4개를 만든후 스캐너 객체로 가로와세로를 입력하여 4개의 객체를 배열에 할당한다 
- getSortingRec 사각형 배열을 내림차순 정렬한다.
- 정렬이 제대로 되었는지 배열에 저장된 객체의 getArea()함수를 순서대로 호출한다.

Rectangle[] rec = new Rectangle[3];
........
Rectangle[] recSorting = Rectangle.getSortingRec(rec) 
......
```
```java
//Answer
import java.util.Arrays;
import java.util.Scanner;

public class Answer20_09 {

	public static void main(String[] args) {
		Rectangle[] rec = new Rectangle[4];
		int[]ar = new int[rec.length];
		int width, height;
		
		Scanner sc = new Scanner(System.in);
		System.out.println("사각형의 가로와 세로를 입력하세요.");
		
		for(int i = 0; i < rec.length; i++) {
			rec[i] = new Rectangle(sc.nextInt(), sc.nextInt());
			ar[i] = rec[i].getArea();
			System.out.println(ar[i]);
		}System.out.println();	
		
		Arrays.sort(ar);
		for(int e : ar) {
			System.out.println(e);
		}	
	}
}

class Rectangle {
	private int width;
	private int height;
	
	Rectangle(int width, int height) {
		this.width = width;
		this.height = height;
	}
	
	public int getWidth() {
		return width;
	}

	public void setWidth(int width) {
		this.width = width;
	}

	public int getHeight() {
		return height;
	}
	
	public void setHeight(int height) {
		this.height = height;
	}
	
	public int getArea() {
		return width*height;
	}
	public int compareTo(Rectangle[] arr) {
		for(Rectangle i : arr) {
			return i.getArea() - this.getArea();
		}
		return getArea();	
	}
}

```
