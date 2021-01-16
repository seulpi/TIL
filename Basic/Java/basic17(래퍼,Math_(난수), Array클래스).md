# 래퍼 클래스(Wrapper Class)
## 1. 정의 
: wrap 감싼다는 의미처럼 Wrapper Class는 무언가를 감싸는 클래스  → 기본자료형의 값을 인스턴스로 감싸는 목적의 클래스이다 <br>
(따라서 포장객체라고도 불림, 기본타입의 값을 내부에 두고 포장하는 상태여서)
```java
// 덧붙이는 설명

- 객체 지향 언어인  JVAV에서는 기본타입의 데이터를 객체로 표현해야하는 경우가 종종 생기고
현대에 나오는 언어들은 대부분 객체화해서 나오는 경우가 대다수 따라서, JAVA도 객체화 시키는게 좋다

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
연산은 기본형에서 쓸 수 있는 연산이 모두 가능하지만 객체 밑도 끝도 없이 생성중 */

int num1 = integer + 5;
```

## 3. Number class
### - Byte, Short, Integer, Long, Float, Double 클래스들의 슈퍼 클래스
- Number 클래스 안에는 기본형 타입의 함수가 정의되어있음
```java
public abstract int intValue();
public abstract double doubleValue();
// 기본타입의 함수들 이렇게 주르륵 정의

* 여기서 주의할점] abstract는 자손이 구현하는거니까 래퍼클래스에서 사용이 가능한 것!
```

## 4. 장점
- 객체로 기본타입을 사용할때는 Number클래스에 있는 함수를 사용할 수 있기 때문에 형 변환 할 필요없이 사용 가능하다
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

---
# Math class
- 알아두면 써먹을 수 있는 수학관련 함수를 담고 있는 클래스
```java
public static void main(String[] args) {
    System.out.println("원주율: " + Math.PI);
    System.out.println("2의 제곱근: " + Math.sqrt(2));
    System.out.println();

    System.out.println("파이에 대한 각도 Degree: " + Math.toDegrees(Math.PI) );
    System.out.println("2 파이에 대한 Degree: " + Math.toDegrees(2.0*Math.PI));
    System.out.println();

    double radian45 = Math.toRadians(45);

    System.out.println("싸인 45: " + Math.sin(radian45));
    System.out.println("코싸인 45: " + Math.cos(radian45));
    System.out.println("탄젠트 45: " + Math.tan(radian45));
    System.out.println();

    System.out.println("로그 25:" + Math.log(25));
    System.out.println("2의 16승: " + Math.pow(2, 16));
}
```
## - 난수(임의의 정수) 생성 
### [난수 생성 방법]
1.  Math클래스 안에 있는 함수를 다이렉트로 사용
```java
						    //몇개 	스타트값
int num = (int) (Math.random()* 6) + 1;

; 6개의 숫자를 랜덤으로 뽑고 시작은 1부터
```
- 적용ex) 1~6까지의 임의의 정수를 뽑아서 어떤 숫자가 나왔는지 출력하시오  
```java
int num = (int) (Math.random()* 6) + 1;

if(num == 1)
    System.out.prinln("1번입니다")
if(num == 2)
    System.out.prinln("2번입니다")
if(num == 3)
    System.out.prinln("4번입니다")
if(num == 4)
    System.out.prinln("4번입니다")
if(num == 5)
    System.out.prinln("5번입니다")
if(num == 6)
    System.out.prinln("6번입니다")
// 출력과정이 숫자가 랜덤으로 뽑히기때문에 출력결과도 랜덤결과에 따라 나타난다
```
2. 객체로 만들어서 사용
>> Random rans = new Random();

<br> 

