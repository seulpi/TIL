# 1. 인스턴스 함수안에 스태틱 변수와 함수가 올수 있는 이유는?
메모리에 올릴 때 static변수와 함수는 Method Area에 먼저 올리게 되고 <br>
인스턴스 변수와 함수는 heap공간에 그 후에 올리게 된다 <br>
함수와 변수는 만들어져 있어야 사용이 가능하기 때문에 static변수와 함수는 Method Area에 **먼저** 만들어져 있기 때문에 인스턴스 함수안에 올 수 있다  <br>BUT! static 함수 안에는 인스턴스가 올 수 없는데 이 이유도 static 함수를 만들때에는 인스턴스 변수나 함수가 메모리에 올라와져 있지 않기 때문에 사용할수 없다 <br>
(단, 지역변수는 인스턴스변수와 전혀 다른 변수이기 때문에 지역변수에는 아무런 영향을 미치지 않는다)

<br>

# 2. 메소드 오버로딩이란?
- 메소드 오버로딩이란 매개변수의 정보(1.갯수 2.데이터타입)가 다르다면 같은 
함수명 사용이 가능한 것이다 <br>
  - c언어에서는 같은 함수명은 **절대 불가능**하지만 java에서는 허용된다 <br>
```java
>> 메소드 오버로딩 EX)

void myRoom() {}
void myRoom(int a) {}
void myRoom(int a, int b) {}
void myRoom(double d) {}
-------------------------------------
int myRoom() {}
double myRoom() {}
//※ 반환형은 매개변수와 관련이 없기 때문에 여기서는 같은 함수명을 쓰면 error 발생
```
- 메소드 오버로딩은 명확한 상황에서 사용해야하고 메소드 오버로딩 활용의 목적은 캡슐화(사용자가 쉽고 편하게 쓰게하기위한 목적)이다
<br>

# 3. 메소드 오버로딩을 적용한 대표적인 함수는?
대표적인 예로는 System.out.println();이 있다 <br>
메소드 오버로딩은 어떠한 데이터타입이 와도 같은 기능을 실행할 때 활용한다

<br>

# 4. this 함수에 대하여 설명하시오
this 함수는 클래수 안에서 또 다른 생성자를 호출할때 사용하는 함수 
```java
public class Person {
    private int regiNum;
    private int passNun;

    Person(int rnum, int pnum) {
        regiNum = rnum;
        passNum = pnum;
    }

    Person(int rnum) {
        this(rnum, 0); 
    }
}
```

<br>

# 5. this란 무엇인가?
this의 종류는 2가지이다 <br>
1. 객체 this(자기자신을 가리킬때)
2. this 함수(또 다른 생성자 호출할때)
```java

// 1. 객체 this
Rectangle(int x, int y, int width, int height) {
	this.x = x;
	this.y = y;
	this.width = width;
	this.height = height;

// 2. this 함수 
public class Person {
    private int regiNum;
    private int passNun;

    Person(int rnum, int pnum) {
        regiNum = rnum;
        passNum = pnum;
    }

    Person(int rnum) {
        this(rnum, 0); 
    }
}
```

<br>

