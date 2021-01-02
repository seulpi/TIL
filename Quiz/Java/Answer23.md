# 1. 아래를 프로그래밍하시오 
```
"그만"이 입력될 때까지 나라 이름과 인구를 입력받아 저장하고
다시 나라 이름을 입력받아 인구를 출력하는 프로그램을 작성하라. 
다음 해시맵을 이용하라.

나라 이름과 인구를 입력하세요.(예: Korea 5000)

나라 이름, 인구 >> Korea 5000

나라 이름, 인구 >> USA 1000000

나라 이름, 인구 >> Swiss 2000

나라 이름, 인구 >> France 3000

나라 이름, 인구 >> 그만
----
인구 검색 >> France

France의 인구는 3000

인구 검색 >> 스위스

스위스 나라는 없습니다.

인구 검색 >> 그만
```
```java
//Answer
package Answer_1223;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Scanner;
import java.util.Set;

public class Answer_01 {

	public static void main(String[] args) {
	
		CountryInfo info = new CountryInfo();
		info.scInput();
		}
	}

class CountryInfo {
	private String country;
	private long population;
	
	HashMap<String, Long> map = new HashMap<>();
	
	Scanner sc = new Scanner(System.in);
	
	public void scInput() {
		
		System.out.println("나라 이름과 인구를 입력하세요.(예: Korea 5000)");
		
		while(true) {
			System.out.println("나라 이름, 인구>> ");
			
			country = sc.next();
			
			if(country.equals("그만")) { // 여기서 그만을 해줘야 나라만 받고 while문 빠져나가는것! 순서!!!
				break;
			}
			
			population = sc.nextLong();

			map.put(country, population); 
			// 어차피 HashMap을 최상위로 올려놨으니까 여기서 입력한걸 다 담으면 밑에서 담겨있는 map을 사용할수있음  
		}

		while(true) {
			System.out.println("인구 검색>> "); 
			
			country = sc.next();
			
			System.out.println(map.get(country).toString()); 
			/*country아닌 sc.next를 넣으면 문자입력받길 또 기다리는 상태가 되는것 
			 입력한걸 country에 넣었으니까 country를 넣어서 출력하면 입력한 나라의 값이 나옴*/			
			if(country.equals("스위스")) {
				System.out.println("스위스 나라는 없습니다.");
			}
			if(country.equals("그만")) {
				break;
			}
		}
	}
}
```

