# main은 왜 public을 붙여야하는가?
JVM이 클래스를 메모리에 올릴려면 main부터 찾는다 <br>
따라서 main은 public이 되어야 다른 패키지에 있는 클래스도 호출이 가능하기 때문에 main은 public을 사용한다

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
```
☞ 여러개의 클래스 중에 이 변수를 어디에 위치시킬것인지?

## - static 변수의 접근 방법
1. 객체명으로 접근(주소로 접근한다)
2. 클래스명으로 접근(다이렉트로 클래스에 직접 접근한다) <br>
클래스 이름으로도 접근가능하기 때문에 클래스변수라는 이름이 붙었다 <br>

▶ 두 가지 방법도 다 사용기 가능하지만 앞서 말한듯이 직접 접근하는것은 메모리적으로나 직관적으로 볼때 바람직한 방법이 아님 따라서 **함수를 통해(1번) 접근하는것이 BEST!**

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
: 클래스 변수와 비슷함 (인스턴스와 관계가 없다)

#  System.out.println / public static void main( )
```java
System.out.println

//System(클래스의이름).out(.찍고 접근한 클래스 변수=System클래스의 위치한 static변수).println(.찍고 메소드 호출)
```
```java
public static void main( ) : 메인메소드 (하나만 존재해야 하는 메소드)
```

# static 초기화 블록
static 변수 값을 직접 할당하는 것이 아니라 값을 얻어와서 초기활할때 사용
