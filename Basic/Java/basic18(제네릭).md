# 제네릭 (강의듣고 다시 정리)
## @ 정의 
: 실시간 에러나는 것을 컴파일 에러로 받아주기 위한 코드 
   - [컴파일 에러: 메모리에 올라갈때부터 에러나는 것 / 빨간줄~]
<br>

### - 탄생 배경 
제네릭이전에는 하나의 부모클래스를 두고 자식들이 여러개 상속받으려면   (예를 들면 과일박스 안에 오렌지도 담고 사과도 담고 등등)너무 많으니까 <br> 
Object로 묵으면 모든 클래스들을 받을 수 있어서 그걸 이용했었는데 문제들이 발생 <br> 
<br> 
▶ 에러들이 발생하지않고 컴파일되고 실행하다 프로그램이 죽게되는 문제들! <br> 
부모클래스에서 자식클래스를 담고있지않는데 자식클래스로 형 변환해서 안에 있는 값을 꺼내오는 것 같은 <br>  말도 안되는 문제들이 생김 
더 문제는 실시간 에러 나는 것도 없이 그냥 출력되는 경우도 다반사, <br> 그래서 이를 보완하기 위한 코드로 제네릭이 생김

## @ 문법
>> 클래스 옆에 <문자> <br> class Box <T> { } : 참조형 타입을 T로 대체 
- 타입 매개변수 Box<T> 에서 'T' : 타입을 결정한다해서 T
```java
Box<Apple> aBox = new Box<Apple>();
// Box<Apple>이 aBox의 타입

▶ 클래스 Box 내부 안에서 'T' 로 선언된 것들이 다 'Apple' 로 바뀜을 의미한다
'Apple' 로 바꾼 클래스를 메모리엔 올린다

* 선언자체에서 객체의 타입을 지정해서 실시간으로 들어가기 때문에 에러날일이 없고 다형성 적용은 되지 않는다 
why? 다이렉트로 지정해서 넣어줬기 때문에 애초에 다른 객체타입이 들어갈일을 막아버렸기 때문 따라서 형변환도 필요 X
```

## @ 특징 
- 매개변수 타입의 이름은 대문자, 한개의 문자로 짓는다 
>> E, K, N , T, V (Element, Key, Number, Type, Value)
- 객체 선언할때 생성자 타입 생략 가능 
```java
Box<Apple> box = new Box<Apple>();
→ Boc<Apple> box = new Box<>();
```
- 타입 매개변수 자리에는 기본형 타입을 사용할 수 X <br>
▶ why? 타입매개변수는 객체로 선언되어야 하기 때문 따라서 기본형을 '래퍼클래스'로 선언해준다!
```java
Box<int> box = new Box<int>(); // error!

public class Class_1221 {
    public static void main(String[] args) {
        DBox<String, Integer> box = new DBox<String, Integer>(); // 매개변수 2개 이상도 가능! 
        box.set("Apple", 25) // 25가 다이렉트로 올 수 있는 이유 → Integer에 대한 오토언박싱 적용되었기 때문 
        System.out.println(box);
    }
}
class DBox<L, R> {
    private L left;
    private R right;

    public void set(L l, R r) {
        left = l;
        right = r;
    }

    @Override 
    public String toString() {
        return left + " & " + right;
    }
}
```
- 매개변수 타입을 매개변수로도 전달 가능하다
```java
public class class_1221 {

	public static void main(String[] args) {
		Box<String> sBox = new Box<>();
		sBox.set("I am so happy");
		
		Box<Box<String>> wBox = new Box<>();
		wBox.set(sBox);
		
		Box<Box<Box<String>>> zBox = new Box();
		zBox.set(wBox);
		
		System.out.println(zBox.get().get().get());
	}

}

class Box<T> {
	private T ob;
	
	public void set(T o) {
		ob = o;
	}
	
	public T get() {
		return ob;
	}
}
```

## @ 타입 매개변수 제한
>> [제한방법1.] class Box<T extends Number> { } : Number가 오거나 Number를 상속받는 것만 타입 매개변수로 가능하다

▶ 제한을 두는 이유? <br>
매개변수 자리에 어떤 타입이 올 지 모르는데 호출할 함수를 그 타입이 가지고 있을지 없을지의 함수유무를 알기 어려움 <br> 
따라서 객체안에 있는 함수를 호출하기 위해서는 제한을 둘 수 밖에 없다
```
Ex.
intValueOf() 함수는 Number클래스 안에 있음
```
>> [제한방법2.] interface 활용 
```java
public class class_1221 {
	public static void main(String[] args) {
		Box2<Apple2> box = new Box2();
		box.set(new Apple2());
		
		Apple2 ap = box.get();
		System.out.println(ap);
	}
}
interface Eatable {
	public String eat();
}

class Apple2 implements Eatable { 
	public String toString() {
		return "I am an apple.";
	}
	
	@Override
	public String eat() { // 구현은 자손이!
		return "It tastes so good!";
	}
}
class Box2<T extends Eatable> {
	private T ob;
	
	public void set(T o) {
		ob = o;
	}
	public T get() {
		System.out.println(ob.eat());
		return ob;
	}
}
========================================
▶ 
It tastes so good!
I am an apple.//ap를 호출하면 toString 자동호출
```

