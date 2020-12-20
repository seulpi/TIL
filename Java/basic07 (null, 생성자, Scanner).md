# null / String 
## @ null
- null은 인스턴스와의 관계를 끊는다(주소를 없애고 null값을 넣는다) <br>
☞ 참조변수의 초기화 <br> + 가비지컬렉션으로 하여금 메모리를 처리해도 좋다(객체정리해도됨)라는 하나의 표시 = 메모리 청소하세요!
<br> 청소하는 시기는 알 수 없고(JNM만 알아) 'null' 한다고해서 바로 없어지는 건 아님!

```java
Circle circle = null;

```
▶ 위 글 예시 그림<br>

![null01](https://user-images.githubusercontent.com/74290204/100847085-a9ae3c00-34c2-11eb-9321-784848ad097f.png)

- null은 참조형만 가능
```java
int a = null; // error
```

- null check (오류를 피하기위한 조건)
```java
if (num = null) // num이 참조하는 인스턴스가 없다면(; "null check") → 실무에선 체크 꼭 해줘야한다 (체크없다면 오류로 이어지기 떄문)

Rectangle rec = null;
rec.getHeight(); // 객체가 없는 상태에서 함수호출 화면상으로는 error 일어나지 않지만 실행하면 → NullPointerException (오류) 발생\

▼  위와 같은 오류피하는 방법
if(rec != null) { // 이게 바로 null check
	rec.getHeight();
}
```
**★ 함수내에서 오는 매개변수는 무조건 null check ★**
<br>

## @ String class

: 문자열을 다루는 class

```java
String str1 = "Happy"; // String이라는 참조형에 str1에 Happy를 가리키는 주소를 담는다

String str1 = "Happy";
String str2 = "Birthday";
	
System.out.println(str1 + " " + str2);
============================================== 
▶ Happy Birthday

```
# 생성자
## - 정의 
생성자(=생성자함수) : 초기화를 대신할 수 있는 메소드이며 class이름과 똑같은 함수
- 특징 
1. 리턴타입(반환형) X 
   - 함수에는 기본적으로 리턴타입이 존재하는데 생성자함수는 리턴타입이 없다는 것은 용도 (생성자 함수의 용도는 초기화의 용도)의 제한을 설정해두었기 때문에 리턴타입이 없다 <br> 
   (리턴타입이 없기때문에 리턴값도 없다)
2. 생성자함수명이 class와 이름이 같음
3. 용도는 초기화 + 객체생성 (객체생성할때 반드시 호출하게 되어있음)


```java
BankAccount = new BankAccount( ); 
				// ↑ 생성자 함수
```

## - 디폴트 생성자

- 인자가 1개도 없고 로직(함수)를 하나도 만들지 않은상태 [껍데기인상태]에서 생성자함수를 호출하게되면 컴파일러가 자동으로 함수를 생성하게 되는 걸 **디폴트 생성자** 라 한다 

- 디폴트 생성자는 개발자가 생성자를 한개라도 만들어놓으면 디폴트를 넣지 않기 때문에 오류 발생! 그래서 개발자가 디폴트 생성자를 따로 만들어줘야함 

```java
>> 메인 메소드

public class TestMain {
	
	public static void main(String[] args) {

		Person kim = new Person("김철수", 27);
		kim.printPerson(); 
	
	}
}
=========================================================

>> Preson 클래스 

public class Person {
	
	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}

	String name;
	int age;
	
	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}
	
	public void printPerson() {
		System.out.println("이름: " + name + '\n' + "나이: " + age);	
	} //이렇게 쓸수도 있지만 get&set함수가 있으니까 밑에도 가능
	 public void printPerson() {
		System.out.println("이름: " + getName() + '\n' + "나이: " + getAge())	

}
```
# Scanner
## - 문법
```java
Scanner scanner = new Scanner(System.in); // (System.in) 키보드
int num = scanner.nextInt(); //실행 멈춤 콘솔창에서 키도르로 입력해주길 기다리는 상태

System.out.println("입력한 숫자는" + num); // 콘솔에 값을 입력해주고 나서 출력

scanner.close(); //스캐너 종료
```
* scanner 받을때 데이터 타입 맞춰줘야함
```java
String name = scanner.next(); // 
String name = scanner.nextLine();
int age = scanner.nextInt(); // int로 변수선언 하니까 scanner 뒤도 int
```
* scanner를 통한 반복문 활용 (yes / no)
```java
>> 점수 클래스를 만들고 스캐너를 통해 값을 입력하고 평균을 내시오(단, 계속할지 반복문사용)

public static void main(String[] args) {
		
	Scanner scanner = null;
		
	while(true) {
		scanner = new Scanner(System.in);
		int math, sien, eng;
			
		System.out.println("국어 과학 영어를 입력하세요");
			
		math = scanner.nextInt();
		sien = scanner.nextInt();
		eng = scanner.nextInt();
			
		Grade me = new Grade(math, sien, eng);
		System.out.println("평균은: " + me.average());
		
		System.out.println("계속하시겠습니까? (Y/N)");
			
// 방법 1
		String yesOrNo = scanner.next();
			  
		 if(yesOrNo == "Y" || yesOrNo == "Y") { /* 이거 안돌아감 why? String은 주소를 참조하는데 문자열이 들어가는게 말이안됨 
따라서 함수 사용해야함 if(yesOrNo.equals("Y") || yesOrNo.equals("y")) 이렇게함수로 비교를 해줘야함 */
			continue; 
		   } else { 
			 break;
		   }
		    	
// 방법 2
		char c = scanner.next().charAt(0); // 문자 하나만 받을거니까 char를 사용한것 charAt(0)는 내가 Yes라고 쓰면 첫번째 Y를 넘겨주는거고 charAt(1)하면 e를 넘겨주는것
		    
		if(c == 'y' || c == 'Y') {
		    continue;
		} else {
		    break;
		}
	}

=========================================================

>>위 스캐너 호출 사용한 Grade 클래스
public class Grade {
	
	int math;
	int sien;
	int eng;
	
	public Grade(int math, int sien, int eng) {
		this.math = math;
		this.sien = sien;
		this.eng = eng;	
	}
	
	public Grade() {
		
	}

	public double average( ) {
		
		int total = math + sien + eng;
		double avg = total / 3.0;
	
		return avg;			
	}	
}
```

* 화폐 단위 개수 구하기
```java
public class Bill {
	
	int m500,m100,m50,m10,m5,m1;
	int temp;
	int money;
	
	public Bill(int money) {
		this.money = money;
	}
	
	public void printResult(){
		
		m500 = m500 / 50000;
		temp = money - (m500 * 50000);
		
		m100 = temp / 10000;
		temp = temp - (m100 * 10000);
		
		m50 = temp / 5000;
		temp = temp - (m50 * 5000);
		
		m10 = temp / 1000;
		temp = temp - (m10 * 1000);
		
		m5 = temp / 500;
		temp = temp - (m5 * 500);
		
		m1 = temp / 100;
		temp = temp - (m1 * 100);
		
		System.out.println("오만원권: " + m500 + "장");
		System.out.println("만원권: " + m100 + "장");
		System.out.println("오천원권: " + m50 + "장");
		System.out.println("천원권: " + m10 + "장");
		System.out.println("오백원: " + m5 + "장");
		System.out.println("백원: " + m1 + "장");
	}
	

public class Main02 {

	public static void main(String[] args) {
		
		Scanner scanner;
		while(true) {
			scanner = new Scanner(System.in);
			System.out.print("액수를 입력하세요: ");
			int money = scanner.nextInt();
			
			Bill moneyPrint = new Bill(money);
			moneyPrint.printResult();
			
			System.out.print("계속하시겠습니까? (y/n) ");
			System.out.println();
			
			char c = scanner.next().charAt(0);
			
			if(c == 'y' || c == 'Y')
				continue;
			else
				break;
		}
		scanner.close();
		
		}
```

### EX
```java
// 1~100까지 소수 구하기
public static void main(String[] args){
  for(int i = 0, i <= 100, i++>){
    if(isPrimeNumber(i)) // 
      System.out.println(i);
  }
}
public static boolean isPrimeNumber  (int num) {          // boolean 타입이 오면 함수명 is 붙이고 시작
  if(num == 1) {
    return false;
  }
  for(int j = 2, 2 < num; j++) {
    if (num % j == 0) {
      return false;
    }
    return true;  // return 종료의 의미
  }
  
}
```