![난수생성](https://user-images.githubusercontent.com/74290204/102632772-d9cc2f00-4192-11eb-8a9b-271b288e7170.PNG)

### 활용 (???Seed값뭔지 이해못함 이부분 개념 정리다시)
- 무작위 수를 돌려야할 때 사용(게임, 로또, 추첨 등)
```java
//random을 돌릴때는 반드시 seed값이 있어야하는데 없다면 자동으로
public Random() { //← Random(lon seed)
    this(System.currentTimeMillis()); 
}
```
---
# 문자열의 토큰 
-  토큰: 구분자':'에 의해 나눠져있는 문자열들을 뜻함 (실무에선 구분자':'를 영어로 토큰이라 표현한다)
>> "PM:08:45" → ':'를 기준으로 PM   08  45

```java
public static void main(String[] args) {
                                            // ↓ 문자열     ↓구분자
    StringTokenizer st1 = new StringTokenizer("PM:08:45", ":"); 
    // (문자열,구분자) 문자열을 구분자로 나눠서 배열로 갖는다(배열 자동 생성)
//StringTokenizer 사용 방법
    while(st1.hasMoreTokens()) { // hasMoreTokens() → 반환할 토큰이 남아있는가? boolean으로 옴
    System.out.print(st1.nextToken() + ' '); // nextToken() → 다음 토큰 반환 String
    }System.out.println(); 
 
    StringTokenizer st2 = new StringTokenizer("12 + 36 - 8 / 2 = 44", "+-/= "); 
    // 한개만 나눌수있는 게 아니고 여러개 나눌수있음
    
    while(st2.hasMoreTokens()) {
    System.out.print(st2.nextToken() + ' ');
    }System.out.println();
}
=============================================================
▶ 
PM 08 45 
12 36 8 2 44 
```

---
# Array class(달달외울필요없이 개념정리만 잘 정리하자)
- 배열은 Immutable → why?
1. copyOf : ctrl c + ctrl v / 내가 방을 따로  안만들어줘도 배열이 만들어져서 원본 그대로 복사
```java
public static void main(String[] args) {
    double[] ar = {1.1, 2.2, 3.3, 4.4, 5.5};
    double[] arCpy1  = Arrays.copyOf(ar, ar.length); // 원본에서, 원본길이만큼 복사해서 arCpy1에 넣겠다
    double[] arCpy2 = Arrays.copyOf(ar, 3); //원본에서, 3개까지의 값을 arCpy2에 넣겠다 

    >> 원본 복사할 index 카운트는 처음을 기준으로(index = 0)
    
    for(double d : arCpy1) {
        System.out.println(d + "\t");
    }
    System.out.println();

    for(double d : arCpy2) {
        System.out.println(d + "\t");
    }
    System.out.println();
}
```

2. arraycopy : 원본이 있어도 옮길 방을 만들어줘야 복사 가능 (배열을 복사할 목적지가 이미 있어야함 ) <br>
→ copyOf랑은 조금 다른 성질 : 원본이랑 똑같이 만들건지 아닌지 공간이 초기화되어있는지 아닌지의 차이

3. 배열의 비교
: 배열의 비교도 결국  Object의 eauals함수 상속하는 것 기본적으로 equals의 함수는 주소를 비교하기 때문에 <br> 배열안에 내용을 비교하고 싶다면 equals 함수를 오버라이딩해서 재정의해줘야한다
```java
// Ex1
public static void main(String[] args) {
    int[] ar1  = {1, 2, 3, 4, 5};
    int[] ar2 = Arrays.copyOf(ar1, ar1.length);
    System.out.println(Arrays.equals(ar1, ar2)); // array도 비교가능하다 , equals 오버로딩! 값을 비교하는것~
}

//Ex2
public class class_1218 {
public static void main(String[] args) {
    INum[] ar1 = new INum[3];
    INum[] ar2 = new INum[3];
    ar1[0] = new INum(1);
    ar1[1] = new INum(2);
    ar1[2] = new INum(3);
    ar2[0] = new INum(1);
    ar2[1] = new INum(2);
    ar2[2] = new INum(3);
    System.out.println(Arrays.equals(ar1, ar2));
    }
}
 
class INum {
    int num;
    public INum(int num) {
        this.num = num;
    }
@Override // 오버라이딩을 하지 않으면 ar1,ar2는 '=='로 주소비교를 하는거여서 false 반환함
    public boolean equals(Object obj) {
        if(this.num == ((INum)obj).num) {
            return true;
        }else {
            return false;
        }
    }
}
```

4. 배열의 정렬(Sorting) : 정렬함수를 사용할때 compareTo함수를 자동으로 호출한다는게 중요 포인트!
>> Arrays.sort(배열변수명) : 오름차순으로 정렬 <br>
if] 내림차순으로 정렬하고 싶다 → compareTo함수 오버라이딩
```java
public static void main(String[] args) {
    int[] ar1 = {1, 5, 3, 2, 4};
    double[] ar2 = {3.3, 4.4, 1.1, 5.5, 2.2};
    Arrays.sort(ar1); // Quicksort방법으로 정렬 (정렬 방법 중 성능이 가장 빠름)
    Arrays.sort(ar2);

    for(int e1 : ar1) {
        System.out.print(e1 + "\t");
    }System.out.println();

    for(double e2 : ar2) {
        System.out.print(e2 + "\t");
    }System.out.println();
}
==========================================
▶
1 2 3 4 5 
1.1 2.2 3.3 4.4 5.5 
```
```java
// comparTo 함수 기본 정의
interface Comparable { 
    int compareTo(Object obj) 
}
// interfac Comparable 안에는 compareTo 함수가 포함되어있음 따라서 자손이 오버라이딩해서 구현할수있다!

//compareTo 오버라이딩EX
import java.util.Arrays;
public class class_1218 {
 
public static void main(String[] args) {
    Person[] ar = new Person[3];
    ar[0] = new Person("Lee", 29);
    ar[1] = new Person("Goo", 15);
    ar[2] = new Person("Soo", 37);

    Arrays.sort(ar); // sort내부에서 compareTo() 호출해서 정렬
    for(Person p : ar) {
        System.out.println(p);
    }
}
}
 
class Person implements Comparable {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    @Override // interface Comparable 안에 있는 함수 오버라이딩
    public int compareTo(Object o) {
        Person p = (Person)o; 
        return this.age - p.age; 
    /* 두개의 값을 비교해서 if(this.age >  p.age) ‘양의정수’ 반환 
    50살 과 20살이면 50-20 : 양의정수이니까 자기자신이 더큰거고 (내가 이긴거야win) 
    * this.age <p.age '음의정수 ‘반환 20 -50 = -30 (내가 진거야ㅠㅠlose쟤가 더커) this.age == p.age 0을 반환하는 함수
    * (음수냐 양수냐에 따라서 오름차순이냐 내림차순이냐 구현가능)???*/
    }
    
    public String toString() {
        return name +": " + age;
    }
}
```

5. 배열 검색
- binarySearch 
   - 반드시 정렬된 상태여야한다(정렬안된 상태에서 사용하면 이상한 거 뿌림)
   - 나란히 한줄로 배열되서 순차적으로 찾는 방식이 아니라 가지치기를 해서 찾아들어가는 방식 

   ![배열위키](https://user-images.githubusercontent.com/74290204/102635577-dfc40f00-4196-11eb-9d4f-f4db1f59590d.png)

```java
public static void main(String[] args) {
    int[] ar = {33,55,11,44,22};
    Arrays.sort(ar); // 1.binarySearch 조건) 정렬 

    for(int n : ar) {
        System.out.print(n + "\t"); // 2. 정렬한 값을 순서대로 뿌려라
    }System.out.println();

    int index = Arrays.binarySearch(ar, 33); //3. 원하는 배열에서 33의 위치가 어디인지 변수에 대입
    System.out.println("Index of 33: " + index); // 4. 원하는 값의 인덱스 번호 출력 
}
============================================
▶ 
11 22 33 44 55 
Index of 33: 2
 
정렬을 하게되면 기존의 넣었던 index의 값이 바뀜 (당연한 결과)
[네모난 그림 그려서 설명 덧 붙이기]
```
