# 메소드 오버로딩 
## 같은 이름의 함수를 파라미터 개수와 타입을 달리하여 만드는 것 
- 메소드 오버로딩의 가장 대표적인 예 System.out.println(); <br>
tip) c언어에서는 같은 이름의 함수명은 **절대 불가**하지만 자바에서는 매개변수의 정보만 달리해주면 **가능**하다
```java
void myRoom() {}
void myRoom(int a) {}
void myRoom(int a, int b) {}
void myRoom(double d) {}

//매개변수의 정보(1.개수 2.데이터타입)가 다르기때문에 사용가능
========================================

int myRoom() {}
double myRoom() {}

// 반환형은 매개변수와 연관이 없기 때문에 이렇게 사용하면 error
=========================================

AAA inst = new AAA();
	inst.simple(7, 'k')
/* 이런 경우 애매한 메소드 오버로딩이라고 말하는데 
'k에 대한 정확한 정의가 없으니 어떤 타입을 써야할지 결정하기 어렵다 
char에 형변환의 범위를 보면 int나 double 둘 다 가능하다 ▶ 삐빅-애매모호하다~
(정확하게 말하자면 형변환의 범위는 좀 더 가까운곳을 따르기 때문에 int로 결정이된다)
전산적으로 이런 애매한 건 지양해야함 (명확한 목적하에 오버로딩 사용해야한다) 
**혹여나 꼭 써야한다면 강제형변을 해줌으로써 어떤 타입인지 명시해줘야한다 */
```
- 활용 : 어떨 때 메소드 오버로딩을 사용하는가? <br>
어떠한 데이터타입이 와도 같은 기능을 실행할 때 (사용자가 쉽고 편하게 사용하게 할떼 = 캡슐화)

<br>

# this
1. 객체 this (자기자신을 가리킬 때)
2. this 함수 (클래스 안에서 또 다른 생성자를 호출할 때)
```java
//1. 객체 this
Rectangle(int x, int y, int width, int height) {
	this.x = x;
	this.y = y;
	this.width = width;
	this.height = height;
}

//2. this 함수 
public class Person {
	private int regiNum;
	private int passNum;
	
	Person(int rnum, int pnum) {
		regiNum = rnum;
		passNum = pnum;
	}
	
	Person(int rnum) { 
		this(rnum, 0); // Person(int rnum, int pnum)호출
	}
}
```

<br>

# String class
```java
public static void main(String[] args) {
		
	String str1= new String("Simple String");
	String str2 = "Best";
		
	System.out.println(str1);
	System.out.println(str1.length()); //length는 문자열 안에 공백과 기호를 포함한 문자의 갯수(길이)를 의미
	System.out.println();
		
	System.out.println(str2);
	System.out.println(str2.length());
	System.out.println();
		
	showString("Funny String");
	}
	
public static void showString(String str) {
	System.out.println(str);
	System.out.println(str.length());
	}
---------------------------------------------------------
public static void main(String[] args) {
		
	String str1 = "Simple String"; // String은 new를 사용안해도 주소 참조 바로 하게끔 허용 
	String str2 = "Simple String";
		/* " " → 문자열 상수 
		 문자열 " "오는거는 컴파일러가 쭉 스캔해서 대소문자를 구분해서 똑같으면 메모리 아끼려고 한개만 메모리 3가지 영역 이외의 인스턴스 풀(Instance Pool)에 넣음
		 한개만 올리고 같은 주소를 참조하는 형태 
         !단, 문자중에 하나라도 소문자가 되거나 똑같지 않으면 다른거라고 인식해서 주소가 달라짐 
		 (대소문자 아스키코드값이 다르기때문에) 
		  */
		
	String str3 = new String("Simple String"); // 문자열은 문자가 모여져있는것 따라서 문자 하나당 char 2byte(유니코드이기 때문에) 따라서 Simple String은 메모리에 총 13*2 = 64byte
	String str4 = new String("Simple String");
	// char배열의 첫번째 주소를 가리키고 있는 상태
	if(str1 == str2)
		System.out.println("str1과 str2는 동일 주소 참조");
	else
		System.out.println("str1과 str2는 다른 주소 참조");
		
	if(str3 == str4)
		System.out.println("str3과 str4는 동일 주소 참조");
	else
		System.out.println("str3과 str4는 다른 주소 참조");

	if(str1 == str3) // 같아보이지만 주소가 완전 다름 근데 주소비교말고 문자열 안에 문자내용이 같은지 확인하고 싶다면 equals라는 함수를 이용하면된다
		System.out.println("str1과 str2는 동일 주소 참조");
	else
		System.out.println("str1과 str2는 다른 주소 참조");
	if(str1.equals(str3)) // equals 이용해서 내용 비교
		System.out.println("str1과 str2는 동일 주소 참조");
	else
		System.out.println("str1과 str2는 다른 주소 참조");
}
=========================================================
▶   str1과 str2는 동일 주소 참조
    str3과 str4는 다른 주소 참조
	str1과 str2는 다른 주소 참조
	str1과 str2는 동일 주소 참조
```
▶ 위 코드 그림
```java
if(str1 == str2)
	System.out.println("str1과 str2는 동일 주소 참조");
else
	System.out.println("str1과 str2는 다른 주소 참조");
```
![Sting클래스01](https://user-images.githubusercontent.com/74290204/101322422-56653080-38aa-11eb-870d-475de3c93725.png)

```java
if(str3 == str4)
	System.out.println("str3과 str4는 동일 주소 참조");
else
	System.out.println("str3과 str4는 다른 주소 참조");
```
![Sting클래스02](https://user-images.githubusercontent.com/74290204/101322461-67ae3d00-38aa-11eb-963b-866a8b6f1a9a.png)

## - Immutable 인스턴스
- Immutable(불변의) ↔ mutable <br>
- String인스턴스는 Immutable인스턴스
- Immutable 인스턴스는 인스턴스의 값을 한번 정해주면 어떻게든 값이 변하지않는다 <br>
(값을 변경하려면 기존 인스턴스를 없애고 새로 만들어야하는 것을 가리켜 말함
)
```java
//1
String str1 = "Simple String";
String str2 = str1;

//2
String str1 = "Simple String";
String str2 = new String("Simple String");
		
/* 2개의 코드가 사실상 같은 코드이지만 2번보다 1번이 더 좋은코드이다
*why? 2번은 하나의 인스턴스를 참조해서 주소를 복사하는 케이스이지만
	1번 같은 경우는 인스턴스를 여러개 만들어내기때문에 예를들어 100개 1000개를 만들어낸다치면 인스턴스 100개 1000개 만드는것
	따라서 메모리나 속도면에서 떨어지기 때문이다 */

>> Immutable인스턴스는 주소값이 변하지 않는다는게 아니라 heap안에 있는 값 자체가 변하지 않는다는 것
따라서 hello값은 그대로 두고 helloHello를 새로 만든다


String s = "hello"; 
s = s + "Hello";
System.out.println(s);
//hello주소와 Hello주소를 담는 s의 주소가 다름 
==============================================
▶ helloHello
```
<br>

## String인스턴스 기반 switch 문
- 자바 1.5버전부터는 switch문 안에 String 가능 
```java
String str = "two";

switch(str) {
case "one": 
	System.out.prinln("one");
	break;
case "two": 
	System.out.prinln("two");
	break;
default:
	System.out.prinln("default");
	break;

/* 이게 가능한 이유는?
str에 들어가건 주소가 들어간다 주소는 4byte 정수 
따라서 주소가 들어갈수있기때문에 String이 가능해진것이다 */
```



