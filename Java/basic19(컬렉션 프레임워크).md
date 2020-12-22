# 컬렉션 프레임워크 
## @ 정의
- 컬렉션 프레임워크란 4객의 객체를 의마한다 <br> Set< E >, List< E >, Queue < E >, Map < K, V> 
- 자료구조 및 알고리즘을 구현해놓은 일종의 라이브러리이기도 하다 (제네릭 기반으로 구현되어있음) 

▶ ppt 캡처본 자리 

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
		// ArrayLis를 LinkedList로 바꿔도 밑에 코드가 돌아간다 (구현내용은 전혀 다른데 함수자체는 똑같아서:오버라이딩)
		
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
- 장점: 저장된 인스턴스의 참조가 빠르다 (연속된 공간이라서)
- 단점 : 배열은 기본적으로 Immutable이기 때문에 저장 공간을 늘리는 과정에서 시간이 많이 걸리고 <br> 인스턴스 삭제 과정에서 많은 연산이 필요하다 (기존에 있던것에서 붙이는 게 안되는 Immutable: 새로 생성해서 저장한다)

2. LinkedList 
- 장점: 저장 공간을 늘리는 과정이 간단하고 인스턴스 삭제 과정이 단순하다
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

▶ 이게 가능한 이유는 같은 Collection의 List를 상속받고 있기 때문 ???
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
3. 목적 : 분류를 시키고 빠르게 탐색하는 것 
4. 역할 :  캐비넷을 만든다 (분류하는 방을 만든다)

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
		
