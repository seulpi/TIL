# main은 왜 public/static을 붙여야하는가?
JVM이 클래스를 메모리에 올릴려면 main부터 찾게되고 <br>
1. st atic인 이유는 메인메소드가 인스턴스 생성과관계없이 가장 먼저 생성되기 때문에
2. public인 이유는 메인은 외부호출이고 다른 패키지에서 언제어디서든 호출 가능해야하기때문에

# **class 변수(★★★★★★)** 
**static(=클래스변수 =공유변수 =정적변수)** <br>
☞ 클래스변수를 이해하려면 먼저 클래스를 메모리에 올릴때 어떠한 구조로 올리는지를 이해해야한다 <br>
★ 메모리에 클래스를 어떻게 올리는가?
  - 자바에서는 메모리 공간을 총 7개정도로 구성되는데 일단 3개로만 이해해도 충분하다 그 3개의 공간은 다음과 같다 <br>
  1. Method Area : 함수를 올리는 영역이며 클래스 정보(01010덩어리)와 **static**으로 선언된 변수와 함수를 여기에 **먼저** 저장한다 (main다음으로)
  2. Calltack : 함수 실행할때 이뤄지는 영역이다 ex) System.out.println , main()
  3. Heap : 객체들이 여기에 저장된다 <br>
  단! Method Area에 static이 한번 저장되면 두번다시 메모리에 반복해서 올리지 않는다(이미 공유하는 거기 때문에 그 정보를 계속 가지고있음)

▶ 메모리 구조에 공간 저장 원리
![static 메모리에 올리는 구조](https://user-images.githubusercontent.com/74290204/101135246-e60ea300-364e-11eb-81c2-8da53070a81e.PNG)
---

```java
// static 변수X

class InstCnt {
	int instNum = 0;
	
	InstCnt() {
		instNum++;
		System.out.println("인스턴스 생성: " + instNum);
	}
}

class classVar {
	public static void main(String[] args) {
		InstCnt cnt1 = new InstCnt();
		InstCnt cnt2 = new InstCnt();
		InstCnt cnt3 = new InstCnt();
		
	}  
} 
===============================
▶ 
인스턴스 생성: 1
인스턴스 생성: 1
인스턴스 생성: 1
```
```java
// static 변수O

class InstCnt {
	static int instNum = 0;
	
	InstCnt() {
		instNum++;
		System.out.println("인스턴스 생성: " + instNum);
	}
}

class classVar {
	public static void main(String[] args) {
		InstCnt cnt1 = new InstCnt();
		InstCnt cnt2 = new InstCnt();
		InstCnt cnt3 = new InstCnt();
		
	}
} 
======================
▶ 
인스턴스 생성: 1
인스턴스 생성: 2
인스턴스 생성: 3

// 단, 이 변수는 클래스 안에 존재해야하고 static이 없다면 인스턴스변수
//참조) 인스턴스변스에서 참조형 변수의 값이 할당되어있지않으면 기본적으로 값을 null로 잡는다 int 는 0으로 초기화
```
☞ 여러개의 클래스 중에 이 변수를 어디에 위치시킬것인지?

## - static 변수의 접근 방법
1. 객체명으로 접근(주소로 접근한다)
2. 클래스명으로 접근(다이렉트로 클래스에 직접 접근한다) <br>
클래스 이름으로도 접근가능하기 때문에 클래스변수라는 이름이 붙었다 <br>

▶ 직관적으로 클래스명을 통해(2번) 접근하는것이 BEST!**

- tip) 클래스 변수는 선하자마자 값을 넣는게 가장 좋다

   ```java
   static int num = 15;
   ```
## ★ 활용 ★
1. 카드에서 가로 세로 사이즈가 같은데 여러개의 객체를 생성해야 할때
2. PI
3. 전제 개수 count <br>

▶ 즉 변하지 않는 공통의 것을 쓸 때 사용한다

