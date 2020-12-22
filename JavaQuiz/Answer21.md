# 1. 제네릭 클래스의 타입 인자 제한하는 방법과 효과는?
- 방법
>> class Box < T extends Number > { } <br> Number가 오거나 Number를 상속하는 것만 타입 매개변수로 가능
- 제한하는 이유는 매개변수 자리에 어떤게 올 지 모르기 때문에 호출할 함수를 가지고 있을지도 알 수 가 없음  <br> 객체 안에 있는 함수를 호출하기 위해 제한을 둠으로써

# 2. 아래와 같이 출력값이 나오도록 프로그래밍 하시오 
```java
class DDBoxDemo {
    public static void main(String[] args) {
        DBox<String, Integer> box1 = new DBox<>();
        box1.set("Apple", 25);

        DBox<String, Integer> box2 = new DBox<>();
        box2.set("Orange", 33);
        
        DDBox<DBox<String, Integer>, DBox<String, Integer>> ddbox = new DDBox<>();
        ddbox.set(box1, box2);

        System.out.println(ddbox);
    }
}
=====================================================================
Apple & 25
Orange & 33
```
```java
// Answer
public class DDBoxDemo {
	public static void main(String[] args) {
	    DBox<String, Integer> box1 = new DBox<>();
        box1.set("Apple", 25);

        DBox<String, Integer> box2 = new DBox<>();
        box2.set("Orange", 33);
        
        DDBox<DBox<String, Integer>, DBox<String, Integer>> ddbox = new DDBox<>();
        ddbox.set(box1, box2);

        System.out.println(ddbox);
	}
}

class DBox <T1, T2> {
	private T1 fruit1;
	private T2 num1;
	
	public void set(T1 f1, T2 n1) {
		fruit1 = f1;
		num1 = n1;
	}
	
	public String toString() {
		return fruit1 + " & " + num1;
	}
}
class DDBox<T3, T4> { 
	private DBox fruit2;
	private DBox num2;
	
	public void set(DBox f2, DBox n2) {
		this.fruit2 = f2;
		this.num2 = n2;
	}
	
	public String toString() {
		return fruit2 + "\n" + num2;
	}
}

```
# 3. 아래와 같이 출력값이 나오도록 프로그래밍 하시오 
```java
 public static void main(String[] args) {
        Box<Integer> box1 = new Box<>();
        box1.set(99);

        Box<Integer> box2 = new Box<>();
        box2.set(55);

        System.out.println(box1.get() + " & " + box2.get());
        swapBox(box1, box2);
        System.out.println(box1.get() + " & " + box2.get());
    }
=====================================================================
99 & 55
55 & 99
```
```java
//Answer
public class Answer_1221 {
	public static void main(String[] args) {
		Box<Integer> box1 = new Box<>();
		box1.set(99);

		Box<Integer> box2 = new Box<>();
		box2.set(55);

		System.out.println(box1.get() + " & " + box2.get());
		swapBox(box1, box2);
		System.out.println(box1.get() + " & " + box2.get());
	}

	private static void swapBox(Box<Integer> box1, Box<Integer> box2) { 
	//제네릭이니까 private static <T extends Number> void swapBox(Box<T> box1, Box<T> box2) 이렇게 사용하자~
		int temp;
		temp = box1.get();
		box1.set(box2.get());
		box2.set(temp);
	}
}
class Box <T> {
	private T num;
	
	public void set(T n) {
		num = n;
	}
	
	public T get() {
		return num;
	}
}
```

# 4. 지네릭 메소드에 대하여 설명하시오
클래스 전체에 제네릭이 아닌 함수 안에서만 제네릭 선언을 하는 것 <br>
클래스 전체에 제네릭 선언하는 것처럼 선언하고 제네릭 메소드도 제한설정이 가능하다

# 5. 와일드 카드와 상한 제한, 하한 제한에 대하여 설명하시오
1. 와일드 카드란 제네릭이 매개타입에 대해 오버로딩이 적용되지 않는 것을 해소하고자 나온 문법 <br>
따라서 와일드 카드는 Integer가 오던 String이 오던 상관없이 사용가능하다 
>> [문법] public static void peekBox(Box<?> b) {  } <br> → 키워드 '?'를 사용 
2. 상한 제한과 하한 제한은 와일드카드를 사용할때 제한을 어디에 두냐에 따라 나눠지는 방식을 말한다 
- 상한 제한: 부모 클래스를 상속받거나 부모클래스만 올 수 있게 제한하는 것 
- 하한 제한: 자식클래스이거나 자식클래스 위 부모클래스까지만 올 수 있게 제한하는 것 
```java
// 상한 제한
public static void peekBox(Box<? extends Number> b) {
    System.out.println(b);
}

// 하한 제한
public static void peekBox(Box<? super Integer> b) {
    System.out.prinln(b);
}
```

# 6. 아래가 에러가 나는 이유를 설명하시오
```java
public static void inBox(Box<? super Toy> box, Toy n) {
   box.set(n);   // 넣는 것! OK!
   Toy myToy = box.get();   // 꺼내는 것! Error!
}
```
위에 함수의 제한은 하한 제한이 이루어진것!<br>
Toy가 오거나 Toy를 상속해주는 부모클래스가 올 수 있는데 'Toy myToy = box.get()'에서 box는 부모클래스를 데이터타입으로 받고 있다 <br> 그런데 자식 = 부모가 될 수 없기 때문에 다형성의 위배 되서 error 발생하는 것  


# 7. 아래와 같이 메소드 오버로딩이 되지 않는 이유는?
```java
// 다음 두 메소드는 오버로딩 인정 안됨.
   public static void outBox(Box<? extends Toy> box) {...}
   public static void outBox(Box<? extends Robot> box) {...}
```
컴파일할때 메모리에 함수를 올리는 과정은 일반적인 클래스에 내용을 채워서 올리기 때문에 컴파일되면 두 함수는 동일한것 (문법적으로만 다른것!!)
<br>  따라서 컴파일되면 < > 안에있는것은 사라지기 때문에 오버로딩 적용이 안되는 것 (사라지게 되면 box = box)


# 8. 아래의 결과가 나오도록 프로그래밍을 완성하시오
```java
 public static void main(String[] args) {
        Box<Integer> box1 = new Box<>();
        box1.set(24);

        Box<String> box2 = new Box<>();
        box2.set("Poly");

        if(compBox(box1, 25))
            System.out.println("상자 안에 25 저장");

        if(compBox(box2, "Moly"))
            System.out.println("상자 안에 Moly 저장");
        
        System.out.println(box1.get());
        System.out.println(box2.get());
    }

=====================================================================
24
Poly
```
```java
//Answer
public class Answer_1221 {

	public static void main(String[] args) {
		Box<Integer> box1 = new Box<>();
		box1.set(24);

		Box<String> box2 = new Box<>();
		box2.set("Poly");

		if (compBox(box1, 25))
			System.out.println("상자 안에 25 저장");

		if (compBox(box2, "Moly"))
			System.out.println("상자 안에 Moly 저장");

		System.out.println(box1.get());
		System.out.println(box2.get());
    }
	
	public static <T> boolean compBox(Box<T> box, T t) {
		Box<T> temp = new Box<>();
		temp.set(t);
		
		if(temp.get() == box.get()) {
			return true;
		}else {
			return false;
		}
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

# 9. 콜렉션 프레임워크란?
## - 4객의 객체를 의미한다 [ Set< E > / List < E > / Queue < E > / Map<K, V>] <br> - 자료구조 및 알고리즘을 구현해 놓은 일종의 라이브러리 



