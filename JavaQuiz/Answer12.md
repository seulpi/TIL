# 1. String 클래스에서 concat 메서드를 설명하시오
## concat은 "문자열"을 연결해주는 함수 
```java
String str1 = "Hello";
String str2 = "Java";

System.out.println(str1.concat(str2));
============================================
▶ HelloJava
```
<br>

# 2. str.substring(2, 4); substring 사용법에 대하여 설명하시오
substring은 String이 가지고 있는 여러개의 함수 중의 하나로 (값)이 호출하는 방에 있는 문자들만 문자열로 호출하라는 함수이다
```java
String str = "abcde";
String output = str.substring(2,4); // 단, 전산적으로 방은 0부터 잡기때문에 01234가 방 넘버이다

System.out.println(output);
============================================
▶ cd
```
<br>

# 3. st1.compareTo(st2); <br> compareTo 사용법에 대하여 설명하시오
compareTo()함수는 사전적으로 어떤 "문자열"이 앞에 오는지 비교하는 함수이다 <br>
```java
String st1 = "Hello";
String st2 = "hello";

int result = st1.compareTo(st2);

if(result == 0) {
    System.out.ptintln("두 문자열은 같습니다");
}else if(result < 0 ) {
    System.out.println("st1이 사전적으로 앞에 옵니다")
}else
    System.out.println("st2가 사전적으로 앞에 옵니다")

/* 페이지수를 생각해보면 간단하다 
페이지수가 같다 (==0)면 두 문자열은 사전적으로 위치가 같고 
페이지수가 0 보다 작으면( < 0) 뒤에 오는 비교 변수가 사전적으로 뒤에 오는 결과 
예) 23page < 0 < 500 page  -> 500page가 사전 뒤쪽*/
==============================================================
▶ st1이 사전적으로 앞에 옵니다
```
<br>

# 4. String.valueOf 에 대하여 설명하시오
String.valueOf()는 어떤 타입이 와도 **"문자열"**로 바꾸어주는 함수 
```java
double e = 3.75;
String str = String.valueOf(e); 

System.out.println(str); // 3.75 → "3.75"
==============================================================
▶ 3.75 (숫자가 아닌 문자열의 3.75)
```
## ＊여기서 잠깐! 그렇다면 String.valueOf()와 toString의 차이는? <br>
 ☞ 함수의 용도 자체가 아예 다른 용도 <br>
 String.valueOf(값or변수)는 함수의 값을 받아서 String으로 바꿔주는것 <br>
 toString은 () String을 정의해주는 함수 / String.valueOf(값or변수)를 할때도 사실상 toString의 과정을 거친다 / 일반적인 자료형을 가진 값이나 변수들에 대해서 사용X 객체에서 자유롭게 사용 가능 <br>
 >> 가장 큰 차이는 객체가 null일때) <br> 
 -toString(): 대상 값이 null이면 NPE(Null PointerException)를 발생 -> Object에 담긴 값이 String이 아니여도 출력 <br>
 -String.valueOf(): "null" 문자열 출력
 - 이런 차이때문에 valueOf은 "null".equals(string) 형태로 다시 한번 체크를 해야하고 NPE(Null PointerException)를 방지해야하는 경우에서는 String.valueOf()를 사용하는 것이 좋다

<br>

# 5. 아래의 연산과정에서 호출되는 함수(원리)를 써서 표현해 보세요
## String str = "age: " + 17;
```java
String str = "age: " + 17;
→ String str = "age: ".concat(17);
→ String str = "age: ".concat(String.valueOf(17));
=========================================================
▶ age: 17
```
<br>

# 6. StringBuilder 와 String 의 차이는?
String은 연산할때 기존의 값에 값을 더해서 붙이는게 아니기 때문에 타입이 다른 변수들의 방를 만들고 다시 저장을 반복한다<br>
(타입이 다른게 많다면 엄청 많은 방을 만들고 메모리를 소모) <br>
<br>
그런데 StringBuilder는 기존의 값에 값을 더할 수 있기 때문에 중간에 객체 생성을 하지않고 방 만드는걸 반복하지 않는다 <br>
같은 주소를 가진 방에 이어서 값들을 연산 가능하게한다 
따라서 String보다 메모리공간↓ 속도 ↑ 

```java
String: immutable
StringBuilder : mutable // 자매품: StringBuffer (쓰레드에서 StringBuilder가 더 안전)
```
<<<<<그림쓰~!>>>>>

<br>