# 2. 아래를 프로그래밍 하시오
```
하나의 학생 정보를 나타내는 Student 클래스에는 이름, 학과, 학번, 학점 평균을 저장하는 필드가 있다.

(1) 학생마다 Student 객체를 생성하고 4명의 학생 정보를 ArrayList<Student> 컬렉션에 저장한 후에, 
ArrayList<Student>의 모든 학생(4명) 정보를 출력하고 학생 이름을 입력받아 
해당 학생의 학점 평균을 출력하는 프로그램을 작성하라.

학생 이름, 학과, 학번, 학점평균 입력하세요.

>> 황기태, 모바일, 1, 4.1

>> 이재문, 안드로이드, 2, 3.9

>> 김남윤, 웹공학, 3, 3.5

>> 최찬미, 빅데이터, 4, 4.25

----------------------------------

이름: 황기태

학과: 모바일

학번: 1

학점평균: 4.1

----------------------------------

이름: 이재문

학과: 안드로이드

학번: 2

학점평균: 3.9

----------------------------------

이름: 김남윤

학과: 웹공학

학번: 3

학점평균: 3.5

----------------------------------

이름: 최찬미

학과: 빅데이터

학번: 4

학점평균: 4.25

----------------------------------

학생 이름 >> 최찬미

최찬미, 빅데이터, 4, 4.25

학생 이름 >> 이재문

이재문, 안드로이드, 2, 3.9

학생 이름 >> 그만
```
```java
//Answer
package Answer_1223;

import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class Answer_02 {
    public static void main(String[] args) {
      
        List<Student> studentList = new ArrayList<>();
        Student student = new Student();

        Scanner sc = new Scanner(System.in);
        System.out.println("학생 이름, 학과, 학번, 학점평균 입력하세요");
        
        for (int i = 0; i < 4; i++) { // 담는 과정
        	
            System.out.println(">> ");
            
            String name = sc.next();
            String major = sc.next();
            int stN = sc.nextInt();
            double grade = sc.nextDouble();

            studentList.add(new Student(name, major, stN, grade));
			/* 담아야하는데...흠... 아.. 변수명이 같아서 ㅎㅎ..헷갈림 도랏..여기서 막혔는데..
          * 스캐너로 넣은 값들이 생성자값으로 들어가는 것  */
        }

        for (int i = 0; i < studentList.size(); i++) {
            student = studentList.get(i);
            student.output();
        }

        while(true) {
            System.out.println("학생 이름 =: ");
            String name = sc.next();
            
            if (name.equals("그만")) {
                break;
            }

            for (Student s : studentList) {
                if (name.equals(s.getName())) {
                    System.out.println(s.toString());
                }
            }
        }
    }
}

class Student { //너무 길어져서 일단 get 함수만 넣어놓음
    private String name;
    private String major;
    private int stN;
    private double grade;

    public String getName() {
        return name;
    }

    public String getMajor() {
        return major;
    }

    public int getStN() {
        return stN;
    }

    public double getGrade() {
        return grade;
    }
	
    public Student() {
    	
    }

    public Student(String name, String major, int stN, double grade) {
        this.name = name;
        this.major = major;
        this.stN = stN;
        this.grade = grade;
    }

    public String toString() {
        return "이름: " + this.name + "\n" + "학과: " + this.major + "\n" + "학번: "
                + this.stN + "\n" + "학점평균: " + this.grade;
    }

    public void output() {
        System.out.println("-------------------");
        System.out.println("이름: " + this.name);
        System.out.println("학과: " + this.major);
        System.out.println("학번: " + this.stN);
        System.out.println("학점 평균: " + this.grade);
    }
}
```
```java
// Answer(teacher)
package Answer_1223;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.Iterator;
import java.util.List;
import java.util.Scanner;
import java.util.StringTokenizer;

public class Answer_02 {
	final static int STUDENT_NUM = 4; // 다른 포인트.1 ( 어차피 4명니까 나중에 숫자 늘어나면 여기서 바꿀 수 있게)

	public static void main(String[] args) {

		Student student = new Student();

		Scanner sc = new Scanner(System.in);
		ArrayList<Student> studentList = new ArrayList<>();

		System.out.println("학생 이름, 학과, 학번, 학점평균 입력하세요");
		try {
			for (int i = 0; i < STUDENT_NUM; i++) { // 담는 과정

				System.out.print(">> "); // 다른 포인트2.print','단위로 받아야해서
				String text = sc.nextLine(); // 줄 전체로 받아들임

				StringTokenizer st = new StringTokenizer(text, ","); // 다른 포인트3. ',' 단위로
				String name = st.nextToken().trim(); // trim 공백제거
				String major = st.nextToken().trim();
				String stN = st.nextToken().trim();
				double grade = Double.parseDouble(st.nextToken().trim()); // 더블을 문자형으로

				studentList.add(new Student(name, major, stN, grade));
			}

			for (int i = 0; i < studentList.size(); i++) {
				student = studentList.get(i);
				student.output();
			}

			while (true) { // 검색하는 방법1. binarySearch 사용 2. for문 사용
				System.out.println("학생 이름 =: ");
				String name = sc.next();

				if (name.equals("그만")) {
					break;
				}

				for (Student s : studentList) { // 다른 포인트4. toString이 필요없네..
					if (name.equals(s.getName().trim())) {
						// 유저가 공백을 넣을수도 있으니까 공백을 넣으면 공백을 제거해라 : trim()
						System.out.println(s.toString() + ",");
						System.out.println(s.getName() + ",");
						System.out.println(s.getMajor() + ",");
						System.out.println(s.getStN() + ",");
						System.out.println(s.getGrade() + ",");// "," 넣워줘야됨(넣어서 출력하라고 했으니까)
					}
				}

			}
		} catch (Exception e) { // 다른 포인트5. 에러처리
			e.printStackTrace();
			System.out.println("프로그램이 에러입니다");

		}
		System.out.println("프로그램 종료되었습니다");
		sc.close();
	}
}
// key 이름 나머지가 객체에서 받으면될듯

class Student { // 너무 길어져서 일단 get 함수만 넣어놓음
	private String name, major, stN; // 학번이나 전화번호는 string으로 하는게 더 좋음
	private double grade;

	public String getName() {
		return name;
	}

	public String getMajor() {
		return major;
	}

	public String getStN() {
		return stN;
	}

	public double getGrade() {
		return grade;
	}

	public Student() {

	}

	public Student(String name, String major, String stN, double grade) {
		this.name = name;
		this.major = major;
		this.stN = stN;
		this.grade = grade;
	}

	public void output() {
		System.out.println("-------------------");
		System.out.println("이름: " + this.name);
		System.out.println("학과: " + this.major);
		System.out.println("학번: " + this.stN);
		System.out.println("학점 평균: " + this.grade);
	}
}
```
# 3. 위와 연관된 문제입니다
```
ArrayList<Student> 대신, HashMap<String, Studnet> 해시맵을 이용하여 다시 작성하라. 
해시맵에서 키는 학생 이름으로 한다.
```
```java
// Answer(teacher)
package Answer_1223;

import java.util.HashMap;
import java.util.Iterator;
import java.util.Scanner;
import java.util.Set;
import java.util.StringTokenizer;

public class Answer_03 {

	final static int STUDENT_NUM = 4;

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		try {
			HashMap<String, Student> stuList = new HashMap<>();

			System.out.println("학생 이름, 학과, 학번, 학점 평균을 입력하세요.");

			for (int i = 0; i < STUDENT_NUM; i++) {
				System.out.print(">> ");
				String text = sc.nextLine();

				StringTokenizer st = new StringTokenizer(text, ",");
				String name = st.nextToken().trim();
				String major = st.nextToken().trim();
				String sNumber = st.nextToken().trim();
				double grade = Double.parseDouble(st.nextToken().trim());

				Student student = new Student(name, major, sNumber, grade);
				stuList.put(sNumber, student);
			}

			Set<String> key = stuList.keySet(); // 해시맵 stuList에 있는 모든 키 Set 컬렉션으로 리턴
			Iterator<String> it = key.iterator(); // Set을 순차검색하는 이터레이터 리턴
			
			while (it.hasNext()) {
				String sNum = it.next();
				Student student = stuList.get(sNum); // 학번을 키로 하여 객체를 얻는다.(이름으로 하면 중복될수있으니까)
				System.out.println("---------------------------");
				System.out.println("이름:" + student.getName());
				System.out.println("학과:" + student.getMajor());
				System.out.println("학번:" + student.getsNumber());
				System.out.println("학점평균:" + student.getGrade());
				System.out.println("---------------------------");
			}

			while (true) {
				System.out.print("학생 번호>> ");
				String name = sc.nextLine();

				if (name.equals("그만"))
					break;
				Student student = stuList.get(name); // 해시맵에서 학번을 키로 검색

				if (student == null) { // 학번이 해시맵에 없다면
					System.out.println(name + " 학생 없습니다.");
				} else { // 해시맵에서 검색된 Student 객체
					System.out.print(student.getName() + ", ");
					System.out.print(student.getMajor() + ", ");
					System.out.print(student.getsNumber() + ", ");
					System.out.println(student.getGrade());
				}

			}

		} catch (Exception e) {
			e.printStackTrace();
			System.out.println("프로그램이 에러 입니다.");
		}

		System.out.println("프로그램이 종료 되었습니다.");
		sc.close();

	}

}

class Student {
	private String name, major, sNumber;
	private double grade;

	public Student(String name, String major, String sNumber, double grade) {
		this.name = name;
		this.major = major;
		this.sNumber = sNumber;
		this.grade = grade;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getMajor() {
		return major;
	}

	public void setMajor(String major) {
		this.major = major;
	}

	public String getsNumber() {
		return sNumber;
	}

	public void setsNumber(String sNumber) {
		this.sNumber = sNumber;
	}

	public double getGrade() {
		return grade;
	}

	public void setGrade(double grade) {
		this.grade = grade;
	}
}
```

