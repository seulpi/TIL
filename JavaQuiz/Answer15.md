# 1. is a 관계와 has a 관계에 대하여 설명하시오
>> B is A : B 는 A 이다 (상속관계) <br>
B has A : B가 A를 가지고 있다 (포함관계)
* 상속관계에서 조금이라도 의구심이 든다면 그건 상속관계라고 볼 수 없다
```java
 B is A = '컴퓨터' 는 '노트북' 이다
 B has A = '컴퓨터'는 'CPU'를 가지고 있다 
 ```

# 2. 다형성이란 무엇인가?
 - 다형성은 하나의 메소드나 클래스가 있을  이것들이 다양한 방법으로 동작하는 것 <br>
 하나의 객체가 여러개의 타입을 가지고 있는 것 (형이 많다)
  <br> 대표적인 예로는 오버라이딩, 오버로딩

# 3. 아래가 되지 않는 이유에 대하여 메모리 그림으로 설명하시오
## MobilePhone(부모) SmartPhone(자식) <br> SmartPhone s = new MobilePhone();
![상속(다형성)](https://user-images.githubusercontent.com/74290204/101921567-7e3cf700-3c10-11eb-9c4f-9e9354cc8381.png)


# 4. 메소드 오버라이딩 이란?
- 오버라이딩이란 상속관계에서 부모와 자식의 함수리턴타입, 함수명, 파라미터값까지 같으나 { 구현하는 부분 } 만 다른 것
- 오버라이딩은 **반드시** 상속관계가 동반된다 <br>
따라서 오버라이딩을 하라는 것은 상속 관계를 정의해서 프로그래밍을 하라는 이야기이다

# 5. 갬블링 게임을 만들어보자
```java
두 사람이 게임을 진행한다.
이들의 이름을 키보드로 입력 받으며 각 사람은 Person 클래스로 작성하라. 
그러므로 프로그램에는 2개의 Person 객체가 생성되어야 한다.
두 사람은 번갈아 가면서 게임을 진행하는데 각 사람이 자기 차례에서 <Enter> 키를 입력하면, 
3개의 난수가 발생되고 이 숫자가 모두 같으면 승자가 되고 게임이 끝난다. 
난수의 범위를 너무 크게 잡으면 3개의 숫자가 일치하게 나올 가능성이 적기 때문에
숫자의 범위는 1~3까지로 한다.
================================================================================
1번째 선수 이름>>수희
2번째 선수 이름>>연수
[수희]:
	3  1  1  아쉽군요!
[연수]:
	3  1  3  아쉽군요!
[수희]:
	2  2  1  아쉽군요!
[연수]:
	1  1  2  아쉽군요!
[수희]:
	3  3  3  수희님이 이겼습니다!
================================================================================
```
```java
//Person class[배열이용] - 이거 사용하려다 값 못냈음 ㅠㅠ 중간에 비교하는거에서 막혀서
public class Person {
	
	String name;
	int[] ar ;
	int num = 3;
	
	Person(String name) {
		this.name = name;
		ar = new int[num];
	}
	
	public boolean game() {
		
		boolean isDuplicate = true;
		
		for(int i =0; i <ar.length; i++) {
			ar[i] = (int)(Math.random()*3 +1);
		}
	
		for(int i =0; i <ar.length; i++) { // 숫자 같은지 비교 하는거에서 막히는구만
			if(ar[0] != ar[i]) { 
				isDuplicate = false;
				break;
			}
		}
		
		for(int i = 0; i < ar.length; i++) { // 출력
			System.out.print("\t" + ar[i] + " ");
		}
		return isDuplicate; 
	}	
}

//Person class[boolean타입만 이용]
class Person {
	private int num1, num2, num3;
	public String name;

	public Person(String name) {
		this.name = name;
	}

	public boolean game() {
		num1 = (int) ((Math.random() * 3) + 1);
		num2 = (int) ((Math.random() * 3) + 1);
		num3 = (int) ((Math.random() * 3) + 1);
		
		System.out.print("\t" + num1 + "  " + num2 + "  " + num3 + "  ");

		if (num1 == num2 && num2 == num3)
			return true;
		else
			return false;
	}
}


// Main class 
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		
		Scanner sc = new Scanner(System.in);
		System.out.println("1번째 선수 이름 >> ");
		String name = sc.next();
		Person person1 = new Person(name);
		
		System.out.println("1번째 선수 이름 >> ");
		name = sc.next();
		Person person2 = new Person(name);
		
		String buffer = sc.nextLine(); // enter 치는거 ( enter를 이용하기 위해 선언한것 string이 아닌 다른게 와도되고 next가 와도됨 )
		
		while(true) { // while문 (무한반복으로 직접 입력하는경우)는 메인에서 고객님이 처리하게 하는게 좋음!
			System.out.println("["+ person1.name + "]: <Enter>");
			buffer = sc.nextLine(); // enter 치는거
			
			if(person1.game()) {
				System.out.println(person1.name + "님이 이겼습니다!");
				break;
			}
			System.out.println("아쉽군요!");
			System.out.println("[" +person2.name + "]: <Enter>");
			buffer = sc.nextLine(); // enter 치는거
			
			if(person2.game()) {
				System.out.println(person2.name + "님이 이겼습니다!");
				break;
			}
			System.out.println("아쉽군요!");
		}
		sc.close();
	}
}
```
# 6. 문제 10의 갬블링 게임을 n명이 하도록 수정하라
```java
행 예시와 같이 게임에 참여하는 선수의 수를 입력받고 각 선수의 이름을 입력받도록 수정하라.

===================================================================================
겜블링 게임에 참여할 선수 숫자>>3
1번째 선수 이름>>황
2번째 선수 이름>>이
3번째 선수 이름>>김
[황]:
	2  3  3  아쉽군요!
[이]:
	1  2  2  아쉽군요!
[김]:
	2  2  3  아쉽군요!
[황]:
	3  2  2  아쉽군요!
[이]:
	1  1  3  아쉽군요!
[김]:
	2  2  1  아쉽군요!
[황]:
	2  2  2  황님이 이겼습니다!

===================================================================================
```

# 7. 다음을 만족하는 클래스 Employee를 작성하시오
```java
- 클래스 Employee(직원)은 클래스 Regular(정규직)와 Temporary(비정규직)의 상위 클래스

- 필드: 이름, 나이, 주소, 부서, 월급 정보를 필드로 선언

- 생성자 : 이름, 나이, 주소, 부서를 지정하는 생성자 정의

-메소드 printInfo() : 인자는 없고 자신의 필드 이름, 나이, 주소, 부서를 출력
```
```java
// Answer
public class Employee {

	private String name, address, department;
	private int age, pay;
	
	Employee() {
		
	}
	Employee(String name, int age, String address, String dapartment) {
		this.name = name;
		this.age = age;
		this.address = address;
		this.department = dapartment;
	}
	
	public void printInfo() {
		System.out.println("이름: " + name);
		System.out.println("나이: " + age);
		System.out.println("주소: " + address);
		System.out.println("부서: " + department);
	}
}
```

# 8. 다음을 만족하는 클래스 Regular를 작성하시오
```java
- 클래스 Regular는 위에서 구현된 클래스 Employee의 하위 클래스

- 생성자 : 이름, 나이, 주소, 부서를 지정하는 상위 생성자 호출

- Setter : 월급 정보 필드를 지정

- 메소드 printInfo() : 인자는 없고 "정규직"이라는 정보와 월급을 출력
```
```java
class Regular extends Employee {
	
	int setter;
	
	Regular() {
		
	}
	
	Regular(String name, int age, String address, String department, int setter) {
		super(name, age, address, department);
		this.setter = setter;
	}
	
	public void printInfo() {
		super.printInfo(); // Employee printInfo 출력 
		System.out.println("정규직 월급: " + setter); // 내꺼(Regular) 출력
	}
}
```
# 9. 아래를 프로그래밍하시오(남궁성자바 연습문제)
![캡처111](https://user-images.githubusercontent.com/74290204/102044395-1659fc80-3e1a-11eb-9a86-7d37516ed4b8.PNG)
![캡처222](https://user-images.githubusercontent.com/74290204/102044453-2d98ea00-3e1a-11eb-87d9-403638e52ab1.PNG)
![캡처333](https://user-images.githubusercontent.com/74290204/102044471-35588e80-3e1a-11eb-9c5c-bafe65ea36ee.PNG)

```java

public class TranningMain {
	public static void main(String[] args) {
		Buyer b = new Buyer();
		b.buy(new Tv());
		b.buy(new Computer());
		b.buy(new Tv());
		b.buy(new Audio());
		b.buy(new Computer());
		b.buy(new Computer());
		b.buy(new Computer());
		
		b.summary();	
	}
}
class Buyer {
	int money = 10000;
	Product[] cart = new  Product[3];
	
	int i = 0; // 
	
	void buy(Product p) {
		// 1. 가진돈과 물건의 가격비교 : 가진 돈 적으면 메서드 종료
		if(money < p.price) {
			System.out.println("잔액이 부족하여" + p + "을/를 살 수 없습니다");
			return;
		}
		//2. 가진 돈 충분하면 
		money = money - p.price;

		//3. 장바구니에 물건을 담는다
		add(p);
	}
	void add(Product p) {
		//1. i의 값이 장바구니의 크기보가 같거나 크면 
		if(i >= cart.length) {
			//1-1 기존의 장바구니보다 2배 더 큰 배열 생성
			Product[] temp = new Product[cart.length*2]; // 배열 2배 생성
			//1-2 기존에 있는 내용을 복사
			System.arraycopy(cart, 0, temp, 0, cart.length);
			
			cart = temp; // 기존에 있던 주소에 대입 why? temp의 범위는 사라지고 cart는 범위가 더 넓기 때문에
		}
		cart[i++] = p; //or cart[i] = p; i++; 해도됨
	}
		
	void summary() {
		String itemList = " ";
		int sum = 0 ;
		
		for(int i = 0; i<cart.length; i++) {
			if(cart[i] == null)
				break;
			itemList += cart[i] + ","; // 목록 담기
			sum = sum + cart[i].price; // 총 가격
		}
		System.out.println("총 구입한 물건" + itemList);
		System.out.println("사용한 금액: " + sum);
		System.out.println("남은 금액: " + money);
	}
}

class Product {
	int price;
	
	Product(int price) {
		this.price = price;
	}
}

class Tv extends Product {
	Tv() {
		super(100); //Tv의 가격 100 (직접 가격세팅한것)
	}
	
	@Override
	public String toString() {
		return "Tv"; 
	}
}

class Computer extends Product {
	Computer() {
		super(200); //Computer의 가격 200(직접 가격세팅한것)
	}
	
	@Override
	public String toString() {
		return "Computer"; 
	}
}

class Audio extends Product {
	Audio() {
		super(300); //Audio의 가격 200(직접 가격세팅한것)
	}
	
	@Override
	public String toString() {
		return "Audio"; 
	}
}
```
