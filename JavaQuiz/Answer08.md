# 1. .생성자란 무엇인가?
생성자란 초기화를 하기 위한 함수로 클래스와 이름이 같은 함수 <br>
생성자는 **반드시** 클래스의 이름가 동일해야한다 <br>
생성자는 리턴타입이 존재하지않는데 그 이유는 생성자는 용도의 제한을 두기 때문이다 (생성자의 용도 1. 객체생성 2. 초기화)
```java
Person kim = new Person();
                // ↑ 생성자
```

<br>

# 2. 디폴트 생성자란 무엇인가?

**디폴트 생성자**란 매개변수나 어떠한 정보를 포함하지 않고 있는 함수(빈껍데기 같은 함수) <br>
즉, (매개변수)가 없는 함수를 개발자가 클래스 내부에 만들지 않았을때 main class에서 생성자를 호출하게 되면 생성자를 자동으로 컴파일러가 만들어주는데 이것을 **디폴트 생성자**라 말한다

<br>

# 3. 생성자의 용도에 대하여 설명하시오
생성자의 용도는 객체를 생성할때 사용되고 함수를 초기화하는데 사용된다 (생성자는 반드시 객체를 생성할때 호출하게 되어있다)<br>
앞서 말한듯이 용도에 의해 생성자의 타입이 제한되는데 이것이 바로 생성자에는 리턴타입(반환형)이 존재하지 않는 이유이다

<br>

# 4. null 에 대하여 설명하시오
null은 참조형인 변수가 주소를 참조하는 것을 '끊어버리는 행위'다 <br>
변수의 주소를 참조하지 못하게 함으로써 방을 비워버리고 초기화 시킨다 <br>
null은 참조형에서만 사용가능하다
```java
Circle circle = null;

int a = null; // error
```
▶ 그림 참고

![null01](https://user-images.githubusercontent.com/74290204/100847085-a9ae3c00-34c2-11eb-9321-784848ad097f.png)

# 5. 금일 프로그래밍 했던 문제
## 5-1] 다음 main() 메소드를 실행하였을 때 예시와 같이 출력되도록 TV 클래스를 작성하라
```java
public static void main(String[] args) {
   TV myTV = new TV("LG", 2017, 32); //LG에서 만든 2017년 32인치
   myTV.show();
}

============================================================
▶ LG에서 만든 2017년형 32인치 TV
```
```java
//Answer

public class TV {
		
	String brand;
	int year;
	int inch;
	
	public TV(String brand, int year, int inch) {
		this.brand = brand;
		this.year = year;
		this.inch = inch;
	}
	
	public void show() {
		System.out.println(brand + "에서 만든 " + year + "년형 " + inch +"인치 TV");
	}
	
}
```

## 5-2] 노래 한 곡을 나타내는 Song 클래스를 작성하라
```java
- 노래의 제목을 나타내는 title
- 가수를 나타내는 artist
- 노래가 발표된 연도를 나타내는 year
- 국적을 나타내는 country

또한 Song 클래스에 다음 생성자와 메소드를 작성하라.
- 생성자 2개: 기본 생성자와 매개변수로 모든 필드를 초기화하는 생성자
- 노래 정보를 출력하는 show() 메소드
- main() 메소드에서는 1978년, 스웨덴 국적의 ABBA가 부른 "Dancing Queen"을
song 객체로 생성하고 show()를 이용하여 노래의 정보를 다음과 같이 출력하라
```
```java
//Answer

public class Song {
	String title,artist,country;
	int year;
	
	public Song() {
		
	}
	
	public Song(String title, String artist, String country, int year) {
		this.title = title;
		this.artist = artist;
		this.country = country;
		this.year = year;
	}
	
	public void show() {
		System.out.println(year + "년 " + country + "국적의 " + artist + "가 부른 " + title);
	}

}
```

## 5-2] 아래와 같이 성적을 연속적으로 입력 받고 평균을 내는  프로그램을 작성하시오
```java
국어 영어 수학을 입력하세요!
100 60 70
평균은 76.66666666666667
계속 하시겠습니까
y
국어 영어 수학을 입력하세요!
90 80 70
평균은 80.0
계속 하시겠습니까
n
프로그램 종료 입니다.
==============================================
```
```java
//Answer

public static void main(String[] args) {

		Scanner scanner = null;

		while (true) {
			scanner = new Scanner(System.in);
			int kor, eng, math;

			System.out.println("국어 영어 수학을 입력하세요!");

			kor = scanner.nextInt();
			eng = scanner.nextInt();
			math = scanner.nextInt();

			Grade grade = new Grade(math, kor, eng);
			System.out.println("평균은: " + grade.average());

			System.out.println("계속하시겠습니까? (Y/N)");

			String yesOrNo = scanner.next();
			if (yesOrNo.equals("Y") || yesOrNo.equals("y"))
				continue;
			else
				break;

		}
```

# 6. 아래의 프로그램을 작성하시오
##  화폐 매수 구하기 (반드시 클래스로 작성할것)
```java
출력
---------------------------------
136000
오만원 : 2장
만원 : 3장
오천원 : 1장
천원 : 1장
오백원 : 0개
백원 : 0개
계속 하시겠습니까
y
1456000
오만원 : 29장
만원 : 0장
오천원 : 1장
천원 : 1장
오백원 : 0개
백원 : 0개
계속 하시겠습니까
```
```java
//Answer

>> 클래스

public class Money {
	
	int money;
	int m500, m100, m50, m10, m5, m1;
	int num;
	
	public Money(int money) {
		this.money = money;
		
		m500 = money/50000; 
		num = money - (m500*50000);
		
		m100 = num/10000; 
		num = num - (m100*10000); 
		
		m50 = num/5000; 
		num = num - (m50*5000); 
		
		m10 = num/1000;
		num = num - (m10*1000);
		
		m5 = num/500;
		num = num - (m5*500);
		
		m1 = num/100;
		num = num - (m1*100);
	}
	
	public void moneyNum() {
		System.out.println("오만원: " + m500 + "장" );
		System.out.println("만원: " + m100 + "장" );
		System.out.println("오천원: " + m50 + "장" );
		System.out.println("천원: " + m10 + "장" );
		System.out.println("오백원: " + m5 + "개" );
		System.out.println("백원: " + m1 + "개" );
	}
}
========================================================

>>메인메소드 실행

	Scanner scanner = null; 
		  
	while(true) {
		  
	    System.out.println("얼마를 넣으시겠습니까?"); 
		scanner = new Scanner(System.in); 
		  
		int money = scanner.nextInt();
		  
		Money coin = new Money(money); 
		coin.moneyNum(); System.out.println();
		System.out.println("계속하시겠습니까 ? (Y/N)");
		  
		String yesOrNo = scanner.next(); 
		  
		if(yesOrNo.equals("Y") || yesOrNo.equals("y")) 
		    continue; 
		else 
			 break;
	}
```
# 7. 자바의 명명 규칙에 대하여 설명하시오
- 클래스 
  - 클래스 이름의 '첫'글자는 **대문자**로 작성
- 메소드와 변수 
  - 메소드와 변수의 이름의 '첫'글자는 **소문자**로 작성 <br>
☞ 합성어일 경우 합성어의 첫글자는 대문자로 표기한다
```java
class PersonName
int personName;
```
- 상수
  - 상수는 '모든 글자'를 **대문자**로 표기하되 합성어일 경우 스네이크("_" : camel)로 작성한다
```java
final int PERSON;
final int PERSON_NAME;
```

---
# ▶  java basic07 정리 참고


