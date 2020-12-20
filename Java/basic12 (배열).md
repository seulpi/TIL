# 배열 [Array]
 - 실무에서 더 좋은 게 있기 때문에 잘 쓰진 않지만 <br> 배열을 기반으로 캡슐화하해서 만든거기 때문에 잘 이해해두자 <br> 
 (코딩시험에는 나옴^^)

 - 배열은 1~3차원까지 존재(실무에선 2차원까지만 쓴다)
 - 배열의 목적 : 배열을 선언하고 값을 할당하고 쓴다  
 - 배열 Immutable
 
>> [ ] : 배열이구나~ 하고 알 수 있는 표식
```java
                     // ↓ int타입이 들어갈 5개의 연속된 방을 만들어라
    int[ ] ref = new int[5]; 
// ↑ int[]을 데이터타입으로 하는 ref 변수 

* ref 안에는 int[5]배열의 주소값이 할당 
(얘도 참조형이기 떄문에 4byte씩 메모리 크기 잡음)
```
```java

// 1. 기본형 배열
int [] ar1 = new int[5]; // ar1에 주소가 들어간다는것 자체가 ar1도 객체 (배열도 객체)
double[] ar2 = new double[7];
		
float[] ar3;

ar3 = new float[9];

System.out.println("배열 ar1 길이: " + ar1.length); // ar1객체이기 때문에 length 함수 호출 가능
System.out.println("배열 ar2 길이: " + ar2.length);
System.out.println("배열 ar3 길이: " + ar3.length);

=================================================================

// 2. 참조형 배열 (그림첨부)
Box[] ar = new Box[5]; Box의 주소를 가리키는 방 5개

객체생성할때 값 할당안해주면 인스턴스 변수에 대해서 기본적으로 컴파일러가 초기화 시켜줌 

따라서 배열도 객체이기때문에 값 할당안해주면 디폴트값이 들어가있음 (String - null / int&double - 0 / boolean - false)
```
▶ ![배열02](https://user-images.githubusercontent.com/74290204/101637737-7c3c3200-3a70-11eb-8877-31f19705c813.PNG)


## @ 배열 선언 & 초기화
>> 배열 선언 : int[] arr = new int[3];
<br>

- 배열 선언 방식에는 2가지의 방식 존재 
```java
1. int[] arr = new int[3];

2. int arr[] = new int[3];

//2번보다 1번이 논리적으로 더 맞음(2번은 참고로만 알고있자)
```

```java
>> 1차원 배열

//선언후 초기화 방법 1
int [] ar = new int[3];

ar[0] = 1;
ar[1] = 2;
ar[2] = 3 ;

int num = ar[0] +ar[1] + ar[2];

// 선언과 동시에 초기화 방법 1 
int[] arr = new int[] {1, 2, 3}; 

배열선언할때는 배열의 크기는 항상 정해줘야하는데 
값 3개를 동시에 넣어주면 컴파일러가 자동으로 크기를 저장 

// 선언과 동시에 초기화 방법 2
int[] arr = {1, 2, 3};

>> 2차원 배열 
int[][] arr = { {1, 2, 3}, {4, 5, 6}, {7, 8, 9}};
```
<br>

### ☞ 배열이 매개변수 자리에 온다면? 가능 핵가능
```java
public static void main(String[] args) { 

	int [] ar = {1, 2, 3, 4, 5, 6, 7};
	int sum = sumofAry(ar);			
}

	
static int sumofAry(int[] ar) { /* static인 이유! main에서 바로 써먹을라고, 
                                매개변수자리에 int[]가 온 이유는 ar의 데이터타입이 int[]이기 때문에 그렇다 */

	int sum = 0;

	for(int i = 0; i < ar.length; i++) {

		sum += ar[i];
	}
	return sum;
}

static int[] makeNewInrAry(int len) { // 배열 데이터타입은 반환형으로도 올 수 있음 

	int [] ar = new int [len];

	return ar;
}
```
<br>

### Q. 방이 7개인 배열 선언하고 할당된 값의 총 문자 개수를 출력해라
```java
String[] sr = new String[7];

sr[0] = new String("Java");
sr[1] = new String("System");
sr[2] = new String("Compiler");
sr[3] = new String("Park");
sr[4] = new String("Tree");
sr[5] = new String("Dinner");
sr[6] = new String("Brunch Cafe");

int cnum = 0;

for(int i = 0; i < sr.length; i++) { // 배열 객체의 length호출 , String[]를 데이터타입으로 하는 sr
	cnum += sr[i].length(); /* for문 안에 length와 다름 (반드시 구분할줄알아야함) 
                            length함수호출 sr[i]의 length함수 호출
                            String을 데이터타입으로 하는 sr[i] */
}
System.out.println("총 문자의 수: " + cnum);
=================================================================
▶ 총 문자의 수: 43
```
<br>

## 배열의 함수
### - fill / arraycopy함수
- fill : 할당해준 값을 배열에 채워서 넣는 함수 
- arraycopy : 배열을 복사하는 함수 
```java
public class ArrayUtils {
	public static void main(String[] args) { 
	    int[] ar1 = new int[10];
		int[] ar2 = new int[10];

		Arrays.fill(ar1, 7); // fill이란 함수는 ar1을 7로 채운다 
		System.arraycopy(ar1, 0, ar2, 3, 4);
        /* ar1, 0 -> ar1[0] :원본 , ar2(복사할대상)
        ar1[0]에 있는것을 ar2의 3번째부터 4개를 넣어라 */
		for(int i = 0; i < ar1.length; i++) {
			System.out.print(ar1[i]+ " ");
		}System.out.println(); 

		for(int i = 0; i < ar2.length; i++) {
			System.out.print(ar2[i] + " ");
		}
	}
}
```
<br>

### - enhanced for문(for-each) 
- 기존의 for문 보다 배열을 이용해 간결하게 사용 가능 
>> for(배열의 타입 변수명 : 배열명)
```java
// 기존 for문 이용

int[] ar = {1, 2, 3, 4, 5};

for(int i = 0; i < ar.length; i++) {
	System.out.println(ar[i]);
}

// enhanced for문 이용

int[] ar = {1, 2, 3, 4, 5};

for(ine e : ar) {
	System.out.println(e);
}
```
<br>

☞ enhanced for문 예제1)
```java
int[] ar = {1, 2, 3, 4, 5};

for(int e: ar) { 
    System.out.print(e + " ");
}
System.out.println(); // 개행

int sum = 0;

for(int e: ar) {
	sum += e;
}System.out.println("sum: " + sum);
=========================================
▶ 
1 2 3 4 5

sum: 15
```
<br>

