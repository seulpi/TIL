# 1. 배열의 디폴트 초기화 방법은?
```java
>> 1차원 배열

// 방법1
int[] arr = new int[3]; // 배열 선언

int[0] = 1; // 초기화
int[1] = 2;
int[2] = 3;

[▼선언과 동시에 초기화]

// 방법2
int[] arr = new int[] {1, 2, 3}; 


//방법3
int[] att = {1, 2, 3};

>> 2차원 배열

int[][] arr  = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}};

int[][] arr2 = {{1}, {2,3}, {4, 5, 6}}; 
/* 행 안에 값의 개수를 달리해서 넣은것 이렇게 할 수 있는 이유는 
배열의 구조는 행안에 열을 참조하는 주소값이 먼저 들어오기 떄문 */
```
<br>

# 2. arraycopy 함수의 사용 방법은?
arraycopy는 배열을 복사하는 함수
```java
int[] arr1 = new int[5];
int[] arr2 = new int[5];

Arrays.fill(ar1, 7);

		<---------------->여기까지 index 시작위치
System.arraycopy(arr1, 0, arr2, 3, 1); // arr1배열의 0의 값을 arr2의 3번째부터 1개 넣어라

for(int i = 0; i<arr1.length; i++) {
    System.out.print(arr1[i]+ " ");
}Sytem.out.println()

for(int j = 0; j <arr2.length; j++) {
    System.out.print(arr2[j]+ " ");
}
=====================================================================
▶
77777
00010

*'0'으로 출력되는것은 arr2에 어떤값도 할당하지 않았기 때문에 int의 defalut값 '0'이 방 안에 있기 떄문
```
<br>

# 3. public static void main(String[] args) 에서 String[] args 의 사용법과 용도는?


# 4. enhenced for문에 대하여 설명하시오
배열의 저장된 모든 요소를 대상으로 연산, 참조 또는 탐색을 할 경우 간편하게 사용할 수 있는 for문
>>  for (타입 변수이름 : 배열이나컬렉션이름) { <br>
    배열의 길이만큼 반복적으로 실행하고자 하는 명령문; <br> } 
- 대입받을 변수의 데이터타입은 배열의 데이터타입과 일치해야함
```java
// 일반적인 for문 사용
int[] ar = {1, 2, 3, 4, 5}; 

for(int i = 0; i < ar.length; i++) {
	System.out.println(ar[i]);
}

// enhenced for문(for-each문) 사용
int[] ar = {1, 2, 3, 4, 5};

for(int e : ar) {
	System.out.println(e);
}

* 일반적인 for문보다 훨씨 간결하기 때문에 for문을 사용해 배열을 쓸 때는 유용하다

*단, enhenced for문은 배열만 사용가능하고 배열의 원본을 카피해서 
임시객체로 사용하는것이기 때문에 배열의 값을 변경하지못하고 
사용만 가능하다 (배열 초기화X)
```
<br>

# 5. 로또 프로그램을 작성하시오 * 必 *
```java
// Lotto class

public class Lotto {
	private int lottoNum;
	
	Lotto() {
		
	}
	
	public void getLottoNum() {
		int[] lotto = new int[6];
		
		for(int i = 0; i < lotto.length; i++) {
			lotto[i] = (int)(Math.random()*45 +1);
			
			for(int j = 0; j < i; j++) {
				if(lotto[j] == lotto[i]) {
					i--;
					break;
				}
			}
		}
		for(int i = 0; i < lotto.length; i++) {
			System.out.print(lotto[i]+ " ");
		}
	}	
}

// Main class 

public static void main(String[] args) {
	Lotto lotto = new Lotto();
	lotto.getLottoNum();
}
```
<br>

# 6. 아래의 프로그램을 참고 하여 Box class 를 짜시오
```java
public static void main(String[] args) {
	Box[] ar = new Box[5];
	ar[0] = new Box(101, "Coffee");
	ar[1] = new Box(202, "Computer");
	ar[2] = new Box(303, "Apple");
	ar[3] = new Box(404, "Dress");
	ar[4] = new Box(505, "Fairy-tale book");

	for (Box e : ar) {
		if (e.getBoxNum() == 505)
			System.out.println(e);
	}
}
```
```java
public class Box {
	private int boxNum;
	private String cname;
	
	Box() {
		
	}
	
	Box(int boxNum, String cname) {
		this.boxNum = boxNum;
		this.cname = cname;
	}
	
	public int getBoxNum() {
		return boxNum;
	}
	
	public String toString() {
		return cname;
	}
}
```

