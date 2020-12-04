# 클래스패스 : 클래스 경로
- 자바는 기본적으로 클래스가 존재하면 메모리에 올려야 되기 때문에 **무조건** .class를 만든다 <br>

```java
class AAA { }
classZZZ { }

class WhatYourName {
	public static void(String[] args) { 
		AAA aaa= new AAA( ) ;
		aaa.showName();
		
		ZZZ zzz= new ZZZ( ) ;
		zzz.showName();

/* WhatYourName을 컴파일시키면 
AAA.class / ZZZ.class / WhatYourName.class 생성 */
```
 - 클래스 찾는 경로 개발자가 직접 지정 가능
 - 클래스 패스를 하는 이유?
   - 메인 메소드를 그대로 둔 상태에서 클래스를 다른 하위폴더에 옮겨도 실행은 가능하지만 에러발생! 그렇기 때문에 이런 경우를 대비해 클래스패스로 경로를 직접 지정해준다

```java
                    //↓현재디렉토리
명령프롬포터(cmd) :  c:\User\AI> javac Hello.java
 
변경방법 : c:\User\AI> set classpath=.;변경할 경로
                               /*   ↑ . 은(cmd가 실행중인 상황에서 현재 디렉토리를 의미)
                               여전히 나는 이 경로에서 찾겟다고 명시해두는것 */
```

## @ 절대경로 & 상대경로
```java

// 절대경로: 가장 처음부터 실행되는것 (가장 최상위부터 주르륵)
C : \PackacgeStusy>set classpath=.;C:\PackageStudy\Myclass

// 상대경로 : 현재 디렉토리를 기준(.)으로  실행
C : \PackacgeStusy>set classpath=.;\Myclass

=================================================
.  → 현재 디렉토리
.. → 이전 디렉토리
```
☞ 클래스 패스를 고정시키기도 하는데 그 이유는 모든 시스템 전체에서 **error없이 사용하기 위해서다**

<br>

# 패키지 
 - 패키지는 하나의 폴더 같은 개념 <br>
☞ 패키지를 사용하는 이유? 
   같은 이름을 가진 파일들이 한 폴더안에서 사용이 물리적으로 불가능하기 때문에 패키지를 만들어 충돌을 방지하기 위해
```java
// bit package
package com.bit;

public class Circle {
	
	public Circle() {
		System.out.println("A사가 만든 Circle");
	}

}
==========================================================
// bat package
package com.bat;

public class Circle {
	
	public Circle() {
		System.out.println("A사가 만든 Circle");
	}
}
============================================================
public class PackageTest {

	public static void main(String[] args) {
		
		Circle circle = new Circle(); /* error 발생 왜 ? 어디에 있는 Circle인지 컴퓨터가 헷갈림 
    따라서 경로를 앞에 붙여줘서 어디에서 오는 함수인지 지정해줘야함 */
	}
}

=======================================================
▷
public class PackageTest {

	public static void main(String[] args) {
		
		com.bat.Circle circle = new com.bat.Circle();
		com.bit.Circle circle2 = new com.bit.Circle();

	}
}
//패키지 선언이란 같은 클래스명을 유일한 클래스(다른 클래스)로 인식하게 만들어 주는 것

```
## - 패키지 명명법
```java
패키지 선언방식
- pakage 인터넷도메인이름역순.클래스의 정의한주체or팀의이름
ex) www.sbns.com -> pakage com.sbns.lsb

// 패키지선언을 함과 동시에 포함되어있는 클래스는 com>sbns>lsb 에 들어있어야 인스턴스 생성이 가능
```

※(주의)
Scanner 임포트 시킬때 java.util에 있는걸로 임포트를 해야하는데 다른거 잘못가져오게되면 다른 패키지에 있는 걸 import하기 때문에 실행안되는 현상을 겪게 될수있음

