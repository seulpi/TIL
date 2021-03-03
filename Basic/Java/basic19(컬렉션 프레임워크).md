# 컬렉션 프레임워크 (★ stack은 반드시 구현할 줄 알아야함!)
## @ 정의
- 컬렉션 프레임워크란 4객의 객체를 의미한다 <br> **Set< E >, List< E >, Queue < E >, Map < K, V>**
- 자료구조 및 알고리즘을 구현해놓은 일종의 라이브러리이기도 하다 (제네릭 기반으로 구현되어있음)
 
![collection](https://user-images.githubusercontent.com/74290204/109606060-7040a780-7b69-11eb-98d5-3757a88acbb8.png)


## 1. List < E > : 객체에 대한 연결
### - 구조 (내가그린걸로 교체할것) : 기차와 같은 연결 구조
![List](https://user-images.githubusercontent.com/74290204/102896195-53249400-44a9-11eb-9cfd-64735dc81797.PNG)

### - 특징 
>> 해당 객체가 들어갈 때 저장순서를 유지, 중복 저장을 허용
- ArrayList< E > : 배열 기반 자료 구조(배열 안쓰고 ArrayList씀 훨씬 좋아서) , 데이터를 배열로 관리 / 배열은 **'연속된 공간'**
- LinkedList < E > : 리스트 기반 자료 구조 , **'No연속된 공간'**
```java
import java.util.ArrayList;
import java.util.List;

public class class_1221 {
	public static void main(String[] args) {
		List<String> list = new ArrayList<>(); 
		// ArrayList를 LinkedList로 바꿔도 밑에 코드가 돌아간다 (구현내용은 전혀 다른데 함수자체는 똑같아서:오버라이딩)
		
		list.add("Toy");
		list.add("Box");
		list.add("Robot");
		
		for(int i = 0; i < list.size(); i++) {
			System.out.print(list.get(i) + '\t');
		}System.out.println();
		
		list.remove(0); // 인덱스 0번째 인스턴스 삭제
		
		for(int i = 0; i <list.size(); i++) {
			System.out.print(list.get(i) + '\t');
		}System.out.println();
	}
}
===========================================
▶
Toy	Box	Robot	
Box	Robot	
```

### - 장단점
1. ArrayList 
- 장점: 저장된 인스턴스의 참조가 빠르다 (연속된 공간이라서), **검색에 용이**
- 단점 : 배열은 기본적으로 Immutable이기 때문에 저장 공간을 늘리는 과정에서 시간이 많이 걸리고 <br> 인스턴스 삭제 과정에서 많은 연산이 필요하다 (기존에 있던것에서 붙이는 게 안되는 Immutable: 새로 생성해서 저장한다)

2. LinkedList 
- 장점: 저장 공간을 늘리는 과정이 간단하고 인스턴스 삭제 과정이 단순하다, **추가&삭제가 용이**
- 단점: 저장된 인스턴스의 참조하는 과정이 복잡하다(연속된 공간이 아니기 때문에 참조 → 참조하는 과정 반복)

☞ 인스턴스의 삭제나 데이터 삽입 多 : LinkedList / 검색 多 = ArrayList

>> [참고] LinkedList도 배열처럼 enhanced for문 사용 가능 <br> (→ Collection은 literable< E >를 상속받고 있기 때문에 가능하다)
```java
public static void main(String[] args) {
	List<String> list = new LinkedList<>();

	list.add("Toy");
	list.add("Box");
	list.add("Robbot");

	for(String s : list) {
		System.out.println(s + '\t');
	}
}
```

----
#### Q. 배열 시험 : 글자 리벌스해서 출력해봐라 
----

### - Iterator : List안에 있는 객체, 반복자
- Iterator 객체 안에는 hasNext() / next() / remove() 가 존재
  - boolean hasNext() : 안에 반환할 함수가 있는지에 대한 Qustion , 증감 없어도 본인이 알아서 커서를 움직이고 다니면서 값이 있는지 확인해줌!
  - Object next() : 반환할 함수 있으면 호출해서 출력
  - void remove() : next()가 호출한 함수 삭제 

```java
public static void main(String[] args {
	List<String> list = Arrays.asList("Toy", "Box", "Robot", "Box"); //List하는 동시에 초기화 Arrays.asList는 형변환 같은 것 ! <배열방을 잡아놓은 것을 List형으로 바꿔주는 역할>
	list = new ArrayList<>(list); 

	for(Iterator<String> itr = list.iterator(); itr.hasNext();) {
		System.out.print(itr.next() + '\t');
	}System.out.println();

	for(Iterator<String> itr = list.iterator(); itr.hasNext();) {
		if(itr.next().equals("Box")) {
			itr.remove();
		}
	}

	for(Iterator<String> itr = list.iterator; itr.hasNext();) {
		System.out.print(itr.next() + '\t');
	}System.out.println();
}
==========================================
▶ 	
Toy	Box	Robot  Box	
Toy	Robot
```
- ArrayList는 배열처럼 선언과 동시에 초기화 X <br>
단, 위 코드가 초기화 한 방법처럼 List<String> list = Arrays.asList("Toy", "Box", "Robot", "Box")의 형태처럼은 가능 <br>
(배열로 만들어서 list객체로 쓰는 방법 , 실무보단 교육용임)

- ArrayListt를 LinkedList로 바꾸고 싶다면?
```
	List<String> list = Arrays.asList("Toy", "Box", "Robot", "Box"); 
		list = new ArrayList<>(list);
 
→	List<String> list = Arrays.asList("Toy", "Box", "Robot", "Box");
		list = new LinkedList<>(list);

▶ 이게 가능한 이유는 같은 Collection의 List를 상속받고 있기 때문
```
- 컬렉션 프레임워크는 기본적으로 '제네릭'으로 구현되어있기 때문에 기본형 타입 선언X 
```java
public static void main(String[] args) {
	//LinkedList<int> list (X)
	LinkedList<Integer> list = new LinkedList<>();
	list.add(10); // Auto Boxing
	list.add(20);
	list.add(30);

	int n;
	for(Iterator<Integer> itr = list.iterator(); itr.hasNext();) {
		n = itr.next(); // Auto UnBoxing
		System.out.print(n + "\t");
	}System.out.println();
}
========================================================================
▶ 10  20  30	
```

### - 양방향 : 양방향~set까지 수업 다시 듣기
```java
class ListIteratorCollection {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("Toy", "Box", "Robot", "Box");
        list = new ArrayList<>(list);
       
        ListIterator<String> litr = list.listIterator();
        String str; 

        // 왼쪽에서 오른쪽으로 
        while(litr.hasNext()) {
            str = litr.next();
            System.out.print(str + '\t');

            if(str.equals("Toy"))
                litr.add("Toy2");  // 저장은 Toy2가 되지만 출력이 안되는 이유는 프로그램이 그렇게 되어있기때문에 이유가 없다 (원리는: next할때 cursor가 움직이고 add할때도 cursor움직임 따라서 추가는 되지만 cursor가 움직이니까 출력은 안됨 
        }
        System.out.println();
        
        // 오른쪽에서 왼쪽으로

        while(litr.hasPrevious()) {   // hasPrevious는 cursor를 움직이지 않고 내용물만 확인
            str = litr.previous(); // cursor를 앞으로 한칸 
            System.out.print(str + '\t');

            if(str.equals("Robot"))
                litr.add("Robot2"); //add는 curosr 한칸 같이 추가된다 거꾸로든 앞으로든 
        }
        System.out.println();

        // 다시 왼쪽에서 오른쪽으로
        for(Iterator<String> itr = list.iterator(); itr.hasNext(); )
            System.out.print(itr.next() + '\t');
        System.out.println();
    }
}
```
## 2. Set < E > : 집합을 구현해 놓은 것
### - 특성 (List와 반대)
1. 데이터의 순서 유지되지 않는다
2. 데이터의 중복을 허용하지 않는다 
```java
public static void main(String[] args) {
	Set<String> set = new HashSet<>();
	set.add("Toy");
	set.add("Box");
	set.add("Robbot");
	set.add("Box");
	System.out.println("인스턴스 수: " + set.size());

	for(Iterator<String> itr) = set.iterator(); itr.hasNext();) {
		System.out.print(itr.next() + '\t');
	}System.out.println();

	for(String s : set) {
		System.out.print(s + '\t');
	}System.out.println();
}
===========================================================================
▶
인스턴스 수: 3
Robbot	Box	Toy	
Robbot	Box	Toy	
```

---
#### Q. Set을 이용해 로또 프로그램을 작성해라
```java
public static void main(String[] args) {
		Set<Integer> lotto = new HashSet<>();
		
		while(lotto.size() < 6 ) { // 6개를 돌리는걸 생각을 못함 while(조건이니까 요렇게..)
			lotto.add((int)(Math.random()*45)+1); // Math() 안넣으니까 숫자가 출력이 안됨..괄호 잘써주기
		}
		//출력 방법1.
		System.out.println(lotto); 
		
		//출력 방법2.
		for(Iterator<Integer> num = lotto.iterator(); num.hasNext();) {
			System.out.println(num.next() + '\t');
		}
	}
```
---

### - Set < E > 의 원리 : List에 비해 사용빈도는 낮지만 원리를 이해하는 게 중요!★

☞ Set 은 중복을 허용하지 않음! 그렇다면 어떤 원리로 중복을 걸러내는가? 동일 인스턴스인지 아닌지 기준이 뭘까? <br>
>> [암기!] **동일 인스턴스의 기준**  <br> 1. 내부적으로는 equals를 호출해서 equals의 값이 같아야 한다 <br> 2. hashCode를 호출해서 hashCode의 값이 같아야 한다 <br> **▶ 2가지 조건이 모두 충족해야(&&) 동일 인스턴스라고 판단**
```
**[동일인스턴스 비교 원리]**
1. hashCode 호출 → 2.리턴값에 따라 집합을 만든다(캐비넷을 만든다) → 3.집합 안에 숫자를 넣는다 
→ 4. 중복되는 요소를 비교한다(이 단계에서 equsals를 호출해서 비교한다)
▶ hashCode @Override를 해서 정의 안해주면 부모에 있는 hashCode를 호출하게됨 (Object에 있는 hashCode) 
- 부모의 hashCode 분류되는 것은 주소값이기 때문에 주소값으로 집합들이 만들어짐
(주소값이 같을 순 없으니까 주소값으로만 비교하게 되면 4000개면 4000개의 집합들이 만들어짐)
```
```java
public class class_1222 {

	public static void main(String[] args) {
		HashSet<Num> set = new HashSet<>();
		set.add(new Num(7799));
		set.add(new Num(9955));
		set.add(new Num(7799));
		
		System.out.println("인스턴스의 수 : " + set.size());
		
		for(Num n : set) {
			System.out.print(n.toString() + '\t');
		}System.out.println();
	}
}

class Num {
	private int num;
	public Num(int n) {
		this.num = n;
	}
	
	@Override
	public String toString() {
		return String.valueOf(num);
	}
	
	@Override
	public boolean equals(Object obj) {
		if(num == ((Num)obj).num) {
			return true;
		} else {
			return false;
		}
	}
}
===================================================================
▶
인스턴스의 수 : 3
7799	7799	9955
```

#### - hashCode
1. hashCode에 대한 이해 
- math에서의 hash table (그림 교체할것)

![hashtable](https://user-images.githubusercontent.com/74290204/102901149-9cc4ad00-44b0-11eb-8e7c-816dfa95289d.PNG)

- hasing : search의 대표적인 알고리즘, key값을 비교해서 탐색하고자 하는 항목의 주소를 찾는 것 
- hashCode : hash table을 hash함수로 바꿔주는 것 <br> (함수안에 리스트를 만들어 함수 key만 입력하면 빨리 찾을 수 있어서 속도가 가장 빠르다)

2. 정의 : hashCode는 객체의 주소 (key값은 실제로 메모리에 잡히는 객체의 실제 주소값)
  - hashCode는 소스코드를 직접 볼 수 없지만 hashCode는 key(값)이 들어와서 hashCode함수안에서 값을 변경해서 결과값을 출력한다
3. 목적 : 분류를 시키고 빠르게 탐색하는 것 / 세밀하게 나눈 만큼 탐색속도 UP (세밀하게 나눈다는 것은 집합을 그만큼 자세하게 나누는것) <br>
- ☞ Q1. 왜 탐색속도가 높아질까? 집합안에 들어가서 equals를 따져야하는데 분류를 자세하게 나눈다면 비교할 횟수가 줄어들기 때문
- ☞ Q2. int로 친다면 분류를 해서 담는 게 빠를까? 아님 각자의 주소로 21억개의 방 만들어서 찾는게 빠를까? <br>

A: 21억개! 검색할 때 방에 대한 주소를 찾아가는 데 (주소를 찾아 가는 시간은 얼마 안 걸림) 분류해 놓으면 들어가서 **'equals'라는 비교연산을 해야해서 시간이 많이 걸림** <br> 단 21억개 주소를 담는 메모리를 만들어야 하기 때문에 뭐가 좋다고는 할 수 X <br>  각각의 상황에 맞게 때에 맞게 사용(적당한 갯수 생각해서, 근데 요새 메모리 잘 안 따짐^_____^;;)
4. 역할 :  캐비넷을 만든다 (분류하는 방을 만든다)
5. Objects hash (Object아님 주의!) : hashCode를 생성하는 역할의 함수 

```java
>> 실제 hashCode에 대한 Object 클래스 내부의 정의 

class Object {
 public native int hashCode();
// native : C, C++에서 만들어져있는 걸 호출하기 위해 쓰는 type
} 
```
```java

import java.util.HashSet;
import java.util.Iterator;
import java.util.Set;

public class class_1222 {
	public static void main(String[] args) {
		HashSet<Num> set = new HashSet<>();
		set.add(new Num(7799));
		set.add(new Num(9955));
		set.add(new Num(7799));
		
		System.out.println("인스턴스의 수 : " + set.size());
		
		for(Num n : set) {
			System.out.print(n.toString() + '\t');
		}System.out.println(); 
	}
}

class Num {
	private int num;
	public Num(int n) {
		this.num = n;
	}
	
	@Override
	public String toString() {
		return String.valueOf(num);
	}
	
	@Override
	public int hashCode() {
		return num % 3; 
		/* num을 3으로 나눴을 때 나머지 값 리턴
		 (그렇다면 hashCode캐비넷은 자동으로 '0','1','2' 캐비넷을 만든다 캐비넷 갯수 총 3개)*/	
	}
	
	@Override
	public boolean equals(Object obj) {
		if(num == ((Num)obj).num) {
			return true;
		} else {
			return false;
		}
	}
}
===========================================================================
▶
인스턴스의 수 : 2
9955	7799
/*									[전제조건]
- 동일 인스턴스(중복처리를 하려면)이면 equals, hashCode값이 같아야함!!
hashCode를 오버라이딩을 안하게되면 주소값을 리턴하게 되는데 hashCode는 7799의 나머지값이 같기 때문에 같은 캐비넷 
- equals와 hashCode 호출하는 주체 set.add */
```

### - TreeSet
: Set은 순서가 없는데 정렬이 필요하다보니 생김 
- 정렬 상태가 유지되면서 인스턴스가 저장된다 (중복 X)
- Tree모양으로 정리하면서 정렬을 만들어서 출력한다 
- Set은 값 삽입 시 **순서가 없기 때문에 (하나의 집합)** 배열(Array)이나 List 처럼 **.get(인덱스)로 값을 가져올 수 없고 Iterator를 통해 가져와야한다**
	- set.iterator()로 set 값을 iterator에 담은 후 .next를 통해 하나씩 뽑아내는 식

▶ 구조와 내용 참고 https://doublesprogramming.tistory.com/185
```java
public class SetTree {

	public static void main(String[] args) {
		TreeSet<Integer> tree = new TreeSet<Integer>();
		tree.add(3);
		tree.add(1);
		tree.add(2);
		tree.add(4);
		System.out.println("인스턴스의 수 : " + tree.size());
		
		// for-each문 이용해서 출력
		for(Integer n : tree) { // treeSet은 정렬상태가 유지되기 때문에 그냥 출력해도 정렬값이 나옴
			System.out.print(n.toString() + '\t');
		}System.out.println();
		
		//반복자 이용해서 출력
		for(Iterator<Integer> itr = tree.iterator(); itr.hasNext();) {
			System.out.print(itr.next().toString() + '\t');
		}System.out.println();

	}
}
========================================
▶ 
인스턴스의 수 : 4
1	2	3	4	
1		2		3		4
```
<br>

#### @ TreeSet의 내림차순 구현
1. Comparble, compareTo 를 Override해서 구현
2. Comparator, compare를 Overrode해서 구현 
- Comparator는 String을 정렬할 때 사용할 수 있음  
```java
package class_1223;

import java.util.Comparator;
import java.util.Iterator;
import java.util.TreeSet;

public class SetTree {

	public static void main(String[] args) {
		TreeSet<Person> tree = new TreeSet<>(new PersonComparator()); // Comparator는 생성자를 통해 구현할수 있다		
        tree.add(new Person("YOON", 37));
		tree.add(new Person("HONG", 53));
		tree.add(new Person("PARK", 22));
		
		for(Person p : tree) {
			System.out.println(p);
		}
	}
}
class Person implements Comparable<Person> {
	String name;
	int age;
	
	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}
	
	@Override 
	public String toString() {
		return name + " : " + age;
	}
	
	@Override 
	public int compareTo(Person p) {
		return this.age - p.age;
	}
}
class PersonComparator implements Comparator<Person> {
	public int compare(Person p1, Person p2) {
		return p2.age - p1.age;
	}
}
========================================
▶ 
HONG : 53
YOON : 37
PARK : 22
```
```java
package class_1223;

import java.util.Comparator;
import java.util.Iterator;
import java.util.TreeSet;

public class SetTree {

	public static void main(String[] args) {
		TreeSet<String> tree = new TreeSet<>(new StringComparator());
		tree.add("Box");
		tree.add("Rabbit");
		tree.add("Robbot");
		
		for(String s : tree) {
			System.out.print(s.toString() + "\t");
		}System.out.println();
		
	}
}
class StringComparator implements Comparator<String> {
	public int compare(String s1, String s2) {
		return s1.length() - s2.length();
	}
}
========================================
▶ Box	Rabbit	
//왜 이렇게 나오나 했더니 길이로 비교해놨더니 Rabbit과 Robbot이 길이가 같아서 같은걸로 인식해서 이렇게 출력
```
```java
public static void main(String[] args) {
		// 증복을 허용하는 List
		List<String> lst = Arrays.asList("Box", "Toy", "Box", "Toy");
		ArrayList<String> list = new ArrayList<>(lst);
		
		// List에 담긴 문자열 차례대로 출력
		for(String s : list) {
			System.out.print(s.toString() + '\t');
		}System.out.println();
		
		// 중복된 인스턴스 허용X: Set , list에 담긴 문자열의 중복을 걸러낸 것을 set에 담겠다
		HashSet<String> set = new HashSet<>(list);
		
		// set에 담긴 것을 다시 list에 옮김
		list  = new ArrayList<>(set);
		
		// 중복없는 결과값 출력
		for(String s : list) {
			System.out.print(s.toString() + '\t');
		}System.out.println();
	}
}
========================================
▶
Box	Toy	Box	Toy	
Box	Toy	
```
<br>

## 3. Queue<E> : FIFO(First In First Out)
; 잘 안쓰지만 개념만 잘 익혀놓자 

 - Queue (FIFO :Firset In First Out) <br>
 : 공간이 막혀있지 않고 뻥- 뚫려있는 구조

 ![큐](https://user-images.githubusercontent.com/74290204/102997322-28e5db80-4568-11eb-8f57-67cd5463b5f4.PNG)

 - Stack (FILO: First In Last Out) <br>
 : 밑에 막힌 구조로 먼저 들어간 값이 제일 마지막에 나오는 구조

 ![스택](https://user-images.githubusercontent.com/74290204/102997330-2d11f900-4568-11eb-9ded-f94d964e5168.PNG)

 - 용어 
    - offer : 넣기 (=add)
    - poll : 꺼내기(=remove)
    - peek : 엿보기, 또 있나~하는 것(=hasNext)

    ```java
    public static void main(String[] args) {
	Queue<String> que = new LinkedList<>();
	/*Queue 사이즈 컨트롤하는거지 안에 객체는 참조형이 들어와도괜찮음
	따라서 LinkedList<>();도 들어갈수있음*/
		
	que.offer("Box");
	que.offer("Toy");
	que.offer("Robot");
		
	// 다음에 나올게 뭔지 확인
	System.out.println("next: " + que.peek());
		
	// 순서대로 출력
	System.out.println(que.poll());
	System.out.println(que.poll());
		
	// 다음에 나올 게 뭔지 확인
	System.out.println("next: " + que.peek());
	// 남은 문자열 출력
	System.out.println(que.poll());
    }
    ```
<br>

### - Deque<E> : 양방향(Queue보다 upgrade)
- 용어
![Deque](https://user-images.githubusercontent.com/74290204/103287986-377c3900-4a27-11eb-9519-3b8193e3629f.PNG)

▶ addFirst와 offerFirst의 차이는? <br>
: 같은건데 **리턴형이 다름** , 뭔가 확인해야할 게 있다면 offerFirst 사용
```java
public static void main(String[] args) {
	Deque<String> deq = new ArrayDeque<>();
		
	// 앞으로 넣고
	deq.offerFirst("1.Box");
	deq.offerFirst("2.Toy");
	deq.offerFirst("3.Robot");
		
	// 앞에서 꺼내기
	System.out.println(deq.pollFirst());
	System.out.println(deq.pollFirst());
	System.out.println(deq.pollFirst());
	}
========================================
▶
3.Robot
2.Toy
1.Box
```
<br>

## 4. Map<K, V>
- map은 Set + List 
- Key : 뒤에 들어가는 데이터를 구분짓는 요소(현관문 열쇠 같은 존재) / 자기요소를 구분지어야하기 때문에 중복 X (따라서 Set으로 구현되어있음)
>> put : 넣기  / .get(key) : key에 있는 요소 꺼내오기, 해당 value값 튀어 나옴

### - HashMap(K, V)
```java
public static void main(String[] args) {
		HashMap<Integer, String> map = new HashMap<>();
		
		//넣기
		map.put(45, "Brown");
		map.put(37, "James");
		map.put(23, "Martin"); 
		//Key가 동일한 경우 :실시간 에러는 안나고, 먼저 들어간 value를 튕겨내고 마지막에 들어간 value로 바뀌어서 출력을 한다 (마지막에 들어간 value값으로)
		map.put(23, "Lee"); 
		
		//꺼내기 
		System.out.println("23번: " + map.get(23));
		System.out.println("37번: " + map.get(37));
		System.out.println("45번: " + map.get(45));
	}
```

```java
public static void main(String[] args) {
		HashMap<Integer, String> map = new HashMap<>();
		map.put(45, "Brown");
		map.put(37, "James");
		map.put(23, "Martin");
		
		//★ks에 map의 key들을 모아놓은 것을 넣음 (key를 Set으로 컨트롤하겠다)
		Set<Integer> ks = map.keySet();
		
		// key값들 출력
		for(Integer n : ks) {
			System.out.print(n.toString() + '\t');
		}System.out.println();
		
		// value값들 출력 (for-each문 활용)
		for(Integer n : ks) {
			System.out.print(map.get(n).toString() + '\t');
		}
		
		// value값들 출력(반복자 활용)
		for(Iterator<Integer> itr = ks.iterator(); itr.hasNext();) {
			System.out.print(map.get(itr.next()) + '\t');
		}System.out.println();
	}
```
<br>

### - TreeMap<K, V> : 순서 유지 (Key값을 순서대로 접근)
```java
위 코드에서 
HashMap<Integer, String> map = new HashMap<>(); → TreeMap<Integer, String> map = new TreeMap<>();
이렇게 변경해서 사용하면 TreeMap임
```
<br>

## 5. 컬렉션프레임워크의 정렬 
>> public static<T extends Comparable < T >> void sort(List< T >list) 
-  sort는 Comparable을 구현해내야한다라는 뜻 **(sort를 하기 위해서 반드시 Comparable 구현)**
- List< T > list 객체를 받아 낼 수 있다  

### - Collections.sort(list); 컬렉션 정렬 방법(기본정렬 - 오름차순)
1. Comparable
2. Comparator : 클래스에서 객체를 만들어 sort에 내가 만든 객체를 집어넣을 수 있고 내부적으로는 객체를 먼저 정렬한다 
```java
// 1. Comparable을 사용해서 정렬

package class_1223;

import java.util.ArrayList;
import java.util.Collection;
import java.util.Collections;
import java.util.Iterator;
import java.util.List;

public class CollectionSort {
	public static void main(String[] args) {
		List<Car2> list = new ArrayList<>();
		list.add(new Car2(1200));
		list.add(new Car2(3000));
		list.add(new Car2(1800));
		
		//정렬
		Collections.sort(list);
		
		for(Iterator<Car2> itr = list.iterator(); itr.hasNext();) {
			System.out.println(itr.next().toString() + '\t');
		}
	}
}
class Car2 implements Comparable<Car2> {
	private int disp;
	
	public Car2(int d) {
		this.disp = d;
	}
	
	@Override
	public String toString() {
		return "cc: " + disp;
	}
	
	@Override //Collection.sort가 호출 
	public int compareTo(Car2 o) {
		return this.disp - o.disp;
	} 
}
------------------------------------------------------------------
// 2. Comparator를 사용해서 정렬

package class_1223;

import java.util.ArrayList;
import java.util.Collection;
import java.util.Collections;
import java.util.Comparator;
import java.util.Iterator;
import java.util.List;

public class CollectionSort {

	public static void main(String[] args) {
		List<Car2> list = new ArrayList<>();
		list.add(new Car2(1200));
		list.add(new Car2(3000));
		list.add(new Car2(1800));
		
		List<ECar> elist = new ArrayList<>();
		elist.add(new ECar(3000, 55));
		elist.add(new ECar(1800, 87));
		elist.add(new ECar(1200, 99));
		
		CarComp comp = new CarComp();
		
		// 각각정렬
		Collections.sort(list, comp);
		Collections.sort(elist, comp);
		
		// 각각출력
		for(Iterator<Car2> itr = list.iterator(); itr.hasNext();) {
			System.out.println(itr.next().toString() + '\t');
		}System.out.println();
		
		for(Iterator<ECar2> itr = elist.iterator(); itr.hasNext();) {
			System.out.println(itr.next().toString() + '\t');
        }
	}
}
class Car2 {
	protected int disp;
	
	public Car2(int d) {
		this.disp = d;
	}
	
	@Override
	public String toString() {
		return "cc: " + disp;
	}
}

class CarComp implements Comparator<Car2> {
	@Override // Car2 정렬을 위한 클래스 'Comparator' 사용
	public int compare(Car2 o1, Car2 o2) {
		return o1.disp - o2.disp;
	}
}

class ECar extends Car2 {
	private int battery;
	
	public ECar(int d, int b) {
		super(d);
		this.battery = b;
	}
	
	@Override
	public String toString() {
		return "cc: " + disp + ", ba: " + battery;
	}
}
```
<br>

### - Search : 찾기 
- binarySearch를 사용하려면 그 전에 반드시 **정렬이 되어있어야 함**
- binarySearch하면 index값을 주게 된다 <br>

▶[참고] basic 17. Arrray 클래스
```java
public static void main(String[] args) {
		List<String> list = new ArrayList<>();
		list.add("Box");
		list.add("Toy");
		list.add("Apple");
		
		Collections.sort(list);
		
		int index = Collections.binarySearch(list, "Toy");
		System.out.println(list.get(index));
}
```
---
# Q. 아래의 IntegerComparator를 내림차순으로 정렬이 되게끔 구현하시오
```java
class ComparatorIntegerDemo {
    public static void main(String[] args) {
        TreeSet<Integer> tr = new TreeSet<>(new  IntegerComparator());
        tr.add(30);
        tr.add(10);    
        tr.add(20);        
        System.out.println(tr);   
    }
}

// 정렬은 2가지 종류인데 객체 올 수 있는게 Comparator이니까 Comparator 사용!
class IntegerComparator implements Comparator<Integer> {

	@Override
	public int compare(Integer o1, Integer o2) {
		return o2.intValue() - o1.intValue(); //intValue는 Integer 객체에서 int형 값을 뽑아내는 함수
	}
}
```