# 7. 양의 정수 10개를 랜덤생성하여 배열에 저장하고 배열에 있는 정수 중에서 3의 배수만 출력해보자
```java
// Num class

public class Num {
	private int plusNum; 
	
	Num() {
		
	}
	
	Num(int plusNum) {
		this.plusNum = plusNum;
	}
	
	public void numRandom() {
		int[] ar = new int[10];
		
		for(int i = 0; i <ar.length; i++) {
			ar[i] = (int)(Math.random()*100 +1);
		}
		
		for(int e: ar) {
			if(e % 3 == 0) {
				System.out.print(e + " ");
			}
	}
}

// Main class

public static void main(String[] args) {
		
		Num num = new Num();
		num.numRandom();
}
```
<br>

# 8. 아래의 프로그램을 짜시오 * 必 *
## - 5개의 숫자를 랜덤으로 받아 배열로 저장 <br> -5개의 숫자중 가장 큰값을 출력
```java
// Num class
public class Num {
	private int num; 
	
	Num() {
		
	}
	
	Num(int num) {
		this.num = num;
	}
	
	public void maxNum() {
		int[] ar = new int[5];
		
		for(int i = 0; i <ar.length; i++) {
			ar[i] = (int)(Math.random()*100 +1);
				 
		}
		
		int max = 0; // int max = ar[0]; 으로 시작해도됨 그럼 밑에 for문 int = 1부터 시작 
	
		for(int i = 0; i < ar.length; i++) {
			if(max < ar[i]) {
				max = ar[i];
			}
		}
		System.out.println(max);
}

// Main class
public static void main(String[] args) {
		
		Num num = new Num();
		num.maxNum();
}
```
▶teacher solution
```java
>> 나와 다른 포인트는 static 함수 사용!

public class Static {
 public static void main(String[] args) {
	 int[] ar = new int[5];
	 for(int i = 0; i <ar.length; i++) {
		ar[i] = (int)(Math.random()*100 +1);
		System.out.println(ar[i] + " ");
	 }
	 System.out.println();
	 int max = ArrUtil.maxArr(ar);
	 System.out.println(max);	
 	}
}

class ArrUtil {
	public static int maxArr(int[]ar) { //static 포인트 배열 바뀌어도 맥스값만 뽑아내면되니까
		if(ar == null) {
			System.out.println("배열이 없습니다");
			return 0; // 리턴값이 정수니까 일단 0으로 리턴 
		}
		
		int max = ar[0];

		for(int i = 1; i < ar.length; i++) {
			if(max < ar[i]) {
				max = ar[i];
			}
		}
		
		return max;
	}
}
```

# 9. 아래의 프로그램을 짜시오
## -5개의 숫자를 랜덤으로 받아 배열로 저장 <br> -5개의 숫자를 내림차순으로 정렬하여 출력
```java
// Num class
import java.util.Arrays;

public class Num {
	private int num; 
	
	Num() {
		
	}
	
	Num(int num) {
		this.num = num;
	}
	
	public void numRandom() {
		
		int[] ar = new int[5];
	
		for(int i = 0; i <ar.length; i++) {
			ar[i] = (int)(Math.random()*100 +1);		 
		}
		
		Arrays.sort(ar,0,5); // 배열을 정리해주는 함수 → ar 배열의 0부터 4까지 오름차순으로 정렬
		
		for(int i = ar.length-1; i >= 0; i--) {
			System.out.println(ar[i] + " " );
		}
	}
}


// Main class
public static void main(String[] args) {
		
		Num num = new Num();
		num.numRandom();
}
```
<br>

# 10. char 배열을 생성하여, 알파벳 A~Z까지 대입 후, 출력해보자. 알파벳은 26개
```java
public class TestMain {

	public static void main(String[] args) {
		char[] alpha = new char[26];
		
		for(int i = 0; i < alpha.length; i++) {
			alpha[i] = (char)(65+i); // 형변환 하기 'A'가 아스키코드값으로 65니까 차례대로
		}
		
		for(int i = 0; i < alpha.length; i++) {
			System.out.print(alpha[i]+ " ");
		}
	}
}
```
<br>

