# Object클래스와 final 선언
## @ final 선언
- 클래스앞에 final = 상속하지말란뜻 (클래스상속X)
- 함수 앞에 final = 다른 클래스에서 오버라이딩 X
<br>

## @ Object클래스
### - 정의 : 모든 클래스의 최상위 클래스
- **★ 모든클래스는 Object클래스를 상속한다★**<br>  
  - 상속하는 클래스가 없다면 컴파일러에의해 java.lang.Object 클래스를 자동으로 상속하게 코드가 구성된다 <br> 비슷한 예로 '디폴트 생성자'
  - 단] 상속은 2개 이상 안되기 때문에 다른 클래스를 상속하면 직접적으로 Object는 상속이X, 간접적으로 상속하는 형태로 상속O 
```java
Shape circle = new Circle(10); 
System.out.println(circle);
------------------------------------
// 왜 error가 안날까? 
▶ Object는 최고의 조상 
어떠한 클래스든 다 받아내기 때문에 최고의 조상을두고 다형성을 적용한 예
```
### - Object 클래스는 함수 11개 정도로 구성되어있음 
▶ [Object 클래스 함수 링크] https://hyeonstorage.tistory.com/178
```java
// 몇개는 반드시 기억하자!

1. String toString() 

public class Main {
	public static void main(String[] args) {
		A a = new A();
		System.out.println(a);
	}
}

class A  { } == class A /*extends Object */ { }
==================================================
▶ A@28a418fc (주소값출력)

class A {
	@Override // toString은 null이 아니면 toString호출
	public String toString() {
		return "이것은 A 클래스입니다";
	}
==================================================
▶ 이것은 A 클래스입니다

public static String valueOf(Object obj) {
        return (obj == null) ? "null" : obj.toString();
    } // 고슬링 아저씨가 toString 정의해놓은 함수 
```

## @ Object 클래스의 함수 종류
### 1.  finalize - 빈도수 ↓ (쓸 필요도없고 걍 아 이런게 있구나하고 넘어가면됨)
: 이 메소드는 인스턴스가 메모리에서 없어질 때 자동으로 호출된다 
- 자식클래스에서 오버라이딩 O
- super.finalize(); → Object에 있는 함수 호출 
   - 호출 안해주면 없어질 때 자동 호출X <br>
   ▶ why? super는 상속키워드임 부모에서 가져와야 부모의 기능을 사용할 수 있는데 함수호출을 안했다? <br>
   → 부모기능 사용 불가  

>> [알아두면 좋을 상식] <br> 자바에서는 버전이 업데이트 되면서 기능이 추가되기도 하지만 <br> 없어지는것도 있는데 그게 바로 finalize 
### 2. **★ equals ★**
```java
public class Main {
	public static void main(String[] args) {
		INum num1 = new INum(10);
		INum num2 = new INum(12);
		INum num3 = new INum(10);

		if (num1.equals(num2)) // equals 객체도 받아내는데 받을때는 이렇게 사용!
			System.out.println("num1, num2 내용 동일하다.");
		else
			System.out.println("num1, num2 내용 다르다");

		if (num1.equals(num3))
			System.out.println("num1, num2 내용 동일하다.");
		else
			System.out.println("num1, num2 내용 다르다");
	}
}
class INum {
	private int num;
	public INum(int num) {
		this.num = num;
	}
	
	@Override /*오버라이딩 해주지 않아도 메인에서 equals 함수 호출 가능하다 
	why?? Object함수는 최상위 함수이기 때문에 모든클래스는 Object를 상속받으니까 
	부모에 있기 때문에 호출이 가능 */
	public boolean equals(Object obj) {
		if(this.num == ((INum)obj).num) {
			return true;
		}
		else {
			return false;
		}
	} // equals 정도는 개발자들이 본인의 사용 목적에 맞게 오버라이딩 많이함!
	
}
===================================================
▶
num1, num2 내용 다르다
num1, num2 내용 동일하다
/* 눈으로 보기에는 10이 같은거지만 프로그래밍 안에선
참조하는 주소값이 다르기 떄문에 다르게 출력되어야 하는데 개발자가 지금 위에서 equals함수를 재정의해줬기 때문에 결과값이 동일하다고 나온 것! 

(헷갈린포인트! 주소값을 비교하는게 아니라 
거기 들어있는 값=num을 비교해서 10=10이 맞다면 true)*/ 
```
```java
// equals의 정의 
public boolean equals(Object obj) {
        return (this == obj);
    } 

1. this : 자기자신이 참조하고 있는 주소 값을 의미
2. obj : 비교할 변수가 참조하고 있는 주소 값을 의미 
``` 

