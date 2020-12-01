# 메소드(=함수 =function)
## 정의 : 함수를 만드는 것 / call = invoke : 함수 호출
### public static void main(String[ ] args) { }

* main ; 함수명
* void ; 리턴타입 존재하지않음(값을 반환하지않는다)
* void main(String[ ] args) { } ; 함수범위
* ( ) ; 대괄호 있으면 함수
* { } ; 함수 만드는 부분에는 중괄호 존재하고 실행(구현)되는 부분있음 / 호출 하는 부분에서는 값만 적어주고 구현하는 부분에서는 변수 선언한다(메모리할당)
* String[] args ; 매개변수 = 파라미터 = 인자 (여러개여도 상관없고 ★ 매개변수 자리에는 변수선언/초기화X★)
* 함수 안에 함수 존재(X) , class안에 존재 , 파라미터 안에 바로 변수 선언하면서 값 할당(X)
```java
public static void main(String[] args) {  // main메소드는 프로그램의 시작이자 끝
	System.out.println("프로그램의 시작");
	hiEveryone(12); // 함수 호출 , 함수 실행 , '12'라는 값(value)할당
	hiEveryone(13);
	System.out.println("프로그램의 끝");
	}
	
public static void hiEveryone(int age) {  // 매개변수 자리에 int age 변수선언 , 함수만든것
	System.out.println("좋은 아침입니다");
	System.out.println("제 나이는 " + age + "세 입니다");
}

/* 값들이 한번 실행되고(호출한번하고) 나면 방 안에 값 사라짐 
그래서 호출할때 값 여러개(hiEveryone(12); hiEveryone(13);)를 호출가능!!	
======================================================================
▶ 
프로그램의 시작
좋은 아침입니다
제 나이는 12세 입니다
좋은 아침입니다
제 나이는 13세 입니다
프로그램의 끝
```
* 메소드는 입력만 있을수도 있고 없을수도 있음
```java
pubilc static void byEveryone( ) {
	System.out.println("다음에 뵙겠습니다.");
}	// 이게 입력 비워두는것! ( ) 안에 비워두면 입력이 없는것 
```
* return : 값의 반환(메소드 호출문을 그 결과로 대체한다)과 동시에 종료
```java
public static void main(String[] args) {
	square(2.5, 3.5);	
}
	
public static double square(double width, double height) {
		
	double squareArea = width*height;
	System.out.println(squareArea);
	return squareArea; // return값은 함수의 데이터타입과 일치해야함
```
		

## 함수를 써야하는 이유와 언제 쓰는지에 대한 궁금증

* 소스의 길이가 엄청 길어짐 (같은 코드를 반복) → 똑같은 소스 코드 2번 이상 들어갈때 함수 사용(;똑같은 기능이 2번 이상 중복될때)
* 하나의 함수는 한개의 기능을 수행해야함  

<br>

#  CLASS : 데이터 + 메소드 
* class 이름의 첫 글자 '대문자' 사용
* 함수명의 첫 글자는 '소문자' / 합성어의 첫 글자는 '대문자' ; ex) powerOfTwo
* class와 인스턴스의 차이 ; class = 변수와 상수 함수를 모아놓은 것

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
<br>

# 객체
## null / String / 생성자 / Scanner

* 기본 8개의 데이터 타입(형) = 프리미티브 타입(primitive type) : boolean~double / 9개의 데이터 타입 = 프리미티브 타입 + 참조형(클래스명)
* 참조형 ; 클래스명을 데이터타입으로 하고 주소를 담는 형 
* new ; 객체 생성 키워드 
* 객체생성(=인스턴스 생성) ; .clss를 메모리로 올리는 행위
* null은 인스턴스의 관계를 끊어버림
* ref = null 만 선언해주면 오류남 ** 꼭 null을 체크하는 조건문을 줘야하고 null 은 참조형에서만 가능
* String :문자열이 String 인스턴스 클래스를 참조하는것 (따라서 우리가 보기에는 문자열을 나타내지만 함수를 보면 값으로 구성되어있다)

```java
if (ref == null) { return;}  //오류를 피하기 위한 조건문

int a = null // 이렇게 사용할 수 없음
```
* String ; 문자열을 다루는 class
```java
String name = "kim" ;
```

* 생성자 : class와 이름이 같은 함수 / 리턴값 (X) 
  * 리턴값이 없기 때문에 리턴타입 표시 X
```java
BankAccount = new BankAccount( ); // new 뒤에 붙는 걸 생성자라 부름
```

* ( ) 안에 함수인데 인자나 함수를 따로 만들지 않고 호출하지 않으면 컴파일러에서 자동으로 넣어줌 '디폴트 생성자'
  * 디폴트 생성자는 개발자가 생성자를 한개라도 만들어놓으면 디폴트를 넣지 않기 때문에 오류 발생! 그래서 개발자가 디폴트 생성자를 따로 만들어줘야함 

* 생성자의 용도 
  * 값들의 초기화

* 생성자는 처음 값을 넣을때 
set은 값을 변경시킬때

```java
Scanner scanner = new Scanner(System.in); // (System.in) 키보드
int num = scanner.nextInt(); //실행 멈춤 콘솔창에 입력해주길 기다리는 상태
System.out.println("입력한 숫자는" + num); // 콘솔에 값을 입력해주고 나서 출력
```
* scanner 받을때 데이터 타입 맞춰줘야함
```java
String name = scanner.next(); // 
String name = scanner.nextLine();
int age = scanner.nextInt(); // int로 변수선언 하니까 scanner 뒤도 int
```
* scanner를 통한 반복문 활용 (yes / no)
```java
public static void main(String[] args{
  Scanner scanner;

  while(true) {
    scanner = new sacnner(System.in);
    System.out.print("이름: ");
    String name = sanner.next();

    System.out.print("나이: ");
    int age = scanner.nexyInt();

    Info info = new info (name, age); //
    info.show();

    System.out.println("계속하시겠습니까? (y/n)");

    char c = scanner.next().chaeAt(0);

    if(c == 'y' || c == 'Y') {
      continue;
    }
    else {
      break;
    }
    scanner close();
  
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

# 재귀함수
* 함수안에서 자신을 호출하는 것
```java
public static int factorial(int n) {
	if(n ==1)
		return 1;   //조건 없으면 무한루프 돎
	else
		return n * factorial(n-1);
```
* ex_재귀함수로 2ⁿ 구하기
```java
public static int powerOfTwo(int n) {
	if(n == 0) { 
    return 1;
  }
	
	else {
    return 2 * powerOfTwo(n-1);
  }
  ```
		