# 4. 아래를 프로그래밍하시오 (Answer22_6번과 같은 문제)

```java
도시 이름, 위도, 경도 정보를 가진 Location 클래스를 작성하고
도시 이름을 '키'로 하는 HashMap<String, Location> 컬렉션을 만들고
사용자로부터 입력 받아 4개의 도시를 저장하라. 
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

# 5. 아래를 프로그래밍하시오
```
이름과 학점(4.5만점)을 5개 입력받아 해시맵에 저장하고
장학생 선발 기준을 입력 받아 장학생 명단을 출력하라.

미래장학금관리시스템입니다.

이름과 학점 >> 적당히 3.1

이름과 학점 >> 나탈락 2.4

이름과 학점 >> 최고조 4.3

이름과 학점 >> 상당히 3.9

이름과 학점 >> 고득점 4.0

장학생 선발 학점 기준 입력 >> 3.2

장학생 명단 : 최고조 상당히 고득점 

[Hint] HashMap의 전체 요소를 검색하여 학점이 3.2 이상인 학생을 알아내야 한다.
```
```java
//Answer
package Answer_1223;

import java.util.HashMap;
import java.util.Scanner;
import java.util.Set;

public class Answer_05 {
	public static void main(String[] args) {
		
		HashMap<String, Double> list = new HashMap<>();
		Scanner sc =  new Scanner(System.in);
		System.out.println("미래 장학금 관리 시스템입니다.");
		
		while(list.size() < 5) {
		System.out.println("이름과 학점 >> ");
		String name = sc.next();
		double grade = sc.nextDouble();
		
		list.put(name, grade);
		}
		Set<String> set = list.keySet(); // set에 list의 key 값을 담음
		
		System.out.println("장학생 선발 학점 기준 입력>> ");
		double standard =sc.nextDouble();
		// 3.2 이상이면 그 학생들 출력
		
		System.out.print("장학생 명단: ");

		for(String k : set) {
			if(standard <= list.get(k)) {
				System.out.println("\t" + k);
			}
		}
	}
}
```

# 6. 큐와 스택에 대하여 설명하시오 (필수)
 - Queue (FIFO :Firset In First Out) <br>
 : 공간이 막혀있지 않고 뻥- 뚫려있는 구조

 ![큐](https://user-images.githubusercontent.com/74290204/102997322-28e5db80-4568-11eb-8f57-67cd5463b5f4.PNG)

 - Stack (FILO: First In Last Out) <br>
 : 밑에 막힌 구조로 먼저 들어간 값이 제일 마지막에 나오는 구조

 ![스택](https://user-images.githubusercontent.com/74290204/102997330-2d11f900-4568-11eb-9ded-f94d964e5168.PNG)


# 7. Map에 대해 설명하시오 
: Map< K, V >은 Set 성질 + List 성질의 Collection  <br>
- Key : Value에 들어가는 데이터를 구분짓는 요소(현관문 열쇠같은 존재) <br> 요소를 구분지어야 하기 때문에 Key는 중복 허용 X , Value는 가능
- Map은 선언시 TreeMap , HashMap , LinkedMap, HashTable으로 선언 가능 

# 8. 아래의 TreeMap의 Value를 확인 하기 위한 소스를 짜시오 (필수)
```java
 TreeMap<Integer, String> map = new TreeMap<>();
   map.put(45, "Brown");
   map.put(37, "James");
   map.put(23, "Martin");