### - 기본적인 상속에서는 다중상속 X , 제네릭은 다중 상속 가능
```java
class Box<T extends Numner&Eatable> { }
```
- 제네릭 상속이 가능하나 같은 타입끼리 가능하다(다형성 적용X) 
>> Integer는 Integer끼리 String은 String끼리!!

▶ [핵심] 제네릭 타입에서 매개타입 변수에는 Object한다고 다형성 적용되지 않음 <br> (→ 상속관계는 메모리에서 부모의 데이터 멤버와 함수를 가져다 쓰는 것이지 부모의 데이터멤버를 바꿔 사용하는 게 X)


## @ 제네릭 메소드 
: 제네릭 메소드란 함수앞에 제네릭 선언한 것을 말한다
```java
public static <T> Box<T> makeBox(T o) { }
// 클래스 전체가 아닌 함수안에서만 제네릭선언을 하고 싶을때 (큰의미는 없음)

>> 클래스에 제네릭 선언한것과 동일하게 제한 가능
public class class_1221 {

	public static void main(String[] args) {
		Box1<String> sBox = BoxFactory.makeBox("Sweet");
		System.out.println(sBox.get());
		
		Box1<Double> dBox = BoxFactory.makeBox(7.59);
		System.out.println(dBox.get());
	}
}

class Box1<T> {
	private T ob;
	
	public void set(T o) {
		ob = o;
	}
	
	public T get() {
		return ob;
	}
}

class BoxFactory {
	public static <T> Box1<T> makeBox(T o) {
		Box1<T> box = new Box1<T>();
		box.set(o);
		return box;
	}
}
``` 

## @ 와일드 카드 (제네릭과 기능은 같으나 다른 것) 
: 와일드 카드는 제네릭에서 다른 타입을 같이 사용하지 못한 부분을 해결해줌(polymorphism이 적용안되는 제네릭의 단점을 보완) <br> * **와일드 카드는 Integer를 넣든 String을 넣든 마음대로 쓸 수 있음** * (? = object로 이해해도됨) 
>> [문법] public static void peekBox(Box<?> b) { } 

### - 와일드 카드의 제한
```java
//Number이거나 Number상속하는거만 올 수 있다
public static void peekBox(Box<? extends Number> b) {
		System.out.println(b);
	}
// Integer이거나 Integer위에 까지만 올 수 있다 (Integer위에는 Number클래스) / 제네릭에는 없는 super는 와일드카드가 나오게되면 생겼다
public static void peekBox(Box<? super Integer> b) {
		System.out.println(b);
	}

// 제한을 써야하는 이유 : ???????????
public class class_1221 {
	public static void main(String[] args) {
		Box1<Toy> box = new Box1<>();
		BoxHandler.inBox(box, new Toy());
		BoxHandler.outBox(box);
	}
}

class Box1<T> {
	private T ob;
	
	public void set(T o) {
		ob = o;
	}
	
	public T get() {
		return ob;
	}
}

class Toy {
	@Override
	public String toString() {
		return "I am a Toy";
	}
}

class BoxHandler {
	public static void outBox(Box1<? extends Toy> box) {
		Toy t = box.get();
		box.set(new Toy()); //error! → polymorphism에 위배되서 안되는 것 (자손은 부모이기 때문에)
		System.out.println(t);
	}
	public static void inBox(Box1<? super Toy>box, Toy n) {
		box.set(n);
	}
}
```

- 제네릭 안에서 매개변수 타입에 대해서는 오버로딩 적용이 X <br>
*why? 컴파일할때 문법상황으로 오는 것일뿐 일반적인 클래스에 내용물을 채워 메모리에 올리기 때문에 <br> 컴파일되면<>안에 있는것들은 사라지기 때문에 Box<? extends Toy > box 나 Box<? extends Robot > box 나 똑같은 box*<br>
▶ 그래서 생긴 대안) **제네릭 + 와일드카드**
```java
public static <T> void outBox(Box<? extends T> box) { }
``` 

## @ interface에서의 제네릭 
: 제네릭 선언가능 단, 제네릭으로 선언해도 자식이 구현해야한다~
```java
public class class_1221 {
	public static void main(String[] args) {
		Box1<Toy> box = new Box1<>();
		box.set(new Toy());
		// Box<T>가 Getable<T>를 구현하므로 참조 가능
		Getable<Toy> gt = box;
		System.out.println(gt.get());
	}
}

interface Getable<T> {
	public T get();
}

class Box1<T> implements Getable<T> {
	private T ob;
	public void set(T o) {
		ob = o;
	}
	
	@Override
	public T get() {
		return ob;
	}
}

class Toy {
	@Override
	public String toString() {
		return "I am a Toy";
	}
}
```
