# 메소드
## public static void main(String[] args) { }

* main ; 함수명
* void ; 리턴타입 존재하지않음(값을 반환하지않는다)
* void main(String[] args) { } ; 함수범위
* ( ) ; 대괄호 있으면 함수
* { } ; 함수 만두는 부분에는 중괄호 존재하고 실행(구현)되는 부분있음 / 호출 하는 부분에서는 값만 적어주고 구현하는 부분에서는 변수 선언한다(메모리할당)
* 매개변수 = 파라미터 = 인자
* 함수 안에 함수 존재(X) , class안에 존재 , 파라미터 안에 바로 변수선언하면서 값 할당(X)

## 함수를 써야하는 이유와 언제 쓰는지에 대한 궁금증

* 똑같은 소스 코드 2번 이상 들어갈때 함수 사용(;똑같은 기능이 2번 이상 중복될때)
* 하나의 함수는 한개의 기능을 수행해야함  


<br>


#  CLASS
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

* 기본 8개의 데이터 타입(형) = 프리미티브 타입(primitive type) : boolean~double
9개의 데이터 타입 = 
프리미티브 타입 + 참조형(클래스명)
* 참조형 ; 클래스명을 데이터타입으로 하고 주소를 담는 형 
* new ; 객체 생성 키워드 
* 객체생성(=인스턴스 생성) ; .clss를 메모리로 올리는 행위
* null은 인스턴스의 관계를 끊어버림
* ref = null 만 선언해주면 오류남 ** 꼭 null을 체크하는 조건문을 줘야하고 null 은 참조형에서만 가능

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

# 반복&조건문
## for문 / 이중for문/ continue / while

* { } 영역 확인: 위치에 따라 결과 달라짐
  * for_example

  
```java
// 1부터 100까지의 짝수합과 홀수합 구하기
 int a = 0;
 int b= 0;

 for(int i = 0, i<= 100, i++){
   if(i % 2 == 0){
     a = a + i;
   }
   else {
     b = b + i;
   }
 }
 System.out.println(a);
 System.out.println(b);
```

```java
/*1부터 100까지의 수 중에서 
짝수의 갯수와 홀수의 갯수 구하기*/
int evenCount = 0;
int oddCount = 0;

for(int i = 0, i <= 100, i++){
  if(i % 2 == 0){
    evenCount++;
  }
  else{
    oddCount++;
  }
System.out.println(evenCount);
System.out.println(oddCount);  
}
```

* 무한루프 주의하기 <br>
(breakf를 통해 무한루프 빠져나오기 & break는 조건문과 전혀 관련없음)
  * while_example
```java
int num = 0;
int count = 0;

while((num++) < 100){
  if((num % 5 != 0) || (num % 7 != 0))
  continue;
	count++;
		System.out.println(num);
} /* num++ 첫번째 실행이 끝난 후 증감 
따라서 0 < 100 하고 증감 if문에는 증감되고 들어가서 1로 조건문 비교 while과 for문은 좀 다름*/
```

```java
/* 6의배수이면 14의 배수인 가장 작은 
자연수 찾는 반복문 */

int num = 1;
		
while(true) {
	if(((num % 6) == 0) && ((num % 14) == 0))
		break;
	num++;
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
		
