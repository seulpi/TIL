# 1. 아래를 프로그래밍하시오 (다시봐야함// France를 뭘로 받아야 같이 출력되는지 구현못했음)
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
package Answer_1223;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Scanner;
import java.util.Set;

public class Answer_01 {

	public static void main(String[] args) {
		/*
		 * "그만"이 입력될 때까지 나라 이름과 인구를 입력받아 저장하고, 
		 * 다시 나라 이름을 입력받아 인구를 출력하는 프로그램을 작성하라. 다음
		 * 해시맵을 이용하라.
		 */
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
package Answer_1223;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.Iterator;
import java.util.List;
import java.util.Scanner;

public class Answer_02 {

	public static void main(String[] args) {
		/*
		 * 하나의 학생 정보를 나타내는 Student 클래스에는 이름, 학과, 학번, 학점 평균을 저장하는 필드가 있다 
		 * (1) 학생마다 Student 객체를 생성하고 4명의 학생 정보를 ArrayList<Student> 컬렉션에 저장한 후에, 
		 * ArrayList<Student>의 모든 학생(4명) 정보를 출력하고 학생 이름을 입력받아 
		 * 해당 학생의 학점 평균을 출력하는 프로그램을 작성하라.
		 * 
		 * 학생 이름, 학과, 학번, 학점평균 입력하세요.
		 * 
		 * >> 황기태, 모바일, 1, 4.1
		 * 
		 * >> 이재문, 안드로이드, 2, 3.9
		 * 
		 * >> 김남윤, 웹공학, 3, 3.5
		 * 
		 * >> 최찬미, 빅데이터, 4, 4.25
		 */
		
		Student student = new Student();
		System.out.println("학생 이름, 학과, 학번, 학점평균 입력하세요");
		student.scInput();
		student.scOutput();

	}
} // key 이름 나머지가 객체에서 받으면될듯
class Student {
	private String name;
	private String major;
	private int stN;
	private double grade;
	
	List<Student> st = new ArrayList<>();

	public void scInput() {
		
		for(int i = 0; i < 4; i++) {
			Scanner sc = new Scanner(System.in);
			System.out.println(">> ");
			
			name = sc.next();
			major = sc.next();
			stN = sc.nextInt();
			grade = sc.nextDouble(); // 담아야하는데...흠...
	
		}
	}
	
	public String toString() {
		return "이름: " + name + "\n" + "학과: " + major + "\n" + "학번: " + stN + "\n" + "학점평균: " + grade;
	}
	
	public void scOutput() {
		for(int i = 0; i < 4; i++) {
			System.out.println(st.get(i).toString());
		}
	}
}
```