### @ String 클래스의 equals
```java
public class Main {
	public static void main(String[] args) {
		String str1 = new String("So Simple");
		String str2 = new String("So Simple");
 
		if (str1 == str2) { // 참조값 비교
			System.out.println("참조값이 같다");
		} else {
			System.out.println("참조값이 다르다");
		}
 
		if (str1.equals(str2)) { // 내용 비교
			System.out.println("내용이 같다");
		} else {
			System.out.println("내용이 다르다");
		}
	}
}
========================================================
▶
참조값이 다르다
내용이 같다
/* why?
배운바에 의하면 equals는 객체 생성하면 객체 생성한 주소값이 달라서 false로 와야하는데 ture가 와서 '내용이 같다'가 출력됨
그 이유는 String이 자기 목적에 맞게 부모에 있는 equals함수를 오버라이딩함 따라서 String은 주소비교가 아닌 내용 비교를 하게 만들었음! */
```

### 3. clone (clone의 개념을 제대로 이해하는 게 중요!)
: 자바에서는 객체 생성 하는 방법이 2가지 <br>
```java
1. new 키워드 사용
2. Object class 안에 있는 clone을 이용
* 'clone'은 주소 뿐 만 아니라 객체 안에 들어있는 값(value)까지 그대로 복사해준다 
``` 
>> //clone 함수 사용 방법 <br> 1단계: class 클래스명 implements Cloneable { } → Cloneable이라는 marker interface가 생성되어있어야함 <br> 2단계: <br> @Ovrride<br> public Object clone() throws CloneNotSupportdeException {<br> return super.clone()<br>}

- 특징 <br> 
1. 처음생성할때는 new ; clone은 대상이 있어야함(복사해야 되니까)
2. 성능상으로는 new를 해서 객체 생성하는 것보다 clone이 더 빠름 
   - (참고) 상속은 clone하면 복잡해지고 따지려면 더 공부해야하니 지금 단계에선 개념만 정확히 알고 넘어가기
3. 활용 : 오락실 비행기게임 → 적이 총알 100개 장전해서 막 쏠 때 → 총알 100개 clone사용해서 랜덤돌림

- ***** Shallow Copy / Deep Copy *****
   1. Shallow Copy 
    - 객체 복사할 때 해당 객체만 복사해서 새 객체를 생성한다
	- 복사된 객체의 인스턴스 변수는 원본 객체의 인스턴스 변수와 <br>
	**같은 메모리 주소를 참조한다**
	- 따라서, 해당 메모리 주소의 값이 변경되면 <br> 원본 객체 및 복사 객체의 인스턴스 변수값은 같이 변경된다 
	>> 클래스안에 참조형이 있다면 클래스 대상은 객체가 아니라 자기가 가지고 있는 데이터멤버까지만 <br> 클래스 안에서 객체 생성을 하면 new 생성자는 클론의 대상X <br> (객체 참조 주소만 복사) 
	2. Deep Copy 
	- 객체를 복사할 때, 해당객체와 인스턴스 변수까지 복사
	- 전부 복사해서 새 주소에 담기 때문에 참조를 공유하지X
```java
public class Main {
	public static void main(String[] args) {
		Point org = new Point(3, 5);
		Point cpy;
		try {
			cpy = (Point)org.clone();
			org.showPosition();
			cpy.showPosition();
		}
		catch(CloneNotSupportedException e) {
			e.printStackTrace();
		}
	}
}
 
class Point implements Cloneable {
	private int xPos;
	private int yPos;
	public PointRe(int x, int y) {
		xPos = x;
		yPos = y;
	}
	public void showPosition() {
		System.out.printf("[%d, %d]", xPos, yPos);
		System.out.println();
	}
	@Override 
	public Object clone() throws CloneNotSupportedException {
		return super.clone();
	}
}
```
<br>
▶ ★ 위 코드 Shallow Copy, Deep Copy 메모리 구조 그림!!!!!!!
<br>

![clone](https://user-images.githubusercontent.com/74290204/102446980-5b786b80-4072-11eb-83a9-409af15b55fc.png)

- String 도 Deep Copy하려면 객체 생성 따로 해서 clone해줘야함 
- clone은 오버라이딩 할 때 반환형 수정 가능 <br> → 반환형을 자식으로 해주면 형 변환 할 필요 X (▼ 밑에 사진 참고)

![KakaoTalk_20201217_225711569_02](https://user-images.githubusercontent.com/74290204/102497220-b8e4da80-40bb-11eb-9453-bf130863003f.jpg)