# 정보은닉 : 클래스 외부에서의 접근을 막는 것
: 접근을 제한없이 허용하게 되면 논리적 오류가 발생하기도 함(ex.클래스가 다른데 각 클래스 함수의 네임이 같다던지 하는)

```java
★★★★★
 /* Q; OOP(Object-oriented programming)가 무엇인가?
 객체지향언어가 무엇인가? */

A: c언어(절자치향적언어)에서 소프트웨어적으로 규모가 커지다보니 c언어의 단점을 보완하고 발전시킨 객체지향언어(대표적인 언어로는 JAVA)가 생김 

"OOP의 특징"

1. 정보은닉 (Hidden Information)
2. 상속 (Inheritance)
3. ★ 다형성(Polymorphism) : 여러가지 형태를 가질 수 있는 능력
                            한 타입의 참조변수로 여러타입의 객체를 참조할 수 있도록 하고 상속관계에서 일어난다
4.캡슐화(Encapsulation)
```
 ## <접근 제한자(Access Modifier) 클래스 외부허용범위>

| pubilc | 외부접근 다 가능 (언제 어디서나 OK) | 소스파일명 = 클래스명(반드시)| 
|:----------|:----------:|----------:|
| protected | 동일패키지, 상속 받은 클래스 접근 가능 | 
| default | 동일 패키지만 접근가능 | 
| private | 같은 클래스 내부에서만 접근가능 |

<br>

```java
pubilc: '다른' 패키지내에 있는 클래스에서 객체생성을 가능하게 한다 
default: '같은' 패키지내에서 객체생성이 가능하다
```

- pubilc / protected /default /private : 클래스 변스 함수 (즉 클래스 내부에 있는건 다 붙일수있음)
- 클래스에서만 붙이기 가능 pubilc default(아무것도 안붙인상태)
- 인스턴스변수와 함수 : pubilc / protected /default /private
  - (지역변수는 어차피 지역을 벗어나면 의미가 없어지기 때문에 public 붙여봤자 의미가 없음) <br>
  - ★ 인스턴스 변수에서는 반드시 **'private'** 붙여줘야한다

☞ set get함수 용도 : 인스턴스 변수의 제한을 두기 위해 생성하는 것 <br>
**인스턴스 변수는 무조건 private 함수는 큰 의미 없다면 public으로 사용**

<br>

```java
>> Circle class

public class Circle {

	private double rad = 0;
	final double PI = 3.14;
	
	public Circle(double r) {
		setRad(r);
	}

	public void setRad(double r) {
		if( r < 0) {
			rad = 0;
			return;
		}
		rad = r;
	
	public double getArea() {
		return (rad * rad) * PI;
	}
}
=========================================================
>> main class

public class HiddenMain {

	public static void main(String[] args) {
		Circle c= new Circle(1.5);
		System.out.println(c.getArea());
		
		c.setRad(2.5); // Circle 클래스의 setRad함수로 접근
		System.out.println(c.getArea());
		c.setRad(-3.3);
		System.out.println(c.getArea());
		
		c.rad = -4.5; /* Circle 클래스 변수의 다이렉트로 접근 
						(error가 발생하지 않지만 다이렉트 접근은 사용하면안됨!)
						이렇게 사용하는걸 방지하기 위해 클래스 안 변수 선언 앞에 
						private (접근제한자) 선언해주면 메인에서 다이렉트 접근 막을 수 있음 */ 
		System.out.println(c.getArea());
	}
}
★★★★★★일반 변수앞에서는 private을 반드시 붙여줘야한다(안붙이면 옥상행)★★★★★
```


- public : 하나의 클래스 안에 하나만 가능 why? 클래스와 소스파일의 '이름'이 같아야되기 때문에
- default : 패키지 선언을 해주지 않은 클래스들끼리 하나의 패키지로 구성하는 것
```java
class ZZZ {  }  // 클래스 타입이 아무것도 선언되지 않음 '디폴트 패키지'