# 7. String 을 이용하여 프로그래밍 하시오 ★ 必 ★
## 입력 : 990925-1012999 <br>출력 : 990925 1012999
```java
public static void main(String[] args) {
		StringBuilder str = new StringBuilder("990925-1012999");
		str.delete(6, 13);
		str.append(" 1012999");
		System.out.println(str);
}
```
<br>

# 8. 아래의 메모리 그림을 그리시오
## int[] ar1 = new int[5];
![배열01](https://user-images.githubusercontent.com/74290204/101460643-37839e80-397d-11eb-9935-45439fec0aae.png)

<br>

# 9. 아래를 프로그래밍 하시오 (단 클래스로 구성할것) ★ 必 ★
## 입력:lee sunkyo <br> 출력:first name: lee second name:sunkyo
```java
// Name class

import java.util.Scanner;

public class Name {
	
		private String firstName;
		private String secondName;
		
		
		public void nameInput() {
			Scanner sc = new Scanner(System.in);
			System.out.println("입력: ");
			firstName = sc.next();
			secondName = sc.next();
			
		}
		
		public void nameOutput() {
			System.out.println("firsrt name: " + firstName);
			System.out.println("second name: " + secondName);
			
		}
	
	}

// Main class

ppublic static void main(String[] args) {
		Name name = new Name();
		name.nameInput();
		name.nameOutput();
	}
}
```
<br>

# 10. 아래를 프로그래밍 하시오
## 입력 : 홍 길동 <br> 출력 : 성 = 홍  이름 = 길동  <br> <br> 입력 : 홍길동 <br>출력 : 공백이 없군요. 다시 입력해주세요
```java
// Name class
import java.util.Scanner;

public class Name {
		
	private String name;
	
		
	public void nameInput() {
		Scanner sc = new Scanner(System.in);
		System.out.println("입력: ");
		name = sc.nextLine();
	}

	public void nameOutput() {
		int index = name.indexOf(" ");
		
		if(index < 0) // indexOf는 발견하지 못하면 -1을 값으로 반환
			System.out.println("공백이 없군요. 다시 입력해주세요.");
		else
			System.out.println("성 = " + name.substring(0, index) + " 이름 = " + name.substring(index+1));	
	}

}


// Main class
public static void main(String[] args) {
	
	Name name = new Name();
	name.nameInput();
	name.nameOutput();
}

```
<br>

# 11. 아래를 프로그래밍 하시오
```java
"Hello.java" 문자열에서 파일명과 확장자인 java를 분리시키는 프로그램을 짜시오.

입력: Hello.java
출력: 파일이름은:Hello 이며 확장자는 java 입니다

입력: Java.avi 
출력: 파일이름은:Java 이며 확장자는 avi 입니다
````
```java
>> Answer

// Hello class

import java.util.Scanner;

public class Hello {
	private String name;
	
	public void nameInput() {
		Scanner sc = new Scanner(System.in);
		System.out.println("파일명을 입력하세요: ");
		name = sc.next();	
	}
	
	public void nameOutput() {
		int index = 0;
		for(int i = 0; i <name.length(); i++)
			if(name.charAt(i) == '.')
				index = i;
		System.out.println("파일이름은: " + name.substring(0, index) + "이며 확장자는: " + name.substring(index+1) + "입니다." );
	}
}


// Main class

public static void main(String[] args) {
	Hello name = new Hello();
	name.nameInput();
	name.nameOutput();
}
```
# 12. 아래를 프로그래밍 하시오
```java
Scanner 클래스를 이용해서 한 줄 읽고, 공백으로 분리된 "단어"가 몇 개 들어 있는지 확인해보자.

"그만"을 입력할 때까지 반복하는 프로그램이다.힌트(split 함수를 이용해 볼것)

예) 입력 : Java Programming 
출력 : 단어의 개수는 2
입력 : 자바 수업은 2층 C클래스 
출력 : 단어의 개수는 4
입력 : 그만 > 출력 : 프로그램 종료
```
```java

// JavaCount class
import java.util.Scanner;

public class JavaCount {
	
	private String name;
	
	public void nameInput() {
		
		Scanner sc = new Scanner(System.in);
	
		while(true) {
		
			System.out.println("문장을 입력하세요: ");
			name = sc.nextLine();
			String[] array = name.split(" ");
			
			if(name.equals("그만")) {
				System.out.println("프로그램 종료");
				break;
			}else {
				int count = 0;
				
				for(int i = 0; i < array.length; i++) {
					count++;
				}
				System.out.println("단어의 개수는 " + count);
			}

		}	
	}
}


// Main class

public static void main(String[] args) {
    JavaCount javaCount = new JavaCount();
	javaCount.nameInput();
}
```