# 11. 배열과 반복문을 이용해 아래의 프로그래밍을 작성하시오
```java
키보드에서 정수로 된 돈의 액수를 입력받아 오만 원권, 만 원권, 천 원권, 500원짜리 동전, 
100원짜리 동전, 50원짜리 동전, 10원짜리 동전, 1원짜리 동전이 각 몇 개로 변환되는지 예시와 같이 출력하라 
이때 반드시 다음 배열을 이용하고 반복문으로 작성하라.


int[] unit = {50000, 10000, 1000, 500, 100, 50, 10, 1}; // 환산할 돈의 종류

금액을 입력하시오 >> 65123
50000 원 짜리 : 1개 
10000 원 짜리 : 1개 
1000 원 짜리 : 5개 
500 원 짜리 : 0개 
100 원 짜리 : 1개 
50 원 짜리 : 0개 
10 원 짜리 : 2개 
1 원 짜리 : 3개 
```
```java
// Money class
public class Money {

	private int[] unit;
	private int userMoney;
	
	Money() {
		
	}
	
	Money(int userMoney) {
		this.userMoney = userMoney;
	}
	
	public void userInput() {
		Scanner sc = new Scanner(System.in);
		System.out.println("금액을 입력하세요: ");
		userMoney = sc.nextInt();
	}
	
	public void moneyArray() {
		int[] unit = {50000, 10000, 5000, 1000, 500, 100, 5, 1};
		
		for(int i = 0; i < unit.length; i++) {
			System.out.println(unit[i] + "원 짜리: " + (userMoney/unit[i]) + "개");
			userMoney = (userMoney % unit[i]);
		}

// Main class
public static void main(String[] args) {
		
	Money money = new Money();
	money.userInput();
	money.moneyArray();	
}
```
<br>

# 12. 정수를 10개 저장하는 배열을 만들고 <br>1에서 10까지 범위의 정수를 랜덤하게 생성하여 배열에 저장하라<br>그리고 배열에 든 숫자들과 평균을 출력하라 * 必 *
```java
// ImportanrNum class
public class ImportanrNum {
	
	private int[] arr = new int[10];
	private double sum;
	
	ImportanrNum() {	
	}
	
	public void numRandom() {
		
		for(int i = 0; i <arr.length; i++) {
			arr[i] = (int)(Math.random()*10 +1);
			
		} System.out.println();
	}
	
	public void numAvg() {
		System.out.print("랜덤한 정수들: ");
		for(int i = 0; i < arr.length; i++) {
		System.out.print(arr[i] + " ");
		sum += arr[i]; 
		}
		System.out.println();
		System.out.println("평균은: " + sum/10.0);/* sum/10.0 → sum/arr.length 이게 정석
							(왜냐면 다이렉트로 값 넣으면 배열 바뀔때마다 여기를 계속 바꿔줘야하니까) */
	}
}


// Main class
public static void main(String[] args) {
	ImportanrNum num = new ImportanrNum();
	num.numRandom();
	num.numAvg();
}
```
<br>

# 13. 4 x 4의 2차원 배열을 만들고 이곳에 1에서 10까지 범위의정수를 랜덤하게 생성하여 정수 16개를 배열에 저장하고 <br> 차원 배열을 화면에 출력하라 * 必 *
```java
8 6 1 1 
7 3 6 9 
4 5 3 7 
9 6 3 1
```
```java
//SecondArray class 
public class SecondArray {
	
	private int[][] ar = new int[4][4];  // 클래스로 만들때 이렇게 잘 사용 안함 밑에 teacher 참고! 
	
	
	public void secondRandom() {
		for(int i = 0; i < ar.length; i++) {
					↓--------------틀림
			for(int j = 0; j < ar.length; j++) { // for(int j = 0; j < ar[i].length; j++) 이게 맞음ㅎㅎ..
				ar[i][j] = (int)(Math.random()*10 +1);
			}
			
		}
	}
	
	public void secondPrint() {
		for(int i = 0; i < ar.length; i++) {
					↓--------------틀림
			for(int j = 0; j < ar.length; j++) { //for(int j = 0; j < ar[i].length; j++) 이게 맞음ㅎㅎ..
				System.out.print(ar[i][j]+ " ");
			}System.out.println();
		}
	}
}

// Main clas 
public static void main(String[] args) {
	SecondArray array = new SecondArray();
	array.secondRandom();
	array.secondPrint();
}
```
▶ teacher참고(배열을 클래스에서 구현할때 사용법)
```java
>> 상수와 배열의 초기화값을 함수로 나눠서 선언
final int ROWS = 4; 
final int COLS = 4; // 배열의 행열을 상수로 선언
	 
int[][] ar; // 배열의 선언
	 
Static { // 인스턴스 함수 배열의 초기화 (위에서 선언한 상수값)
	ar = new int[ROWS][COLS];
}

밑 프로그래밍 코드는 위와 동일
```
<br>

# 14. 아래를 메모리 구조로 표현하시오
## int[][] arr = new int[3][4]
![2차원배열구조](https://user-images.githubusercontent.com/74290204/101623767-41c89a00-3a5c-11eb-8799-14809525797d.png)
