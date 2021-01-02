# 1. ArrayList 와 LinkedList 의 장단점은?
## - ArrayList
 - 장점 : 저장된 인스턴의 참조가 빠름
 - 단점 : 저장공간을 늘리는 과정에서 시간이 많이 걸린다 , 인스턴스 삭제 과정에서도 많은 연산이 필요함<br> 
 (배열은 Immutable이기 때문에 배열을 늘릴때도 복사하고 새로 만들어줘야함) 

## - LinkedList
 - 장점 : 배열에 비해 저장공간을 늘리는 과정이 간단하고 저장된 인스턴스의 삭제 과정이 단순하다<br> 
 (시간이 배열에 비해 얼마 안걸림)
 - 단점: 저장된 인스턴스의 참조과정이 복잡함(LinkedList는 연속된 공간이 아니기 때문에  참조 → 참조를 찾아가는 과정)
 
# 2. Scanner 클래스로 -1이 입력될 때까지 양의 정수를 입력받아  저장하고 <br>검색하여 가장 큰 수를 출력하는 프로그램을 작성하라
## - 정수(-1이 입력될 때까지)>> 10 6 22 6 88 77 -1
## - 가장 큰 수는 88
## ★ 큰 수 비교하는 로직 반드시 익히고있을것!(다른조 대답에 로직) - 다시해보기
```java
//Answer(me)
public class Text {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		System.out.println("정수(-1이 입력될 때까지)>> ");
		
		List<Integer> num = Arrays.asList(sc.nextInt());
		num = new ArrayList<>();
		
		while(true) {
			num.add(sc.nextInt());
			
			if(num.get(num.size()-1) == -1) { // -1이 입력되는 순간에 나올방법은?
				break;
			}
		}
	
		Collections.sort(num); //Arrays.sort로 했더니 error
		System.out.println(num.get(num.size()-1)); // 제일 마지막 인덱스 값
	}
}
```
```java
//Answer(다른조)
public static void main(String[] args) {
		int num = 0;
		ArrayList<Integer> list = new ArrayList<>(); // 컬렉션 인스턴스 생성
		Scanner sc = new Scanner(System.in);
                // 스캐너 while문 안에 넣게 되면 다른 결과 나옴
		
		System.out.print("정수(-1이 입력될 때까지)>> ");
		
		while (true) {
			
			num = sc.nextInt(); // 단어 하나 단위로 숫자 입력받고 저장
			
			if (num == -1) 
				break;
			
			list.add(num); // 사용자가 입력하는 숫자들 저장됨.
		}

		int maxNum = list.get(0); // 그 중 가장 큰 수
		for (int i = 1; i < list.size(); i++) {
			if (maxNum < list.get(i)) 
				maxNum = list.get(i);	
		}
			
		System.out.println("가장 큰 수는 " + maxNum);
	}
```

# 3. 로또 프로그램을 작성하시오 (Set 으로)
```java
//Answer
public static void main(String[] args) {
		Set<Integer> set = new HashSet<>();
		
		while(set.size()<6) {
			set.add((int)(Math.random()*45)+1);
		}System.out.println("로또 번호 " + set);
}
```

# 4. Set에 대하여 설명하시오
: 수학에서의 개념을 가져온 Collection , 집합을 구현해놓은 것
- 특성 : List와는 다르게 순서가 유지되지 않고, 중복을 허용하지 않는다 
```
Set은 중복을 허용하지 않기 때문에 동일 인스턴스인지 아닌지 구분하는 원리가 중요!

[동일인스턴스의 기준]
1. equals 호출 결과가 같아야함
2. hashCode의 호출 결과가 같아야함 
▶ 2가지 다 만족해야(&&) 동일인스턴스
```

# 5. 아래를 프로그래밍하시오
```java
도시 이름, 위도, 경도 정보를 가진 Location 클래스를 작성하고,
도시 이름을 '키'로 하는 HashMap<String, Location> 컬렉션을 만들고, 사용자로부터 입력 받아 4개의 도시를 저장하라. 
그리고 도시 이름으로 검색하는 프로그램을 작성하라.

도시, 경도, 위도를 입력하세요.

>> 서울, 37, 126

>> LA, 34, -118

>> 파리, 2, 48

>> 시드니, 151, -33

----------------------------------

서울 37 126

LA 34 -118

파리 2 48

시드니 151 -33

----------------------------------

도시 이름 >> 피리

피리는 없습니다.

도시 이름 >> 파리

파리 2 48

도시 이름 >> 그만
```
```java
//Answer
public class Answer_1222 {

	public static void main(String[] args) {
		
		Scanner sc = new Scanner(System.in);
		System.out.println("도시, 경도, 위도를 입력하세요.");
		
		
		Map<String, Position> map = new HashMap<>();
		
		for(int i = 0; i < 4; i++) {
			System.out.print(">> ");
			map.put(sc.next(), new Position(sc.nextInt(), sc.nextInt()));
		}
		System.out.println("-------------------------");
		Set<String> set = map.keySet();
		
		for(String key : set) {
			System.out.print(key);
			System.out.println("\t" + map.get(key));
		}
		System.out.println("-------------------------");
		
		while(true) {
			System.out.println("도시 이름>> ");
			String str = sc.next();
			
			if(str.equals("피리")) {
				System.out.println("피리라는 도시는 없습니다");
				continue;
			}else if(str.equals("파리")) {
				System.out.println(map.get("파리"));
				continue;
		
			}else if(str.equals("그만")) {
				break;
			}
			break;
		}

	}
}
class Position {
	private int n1;
	private int n2;
	
	public Position(int n1, int n2) {
		this.n1 = n1;
		this.n2 = n2;
	}
	
	public String toString() {
		return n1 + "," + n2;
	}
	
}
```

