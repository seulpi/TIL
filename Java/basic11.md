# Strig 함수
>> String은 문자들의 배열로 만들어진 문자열 함수 <br> (문자열이 방 하나하나에 문자가 들어가 있다)

## @ concat / substring
- concat :  문자열과 문자열을 이어주는 함수 
```java
String str1 = "Coffee";
String str2 = "Bread";
		
String str3 = str1.concat(str2); //concat : 앞뒤 문자열 연결 
System.out.println(str3);

String str4 = "Fresh".concat(str3);//"Fresh" 하나의 객체이기때문에 다이렉트로 함수호출 가능 
System.out.println(str4);
=======================================
▶	CoffeeBread
	FreshCoffeeBread
```
- substring 
(index 범위)안에 있는 방의 값을 호출하는 함수 
```java
String str = "abcdefg";
str.substring(3); // 2번째 방에서부터 출력해라
str.substring(2, 4); //2번째방에서부터 3번째방까지 위치한 문자열 출력해라 
// 같은 함수명 호출됐는데 에러가 안남 "오버로딩" 적용된것
```
>> 우리는 보통 index의 번호를 매기면 **'1'**부터 시작하지만 <br> 
전산상의 index는 **'0'** 부터 시작

## @ equals / compaerTo / compareToIgnoreCase
- equals : 문자열이 같은지 비교하는 함수 (기본데이터형의 == 와 동일)
- compareTo : 비교했을때 **'사전적으로'** 문자열의 순서를 비교하는 함수
- compareToIgnore : 대소문자 **'관계없이'** 문자열이 같은지 비교하는 함수
```java
public static void main(String[] args) {
	String st1 = "Lexicographically";
	String st2 = "lexicographically"; // 두 변수의 주소는 다르다 'L' 'l' 대소문자 차이
	int cmp;
		
	if(st1.equals(st2)) // 주소아닌 글자 내용의 비교 
		System.out.println("두 문자열은 같습니다");
	else
		System.out.println("두 문자열은 다릅니다");
		
	cmp = st1.compareTo(st2); /* 사전상에서 앞위냐 뒤냐 비교하는 함수 → 페이지수생각해서 부등호 기억 0이면 페이지수가 같다, 수가 크면 페이지수 뒷장 (이 함수에 리턴값은 정수)*/
			
	if(cmp == 0)
		System.out.println("두 문자열은 일치합니다");
	else if(cmp < 0)
		System.out.println("사전의 앞에 위치하는 문자: " + st1);
	else
		System.out.println("사전의 앞에 위취하는 문자: " + st2);
		
	if(st1.compareToIgnoreCase(st2) == 0) // Case는 대소문자를 가리킨다 → 대소문자 무시하고 비교하는 함수
		System.out.println("두 문자열은 같습니다");
	else
		System.out.println("두 문자열은 다릅니다");
}
```

## @ String.valueOf  (★암기★)
- String.valueOf : 데이터타입을 String(문자열)로 바꿔주는 함수
```java
double e = 2.78;
String se = String.valueOf(e); /* .접근하는것 → .valueOf(e)이 static함수 
                                se안에는 "2.78"의 문자열의 첫번째 주소가 할당된다 
                                valueOf()는 모든타입을 문자열로 바꿔준다*/

String str = "age: " + 17;
// String str = "age: ".concat(String.valueof(17)) 이 과정을 자동으로 거침 
System.out.println(str);
==========================
▶ age: 17
```
☞ 여러개의 타입을 String으로 연산하게 되면 할 때마다 객체생성을 하기 때문에 비용이 비싸진다( =메모리낭비,속도저하)
따라서 더 최적화된 방법으로 Stringbuilder 를 사용하는게 좋다

## @ StringBulider / StringBuffer
>>  StringBulider / StringBuffer 두 함수 모두 중간에 객체생성을 하지 않는다 <br> (mutable)
```java
 StringBulider / StringBuffer 는 String과 달리 기존의 주소에 담겨있는 참조값에 붙여서 문자열 생성
 → 같은 주소 안에서 문자열이 계속 바뀌는게 가능 
 → StringBulider / StringBuffer의 차이점은 StringBuffer가 쓰레드(Thred)에 더 안전함 
	: 쓰레드에 사용하지 않아도 된다면 더 효율적인 StringBuilder를 사용!
```
 ### - StringBulider의 함수 종류
 >> append, delete, replace, reverse, substring
```java
public static void main(String[] args) {

	StringBuilder stbuf = new StringBuilder("123");

	stbuf.append(45678); // append 붙여서 출력

	System.out.println(stbuf.toString()); /* 왜 toString으로 바꿔줘야되는가?
											println은 기본형만 올 수 있고 함수를 받는 오버로딩을 만들어져있지않음 
											따라서 toString을 받아서 String타입으로 바꿔줘야한다
											println은 String타입 담을 수 있으니까 */

	stbuf.delete(0, 2); // 0~1까지 문자열 삭제 12345678 → 345678

	System.out.println(stbuf.toString()); 

	stbuf.replace(0, 3, "AB"); // 0~2까지 문자열을 "AB"로 대체 345678 → AB678

	System.out.println(stbuf.toString());

	stbuf.reverse(); // 문자열 반대로 뒤집기 AB678 → 876BA

	System.out.println(stbuf.toString());

	String sub = stbuf.substring(2, 4); // 2~3까지만 문자열로 반환 → 6B

	System.out.println(sub);
}
```

# 콘솔입출력 
>> System.ou.println() 함수는 '객체' 올 수 있다 → '다형성'적용되서 가능한 것 <br> 실행되는 원리는 객체가 오면 toString을 자동으로 호출해서 출력되는 원리 
```java
>>Box class

static class Box {

	private String conts;

	Box(String cont) {

		this.conts = cont;

	}

	public String toString() {

		return conts;
	}

//Main class
public static void main(String[] args) {

	StringBuilder stb = new StringBuilder("12");

	stb.append(34);

	System.out.println(stb.toString());

	System.out.println(stb);

	Box box = new Box("camera");

	System.out.println(box.toString()); // 일반적인 객체가 넘어오면 toString으로 호출된다

	System.out.println(box); // 따라서 위에 toString을 직접 호출해도 되고 아님 다이렉트로 객체 넣어도 됨 
}
```	
<br>
 
- skip가능) printf (자바에서는 잘 안씀 c언어에서 주로 쓴다)
```java
System.out.printf("정수는 %d, 실수는%f, 문자는 %c", 12, 24.5, 'A');
======================================================
▶ 정수는 12, 실수는 24.500000, 문자는 A
```
### println() toString을 자동으로 호출 
 

 