```
```java
// Answer
package Answer_1223;

import java.util.Set;
import java.util.TreeMap;

public class Answer_08 {
	public static void main(String[] args) {

		TreeMap<Integer, String> map = new TreeMap<>();
		map.put(45, "Brown");
		map.put(37, "James");
		map.put(23, "Martin");
		
		Set<Integer> set = map.keySet();
		
		for(Integer e : set) {
			System.out.println(map.get(e));
		}
	}
}
```

# 9. 아래의 IntegerComparator를 내림차순 정렬이 되게끔 구현하시오
```java
class ComparatorIntegerDemo {
    public static void main(String[] args) {
        TreeSet<Integer> tr = new TreeSet<>(new IntegerComparator());
        tr.add(30);
        tr.add(10);    
        tr.add(20);        
        System.out.println(tr);	
    }
}
```
```java
// Answer
package Answer_1223;

import java.util.Comparator;
import java.util.TreeSet;

public class Answer_09 {

	public static void main(String[] args) {
		//아래의 IntegerComparator를 내림차순 정렬이 되게끔 구현하시오
		TreeSet<Integer> tr = new TreeSet<>(new IntegerComparator());
		tr.add(30);
		tr.add(10);
		tr.add(20);
		System.out.println(tr); // println에서 자동으로 정렬 호출

	}
}

class IntegerComparator implements Comparator<Integer> {
	
	@Override
	public int compare(Integer n1, Integer n2) {
		return n2.intValue() - n1.intValue();
	}
}
```

# 10. Objects.hash의 용도와 사용법은?
: 주어진 파라미터 값을 이용해 hashCode를 생성하는 역할의 함수 <br> 
hash알고리즘은 자기가 가지고 있는 객체를 넣으면 Objects가 알아서 그 객체의 정보를 토대로 hashCode를 만들어 줌 <br>
hashCode에 대해서 사용이 어려울 때 사용하면 되는 함수 (중복될 수 있는 확률 확실히 더 줄일 수 있음!) : 나중에 실무에서 Spring 롬복 사용
Object와는 다름!