# 6. 아래를 프로그래밍하시오
```java
Scanner 클래스를 사용하여 6개 학점('A', 'B', 'C', 'D', 'F')을 문자로 입력받아 ArrayList에 저장하고, 
ArrayList를 검색하여 학점을 점수(A=4.0, B=3.0, C=2.0, D=1.0, F=0)로 변환하여 평균을 출력하는 프로그램을 작성하라.

6개의 학점을 빈 칸으로 분리 입력(A/B/C/D/F) >> A C A B F D

2.3333333333333335
```
```java
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class Answer21_06 {

	public static void main(String[] args) {
		Scan scanGrade = new Scan();
		System.out.println("6개의 학점을 빈 칸으로 분리 입력(A/B/C/D/F) >> ");
		scanGrade.scInput();
		System.out.println(scanGrade.avg());
	}

}
class Scan {
	
	List<String> grade = new ArrayList<>();
	
	public void scInput() {
	Scanner sc = new Scanner(System.in);
	
	for(int i = 0; i < 6; i++) {
		grade.add(sc.next());
	}
	sc.close();
	}
	
	public double avg() {
		
		double sum = 0;
		
		for(int i =0; i <grade.size(); i++) {
			switch(grade.get(i)) {
			case "A" : 
				sum += 4.0;
				break;
			case "B" : 
				sum += 3.0;
				break;
			case "C" : 
				sum += 2.0;
				break;
			case "D" : 
				sum += 1.0;
				break;
			default: 
				sum += 0.0;
				break;
			}
		}
		return sum / grade.size();
	}
}
```

# 7. 출력이 아래와 같이 나오도록 하시오 (필수)
```java
    HashSet<Num> set = new HashSet<>();
    set.add(new Num(7799));
    set.add(new Num(9955))s
    set.add(new Num(7799));

    System.out.println("인스턴스 수: " + set.size());

    for(Num n : set)
        System.out.print(n.toString() + '\t');

    System.out.println();

====출력===========================================
인스턴스 수: 2
7799	9955
```
```java
// Answer
public class Ansewer_1222_7 {

	public static void main(String[] args) {
        HashSet<Num> set = new HashSet<>();
        set.add(new Num(7799));
        set.add(new Num(9955));
        set.add(new Num(7799));

        System.out.println("인스턴스 수: " + set.size());

        for(Num n : set)
            System.out.print(n.toString() + '\t');

        System.out.println();

	}
}
class Num {
	private int num;
	public Num(int n) {
		this.num = n;
	}
	
	public String toString() {
		return String.valueOf(num);
	}
	
	public int hashCode() { 
		return num % 3; // hashCode 출력될때는 순서가 자유로움(그건 개의치않아도됨) 
		
	}
	
	public boolean equals(Object ob) {
		if(this.num == ((Num)ob).num) {
			return true;
		}
		else {
			return false;
		}
	}
}
```

# 8. Set 호출되는 원리와 순서를 설명하시오
```
1. hashCode 호출 
2. 리턴값에 따라 캐비넷을 만든다
3. 집합(캐비넷)안에 값을 넣는다 
4. 중복되는 요소를 비교한다(이때, equasl호출)
```

# 9. 아래와 같이 출력되도록 하시오(필수)
```java
HashSet<Person> hSet = new HashSet<Person>();
hSet.add(new Person("LEE", 10));
hSet.add(new Person("LEE", 10));
hSet.add(new Person("PARK", 35));
hSet.add(new Person("PARK", 35));

System.out.println("저장된 데이터 수: " + hSet.size());
System.out.println(hSet);
============================================================
저장된 데이터 수: 2
[LEE(10세), PARK(35세)]
```
```java
//Answer
public class Ansewer_1222_7 {

	public static void main(String[] args) {
		HashSet<Person> hSet = new HashSet<Person>();
        hSet.add(new Person("LEE", 10));
        hSet.add(new Person("LEE", 10));
       	hSet.add(new Person("PARK", 35));
        hSet.add(new Person("PARK", 35));

        System.out.println("저장된 데이터 수: " + hSet.size());
        System.out.println(hSet);

	} /*
		 * 출력 저장된 데이터 수: 2 [LEE(10세), PARK(35세)]
		 */
}

class Person {
	private String firstN;
	private int age;
	
	public Person(String n, int a) {
		this.firstN = n;
		this.age = a;
	}
	
	@Override
	public String toString() { // 기본적으로 []는 HashSet에서 출력되게끔 짜여있음
		return  firstN + "(" + age + "세)";
	}
	
	@Override
	public int hashCode() {
		return age;
	}
	
	@Override
	public boolean equals(Object ob) { 
		if(this.firstN == ((Person)ob).firstN) { 
		/* 주소비교로 하지말 것 만약 객체로 들어와서 비교하게 되면 이거 false남 if(firstN.equals~) 그리고 나이도 비교해줘야지 
		if(this.firstN.equals(((Person)ob).firstN && this.age == ((Person)ob).age) { } */
			return true;
		} else {
			return false;
		}
	}	
}
```
