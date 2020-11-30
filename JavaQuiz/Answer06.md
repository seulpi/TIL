# 1. 반복문 3가지의 무한루프 만드는 방법은
## if(;;) { } / while(true) { } / do while(true) { }

<br>

# 2. 구구단 출력을 하시오
```java

for(int i = 2; i < 10; i++) {
	for(int j = 1; j < 10; j++) {
		System.out.println(i + " * " + j + " = " + (i*j));
		}
	}
```

<br>

# 3. 짝수단만 찍으시오
```java
// 방법 1 (가장 nice한 방법 ; 직관적이라서)

for(int i = 2; i < 10; i++) {

	if(i % 2 != 0) 
		continue;

	for(int j = 1; j < 10; j++) {
		System.out.println(i + " * " + j + " = " + (i*j));
	}
}
===================================================================
// 방법 2

for(int i = 2; i < 10; i++) {

	for(int j = 1; j < 10; j++) {

		if(i % 2 == 0) {

			System.out.println(i + " * " + j + " = " + (i*j));
		}	
	}
}

===================================================================
// 방법 3

for(int i = 2; i < 10; i++) {

	if(i % 2 == 0) {

		for(int j = 1; j < 10; j++) {

			System.out.println(i + " * " + j + " = " + (i*j));
		}
	}		
}
```
<br>

# 4. 3의 배수인 단만 출력하시오
```java
for(int i = 2; i < 10; i++) {
	if(i % 3 != 0)
		continue;
			
	for(int j = 1; j < 10; j++) {
		System.out.println(i + " * " + j + " = " + (i*j));
	}
}
▶ 3,6,9단 출력

방법들 중에는 3번과 같은 방법들이 있음		
```
<br>

# 5. 아래의 Star를 찍으시오
## 5-1 )
★★★★★ <br>
★★★★★ <br>
★★★★★ <br>
★★★★★ <br>
★★★★★ <br>

```java
for(int i = 1; i <= 5; i++) {
	for(int j = 1; j <= 5; j++) {
		System.out.print("★");
	} System.out.println();
}
```

## 5-2)

★ <br>
★★ <br>
★★★ <br>
★★★★ <br>
★★★★★ <br>

```java
for(int i = 1; i <= 5; i++) {
	for(int j = 1; j <= j; j++) {
		System.out.print("★");
	} System.out.println();
}
// 1행일때 1개찍고 2행일때 2개찍고 하면되니까 i랑 같아지면서 별찍으면 됨
```

## 5-3)

★★★★★ <br>
★★★★ <br>
★★★ <br>
★★ <br>
★ <br>

```java

for(int i = 5; i >= 1; i--) {
	for(int j = 1; j <= i; j++) {
		System.out.print("★");
	} System.out.println();
}
```

## 5-4)

_________★ <br>
_______★★ <br>
_____★★★ <br>
___★★★★ <br>
_★★★★★ <br>

```java
// 방법 1

for(int i = 1; i <= 5; i++) {
	for(int j = 1; j <= i -1; j++) {
		System.out.print(" ");
	} 
	for(int j = 5; j >= i; j--) {
		System.out.print("★");
	}
    System.out.println();
}

===================================================================
// 방법 2
for(int i = 5; i >= 1; i--) {
	for(int j = 5; j > i; j--) {
		System.out.print(" ");
	} 
	for(int j = 1; j <= i; j++) {
		System.out.print("*");
	}
	System.out.println();
}

```

## 5-5) ???????? 소스 봐도 모르겠음 ㅠㅠㅠ
____ ★  <br>
__ ★★★ <br>
_ ★★★★★ <br>
★★★★★★★ <br>

```java
for(int i = 0; i <5; i++) {
	for(int j = 4; j > i; j--) {
		System.out.print(" ");
	} 
	for(int j = 1; j <= i*2 +1; j++) {
		System.out.print("*");
	}
	System.out.println();
}
```
<br>

# 6. 함수는 어떻게 알아 볼수 있는가
 함수는 ( ) 대괄호를 보면 식별이 가능하다
 함수는 만들 줄 알아야하고 쓸 줄 알아야하는데 함수안에는 함수가 존재할 수 없으며 ( )안에는 매개변수가 오게 된다 <br> → 매개변수는 변수선언을 하고 바로 값을 할당할 수 없다 <br> 
 값을 할당(메모리에 올리는것)하는 것은 함수를 호출하는 쪽에서 가능하다 [값 = value]
 +매개변수(=파라미터=인자)는 여러개의 변수 선언이 가능
 void: 값을 반환하지 않는다(return이 필요없다)


<br>

# 7. 아래의 함수를 만드시오

함수이름: starPrint <br> 
매개변수: type 1개 <br> 
기능: 매개변수에 3를 전달하면 3층 석탑, 5를 전달하면 5층석탑 <br> 
EX) 3전달시 3층석탑 <br> 
★ <br> 
★★ <br> 
★★★ <br> 

5전달시 5층석탑
★ <br> 
★★ <br> 
★★★ <br> 
★★★★ <br> 
★★★★★ <br> 

```java

// 3 넣은 결과

public static void main(String[] args) {
		
		starPrint(3);	
}
	
public static void starPrint(int num) {
		
        for(int i = 1; i <= num; i++) {
		    for(int j = 1; j <= i; j++) {
			    System.out.print("★");
		    }System.out.println();
	    }
    }
}

===================================================================
// 5 넣은 결과

public static void main(String[] args) {
		
		starPrint(5);	
}
	
public static void starPrint(int num) {
		
        for(int i = 1; i <= num; i++) {
		    for(int j = 1; j <= i; j++) {
			    System.out.print("★");
		    }System.out.println();
	    }
    }
}
```		
<br>

# 8. 아래의 함수를 만들고,해당함수를 호출하여 확인하시오
함수이름: getGrade() <br>
매개변수: double type 1개 <br>
리턴: 수 우 미 양 가 중 하나의 char 타입 <br>

```java

public static void main(String[] args) {
		
	getGrade(87.3);	
}
	
public static double getGrade(double num) {
		
	if(num >= 90) {
		System.out.println('수');
	}else if(num >= 80) {
		System.out.println('우');
	}else if(num >= 70) {
		System.out.println('미');
	}else if(num >= 60) {
		System.out.println('양');
	}else {
		System.out.println('가');
	}
		
	return num;
		
}
======================================================
▶ 우 
```
	
<br>

# 9. 매개변수 하나를 받아 원의 넓이를 리턴하는 함수를 작성하시오
```java
public static void main(String[] args) {
		
	circle(3.5);	
}
	
public static double circle(double num) {
		
	double circleRadius = num*2*Math.PI;

	System.out.println(circleRadius);
    
	return circleRadius;
}
=========================================
▶  21.991148575128552
``` 
<br>

# 10. 매개변수 두개를 받아, 사각형의 넓이를 리턴하는 함수를 작성하시오
```java
public static void main(String[] args) {

	square(2.5, 3.5);	
}
	
public static double square(double width, double height) {
		
    double squareArea = width*height;

	System.out.println(squareArea);

	return squareArea;
}
=========================================
▶  8.75
```
 