☞ enhanced for문 예제2)
```java
// Main class
public static void main(String[] args) {	
	Box[] ar = new Box[5];

	ar[0] = new Box(101, "Coffee");
	ar[1] = new Box(202, "Computer");
	ar[2] = new Box(303, "Apple");
	ar[3] = new Box(404, "Dress");
	ar[4] = new Box(505, "Fairy-tale Book");

	for(Box e: ar) {
		if(e.getBoxNum() == 505) {
			System.out.println(e);
		}
	}
}
 

// Box class
public class Box {
	private int boxNum; // 메인에서 getBoxNum이 있으니까 변수명을 box으로 설정한것
	private String contents;

	Box(int num, String contents) {
		this.boxNum = num;
		this.contents = contents;
	}

	public int getBoxNum() {  // 이 단계까지만 있으면 문자열 출력안되고 주소값 출력
		return boxNum;
	}

	public String toString() { // 따라서 toString함수를 넣어줘야 원하는 문자열을 출력할 수 있음
		return contents;
	}
}
```
<br>

### Q. 배열을 이용해서 로또를 출력해라(단, 숫잔는 중복되지않게)
#### 로또 번호는 1~45
```java
>>  Main에서 했을때

public static void main(String[] args) {

	int[] lotto = new int[6];

	for(int i = 0; i < lotto.length; i++) { // 숫자를 배열에 넣는것
		lotto[i] = (int)(Math.random()*45 +1);
			
			for(int j = 0; j < i; j++) { // 숫자 중복제거를 하려면 이전의 숫자를 메모리에 저장을 해야 중복을 피할수 있다 
				if(lotto[j] == lotto[i]) { /* 배열이 다른게 아님 j는 index값! 
                                        따라서 중복된 숫자가 나오면 그 이전 배열 자체에서 랜덤 수를 다시 뽑는것 j가 뽑는게 아니고ㅍ */
					i--; 

					break;
				}
			}			
		}

		for(int k = 0; k < lotto.length; k++) { // 배열에 저장되어있는 숫자 출력
			System.out.print(lotto[k]+ " ");
		}


>> 클래스와 메인으로

// Lotto class
public class Lotto { // 로또의 총 번호는 1~45 랜덤으로 6개 출력 중복없이
	int[] lotto; //lotto 는 인스턴스 변수

	public Lotto() {
	}

	public int[] getLotto() {

		int[] lotto = new int[6]; // 위에 int[] lotto = new int[6];와는 완전히 다른것 여기 lotto는 지역변수
		for(int i = 0; i < lotto.length; i++) { 
			lotto[i] = (int)(Math.random()*45 +1);
			
            for(int j = 0; j < i; j++) { 
				if(lotto[j] == lotto[i]) {
					i--;
					break;
				}
			}
		}
		return lotto;
	}

	public void getLottoNum() {

		lotto = getLotto();

		for(int i=0; i < lotto.length; i++) {
			System.out.println(lotto[i]); 
		}
	}
}

//Main

public static void main(String[] args) {

	Lotto lotto = new Lotto();

	lotto.getLotto();

	lotto.getLottoNum();
}
```
## 2차원 배열 
>> int[행][열] arr2 = new int[3][4]; 
- 2차원배열은 이중for문으로 사용하는게 공식!

### ★ 2차원 배열의 실제구조 ★
- 행에 해당하는 방이 연속적으로 잡혀있고 방 안에는 열을 가리키는 주소를 담고 있다<br>
(1차원배열에서는 값이 들어간것과는 차이점이있다) <br>

▶ 그림참고 (**구조 꼭 암기하자**)
![2차원배열구조](https://user-images.githubusercontent.com/74290204/101637926-ba395600-3a70-11eb-8fad-b1a89f3f1aea.PNG)
<br>

```java
// 2차원 배열 구조의 이해
int[][] arr = {
	{11},
	{22, 33},
	{44, 55, 66}
};

for(int i = 0; i < arr.length; i++) {
	for(int j = 0; j < arr[i].length; j++) {
		System.out.print(arr[i][j] + "\t");
		}
	System.out.println();
	}
==============================================================
▶
11	
22	33	
44	55	66
```
<br>

### - 2차원 배열의 접근 
```java
int[][] arr = { {1, 2, 3}, {4, 5, 6}, {7, 8, 9};

* 1차원배열의 접근은 arr.length로 접근했는데(다이렉트로 집어넣지않고)

- 2차원배열의 접근은 ▼
for(int i = 0; i < arr.length; i++) //arr.length;행을 먼저
	for(int j = 0; j < arr[i].length; j++) // arr[i].length 행 안에 있는 열의 길이만큼
```
----
# ▶ 배열문제에서  <br>  1.최댓값&최소값 구하기 <br> 2.오름차순&내림차순 정렬  반드시 할 줄 알아야함





 

