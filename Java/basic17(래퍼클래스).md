# 래퍼 클래스(Wrapper Class)
## 1. 정의 
: wrap 감싼다는 의미처럼 Wrapper Class는 무언갈 감싸는 클래스 <br>
→ 기본자료형의 값을 인스턴스로 감싸는 목적의 클래스이다 <br>(따라서 포장객체라고도 불림, 기본타입의 값을 내부에 두고 포장하는 상태여서)
```java
// 덧붙이는 설명
- 객체 지향 언어인  JVAV에서는 기본타입의 데이터를 객체로 
표현해야하는 경우가 종종 생기고
현대에 나오는 언어들은 대부분 객체화해서 나오는 경우가 대다수
따라서, JAVA도 객체화 시키는게 좋다

- 기본타입 값을 변경하고 싶다면 새로운 포장객체를 만들어서 사용해야함
```
## 2. 관련 용어
- 박싱(Boxing) : 기본형 → 객체로 만드는 것 
- 언박싱(UnBoxing) : 객체 → 기본형으로 꺼내는 것
```java
// Boxing
Integer integer  = new Integer(3);
Double double1 = new Double(3.0);

- Integer : 정수 int

// UnBoxing
int num1 = integer.intValue();
double num2 = double1.doubleValue(); 
```
- 오토 언박싱(Auto UnBoxing) <br> : 함수를 호출하지 않아도 컴파일러가 자동으로 호출 해주는 것<br> (new 생략이 가능하다)
```java
// Auto UnBoxing 1
Integer integer = 10; 
Double double1 = 3.14;

int num1 = integer; 
double num2 = double1;

// Auto UnBoxing 2 
Integer integer = 10;
integer++; 
/* new Integer(integer.intValue()+1); → 객체를 새로 생성하는 것 ; 메모리에 계속 객체가 쌓이는 것
연산은 기본형에서 쓸 수 있는 연산을 쓸 수 있지만 객체 밑도끝도 없이 생성중 */
int num1 = integer + 5;
```

## 3. Number class
### - Byte, Short, Integer, Long, Float, Double 클래스들의 슈퍼 클래스
- Number 클래스 안에는 기본형 타입의 함수가 정의되어이씀
```java
public abstract int intValue();
public abstract double doubleValue();
// 기본타입의 함수들 이렇게 주르륵 정의

* 여기서 주의할점] abstract는 자손이 구현하는거니까 래퍼클래스에서 사용이 가능한 것!
```

## 4. 장점
- 객체로 기본타입을 사용할때는 Number클래스에 있는 함수를 사용할 수 있기 때문에 <br> 형 변환이 할 필요없이 사용 가능하다
- Number안에 함수를 다 이용할 수 있다<br> (그래서 안에 있는 함수를 알면 알수록 좋음! → 써먹을 수 있으니까)
```java
Integer num1 = new Integer(29);
System.out.println(num1.intValue());
System.out.println(num1.doubleValue());
Double num2 = new Double(3.14);
System.out.println(num2.intValue()); // 1. Double인데 int로 형 변환 없이 가능
System.out.println(num2.doubleValue());
 
Integer n1 = Integer.valueOf(5); 
/* String.valueOf 함수 오버로딩이 적용된것!
valueOf는 static */
int n2 = Integer.valueOf("1024"); // 오토언박싱이어서 가능, 실무에서 이렇게 많이 사용!
System.out.println(n2);
```
```java
>> Number class안에 있는 함수 이용
public static void main(String[] args) {
    Integer n1 = Integer.valueOf(5); 
    Integer n2 = Integer.valueOf("1024");

    // 대소 비교와 합을 계산하는 메소드
    System.out.println("큰 수: " + Integer.max(n1, n2));
    System.out.println("작은 수: " + Integer.min(n1, n2));
    System.out.println("합: " + Integer.sum(n1, n2));
    System.out.println();

    // 정수에 대한 2진,8진,16진수 표현결과를 반환하는 메소드 
    System.out.println("12의 2진 표현: " + Integer.toBinaryString(12));
    System.out.println("12의 8진 표현: " + Integer.toOctalString(12));
    System.out.println("12의 16진 표현: " + Integer.toHexString(12)); 
}
==========================================
▶ 
큰 수: 1024
작은 수: 5
합: 1029
 
12의 2진 표현: 1100
12의 8진 표현: 14
12의 16진 표현: c
```
