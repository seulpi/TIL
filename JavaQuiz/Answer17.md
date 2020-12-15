# 1. Maker 인터페이스에 대하여 설명하시오
: 마킹을 한다해서 마커인터페이스라고 명칭하며 인터페이스의 한 종류이다 <br> 마커인터페이스는 인터페이스와 동일하지만 사실상 어떠한 메소드도 선언하지 않은 인터페이스<br>
(비어있는 빈 깡통의 인터페이스)
- 목적: 클래스 분류를 위해 사용
- 사용: 오버라이딩 할 메소드도 없기때문에 상속을 받아 사용한다
```java
>> EX

interface Upper { }
interface Lower { }
/* 이게 바로 Marker interface 
구현할 내용은 없고 상속하면 클래스를 분류한다 */

▶ 대표적인 Marker interface 는 Serializable, Cloneable 등이 존재 
```
<br>

# 2. 추상클래스에 대하여 설명하시오
: 추상클래스는 abstract가 붙은 클래스 
- 추상클래스는 메소드가 추상메소드이면 클래스 앞에 무조건 abstract를 붙여야 하기 때문에 추상클래스가 생김 <br>
(= 추상메소드가 존재하기때문에 추상클래스가 존재)
- 추상클래스도 추상메소드와 마찬가지로 자손이 구현해라라는 뜻 내포

<br>

# 3. 추상클래스와 인터페이스의 차이는?
1. 공통점 :  구현부분이 없기 때문에 객체생성X / 참조형 변수 선언만 O
2. 차이점 : 추상클래스는 함수 구현까지 가능 
```java
// 최근에 들어서는 차이점의 경계가 애매모호해짐
why? interface에서 static 과 default 가 지원이 되면서 함수 구현도 가능해졌기 때문
```
<br>

# 4. 에러와 예외의 차이는?
- error : 코드상의 문제가 아니라 시스템적상의 문제일때 발생하는 것 <br>
    - EX) 메모리가 가득찼다, 하드디스크에 손상이 왔다 등 
- exception : 코딩상의 문제 (개발자의 로직 오류)
    - EX) 형변환, 스캐너입력시 0을 나눴다 등 
<br>

# 5. unchecked 와 cheked 예외의 차이는?
: 예외(exception) 처리는 두가지의 종류가 있는데 그게 바로 **unchecked exception, checked exception**이다 
- unchecked exception : 예외처리를 해주지 않아도 되는 에러
- checked exception : 예외처리를 해줘야 되는 에러 <br>
▶ 아래 6.그림 참조

<br>

# 6. 예외처리 UML를 그리시오
![예외처리UML](https://user-images.githubusercontent.com/74290204/102187064-da489980-3ef6-11eb-9ebd-92b2deb58829.png)

<br>

# 7. 사칙연산 계산기를 아래의 조건으로 짜시오
## -interface 를 활용할것 <br> -예외처리 메커니즘을 적용할것
```java

public class TranningMain {
	public static void main(String[] args) {
	// 사칙연산 계산기 interface 활용 예외처리 메커니즘
		
		ICalculator calculator = new Calculator();
		
		try {
		calculator.add(5, 7);
		calculator.min(10, 2);
		calculator.mul(8, 4);
		calculator.div(4, 0);
		} 
		catch(ArithmeticException e) {
			e.printStackTrace();
			System.out.println(e.getMessage());
		}
	}
}

interface ICalculator {
	public void add(int x, int y);
	public void min(int x, int y);
	public void mul(int x, int y);
	public void div(int x, int y);
}

class Calculator implements ICalculator {
	int x;
	int y;
	
	@Override
	public void add(int x, int y) {
		System.out.println("두 수의 덧은: " + (x + y));
	}
	@Override
	public void min(int x, int y) {
		System.out.println("두 수의 덧셈: " + (x - y));
	}
	@Override
	public void mul(int x, int y) {
		System.out.println("두 수의 곱셈: " + (x * y));
	}
	@Override
	public void div(int x, int y) {
		System.out.println("두 수의 나눗셈: " + (x / y));
	}
}
```
<br>

# 8. 다음 Stack 인터페이스를 상속받아 실수를 저장하는 <br>StringStack 클래스를 구현하라<br> (구현할수 있도록 할것)   * 수정중 *
```java
interface Stack {
   int length(); // 현재 스택에 저장된 개수 리턴
   int capacity(); // 스택의 전체 저장 가능한 개수 리턴
   String pop(); // 스택의 톱(top)에 실수 저장
   boolean push(String val); // 스택의 톱(top)에 저장된 실수 리턴
}
그리고 다음 실행 사례와 같이 작동하도록 StackApp 클래스에 main() 메소드를 작성하라.

총 스택 저장 공간의 크기 입력 >> 3
문자열 입력 >> hello
문자열 입력 >> sunny
문자열 입력 >> smile
문자열 입력 >> happy
스택이 꽉 차서 푸시 불가!
문자열 입력 >> 그만
스택에 저장된 모든 문자열 팝 : smile sunny hello 
```
```java
//Answer
import java.util.Scanner;

public class TranningMain {

	public static void main(String[] args) {
		
		Scanner sc = new Scanner(System.in);
		System.out.print("총 Stack 저장 공간의 크기 입력 >> ");
		
		StackApp app = new StackApp();
		
		int arRoomCount;
		arRoomCount = app.length();
		System.out.println(arRoomCount);
		
		while(true) {

			System.out.println("문자열 입력 >> ");
			
			String str1, str2, str3, str4, str;
			str1 = sc.nextLine();
			System.out.println("문자열 입력 >> ");
			str2 = sc.nextLine();
			System.out.println("문자열 입력 >> ");
			str3 = sc.nextLine();
			System.out.println("문자열 입력 >> ");
			str4 = sc.nextLine();
			
			app.capacity();
			
			str = sc.nextLine();
			
			if(str.equals("그만")) {
				break;
			}
			
			System.out.print(app.pop());
			app.push(str1);
			app.push(str2);
			app.push(str3);
			
		}
		sc.close();
	}
}

interface Stack {
	int length();
	int capacity();
	String pop();
	boolean push(String val);
}

class StackApp implements Stack {
	String[] arr = new String[3];
	int index = -1;
	
	
	public int length() {
		return arr.length;
	}
	
	public int capacity() {
		for(int i = 0; i <arr.length; i++) {
			if(index >= arr.length) {
				System.out.println("스택이 꽉 차서 푸시 불가!");
			}
		}return index = arr.length - index;
	}
	
	public String pop() {
		for(int i = arr.length; i < 0; i--) {
			System.out.println(arr[i]);
		}
		return "스택에 저장된 모든 문자열 팝: " ;
	}
	
	public boolean push(String val) {
		for(int i = 0; i <arr.length; i++) {
			if(val == arr[i]) {
				index++;
				System.out.print(arr[i] + " ");
			}
		}return true;
	}
}
```
