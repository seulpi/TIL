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