### - 간략 정리!!!!
1. 클래스 안에 존재
2. 단 하나만 존재하고 어디서든 공유 가능
3. 인스턴스 변수와 다름
4. 초기화는 인스턴스 내부에서 일어나지 X
   - why? 인스턴스 내부에서 일어나면 인스턴스를 생성할때마다 값의 초기화 발생 <br> 
   → 공유하는 변수이기 때문에 계속 초기화되면 다른 클래스에서 공유할 수 없게 됨 (※위치 주의)
5. 활용: 변하지 않는 상수에 사용됨 
```java
static final double PI = 3.1415; /*파이값은 변하지 않는 상수이기때문에 각각의 클래스의 변수가 필요하지 않다
따라서 클래스 변수로 선언해주는게 바람직 */
```

# class 메소드 (=static 메소드)
## class 메소드란 class앞에 static이 붙은 것을 말한다
- static이 붙었을 때 호출하는 방법 
1. 클래스명 
2. 객체
```java
public class Static {

	private int myNum = 0;
	
	static void showInt(int n) {
		System.out.println(n);
	}
	
	static void showDouble(double n) {
		System.out.println(n);
	}
	
	void setMyNumber(int n) {
		myNum = n;
	}
	void showMyNumber() { 
		showInt(myNum);
	}
	
	public static void main(String[] args) { 
		Static.showInt(20); // 1.클래스명으로 접근
		Static np = new Static();
		np.showDouble(3.15);
		np.setMyNumber(75);
		np.showMyNumber(); // 2.객체로 접근
	}
}
// 클래스메소드나 클래스변수는 클래스명으로 접근하는게 좋다(직관적)
```
<br>

- 활용 <br>
 단순 기능 제공 목적인 메소드일때(객체생성시킬 때 메모리에 한 번 올려놓고 계속 써야할 때 )
```java
 System.out.println(n); // 대문자 . 하고 접근하는것은 static함수하고 생각해야함

 >>  System.out.println(n);을 static으로 선언하지 않으면 매번 호출할때마다 객체생성을 해줘야하기 때문에 불편하고 메모리 낭비로 이루어짐
 ```


# ★static함수 안에 '인스턴스변수 & 함수'가 오지 못하는 이유!
☞ 메모리 공간에 .class를 올릴때 static은 **'먼저'** Method Area에 들어가게 되고 그 후에 Heap이라는 공간에 인스턴스 변수와 함수가 들어가게 된다 <br>
이 말은 즉 함수나 변수는 사용하려면 만들어져야 하는데 static함수를 사용할때는 인스턴스 변수나 함수가 아직 메모리에 **올라가기전**이기 때문에 사용할 수 X (인스턴스 변수나 함수는 생성자가 생성될때 만들어짐) <br>
 >> 클래스를 설계할때는 static함수 안에는 static만 가능(끼리끼리/static변수 컨트롤은 static함수만 할 수 있음)

 ※ 주의!!) **지역변수**와 **인스턴스변수**는 다른 존재 
   - static안에 지역변수가 와도 error 발생 X (지역변수는 지역안에서 유효한 존재이기 떄문에)

<br>

# static 초기화 블록
static 변수 값을 직접 할당하는 것이 아니라 값을 얻어와서 초기활할때 사용 <br>
static 안에 연산이 들어가야할 때 (연산도 **한번만** 초기화! 메모리에 한번 올리니까)
```java
import java.time.LocalDate;

public class DateOfExecution {
	static String date;
	
	static {
		LocalDate nDate = LocalDate.now();  //'초기화블록' 인스턴스 생성과 관계없이 static변수가 메모리 공간에 할당할때 실행 (static 연산할때는 이런 로직도 가능하다)
		date = nDate.toString();
	}
	
	public static void main(String[] args) {
		System.out.println(date);
	}
}
```

#  System.out.println / public static void main( )
```java
System.out.println

//System(클래스의이름).out(.찍고 접근한 클래스 변수=System클래스의 위치한 static변수).println(.찍고 메소드 호출)
```
```java
public static void main( ) : 메인메소드 (하나만 존재해야 하는 메소드)
```