# 6. 스트링 객체를 생성하는 2가지 방법은?
아래의 결과를 예측하고 이유를 설명하시오
```java
String str1 = "Simple String";
String str2 = "Simple String";
   
String str3 = new String("Simple String");
String str4 = new String("Simple String");
   
 if(str1 == str2)
    System.out.println("str1과 str2는 동일 인스턴스 참조");
else
    System.out.println("str1과 str2는 다른 인스턴스 참조");
   
if(str3 == str4)
    System.out.println("str3과 str4는 동일 인스턴스 참조");
else
    System.out.println("str3과 str4는 다른 인스턴스 참조");
```
```java
//Answer
▶
str1과 str2는 동일 인스턴스 참조
str3과 str4는 다른 인스턴스 참조

//str1과 str2는 동일 인스턴스 참조
str1 과 str2는 초기화를 할 때 문자열안에 있는 대소문자를 다 구분해 동일하면 하나를 지우고 하나만 메모리에 올린다 → 그리고 같은 주소를 참조하게 되는 형태
따라서 동일한 인스턴스의 주소를 참조한다는 결과가 나오는 것

//str3과 str4는 다른 인스턴스 참조
str3 과 str4는 각각 객체를 생성하고 각자 다른 주소를 참고하는 있는 형태기 때문에 다른 인스턴스 참조한다는 문자열이 출력되는것이다
```
![Sting클래스01](https://user-images.githubusercontent.com/74290204/101322422-56653080-38aa-11eb-870d-475de3c93725.png)

![Sting클래스02](https://user-images.githubusercontent.com/74290204/101322461-67ae3d00-38aa-11eb-963b-866a8b6f1a9a.png)

<br>

# 7. imutable 에 대하여 설명하시오
immutable은 값이 변하지 않는 것을 말한다 <br>
immutable은 Method Area에 있는 주소값이 변경되지 않는다는 것을 의미하는게 아니라 heap공간에 있는 값이 변하지 않음을 의미한다 <br>
String 클래스가 대표적인 immutable의 예이다 
```java
String s = "hello"; 
s = s + "Hello";
System.out.println(s);
/* 여기서의 hello의 값과 출력되서 나오는 helloHello는 주소가 다른데 
기존의 hello에 Hello값을 넣어 붙이는게 아닌 방을 새로 만들어 출력값을 담는다*/
```

<br>

# 8. 사용자로부터 받은 문자열(영문으로)에서 자음과 모음 개수를 계산하는 프로그램을 작성하라
```java
public static void main(String[] args) {
	
    System.out.println("문자열을 입력하세요: ");
	Scanner sc = new Scanner(System.in);
	String str = sc.next();
		
	int count1 = 0;
	int count2 = 0;
		
	str = str.toLowerCase(); // 알파벳을 모두 소문자로 바꿔주는 함수 안그러면 대문자 소문자 다 구별해서 count 조건문 넣어줘야함
		
	for(int i = 0; i < str.length(); i++) {
		if(str.charAt(i) == 'a' || str.charAt(i) == 'e' || str.charAt(i) == 'i' || str.charAt(i) == 'o' || str.charAt(i) == 'u')
			count1++;
		else
			count2++;

	}
	System.out.println("모음갯수는: " + count1);
	System.out.println("자음갯수는: " + count2);	
}
```
<br>

# 9. 표준체중에 대한 몸무게 판단 프로그램을 작성하라
```java
// Main class
public static void main(String[] args) {

    System.out.println("체중과 키를 입력하세요: ");
	Scanner sc = new Scanner(System.in);
	Weight mykg = new Weight(sc.nextDouble(), sc.nextDouble());
	mykg.resultShow();
		
}

// Weight class
private double standardKg;
private double height;
private double kg;
	
Weight(double kg, double height) {
	this.kg = kg;
	this.height = height;
}
	
void resultShow() {
	standardKg = (height - 100) * 0.9;
	if(kg > standardKg) 
		System.out.println("과체중입니다");
	else if(kg == standardKg)
		System.out.println("표준입니다");
	else
		System.out.println("저체중입니다");
}
```

# 10. 2와 100 사이에 있는 모든 소수(prime number)를 찾는 프로그램을 작성하라 (다시 check)
## 주어진 정수 k를 2부터 k-1까지의 숫자로 나누어서 나머지가 0인 것이 하나라도 있으면 소수가 아니다
```java

public static void main(String[] args) {
		
	int count = 0;
		
	for(int k = 2; k <= 100; k++)
		if(isPrimeNum(k)) 
			count++;
		
private static boolean isPrimeNum(int k) {
	if(k == 1)
		return false;
	for(int i = 2; i <= k; i++)
		if(k % i == 0)
			return false;
}	
System.out.println("2에서 100까지의 소수의 갯수는: " + count);
```

# 11. 사용자에게 받은 문자열을 역순으로 화면에 출력하는 프로그램을 작성하시오
## 입력:abcde <br> 출력:edcba
```java
//Answer
public static void main(String[] args) {

    System.out.println("문자열을 입력하세요: ");
	Scanner input = new Scanner(System.in);
	String str = input.next();
		
	for(int i = str.length()-1; i>= 0; i--)
		System.out.print(str.charAt(i));
}
```

---
# ▶  java basic09-10 정리 참고
